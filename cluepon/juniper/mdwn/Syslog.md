---
title: Syslog
permalink: /Syslog/
---

This syslog filter removes all ssh logins (think rancid) and license warnings (think ospf3) from appearing in your splunk[1](http://www.splunk.com) or logstash[2](http://code.google.com/p/logstash/) aggregator:

`system {`
`    syslog {`
`        file messages {`
`            match "!(.*requires a license.*|.*LICENSE_EXPIRED.*|.*STL library initialized.*|.*kernel time sync enabled.*)";`
`        }`
`    }`
`}`