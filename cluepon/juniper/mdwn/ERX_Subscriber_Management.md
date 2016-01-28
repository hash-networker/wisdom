---
title: ERX Subscriber Management
permalink: /ERX_Subscriber_Management/
---

Overview
--------

First off, a note of caution. The juniAaaSubscriberTable is obviously going to get pretty huge when there are a high number of subscribers connected to a chassis, and walking the entire thing (or even just a single object within the table) is going to take a long time. This may have a negative impact on SRP CPU load, particularly if its being done regularly.

Subscriber information is accessible using the juniAaa mib, which can be found [here](http://www.juniper.net/techpubs/software/erx/). The mibs are available for download as a tarball; just follow your nose. They are split into four categories - the juniAaa mib is in the juniperMibs directory.

For SNMP to work at all, you'll need it enabled on your router. A quick config example enabling SNMP read-only access from host 10.100.5.1 only:

     snmp-server
     snmp-server community public ro snmp-acl
     access-list "snmp-acl" permit ip host 10.100.5.1 any
     access-list "snmp-acl" deny ip any any

juniAaaSubscribers
------------------

The juniAaaSubscribers object contains the subscriber table, which stores information on every subscriber currently logged in on the router (in all virtual routers). The full OID:

    enterprises.juniperUni.juniperUniMibs.juniMibs.juniAaaMIB.juniAaaObjects.juniAaaSubscribers

which is equivalent to

    .1.3.6.1.4.1.4874.2.2.20.1.8

This object can be used to find out how many subscribers are currently logged in and the peak number of subscribers; to list all users logged in, including information such as interface IDs, virtual routers, and IP addresses; and disconnect individual users.

### The juniAaaSubscriberTable

### Logging out users with juniAaaSubscriberRowStatus

Setting the juniAaaSubscriberRowStatus for a specific user to 'destroy' will cause that subscriber to be disconnected from the ERX. This requires that you know the table index for the subscriber in question (I don't know of a way to identify a users index without walking the entire table. If someone does, please post it). For example:

    linux:~# snmpwalk -v2c -c private 192.168.1.1 .1.3.6.1.4.1.4874.2.2.20.1.8.4.1.2 | grep testline
    SNMPv2-SMI::enterprises.4874.2.2.20.1.8.4.1.2.33924347 = STRING: "testline@testdomain.com"

    linux:~# snmpset -v2c -c private 192.168.1.1 .1.3.6.1.4.1.4874.2.2.20.1.8.4.1.15.33924347 integer 6
    SNMPv2-SMI::enterprises.4874.2.2.20.1.8.4.1.15.33924347 = INTEGER: 6

This will cause the subscriber session to be terminated, and the subscriber to be logged out. Note that because this is a set operation, an SNMP community with write access must be used.