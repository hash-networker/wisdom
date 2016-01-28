---
title: L2TP QOS
permalink: /L2TP_QOS/
---

**This configuration is applied on the LNS**

------------------------------------------------------------------------

**Configuration Task**

------------------------------------------------------------------------

-   -   First, create a variable that will be used by RADIUS to define the total \[max\] bandwidth of an individual subscriber;

**hostname(config)\#**qos-parameter-define max-subscriber-bandwidth

**hostname(config-qos-parameter-define)\#**controlled-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**instance-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**subscriber-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**range X Y *(x=lower value; y=upper value - optional)*

**hostname(config-qos-parameter-define)\#**exit

------------------------------------------------------------------------

-   -   Second, create a variable that will be used by RADIUS to define the amount of G.711 calls;

**hostname(config)\#**qos-parameter-define number-of-lines

**hostname(config-qos-parameter-define)\#**controlled-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**instance-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**subscriber-interface-type l2tp-session

**hostname(config-qos-parameter-define)\#**range X Y *(x=lower value; y=upper value - optional)*

**hostname(config-qos-parameter-define)\#**exit

------------------------------------------------------------------------

-   -   Next, create the schedulers for the variables above;

**hostname(config)\#**scheduler-profile subscriber-voice

**hostname(config-scheduler-profile)\#**shaping-rate number-of-lines \* 150000

**hostname(config-scheduler-profile)\#**strict-priority

**hostname(config-scheduler-profile)\#**exit

**hostname(config)\#**scheduler-profile subscriber

**hostname(config-scheduler-profile)\#**shared-shaping-rate max-subscriber-bandwidth auto

**hostname(config-scheduler-profile)\#**exit

------------------------------------------------------------------------

-   -   Create the Traffic-Class and Traffic-Class-Group

**hostname(config)\#**traffic-class VoIP_Traffic_Class

**hostname(config-traffic-class)\#**fabric-strict-priority

**hostname(config-traffic-class)\#**exit

**hostname(config)\#**traffic-class-group EF auto-strict-priority

**hostname(config-traffic-class-group)\#**traffic-class VoIP_Traffic_Class

**hostname(config-traffic-class-group)\#**exit

------------------------------------------------------------------------

-   -   Create the Classifier-list, IP Policy-list and apply it to the subscriber profile;

**hostname(config)\#**ip classifier-list VoIP_Server udp host X.X.X.X range 5060 5061 any *&lt;--SIP Packets*

**hostname(config)\#**ip classifier-list VoIP_Server ip X.X.X.X X.X.X.X any *&lt;--RTP Packets*

**hostname(config)\#**ip policy-list VoIP_Packets

**hostname(config-policy-list)\#**classifier-group VoIP_Server

**hostname(config-policy-list-classifier-group)\#**traffic-class VoIP_Traffic_Class

**hostname(config-policy-list-classifier-group)\#**exit

**hostname(config)\#**profile Profile_Name_Here

**hostname(config-profile)\#**ip policy output VoIP_Packets statistics enabled

**hostname(config-profile)\#**exit

------------------------------------------------------------------------

-   -   Next, remove the best-effort ip queue to the server-default qos-profile to enable L2TP Session queues (as per documentation);

**hostname(config)\#**qos-profile server-default

**hostname(config-qos-profile)\#**no ip queue traffic-class best-effort

------------------------------------------------------------------------

-   -   Finally, create the QOS-Profile for the L2TP Sessions that will be assigned via RADIUS (or can be to intf);

**hostname(config)\#**qos-profile l2tpQOSprofile1

**hostname(config-qos-profile)\#**l2tp-session queue traffic-class VoIP_Traffic_Class scheduler-profile subscriber-voice

**hostname(config-qos-profile)\#**l2tp-session queue traffic-class best-effort scheduler-profile subscriber

**hostname(config-qos-profile)\#**exit

------------------------------------------------------------------------

**RADIUS Attributes used:** \[26-82\] Qos-Parameters

**Attribute \[26-82\]**

number-of-lines = X *&lt;-- Populate X with the number of G.711 VoIP accounts for the sub.*

max-subscriber-bandwidth = Y *&lt;-- Populate Y with the download bandwidth of the sub.*

------------------------------------------------------------------------

**Attribute \[26-26\]**

Qos-profile-name = l2tpQOSprofile1

Apply above via RADIUS to any subscriber that requires shaping or apply profile to ATM interface which affects ALL subscribers