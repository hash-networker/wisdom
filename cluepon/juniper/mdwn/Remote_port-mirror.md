---
title: Remote port-mirror
permalink: /Remote_port-mirror/
---

This procedure requires a PIC or router that is capable of GRE tunneling. **It can also cause DoS-type consequences if you don't know what you're doing.** Packet capturing software such as tcpdump, ethereal, or wireshark can be run on a remote \*NIX or Linux box. In order for this to work, the remote capture server **must not** have a route to the packets that are being captured.

Router Configuration
====================

1.  Configure a GRE tunnel interface. (note the family inet address is required, even if not used (See alternate capture server configuration)
        interfaces {
            gr-0/2/0 {
                unit 100 {
                    tunnel {
                        source 192.168.50.10;
                        destination 192.168.51.20;
                        ttl 10;
                    }
                    family inet {
                        address 172.16.28.1/30;
                    }
                }
            }
        }

2.  Configure forwarding. Your rate and run-length should take into account the amount of traffic being monitored.
        forwarding-options {
            port-mirroring {
                family inet {
                    input {
                        rate 1;
                        run-length 1;
                    }
                    output {
                        interface gr-0/2/0.100;
                        no-filter-check;
                    }
                }
            }
        }

3.  Configure a filter and apply it to an interface.
        interfaces {
            t1-1/0/3:3 {
                description Customer-A;
                unit 0 {
                    family inet {
                        filter {
                            output capture-filter;
                        }
                    address 192.168.35.1/30;
                }
            }
        }
        ...
        routing-options {
            static {
                route 192.168.70.0/24 next-hop 192.168.35.2;
            }
        }
        ...
        firewall {
            family inet {
                filter capture-filter {
                    term 0-capture {
                        from {
                            source-address {
                                192.168.70.0/24;
                                192.168.35.0/30;
                            }
                        }
                        then {
                            port-mirror;
                            accept;
                    }
                    term 5-accept_all {
                        then accept;
                    }
                }
            }
        }

Setting up a tunnel interface on an MX DPC
==========================================

1.  On MX series DPCs, you need to allocate a tunnel-service - for this example the interface will be gr-0/3/10:
        fpc 0 {
            pic 3 {
                tunnel-services {
                    bandwidth 1g;
                }
            }
        }

Capture Server Configuration
============================

1.  Remove your default route.
        [root@foo]# route del -net default

2.  Configure a GRE tunnel.
        [root@foo]# modprobe ip_gre
        [root@foo]# ifconfig eth0 192.168.51.20 netmask 255.255.255.0 up
        [root@foo]# ip tunnel add netb mode gre remote 192.168.50.10 local 192.168.51.20 ttl 255
        [root@foo]# ip link set netb up
        [root@foo]# ip addr add 172.16.28.2/30 dev netb

3.  Configure a route to the originating router.
         [root@foo]# route add -net 192.168.50.10 netmask 255.255.255.255 gw 192.168.51.1

4.  Attempt to ping across the tunnel.
        [root@foo]# ping 172.16.28.1
        PING 172.16.28.1 (172.16.28.1) 56(84) bytes of data.
        64 bytes from 172.16.28.1: icmp_seq=0 ttl=64 time=0.587 ms
        64 bytes from 172.16.28.1: icmp_seq=1 ttl=64 time=0.685 ms
        64 bytes from 172.16.28.1: icmp_seq=2 ttl=64 time=0.611 ms
        64 bytes from 172.16.28.1: icmp_seq=3 ttl=64 time=0.595 ms

        4 packets transmitted, 4 received, 0% packet loss, time 3010ms
        rtt min/avg/max/mdev = 0.587/0.619/0.685/0.046 ms, pipe 2

5.  Start the traffic capture.
        [root@foo]# tethereal -i netb
        tethereal: arptype 778 not supported by libpcap - falling back to cooked socket.

        Capturing on netb
         0.000000 192.168.70.5 -> 10.0.45.8 UDP Source port: 49378  Destination port: 18252
         0.004236 192.168.70.5 -> 10.0.45.8 UDP Source port: 49380  Destination port: 16968
         0.020132 192.168.70.5 -> 10.0.45.8 UDP Source port: 49378  Destination port: 18252
         0.024303 192.168.70.5 -> 10.0.45.8 UDP Source port: 49380  Destination port: 16968
         0.040096 192.168.70.5 -> 10.0.45.8 UDP Source port: 49378  Destination port: 18252
         0.044314 192.168.70.5 -> 10.0.45.8 UDP Source port: 49380  Destination port: 16968
         ^C
        7 packets captured
        [root@foo]#

Capture Server Configuration (Alternate)
========================================

As most tools (tcpdump/tshark) are able to "look through" the GRE, a simpler server configuration is possible. No modification to the routing table is required, as the GRE is directed at the host, which is configured to drop them. tcpdump/tshark/etc can be used to sniff the appropriate traffic from the relevant interface without requiring anything further from the capture server.

1.  Drop inbound GRE to prevent ICMP unreachable response generation.
        [root@foo]# iptables -A INPUT -p 47 -j DROP

2.  Start the traffic capture.

        [root@foo]# tcpdump -nn -i <interface> host <GRE source IP> and ip proto 47
        10:11:15.842147 IP 192.168.50.10 > 192.168.51.20: GREv0, length 88: IP 192.168.70.5 > 10.0.45.8: ICMP echo request, id 60517, seq 8, length 64
        10:11:16.843036 IP 192.168.50.10 > 192.168.51.20: GREv0, length 88: IP 192.168.70.5 > 10.0.45.8: ICMP echo request, id 60517, seq 9, length 64
        2 packets captured
        2 packets received by filter
        0 packets dropped by kernel

        [root@foo]# tshark -nn -i <interface> host <GRE source IP> and ip proto 47
          0.000000 192.168.70.5 -> 10.0.45.8 ICMP Echo (ping) request
          1.000552 192.168.70.5 -> 10.0.45.8 ICMP Echo (ping) request
        2 packets captured

In this example tshark (tethereal) doesn't even show the outer GRE encap, only showing the inner payload.

[Category:Unsupported activities](/Category:Unsupported_activities "wikilink")