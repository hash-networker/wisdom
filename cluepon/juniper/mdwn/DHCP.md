---
title: DHCP
permalink: /DHCP/
---

do you want to set your NTP servers via DHCP pools?

according to the [IANA codes](http://www.iana.org/assignments/bootp-dhcp-parameters/bootp-dhcp-parameters.xml) the options to pass are 42 (and probably 4 for older implementations)

`{master:1}[edit system services dhcp]`
`charlie@juniper# set pool 10.222.14.0/24 option 42 array ip-address 192.168.1.200 `
`{master:1}[edit system services dhcp]`
`charlie@juniper# set pool 10.222.14.0/24 option 4 array ip-address 192.168.1.200     `
`{master:1}[edit]`
`charlie@juniper# sh|compare `
`[edit system services dhcp pool 10.222.14.0/24]`
`+      option 42 array ip-address 192.168.1.200;`
`+      option 4 array ip-address 192.168.1.200;`

If you want multiple values, put them in order of preference.