---
title: ER Remove telnet user password
permalink: /ER_Remove_telnet_user_password/
---

Problem
-------

Currently, Juniper has no mechanism to disable the need for a login/password authentication when connecting. While this makes sense for normal production router use, it creates an unnecessary step when Juniper routers are used as publicly accessible route-servers.

Solution
--------

Implement a "default account" which all connections without an explicit username (such as via the telnet protocol) could assume, without the need to type a login or password.

Status
------

Unknown.

[Category: Feature Requests](/Category:_Feature_Requests "wikilink")