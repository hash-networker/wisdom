---
title: IQ2 Class of Service
permalink: /IQ2_Class_of_Service/
---

IQ2 PIC Class of Service (CoS, aka Quality of Service, QoS) is a bit of a dark art due to gaps in the documentation. This page aims to document things that are not in the Juniper documentation or are but are written ambiguously.

-   Multi-field classification with regular firewall filters happens **after** ingress scheduling. Simple filters allow classification before ingress scheduling but cannot match on DSCP values, although the [JUNOS documentation](http://www.juniper.net/techpubs/software/junos/junos91/swconfig-policy/configuring-simple-filters.html) does not mention this.

<!-- -->

-   A `high` priority queue is always implemented as `strict-high`, although the documentation reads as if a `high` priority queue is implemented as `strict-high` **only** when more than one `medium-high`, `high`, or `strict-high` scheduler is configured in a scheduler-map:

> Gigabit Ethernet IQ2 PICs support only one queue in the scheduler map with medium-high, high, or strict-high priority. If more than one queue is configured with high or strict-high priority, the one that appears first in the configuration is implemented as strict-high priority. This queue receives unlimited transmission bandwidth. The remaining queues are implemented as low priority, which means they might be starved. (From [Differences Between Gigabit Ethernet IQ and Gigabit Ethernet IQ2 PICs](http://www.juniper.net/techpubs/software/junos/junos91/swconfig-cos/differences-between-gigabit-ethernet-iq-and-gigabit-ethernetiq2-pics.html).)