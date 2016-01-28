---
title: ER Backup RE NTP sync
permalink: /ER_Backup_RE_NTP_sync/
---

The clock in backup RE seems to stray a lot, which is causing a lot of problems. We would like to use NTP to sync the clock on backup RE as well. However, apparently this is very difficult to get to work properly unless you have separate infrastructure connecting fxp0's in all the routers. We don't have this, and I believe there are a lot of networks in the similar situation.

We also tried to hook up fxp0's in master and backup RE with a cross-over cable. This either requires that "default-address-selection" is disabled (we have it enabled) because otherwise backup RE will use the same loopback address as RE0 as source for NTP packets, or we must manually configure the NTP source address. The latter is unacceptable, because if the backup RE becomes active, NTP service ceases to work as a private address can not be used as a source address, and using a globally routable address for fxp0 is unacceptable.

Therefore, there does not seem to be a good way to solve the clock sync problem at the moment with the following constraints:

`1) separate management infrastructure at fxp0 cannot be assumed,`
`2) default-address-selection is used, and`
`3) the router must be able act as a public NTP server even if backup RE`
`   becomes the primary RE.`
`                                                                                                                       `

Two ways how this _might_ work is to implement "no-default-address-selection" knob for NTP, or to use TNP through fxp1's to sync the clock. There may be others.