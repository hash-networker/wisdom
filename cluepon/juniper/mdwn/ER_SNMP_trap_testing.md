---
title: ER SNMP trap testing
permalink: /ER_SNMP_trap_testing/
---

I have not found a way to generate a test SNMP trap without bouncing a physical interface or an active BGP session. Other vendors allow you to use a Loopback interface or provide explict test commands to generate SNMP traps for testing your Network Management System.

Example commands:

-   test snmp interface so-6/0/0
-   test snmp bgp 1.1.1.1
