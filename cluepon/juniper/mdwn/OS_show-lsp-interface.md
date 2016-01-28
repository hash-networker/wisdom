---
title: OS show-lsp-interface
permalink: /OS_show-lsp-interface/
---

Information
-----------

This Op Script lists LSPs like "**show mpls lsp**" does but also adds ingress and/or egress interface information (interface name, description and next-hop IP address). Using this command together with "**match**" filter makes it easy to find out all LSPs that go through specific interface for example.

Now it is also possible to limit LSPs which use certain interfaces by different arguments using "**interface**", "**srcif**" or "**dstif**"

Up to date information is always avaliable at <http://wiki.version6.net/juniper> page.

Usage
-----

    cougar@router1> op show-lsp-interface interface ae4.1
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    t name                                                                                                                              dst             next-hop        dst-if        dst-ifdescr
    I router1->router2                                                                                                                  203.0.113.1     203.0.113.192   ae4.1         -> router2 ae4.1
    I router1->router5                                                                                                                  203.0.113.2     203.0.113.192   ae4.1         -> router2 ae4.1
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    t name                             src             prev-hop        src-if        src-ifdescr
    E router5->router1                 203.0.113.2     203.0.113.192   ae4.1         -> router2 ae4.1
    E router2->router1                 203.0.113.1     203.0.113.192   ae4.1         -> router2 ae4.1
    ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    t name                             src             prev-hop        src-if        src-ifdescr                                        dst             next-hop        dst-if        dst-ifdescr
    T router5->router3                 203.0.113.2     203.0.113.192   ae4.1         -> router2 ae4.1                                   203.0.113.10    203.0.113.121   ae5.1         -> router3 ae5.1
    T router3->router2                 203.0.113.10    203.0.113.121   ae5.1         -> router3 ae5.1                                   203.0.113.1     203.0.113.192   ae4.1         -> router2 ae4.1
    T router3->router5                 203.0.113.10    203.0.113.121   ae5.1         -> router3 ae5.1                                   203.0.113.2     203.0.113.192   ae4.1         -> router2 ae4.1

Bugs
----

This script doesn't list fast-reroute *detour* LSPs. It is quite complicated to add fast-reroute detour LSPs because there can be either more than one detour LSP (easy case) or normal LSP with detour information which is much more complicated to parse.

ChangeLog
---------

-   1.2 - interface, srcif, dstif and showbypass arguments, much better printf() based output format
-   1.1 - only one invoke of "**show interfaces descriptions**" which makes it many times faster
-   1.0 - Initial Version
