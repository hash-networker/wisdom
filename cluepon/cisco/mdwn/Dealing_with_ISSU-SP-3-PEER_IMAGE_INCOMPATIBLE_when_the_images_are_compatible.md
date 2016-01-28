---
title: Dealing with ISSU-SP-3-PEER IMAGE INCOMPATIBLE when the images are compatible
permalink: /Dealing_with_ISSU-SP-3-PEER_IMAGE_INCOMPATIBLE_when_the_images_are_compatible/
---

Most likely you are also getting this:

    %PFREDUN-SP-4-INCOMPATIBLE_ISSU_MATRIX: Compatibility Matrix check failed. reason 100

This is preventing you moving to SSO.

reason 100 refers to some configuration commands being mismatched between synchronised versions of the startup-config, use the command

**redundancy config-sync validate mismatched-commands**

to check which these are, fix them and then issue

**show issu config-sync mismatched-commands**

You are now ready to reload the standby.