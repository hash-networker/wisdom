---
title: ERSPAN to PCAP script
permalink: /ERSPAN_to_PCAP_script/
---

6500s can SPAN (port or vlan mirror) to GRE encapsulation, allowing mirroring over multiple layer3 hops. Very useful

This feature is ESPECIALLY useful on 6500s - see [6500 SPAN the RP](/6500_SPAN_the_RP "wikilink") - allowing you to use the \*hardware\* to mirror the traffic that goes to the CPU

The following script runs on Linux (and possibly other Unices). It captures the GRE traffic and either outputs a summary or, if the -pcap argument is given, outputs an ethernet-style PCAP data stream (which can be piped to "tcpdump -r -")

This script is Licensed under the GPL version 2 ONLY

The script:


    #!/usr/bin/python
    #
    # Copyright 2007 - Phil Mayers phil dot mayers at gmail dot com
    # This software is licensed under the GPL version 2 ONLY

    # For the latest public version, check:
    #  http://cisco.cluepon.net/index.php/ERSPAN_to_PCAP_script

    #    This program is free software; you can redistribute it and/or modify
    #    it under the terms of version 2 of the the GNU General Public License
    #    as published by the Free Software Foundation
    #
    #    This program is distributed in the hope that it will be useful,
    #    but WITHOUT ANY WARRANTY; without even the implied warranty of
    #    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    #    GNU General Public License for more details.
    #
    #    You should have received a copy of the GNU General Public License
    #    along with this program; if not, write to the Free Software
    #    Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301  USA

    # This program opens a GRE socket and received Cisco ERSPAN (port mirror
    # over GRE) packets, and either outputs a one-line summary or if the -pcap
    # command line argument is given, outputs an "ethernet" pcap stream which
    # can be written to a file, or piped to "tcpdump -r -"

    # This program contains a number of assumptions that are probably only valid
    # in simple cases. There are a number of fields in the ERSPAN header that
    # undoubtedly indicate non-ethernet packets (e.g. for POS cards on 7600s)
    # but I don't have the hardware to test it, so can't verify that. It would
    # be wonderful if Cisco documented (or even RFCed!) the ERSPAN format. Then,
    # wireshark could implement it natively and it would all be good



    from socket import *
    import struct
    import time
    import sys

    # magic, major_ver, minor_ver, gmt_offset, sigfigs, snaplen, captype
    PCAP_FILE_MAGIC = 0xa1b2c3d4L
    PCAP_FILE_MAJOR = 2
    PCAP_FILE_MINOR = 4
    PCAP_FILE_HEADER = "IHHiIII"
    PCAP_FILE_HEADER_LEN = struct.calcsize(PCAP_FILE_HEADER)
    PCAP_RECORD_HEADER = "llII"
    PCAP_RECORD_HEADER_LEN = struct.calcsize(PCAP_RECORD_HEADER)

    def parseIP(d):
            # parse an IP packet
            b0 = ord(d[0])
            version = b0 >> 4
            if version!=4:
                    return {}
            hlen = b0 & 0xf
            hlen = hlen * 4
            if hlen<20:
                    return {}
            tos, dlen, id, flags, ttl, protocol, cksum = struct.unpack('>BHHHBBH', d[1:12])
            src, dst = [inet_ntoa(x) for x in (d[12:16], d[16:20])]
            offset = flags & 0x1fff
            df = flags & 0x4000
            mf = flags & 0x2000
            if hlen>20:
                    options = d[20:hlen-20]
            else:
                    options = ''
            payload = d[hlen:]
            del d
            return locals()

    s = socket(AF_INET, SOCK_RAW, IPPROTO_GRE)
    if '-pcap' in sys.argv[1:]:
            pcap = True
            sys.stdout.write(struct.pack(PCAP_FILE_HEADER, PCAP_FILE_MAGIC, PCAP_FILE_MAJOR, PCAP_FILE_MINOR, 0, 0, 65535, 1)) #pcap.DLT_EN10MB))
    else:
            pcap = False

    while True:
            d, (ssrc ,port) = s.recvfrom(65535)
            now = time.time()
            sec = int(now)
            usec = int((now-sec)*1000000)
            pkt = parseIP(d)
            if not pkt:
                    continue
            elif pkt['protocol']!=47:
                    continue
            elif len(pkt['payload'])<8+14+20:
                    # unknown+ether+ip
                    continue
            # GRE
            hdr, data = pkt['payload'][:4], pkt['payload'][4:]
            f, proto = struct.unpack('>HH', hdr)
            f_1 = (f & 0xf000) >> 12
            f_2 = (f & 0x0f00) >> 8
            flg = (f & 0x00ff) >> 5
            ver = (f & 0x0007)

            if f_1 & 0x8:
                    cksum_present = True
            else:
                    cksum_present = False

            if f_1 & 0x4:
                    routing_present = True
            else:
                    routing_present = False

            if f_1 & 0x2:
                    key_present = True
            else:
                    key_present = False

            if f_1 & 0x1:
                    seq_present = True
            else:
                    seq_present = False

            if cksum_present or routing_present:
                    cksum, offset = struct.unpack('>HH', data[:4])
                    data = data[4:]
            else:
                    cksum = 0
                    offset = 0

            if key_present:
                    key = struct.unpack('>I', data[:4])
                    data = data[4:]
            else:
                    key = 0
            if seq_present:
                    seq, = struct.unpack('>I', data[:4])
                    data = data[4:]
            else:
                    seq = 0


            unknown, rest = data[:8], data[8:]
            if pcap:
                    sys.stdout.write(struct.pack(PCAP_RECORD_HEADER, sec, usec, len(rest), len(rest))+rest)
                    sys.stdout.flush()
                    continue
            ethdst = ':'.join(['%02x' % ord(x) for x in rest[:6]])
            ethsrc = ':'.join(['%02x' % ord(x) for x in rest[6:12]])
            ethtype = ( ord(rest[12]) << 8 ) + ord(rest[13])
            inner_payload = rest[14:]
            if ethtype==0x800:
                    inner_pkt = parseIP(inner_payload)
                    if not inner_pkt:
                            msg = '- - -'
                    else:
                            msg = '%(dst)s %(src)s %(protocol)i' % inner_pkt
            else:
                    msg = '* * *'

            print ssrc, seq, ':'.join(['%02x' % ord(x) for x in unknown]), ethdst, ethsrc, hex(ethtype), msg