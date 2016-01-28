---
title: Sup720 load balancing
permalink: /Sup720_load_balancing/
---

There is a weird problem about load balancing on 7600 with sup720 when doing eBGP with loopbacks (ECMP). If you have an even number of links, one of them will always get more traffic than the others.

This is an excerpt of a reply I got from TAC:


    The sup720 load balancing algorithm works by default in the following way:

    1: 1
    2: 7-8
    3: 1-1-1
    4: 1-1-1-2
    5: 1-1-1-1-1
    6: 1-2-2-2-2-2
    7: 1-1-1-1-1-1-1
    8: 1-1-1-2-2-2-2-2

    This means that when you have 2 equal cost paths,
    load-sharing will be 46%-54%, not 50%-50%.
    If on 3 equal cost paths, load-sharing will 33%-33%-33% (expected).
    When we have 4 paths, we will have 20%-20%-20%-40% and not
    25%-25%-25%-25% as we could expect.

    Sup720 uses by default the load-sharing weight described earlier but there is a
    knob to change this behaviour.
    This knob has been introduced in 12.2(17d)SXB2 and is then available in IOS
    release you are running.
    The command is the following :

    (config)# mls ip cef load-sharing full simple

    Try using this command and the traffic should load-balance equally on th 4
    paths.

As an aside, another bug prevents this knob to work in SRA2. And it's not fixed in SRA3/4.