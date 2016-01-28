---
title: Cflowd configuration
permalink: /Cflowd_configuration/
---

Here's a nice code snippet you cam put in your M40's to enable Netflow/cflowd record exports to an analyzer. I have sucessfully used this utility to track flows in 7 simultaneous Juniper M-series routers.

Here's my JUNOS 7.3 configuration, but works for any 7.x release and even possibly earlier.

First thing to do, is to enable packet sampling and set your output/target for the flows to be sent:

`forwarding-options { `
`   sampling { `
`       input { `
`           family inet { `
`               rate 10; `
`               run-length 10; `
`               max-packets-per-second 7000; `
`           } `
`       } `
`       output { `
`           cflowd 172.28.1.14 { `
`               port 9996; `
`               source-address 10.2.1.11; `
`               version 5; `
`               no-local-dump; `
`               autonomous-system-type origin; `
`           } `
`       } `
`   } `
`} `

Replace 10.2.1.11 with the IP address you wish to "show up" as being the source of the flows. Replace 172.28.1.14 with the IP address of the server where NetFlow Analyzer is running.

Next, enable packet sampling on the particular interface(s) you wish to do netflow analysis for:

`interfaces { `
`   ge-1/3/0 { `
`       vlan-tagging; `
`       unit 101 { `
`           vlan-id 101; `
`           family inet { `
`               sampling { `
`                   input; `
`                   output; `
`               } `
`               address 2XX.X.X.X/25 `
`           } `
`       } `
`   } `
`} `

For example, this enables "sampling" on interface ge-1/3/0.101 (gigabit ethernet 1/3/0 VLAN 101). This will then send a copy of the traffic to the sampling engine, which will then export it in cflowd version 5 format.

I will add, that the "internal" cflowd engine only handles 7000 packets per second - hence, you may wish to "cut that down" i.e. sampling rate, so that we dont overload the engine. here's what I do:

`forwarding-options { `
`   sampling { `
`       input { `
`           family inet { `
`               rate 100; `
`               run-length 9; `
`               max-packets-per-second 7000; `
`           } `
`       } `

This makes it same 1/10th the ammount of traffic. (take 1 packet, plus 9 packets more, every 100 packets, for a total of 10/100 packets).

NOTE: This \*also\* means that your Netflow analyzer will only report 1/10th the ammount of "real" traffic. So - if netflow says it's "2.2 Megabits/sec", it's really 22 Megabits/sec! =) (just multiply by 10 in your head for the right number)

As a side note, here's a neat trick to keep your SNMP-reported bandwidth values correct, in case you're using VLANs:

`interfaces { `
`   ge-1/3/0 { `
`       vlan-tagging; `
`       unit 101 { `
`           bandwidth 4m; `
`           vlan-id 101; `
`           family inet { `
`               sampling { `
`                   input; `
`               } `
`               address x.x.x.x/25 { `
`                   vrrp-group 102 { `
`                       virtual-address x.x.x.1; `
`                       priority 100; `
`                       preempt; `
`                       accept-data; `
`                   } `
`               } `
`           } `
`       } `
`       unit 602 { `
`           bandwidth 10m; `
`           vlan-id 602; `
`           family inet { `
`               sampling { `
`                   input; `
`               } `
`               address x.x.x.x/27 { `
`                   vrrp-group 22 { `
`                       virtual-address x.x.x.1; `
`                       priority 200; `
`                       preempt; `
`                       accept-data; `
`                   } `
`               } `
`           } `
`       } `
`       unit 701 { `
`           bandwidth 10m; `
`           traps; `
`           vlan-id 701; `
`           family inet { `
`               filter { `
`                   input Inbound_Transit_Filter; `
`                   output Outbound_Transit_Filter; `
`               } `
`               sampling { `
`                   input; `
`               } `
`               address x.x.x.x/30 { `
`                   preferred; `
`               } `
`           } `
`       } `
`   } `
`} `

Note the use of the "bandwidth ___m" entries. (You can also use "k" for kilobits, "g" for gigabits etc.. - yes, JunOS is very nice to work with. =)

Since I am sampling 1/10th the ammount of actual traffic; I am "scaling" down the reported bandwidth of each virtual interface by a factor of 10. This lets the analyzer calculate the "true" percentage utilization of each virtual-interface. (As the NetFlow exporter is only reporting 1/10th the ammount of traffic). So, 4 M is actually 40 megabits (aka a DS3), and 10M is actually 100Mbits (fast-e)

Hope this helps.

juniperdude@gmail.com =)