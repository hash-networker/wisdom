---
title: L3VPN
permalink: /L3VPN/
---

Network Diagram
---------------

        f0   e1 e0   e0 e1  e1   e0  f0
    [CE1] -- [PE1] -- [P] -- [PE2] -- [CE2]
    (IOS)     (       JunOS       )   (IOS)

Configs
-------

### CE1

    version 12.0
    !
    hostname ce1
    !
    !
    ip subnet-zero
    !
    interface FastEthernet1/0
     description connected to WANProvider for Managed WAN
     ip address 192.168.90.1 255.255.255.252
     no ip directed-broadcast
     no ip proxy-arp
     speed 100
     full-duplex
    !
    ip classless
    ip route 192.168.90.4 255.255.255.252 192.168.90.2
    !
    !
    !
    line con 0
    line aux 0
    line vty 0 4
     login
    !

### PE1

    interfaces {
        em0 {
            mtu 1526
            unit 0 {
                description "Connected to p1 em0";
                family inet {
                    address 192.168.1.2/30;
                }
                family iso;
                family mpls;
            }
        }
        em1 {
            unit 0 {
                description "To customer for Managed WAN";
                family inet {
                    address 192.168.90.2/30;
                }
            }
        }
        lo0 {
            unit 0 {
                family inet {
                    address 10.0.0.1/32;
                }
                family iso {
                    address 49.1234.0200.0100.0000.0001.00;
                }
            }
        }
    }
    routing-options {
        autonomous-system 1234;
    }
    protocols {
        mpls {
            icmp-tunneling;
            interface em0.0;
            interface lo0.0;
        }
        bgp {
            log-updown;
            family inet {
                unicast;
            }
            family inet-vpn {
                unicast;
            }
            export INET-REDISTRIBUTE;
            remove-private;
            group internal {
                type internal;
                local-address 10.0.0.1;
                peer-as 1234;
                neighbor 10.0.0.2;
                neighbor 10.0.0.3;
            }
        }
        isis {
            level 1 disable;
            level 2 wide-metrics-only;
            interface em0.0;
            interface lo0.0 {
                passive;
            }
        }
        ldp {
            interface em0.0;
            interface lo0.0;
        }
    }
    policy-options {
        policy-statement INET-REDISTRIBUTE {
            term connected {
                from protocol direct;
                then accept;
            }
            term static {
                from protocol static;
                then accept;
            }
            term local {
                from protocol local;
                then accept;
            }
            term else {
                then reject;
            }
        }
    }
    routing-instances {
        VRFA {
            instance-type vrf;
            interface em1.0;
            route-distinguisher 1234:1;
            vrf-target target:1234:1;
        }
    }

### P1

    interfaces {
        em0 {
            mtu 1526
            unit 0 {
                description "Connected to pe1 em0";
                family inet {
                    address 192.168.1.1/30;
                }
                family iso;
                family mpls;
            }
        }
        em1 {
           mtu 1526
            unit 0 {
                description "Connected to pe2 em1";
                family inet {
                    address 192.168.1.5/30;
                }
                family iso;
                family mpls;
            }
        }
        lo0 {
            unit 0 {
                family inet {
                    address 10.0.0.2/32;
                }
                family iso {
                    address 49.1234.0200.0100.0000.0002.00;
                }
            }
        }
    }
    routing-options {
        autonomous-system 1234;
    }
    protocols {
        mpls {
            icmp-tunneling;
            interface em0.0;
            interface em1.0;
            interface lo0.0;
        }
        bgp {
            log-updown;
            family inet {
                unicast;
            }
            family inet-vpn {
                unicast;
            }
            export INET-REDISTRIBUTE;
            remove-private;
            group internal {
                type internal;
                local-address 10.0.0.2;
                peer-as 1234;
                neighbor 10.0.0.1;
                neighbor 10.0.0.3;
            }
        }
        isis {
            level 1 disable;
            level 2 wide-metrics-only;
            interface em0.0;
            interface em1.0;
            interface lo0.0 {
                passive;
            }
        }
        ldp {
            interface em0.0;
            interface em1.0;
            interface lo0.0;
        }
    }
    policy-options {
        policy-statement INET-REDISTRIBUTE {
            term connected {
                from protocol direct;
                then accept;
            }
            term static {
                from protocol static;
                then accept;
            }
            term local {
                from protocol local;
                then accept;
            }
            term else {
                then reject;
            }
        }
    }

### PE2

    interfaces {
        em0 {
            unit 0 {
                description "To customer for Managed WAN";
                family inet {
                    address 192.168.90.5/30;
                }
            }
        }
        em1 {
            mtu 1526
            unit 0 {
                description "Connected to p1 em1";
                family inet {
                    address 192.168.1.6/30;
                }
                family iso;
                family mpls;
            }
        }
        lo0 {
            unit 0 {
                family inet {
                    address 10.0.0.3/32;
                }
                family iso {
                    address 49.1234.0200.0100.0000.0003.00;
                }
            }
        }
    }
    routing-options {
        autonomous-system 1234;
    }
    protocols {
        mpls {
            icmp-tunneling;
            interface em1.0;
            interface lo0.0;
        }
        bgp {
            log-updown;
            family inet {
                unicast;
            }
            family inet-vpn {
                unicast;
            }
            export INET-REDISTRIBUTE;
            remove-private;
            group internal {
                type internal;
                local-address 10.0.0.3;
                peer-as 1234;
                neighbor 10.0.0.1;
                neighbor 10.0.0.2;
            }
        }
        isis {
            level 1 disable;
            level 2 wide-metrics-only;
            interface em0.0;
            interface lo0.0 {
                passive;
            }
        }
        ldp {
            interface em1.0;
            interface lo0.0;
        }
    }
    policy-options {
        policy-statement INET-REDISTRIBUTE {
            term connected {
                from protocol direct;
                then accept;
            }
            term static {
                from protocol static;
                then accept;
            }
            term local {
                from protocol local;
                then accept;
            }
            term else {
                then reject;
            }
        }
    }
    routing-instances {
        VRFA {
            instance-type vrf;
            interface em0.0;
            route-distinguisher 1234:1;
            vrf-target target:1234:1;
        }
    }

### CE2

    version 12.0
    !
    hostname ce2
    !
    !
    ip subnet-zero
    !
    interface FastEthernet1/0
     description connected to WANProvider for Managed WAN
     ip address 192.168.90.6 255.255.255.252
     no ip directed-broadcast
     no ip proxy-arp
     speed 100
     full-duplex
    !
    ip classless
    ip route 192.168.90.0 255.255.255.252 192.168.90.5
    !
    !
    !
    line con 0
    line aux 0
    line vty 0 4
     login
    !

Notes
-----

-   If you are using olive to simulate this, you may have to configure **vrf-table-label** in the routing instance to make it work, it is not common to use this configuration directive as it causes egress route lookup
-   MTU 1526 should be ethernet (14) + up to 3 shims for payload to be 1500 without adjustment in mpls family, this wont work on olive fxp0 and em0
