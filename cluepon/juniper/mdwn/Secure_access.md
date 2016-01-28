---
title: Secure access
permalink: /Secure_access/
---

Firewall filters allow for the controlling of packets destined to and from the routing engine. Gigantic filtersets can be added with no impact to forwarding performance, although it is possible to construct firewall filters which reduce performance. There is no obvious relationship between filter size or actions which makes it clear that the filter is performance impacting, although performance impacting filters are difficult to produce and likely would not be made on accident.

When the router receives a packet on an interface with an attached input filter, the packet will be processed according to the applied filter before being routed normally. When a filter is applied and there is no explicit accept statement present, JunOS will automatically discard the packet.

If you want to apply a filter to restrict access to the routing engine but not to transit traffic, use *lo0* as matching-interface.

Let's assume you want to restrict access to the routing engine to only employees within the company. Because we know who we want to give access we can use a simple *prefix-list* to mark the employees.

` [edit]  `
` user@juniper# show policy-options prefix-list`
`     prefix-list employees {`
`         192.168.0.1/24;`
`         192.168.15.1/32;`
`         10.0.12.0/30;`
`     }`
` `
` [edit]`
` user@juniper# show policy-options prefix-list employees | display set`
`     set policy-options prefix-list employees 192.168.0.1/24`
`     set policy-options prefix-list employees 192.168.15.1/32`
`     set policy-options prefix-list employees 10.0.12.0/30`

Now that we have a list of our employees we can create the policy allowing their access to the routing engine.

` [edit]`
` user@firewall# show firewall`
`     filter protect-router {`
`         term ssh {`
`             from {`
`                 prefix-list {`
`                     world;`
`                     employees except;`
`                 }`
`                 protocol tcp;`
`                 destination-port ssh;`
`             }`
`             then {`
`                 reject;`
`             }`
`         }`
`         term snmp {`
`             from {`
`                 prefix-list {`
`                     world;`
`                     employees except;`
`                 }`
`                 protocol udp;`
`                 destination-port snmp;`
`             }`
`             then {`
`                 reject;`
`             }`
`         }`
`         term telnet {`
`             from {`
`                 prefix-list {`
`                     world;`
`                     employees except;`
`                 }`
`                 protocol tcp;`
`                 destination-port telnet;`
`             }`
`            then {`
`                 reject;`
`             }`
`         }`
`         term accept {`
`             then accept;`
`         }`
`     }`
` `
` [edit]`
` user@juniper# show firewall | display set`
`     set firewall filter protect-router term ssh from prefix-list world`
`     set firewall filter protect-router term ssh from prefix-list employees except`
`     set firewall filter protect-router term ssh from protocol tcp`
`     set firewall filter protect-router term ssh from destination-port ssh`
`     set firewall filter protect-router term ssh then reject`
`     set firewall filter protect-router term snmp from prefix-list world`
`     set firewall filter protect-router term snmp from prefix-list employees except`
`     set firewall filter protect-router term snmp from protocol udp`
`     set firewall filter protect-router term snmp from destination-port snmp`
`     set firewall filter protect-router term snmp then reject`
`     set firewall filter protect-router term telnet from prefix-list world`
`     set firewall filter protect-router term telnet from prefix-list employees except`
`     set firewall filter protect-router term telnet from protocol tcp`
`     set firewall filter protect-router term telnet from destination-port telnet`
`     set firewall filter protect-router term telnet then reject`
`     set firewall filter protect-router term accept then accept`

The only thing we need to do is actually apply the filter to the preferred interface. (In this case *lo0*). If you apply major changes to your JunOS configuration file **ALWAYS** use the *[commit](/commit "wikilink") confirmed* command. When using the confirmed option JunOS will automatically roll-back to the previous configuration in case you don't explicitly confirm the new configuration.

` [edit]`
` user@juniper# `**`set` `interfaces` `lo0` `unit` `0` `family` `inet` `filter` `input` `protect-router`**
` user@juniper# commit confirmed`