---
title: J series routing clusters
permalink: /J_series_routing_clusters/
---

Aim:

set up two clustered routers in active/standby mode, sharing a single routing engine, with redundant interfaces between the routers.

Hardware:

Any J series should do AFAIK. You will need additional interfaces in your routers, as clustering takes up three interfaces. In this example, I've used the onboard interfaces ge-0/0/1, 2 and 3 for the cluster links. These must be direct cables (crossover or straight both work for me), apparently clustering will not work with a switch in the way.

Interfaces used:

-   Ge-0/0/3 will become fxp1 and used as the control link between the two routers
-   Ge-0/0/2 will be used as fxp0 for individual management of each of the routers
-   Ge-0/0/1 will be used as the fab link (known as Ge-7/0/1 on the second node)

Notes:

-   Individual routers are called nodes.
-   A group of nodes is a cluster.
-   Physical interfaces are joined to form a redundant interface (reth)
-   Multiple reth's can be placed in a redundancy group
-   All reth's in a redundancy group fail over together
-   You can have more than one redundancy group in a cluster

Initial preparation:

On router 1:

    > set chassis cluster cluster-id 1 node 0 reboot
    > configure

    # set system root-authentication plain-text-password #only needed if not yet set
    # set group node0 system host-name $hostname of first node
    # set group node1 system host-name $hostname of second node
    # set group node0 interfaces fxp0 unit 0 family inet address 10.0.0.1/30
    # set group node1 interfaces fxp0 unit 0 family inet address 10.0.0.2/30
    # set chassis cluster node 0
    # set chassis cluster redundancy-group 1 node 0 priority 100
    # set chassis cluster node 1
    # set chassis cluster redundancy-group 1 node 1 priority 1
    # set apply-groups "${node}"

On router 2:

    > set chassis cluster cluster-id 1 node 1 reboot
    > configure
    # set system root-authentication plain-text-password #only needed if not yet set

NB. to remove clustering on a router, run:

    > set chassis cluster cluster-id 0 node 0 reboot

Create FAB links (data plane links for RTO sync, etc)

On router 1:

       # set interfaces fab0 fabric-options member-interfaces ge-0/0/1    #fab0 is node0 interface for the data link
       # set interfaces fab1 fabric-options member-interfaces ge-7/0/1    #fab1 is node1 interface for the data link

Set up redundant interface on interfaces g1/0/0 and g8/0/0 (ie. the first port on the first PIM on a J4350): Note for a J2320 this would be g1/0/0 and g5/0/0

on router 1:

    set chassis cluster reth-count 9 #or however many spare interfaces you want to use for clustering
    set interfaces reth0 redundant-ether-options redundancy-group 1
    set interfaces reth0 unit 0 family inet address 192.168.0.1/24
    set interfaces ge-1/0/0 gigether-options redundant-parent reth0
    set interfaces ge-8/0/0 gigether-options redundant-parent reth0
    set chassis cluster redundancy-group 1 interface-monitor ge-1/0/0 weight 255
    set chassis cluster redundancy-group 1 interface-monitor ge-8/0/0 weight 255
    set chassis cluster redundancy-group 1 preempt

Optional changes to heartbeating

    set chassis cluster heartbeat-interval 1000 #- minimum, milliseconds
    set chassis cluster heartbeat-threshold 1 #- minimum

Querying the cluster:

       > show chassis cluster status
       > show chassis cluster statistics
       > show chassis cluster interfaces

Forcing a failover:

    >request chassis cluster failover node $number redundancy-group $number

References:

Main documentation: <http://www.juniper.net/techpubs/software/junos-security/junos-security95/junos-security-swconfig-security/cc-chapter.html#cc-chapter>

Howto: <http://kb.juniper.net/index?page=content&id=KB12397&actp=search&searchid=1243002411264>

Before you start: <http://kb.juniper.net/index?page=content&id=KB12819&actp=search&searchid=1239705554214>