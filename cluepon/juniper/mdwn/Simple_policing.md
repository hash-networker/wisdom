---
title: Simple policing
permalink: /Simple_policing/
---

Start by defining some policers in the \[edit firewall\] hierarchy:

`firewall {`
`  policer 1M {`
`      if-exceeding {`
`          bandwidth-limit 1m;`
`          burst-size-limit 100k;`
`      }`
`      then discard;`
`  }`
`  policer 5M {`
`       if-exceeding {`
`          bandwidth-limit 5m;`
`          burst-size-limit 100k;`
`      }`
`      then discard;`
`  }`
`  policer 20M {`
`      if-exceeding {`
`          bandwidth-limit 20m;`
`          burst-size-limit 100k;`
`      }`
`      then discard;`
`  }`
`}`

Then choose the method that which you'd like to use the policer.

Method 1 - You can apply a "blanket" policer against an interface at the \[edit interfaces iface-x/x/0 unit x\] hierarchy. This will count for all traffic going through that particular unit, as such:

`interfaces {`
`  ge-0/0/2 {`
`      description "Some Interface";`
`      vlan-tagging;`
`      speed 100m;`
`      link-mode full-duplex;`
`      unit 510 {`
`          vlan-id 510;`
`          family inet {`
`              policer {`
`                  input 20M;`
`                  output 20M;`
`              }`
`              address 1.2.3.4/24;`
`          }`
`      }`
`      unit 511 {`
`          vlan-id 511;`
`          family inet {`
`              policer {`
`                  input 10M;`
`                  output 10M;`
`              }`
`              address 4.3.2.1/24;`
`          }`
`      }`
`  }`

Note how you can define different policers for both the inbound and outbound traffic. This is the easiest method for applying a policer, but it affects all traffic.

Method 2 - You can apply a policer to a particular type of traffic via a filter. First step, is to create a filter that uses that policer at the \[edit firewall\] hierarchy. I will use udp/123 (ntp) traffic as an example:

`firewall {`
`  filter police-ntp-traffic {`
`      term ntp {`
`          from {`
`              destination-prefix-list {`
`                  my-customers-ip-addresses;`
`              }`
`              protocol udp;`
`              destination-port ntp;`
`          }`
`          then {  `
`              policer 1M;`
`              accept;`
`          }`
`      }`
`      term accept-all-else {`
`          then accept;`
`      }`
`   }`
`}`

NOTE: I have to explicitly state an "accept" for all other traffic. Firewalls have an implicit "deny" at the end of them make sure you accept all other types of traffic, or you'll black-hole all traffic using this filter.

Also note that I am making this filter somewhat directional, by saying "if the destination IP is one of my customer's IPs, and the destination port is udp/123 (ntp), then police it". You can get VERY VERY granular with your matches; restricting it to one IP, a bunch of IPs, tcp ports, up ports, protocol types etc; as normal firewall filter policy commands apply here (Get Creative! =)...)

Secondly, note how I use a "prefix list" rather tan giving a list of IP addresses. Prefix-lists make your life easy, if you need to use the same bunch-o-addresses over and over in different firewall filters. You declare prefix lists here at the \[edit policy-options prefix-list\] hierarchy:

`policy-options {`
`  prefix-list my-customers-ip-addresses {`
`      216.126.16.0/20;`
`      216.145.211.0/24;`
`      216.83.0.0/16;`
`      216.95.194.224/19;`
`   }`
`}`

You then need to apply that firewall filter (which makes use of the policer) onto an interface, as such:

`interfaces {`
`  ge-0/0/0 {`
`      unit 0 {`
`          family inet {`
`              filter {`
`                  output police-ntp-traffic;`
`              }`
`              address 4.3.2.1/24;`
`          }`
`      }           `
`  }`

You can also use the filter on ingress as well:

`interfaces {`
`  ge-0/0/0 {`
`      unit 0 {`
`          family inet {`
`              filter {`
`                  output police-ntp-traffic;`
`                  input police-ntp-traffic;`
`              }`
`              address 4.3.2.1/24;`
`          }`
`      }           `
`  }`

Hope this makes sense! I tried to make it straight-forward enough; but give you some good insights on the things you can do with them...

juniperdude at gmail.com