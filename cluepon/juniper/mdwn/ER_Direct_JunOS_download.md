---
title: ER Direct JunOS download
permalink: /ER_Direct_JunOS_download/
---

Problem
-------

In order to download new JUNOS software to a router, a user must download from Juniper's website, copy the image onto a management server, and then copy the image on to an individual router. Juniper offers no FTP mechanism, and the support web-page is not particularly friendly for fetching JUNOS images directly to unix machines (requires cookies and a complex series of steps in lynx to authenticate and find and download the images).

Solution
--------

Implement a way to directly download the images from Juniper's website to a router, using a CLI-friendly interface for the login/password authentication.

Status
------

Unknown.

[Category:Feature Requests](/Category:Feature_Requests "wikilink")