---
title: Using capture buffer with ELAM
permalink: /Using_capture_buffer_with_ELAM/
---

What can ELAM do for me?
========================

Trace traffic as it moves through ASIC rewrite operations (Superman/Tycho etc..)

What do I need for it?
======================

    service internal

Help
====

    65k#show platform capture elam help

    If you've reached this point, you're in trouble...
    Well, ELAM can't do miracles, but can help you solve issues...

    To make ELAM working for you, here the basic steps you've to follow:
    1 - select a valid ELAM ASIC
        cmd:     show platform capture elam asic
        example: ...asic superman slot 5
                 - select the Superman ASIC on slot 5
    2 - enter a valid trigger
        cmd:     show platform capture elam trigger
        example: ...trigger dbus ipv4 if ip_da=224.1.0.0[255.255.0.0]
                 - capture any IPv4 packet with DESTIP == 224.1.0.0/16
                 ...trigger rbus ip if ccc=mcast_l3_rw met3=0x0300[0xff00]
                 - capture any IP packet generating a ML3_RW result with
                   MET3 pointer equal to 3xx
                 ...trigger dbus help
                 - return an extensive list of dbus field you can use
    3 - start the capture
        cmd:     show platform capture elam start
    4 - check the capture status
        cmd:     show platform capture elam status
    5 - print the captured data
        cmd:     show platform capture elam data

    simple enough, isn't it ?

Example from c-nsp
==================


    R3#sh ip int br
    Interface IP-Address OK? Method Status Protocol
    GigabitEthernet2/10 192.168.10.1 YES manual up up

    R3#sh ver
    Cisco IOS Software, c7600s3223_rp Software (c7600s3223_rp-ADVIPSERVICES-M), Version 12.2(33)SRC1, RELEASE SOFTWARE (fc1)
    Technical Support: http://www.cisco.com/techsupport
    Copyright (c) 1986-2008 by Cisco Systems, Inc.
    Compiled Fri 23-May-08 01:37 by prod_rel_team

    ROM: System Bootstrap, Version 12.2(17r)SX3, RELEASE SOFTWARE (fc1)

    R3 uptime is 1 week, 6 days, 5 hours, 55 minutes
    Uptime for this control processor is 1 week, 6 days, 5 hours, 56 minutes
    System returned to ROM by power-on (SP by power-on)
    System image file is "bootdisk:c7600s3223-advipservices-mz.122-33.SRC1"
    Last reload type: Normal Reload

    cisco CISCO7606 (R7000) processor (revision 1.0) with 458752K/65536K bytes of memory.
    Processor board ID FOX1221GAQF
    R7000 CPU at 300Mhz, Implementation 0x27, Rev 3.3, 256KB L2, 1024KB L3 Cache
    Last reset from power-on
    1 Virtual Ethernet interface
    33 Gigabit Ethernet interfaces
    2 Ten Gigabit Ethernet interfaces
    1915K bytes of non-volatile configuration memory.


    R3#sh ip arp
    Protocol Address Age (min) Hardware Addr Type Interface
    Internet 192.168.10.1 - 001b.0def.7240 ARPA GigabitEthernet2/10
    Internet 192.168.10.2 175 001e.be8a.f880 ARPA GigabitEthernet2/10
    Internet 192.168.10.9 11 001b.0def.7280 ARPA GigabitEthernet2/3
    Internet 192.168.10.10 - 001b.0def.7240 ARPA GigabitEthernet2/3


    R3#ping 192.168.10.2

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.10.2, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/4 ms

    R3#config t
    Enter configuration commands, one per line. End with CNTL/Z.
    R3(config)#serv int
    R3(config)#end

    R3#sh mod
    *Sep 25 18:10:37.377: %SYS-5-CONFIG_I: Configured from console by console

    R3#sh mod | incl Act
    5 3 Supervisor Engine 32 10GE (Active) WS-SUP32-10GE-3B SAL1051BEKF

    R3#show plat capture elam asic super slot 5

    R3#$show plat elam trigger dbus ipv4 if IP_SA=192.168.10.1 IP_DA=192.168.10.2

    R3#show plat capture elam status
    active ELAM info:
    Slot Cpu Asic Inst Ver PB Elam
    ---- --- -------- ---- --- -- ----
    5 0 SUPERMAN 0 1.3 Y
    DBUS trigger: FORMAT=IP L3_PROTOCOL=IPV4 IP_SA=192.168.10.1 IP_DA=192.168.10.2

    R3#show plat cap elam start

    R3#show plat capture elam status
    active ELAM info:
    Slot Cpu Asic Inst Ver PB Elam
    ---- --- -------- ---- --- -- ----
    5 0 SUPERMAN 0 1.3 Y
    DBUS trigger: FORMAT=IP L3_PROTOCOL=IPV4 IP_SA=192.168.10.1 IP_DA=192.168.10.2
    elam capture in progress


    R3#!send a ping
    R3#ping 192.168.10.2 rep 1

    Type escape sequence to abort.
    Sending 1, 100-byte ICMP Echos to 192.168.10.2, timeout is 2 seconds:
    !
    Success rate is 100 percent (1/1), round-trip min/avg/max = 1/1/1 ms


    R3#show plat capture elam status
    active ELAM info:
    Slot Cpu Asic Inst Ver PB Elam
    ---- --- -------- ---- --- -- ----
    5 0 SUPERMAN 0 1.3 Y
    DBUS trigger: FORMAT=IP L3_PROTOCOL=IPV4 IP_SA=192.168.10.1 IP_DA=192.168.10.2
    elam capture completed

    R3#!elam completed

    R3#show plat cap elam data
    DBUS data:
    SEQ_NUM [5] = 0x3
    QOS [3] = 0
    QOS_TYPE [1] = 0
    TYPE [4] = 0 [ETHERNET]
    STATUS_BPDU [1] = 0
    IPO [1] = 1
    NO_ESTBLS [1] = 0
    RBH [3] = b000
    CR [1] = 0
    TRUSTED [1] = 0
    NOTIFY_IL [1] = 0
    NOTIFY_NL [1] = 0
    DISABLE_NL [1] = 0
    DISABLE_IL [1] = 0
    DONT_FWD [1] = 0
    INDEX_DIRECT [1] = 0
    DONT_LEARN [1] = 0
    COND_LEARN [1] = 0
    BUNDLE_BYPASS [1] = 0
    QOS_TIC [1] = 0
    INBAND [1] = 0
    IGNORE_QOSO [1] = 0
    IGNORE_QOSI [1] = 1
    IGNORE_ACLO [1] = 0
    IGNORE_ACLI [1] = 1
    PORT_QOS [1] = 0
    CACHE_CNTRL [2] = 0 [NORMAL]
    VLAN [12] = 1030
    SRC_FLOOD [1] = 0
    SRC_INDEX [19] = 0x380
    LEN [16] = 118
    FORMAT [2] = 0 [IP]
    MPLS_EXP [3] = 0x0
    REC [1] = 0
    NO_STATS [1] = 0
    VPN_INDEX [10] = 0x0
    PACKET_TYPE [3] = 0 [ETHERNET]
    L3_PROTOCOL [4] = 0 [IPV4]
    L3_PT [8] = 1 [ICMP]
    MPLS_TTL [8] = 0
    SRC_XTAG [4] = 0x0
    DEST_XTAG [4] = 0x0
    FF [1] = 0
    MN [1] = 0
    RF [1] = 0
    SC [1] = 0
    CARD_TYPE [4] = 0x0
    DMAC = 001e.be8a.f880
    SMAC = 001b.0def.7240
    IPVER [1] = 0 [IPV4]
    IP_DF [1] = 0
    IP_MF [1] = 0
    IP_HDR_LEN [4] = 5
    IP_TOS [8] = 0x0
    IP_LEN [16] = 100
    IP_HDR_VALID [1] = 1
    IP_CHKSUM_VALID [1] = 1
    IP_L4HDR_VALID [1] = 1
    IP_OFFSET [13] = 0
    IP_TTL [8] = 255
    IP_CHKSUM [16] = 0x2640
    IP_SA = 192.168.10.1
    IP_DA = 192.168.10.2
    ICMP_TYPE [8] = 0x8
    ICMP_CODE [8] = 0x0
    ICMP_DATA [104]
    0000: 8D CD 00 01 00 00 00 00 00 00 3B AB CD "..........;.."
    CRC [16] = 0x0

    RBUS data:
    SEQ_NUM [5] = 0x3
    CCC [3] = b100 [L3_RW]
    CAP1 [1] = 0
    CAP2 [1] = 0
    QOS [3] = 0
    EGRESS [1] = 0
    DT [1] = 0 [IP]
    TL [1] = 0 [B32]
    FLOOD [1] = 1
    DEST_INDEX [19] = 0x406
    VLAN [12] = 1030
    RBH [3] = b011
    RDT [1] = 0
    GENERIC [1] = 0
    EXTRA_CICLE [1] = 0
    FABRIC_PRIO [1] = 0
    L2 [1] = 1
    FCS1 [8] = 0x1
    IP_TOS_VALID [1] = 1
    IP_TOS_OFS [7] = 15
    IP_TOS [8] = 0x0
    IP_TTL_VALID [1] = 0
    IP_TTL_OFS [7] = 22
    IP_TTL [8] = 255
    IP_CSUM_VALID [1] = 1
    IP_CSUM_OFS [7] = 24
    IP_CSUM [16] = 0x2640
    DELTA_LEN [8] = 0
    REWRITE_INFO
    i0 - no rewrite.
    FCS2 [8] = 0x5B

    R3#sh vlan internal us | incl 1030
    1030 GigabitEthernet2/10

    R3#sh ip ro 192.168.10.2
    Routing entry for 192.168.10.0/30
    Known via "connected", distance 0, metric 0 (connected, via interface)
    Routing Descriptor Blocks:
    * directly connected, via GigabitEthernet2/10
    Route metric is 0, traffic share count is 1

    R3#!that was the frame on egress from the box

    R3#sh ver | incl IOS
    Cisco IOS Software, c7600s3223_rp Software (c7600s3223_rp-ADVIPSERVICES-M), Version 12.2(33)SRC1, RELEASE SOFTWARE (fc1)

    R3#

Match by data
=============

Most elam capture options provide a way to mask out some information. In general, OPT=data\[mask\] is the form you want to use. One very useful form of this is to match a packet based on the data the packet contains. For example, there's no pre-cooked way to capture ARP packets (as of SXF15, the newest I've used). Here's a way to match, courtesy of TAC, to catch ARP packets from 10.0.0.1 (note that 10.0.0.1 in hex is 0xa0000001):

    router#show platform capture elam trigger dbus others if data=0 0 0 0x08060000 0 0 0 0xA0000001 0 0 0 [ 0 0 0 0xffff0000 0 0 0 0xffffffff 0 0 0 ]