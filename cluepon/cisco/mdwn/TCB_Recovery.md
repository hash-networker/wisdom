---
title: TCB Recovery
permalink: /TCB_Recovery/
---

This page describes the procedure to remotely delete, using SNMP, stuck telnet sessions on a Cisco router.

### When you should use this procedure

Routers are usually configured with a limited number of concurrent management sessions (via telnet or ssh) and they usually are configured to automatically timeout after some time.

Under particular conditions it may happen that the management sessions remain stuck and the router will not clear them. These conditions have been observed to cause this problem:

-   Operator error: users annoyed by the timeout disable it and then they whine they can't login anymore.
-   Operator error: users annoyed by the local password for console access disable it and then they whine they can't login anymore.
-   Firewall failure: crashes of the firewall may leave the telnet/ssh sessions in a state that the router can't recover. This has been observed with both PIX and RH-Firewall-1. Reason is unknown, probably an IOS bug. Using this procedure is easier than opening a TAC case, since it's difficult to reproduce the condition.

### How to identify the stuck sessions

The following command will dump all the TCBs of the remote router:

`$ snmpwalk -v 2c -c `*`your_readwrite_community`*` `*`router`*` 1.3.6.1.2.1.6.13.1.1`

The output will look somewhat like this:

    RFC1213-MIB::tcpConnState.172.16.27.11.23.172.16.27.1.1855 = INTEGER: established(5)
    RFC1213-MIB::tcpConnState.172.16.216.146.11018.172.16.216.145.179 = INTEGER: established(5)
    RFC1213-MIB::tcpConnState.172.16.216.154.20081.172.16.216.153.179 = INTEGER: established(5)
    RFC1213-MIB::tcpConnState.172.16.216.185.646.172.16.216.186.11028 = INTEGER: established(5)

Each line of output will have the local IP address, followed by the local TCP port, then the remote IP address and the remote TCP port. Of course, you're interested in local ports 22 (ssh) and 23 (telnet). Resetting BGP sessions will not gain you access to your router. So in this example, the first line is the interesting one. In a real case, there could be several pageful of output which you need to scan. Please note that you're only interested in the part to the left of the '='.

### How to reset the stuck sessions

Once you have done the previous command and identified the interesting line(s), you have to issue the following command:

`$ snmpset -v 2c -c `*`your_readwrite_community`*` `*`router`*` `*`the_abovementioned_line`*` i 12`

So in this particular case, the command you'd have to issue would be:

`$ snmpset -v 2c -c `*`your_readwrite_community`*` `*`router`*` RFC1213-MIB::tcpConnState.172.16.27.11.23.172.16.27.1.1855 i 12`

The 'i 12' at the end of the line is what tells the router to clear the session. The router will reply with:

    RFC1213-MIB::tcpConnState.172.16.27.11.23.172.16.27.1.1855 = INTEGER: deleteTCB(12)

You should now be able to log into the router. If not, it means you cleared the wrong session, and you need to repeat the procedure.

### How to prevent this problem

Easy and simple:

-   Kill users modifying authentication and access configuration.
-   Kill the firewall management group if they do maintenances during working hours.
-   Don't have a long exec-timeout (or 0!) when you manage device through a firewall. The firewall may time out your TCP session, and not send RSTs or anything, resulting in stuck sessions.
-   Use 'service tcp-keepalives-in' and 'service tcp-keepalives-out'. The device will detect when a session has become non-responsive and remove it.

Apply torture ad libitum.