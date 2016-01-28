---
title: Sourcing Scripts
permalink: /Sourcing_Scripts/
---

The easiest way to manage scripts over several routers, is to pull them in from a webserver

**Configuring Router to Use Scripts**

`   scripts {`
`          op {`
`          file peerdown.slax {`
`           command peerdown;`
`           source `[`http://192.168.2.1/path/to/script/peerdown.slax`](http://192.168.2.1/path/to/script/peerdown.slax)`;`
`          }`
`       }`
`    }`

**Refreshing Scripts**

This will download the script from the webserver.

`    edit system scripts`
`    set op refresh   `
`    refreshing 'peerdown.slax' from '`[`http://192.168.2.1/path/to/script/peerdown.slax`](http://192.168.2.1/path/to/script/peerdown.slax)`'`
`    /var/home/noc/...transferring.file.........qYZabR/peerdown.slax100% of 1067  B  340 kBps`

**Executing Script**

This can then be executed using

`    op peerdown`