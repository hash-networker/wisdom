---
title: OS dom
permalink: /OS_dom/
---

Information
-----------

This little script will make an easy to read table of power measurements out of your DOM-supporting optics.

Usage
-----

Config as:

    me@router-re0> show configuration system scripts op
    file dom.slax {
        command dom;
        description "Show optical power levels in tabular form";
    }

Call it as:

    me@router-re0> op dom
    Interface    In mW  In dBm Out mW Out dBm Temp C Description
    ge-3/0/3                   0.0000           36.0
    xe-3/2/0     0.2559  -5.92 1.3310    1.24   33.3 some active link
    xe-3/3/0     0.0013 -28.86 0.0000           30.3 some link pending activation


    {master}

Bugs
----

~~I assume the meaningful description is on unit 0, not on the physical interface. Easily tweakable to your needs, though. ge- interfaces return a different xml for rx power.~~

ChangeLog
---------

-   20100504 - Collect all interface descriptions with one command invoke and show physical interface description if unit 0 description does not exist - Cougar
-   20100408 - Added check for different xml field for rx power returned by ge interfaces.
-   20100312 - Initial Version
