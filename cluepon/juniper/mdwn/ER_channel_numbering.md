---
title: ER channel numbering
permalink: /ER_channel_numbering/
---

Problem
-------

Channelized interfaces all start at channel 0:

`interfaces {`
`    t1-2/1/0:0 {`
`    ...`
`    t1-2/1/0:1 {`
`    ...`
`    t1-2/1/0:27 {`
`}`

However, when connected to other equipment, such as a DS3 mux or other vendor's channelized equipment, channel numbers start at 1.

Solution
--------

One should be given the option to configure a format for channel numbering:

`chassis {`
`     channelized-devices {`
`          channel-start [ 0 | 1 ];`
`     }`
`}`