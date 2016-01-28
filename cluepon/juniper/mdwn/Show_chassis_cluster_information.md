---
title: Show chassis cluster information
permalink: /Show_chassis_cluster_information/
---

On SRX series firewalls in a cluster setup, this shows detailed information about the current cluster status and past events.

Examples
--------

    user@srx-node0> show chassis cluster information
    node0:
    --------------------------------------------------------------------------
    Control link statistics:
        Control link 0:
            Heartbeat packets sent: 206369
            Heartbeat packets received: 206367
            Heartbeat packet errors: 0
            Duplicate heartbeat packets received: 0
        Control recovery packet count: 0
        Sequence number of last heartbeat packet sent: 206367
        Sequence number of last heartbeat packet received: 430097
    Fabric link statistics:
        Probes sent: 206367
        Probes received: 206328
        Probe errors: 0
        Probes not processed: 22547
        Probes dropped due to control link down: 0
        Sequence number of last probe sent: 206367
        Sequence number of last probe received: 430097
    Chassis cluster LED information:
        Current LED color: Green
        Last LED change reason: No failures
    Control port tagging:
        Enabled

    Cold Synchronization:
        Status:
            Cold synchronization completed for: flowd
            Cold synchronization failed for: N/A
            Cold synchronization not known for: N/A
            Current Monitoring Weight: 0

        Statistics:
            Number of cold synchronization completed: 1
            Number of cold synchronization failed: 0

                PFE             Cold-Sync complete           Cold-Sync failed
                ----------------------------------------------------------------
             flowd                   1                             0

        Events:
            Dec 14 18:10:12.861 : Cold sync completed for PFE flowd

    Redundancy group: 0, Threshold: 255, Monitoring failures: none
        Events:
            Dec 14 18:09:15.720 : hold->secondary, reason: Hold timer expired

    Redundancy group: 1, Threshold: 255, Monitoring failures: none
        Events:
            Dec 14 18:09:15.726 : hold->secondary, reason: Hold timer expired

    Interface monitoring:
        Statistics:
            Monitored interface failure count: 0

        Events:
            Dec 14 18:09:22.949 : Interface ge-5/0/9 monitored by rg 1, changed state from Down to Up
            Dec 14 18:09:23.442 : Interface ge-5/0/11 monitored by rg 1, changed state from Down to Up
            Dec 14 18:09:23.909 : Interface ge-5/0/4 monitored by rg 1, changed state from Down to Up
            Dec 14 18:10:06.933 : Interface ge-0/0/4 monitored by rg 1, changed state from Down to Up
            Dec 14 18:10:07.002 : Interface ge-0/0/8 monitored by rg 1, changed state from Down to Up
            Dec 14 18:10:07.185 : Interface ge-0/0/10 monitored by rg 1, changed state from Down to Up

    Fabric monitoring:
        Status:
            State: Disabled
            Activation status: Active

    node1:
    --------------------------------------------------------------------------
    Control link statistics:
        Control link 0:
            Heartbeat packets sent: 430099
            Heartbeat packets received: 429970
            Heartbeat packet errors: 0
            Duplicate heartbeat packets received: 0
        Control recovery packet count: 0
        Sequence number of last heartbeat packet sent: 430097
        Sequence number of last heartbeat packet received: 206367
    Fabric link statistics:
        Probes sent: 430097
        Probes received: 429890
        Probe errors: 0
        Probes not processed: 41151
        Probes dropped due to control link down: 0
        Sequence number of last probe sent: 430097
        Sequence number of last probe received: 206367
    Chassis cluster LED information:
        Current LED color: Green
        Last LED change reason: No failures
    Control port tagging:
        Enabled

    Cold Synchronization:
        Status:
            Cold synchronization completed for: flowd
            Cold synchronization failed for: N/A
            Cold synchronization not known for: N/A
            Current Monitoring Weight: 0

        Statistics:
            Number of cold synchronization completed: 1
            Number of cold synchronization failed: 0

                PFE             Cold-Sync complete           Cold-Sync failed
                ----------------------------------------------------------------
             flowd                   1                             0

        Events:
            Dec  9 12:28:37.551 : Cold sync completed for PFE flowd

    Redundancy group: 0, Threshold: 255, Monitoring failures: none
        Events:
            Dec  9 12:27:37.349 : hold->secondary, reason: Hold timer expired
            Dec 14 18:04:30.980 : secondary->primary, reason: Device timer expired

    Redundancy group: 1, Threshold: 255, Monitoring failures: none
        Events:
            Dec  9 12:27:37.354 : hold->secondary, reason: Hold timer expired
            Dec 14 18:04:31.008 : secondary->primary, reason: Device timer expired

    Interface monitoring:
        Statistics:
            Monitored interface failure count: 3

        Events:
            Dec  9 12:27:47.741 : Interface ge-0/0/4 monitored by rg 1, changed state from Down to Up
            Dec  9 12:28:31.677 : Interface ge-5/0/4 monitored by rg 1, changed state from Down to Up
            Dec  9 12:28:31.842 : Interface ge-5/0/9 monitored by rg 1, changed state from Down to Up
            Dec  9 12:28:32.256 : Interface ge-5/0/11 monitored by rg 1, changed state from Down to Up
            Dec 14 18:06:28.296 : Interface ge-0/0/4 monitored by rg 1, changed state from Up to Down
            Dec 14 18:06:28.383 : Interface ge-0/0/8 monitored by rg 1, changed state from Up to Down
            Dec 14 18:06:28.472 : Interface ge-0/0/10 monitored by rg 1, changed state from Up to Down
            Dec 14 18:10:07.506 : Interface ge-0/0/4 monitored by rg 1, changed state from Down to Up
            Dec 14 18:10:07.550 : Interface ge-0/0/8 monitored by rg 1, changed state from Down to Up
            Dec 14 18:10:07.551 : Interface ge-0/0/10 monitored by rg 1, changed state from Down to Up

    Fabric monitoring:
        Status:
            State: Disabled
            Activation status: Active

References
----------

<http://kb.juniper.net/KB15421>