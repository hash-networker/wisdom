---
title: ER channelized parent config
permalink: /ER_channelized_parent_config/
---

Problem
-------

Configurations for the parent channelized device must be configured in the first channel:

`interfaces {`
`    t1-2/1/0:0 {`
`        t3-options {`
`            no-cbit-parity;`
`        }`
`    }`
`    ...`
`    t1-2/1/1:0 {`
`        t3-options {`
`            no-cbit-parity;`
`        }`
`    }`
`    ...`
`    t1-2/1/2:0 {`
`        t3-options {`
`            no-cbit-parity;`
`        }`
`    }`
`}`

If a circuit is configured on the first channel, then de-provisioned, the config for the parent device is also removed. This has the effect of causing all circuits on the parent device to go down.

Workaround
----------

A workaround is to configure the channelized device with a 'group' and always make sure that the first channel is always configured (how to do this is up in the air - possibly with a config-check script which can run prior to a commit?):

`t3-configs {`
`    interfaces {`
`        <t1-*/*/*:0> {`
`            t3-options {`
`                no-cbit-parity;`
`            }`
`        }`
`    }`
`}`
`interfaces {`
`    apply-groups t3-configs;`
`    t1-2/1/0:0 {`
`        disable;`
`    }`
`}`

Solution
--------

Configuration of the parent channelized device should be done separately from the first channel:

`interfaces {`
`     ct3-2/1/0 {`
`          no-cbit-parity;`
`     }`
`     t1-2/1/0:1 {`
`          description Circuit B;`
`          unit 0 {`
`               family inet {`
`                    address 192.2.0.1/30;`
`               }`
`          }`
`     }`
`}`

Status
------

A solution can be achieved with Junos 7.4 using a [Commit Script](http://www.juniper.net/techpubs/software/junos/junos74/swconfig74-scripts/html/commit-scripts-overview.html#1033561).