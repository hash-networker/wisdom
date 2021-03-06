---
title: Feature Requests
permalink: /Feature_Requests/
---

While Juniper has produced some of the best router hardware and software available to date, there are still some areas which can be improved through innovative ideas and the practical experiences of network operators. Juniper accepts Feature Requests from its customers, but due to obvious business concerns will often give priority to requests which have significant revenue attached (for example, a large sale that is dependent on the implementation of a specific feature).

We hope that this list will both result in the adoption of the features listed, and inspire others to think of even more useful features. If you see a feature here which you think would be a good idea, please contact your Juniper representative and encourage them to open an enhancement request. The more companies that request a feature, and the more revenue that is associated with those requests, the more likely it is that the feature will be implemented.

Software-Only Feature Requests
------------------------------

-   [Ignore BGP Origin attribute in best-path selection](/ER_BGP_Path-Selection_Origin-Ignore "wikilink")
-   [LNS capability on J series](/ER_LNS_capability "wikilink")
-   [Default firewall filter term/action](/ER_Firewall_default_term "wikilink")
-   [Disable command for BGP neighbors and groups](/ER_BGP_neighbor_disable "wikilink")
-   [Symbolic community definitions in static routes](/ER_Symbolic_community_use "wikilink")
-   [Unnamed AS-SET and Community references in policy-statements](/ER_Unnamed_AS-PATH_and_Community_references "wikilink")
-   [Automatic mapping of Ethernet unit numbers to VLAN ID](/ER_Automatic_VLAN_ID "wikilink")
-   [Search and Sort options for the "show bgp summary" command](/ER_Enhance_BGP_summary "wikilink")
-   [Allow JunOS to be downloaded from Juniper from the router](/ER_Direct_JunOS_download "wikilink")
-   [Configurable per-protocol default-actions](/ER_Configure_protocol_defaults "wikilink")
-   [Allow telnet to login without user/password, for route-server use](/ER_Remove_telnet_user_password "wikilink")
-   [BGP neighbor level prefix-list function, outside of policy-statements](/ER_BGP_Neighbor_prefix_list "wikilink")
-   [Reduce SNMP traps generated by BGP state changes](/ER_Reduce_BGP_SNMP_traps "wikilink")
-   [Automatic sizing of policer burst values](/ER_Automatic_policer_burst_sizes "wikilink")
-   [Enhance curses-interactive CLI "monitor" commands](/ER_Enhance_monitor_commands "wikilink")
-   [IS-IS overload wait-for-bgp (RFC3277)](/ER_IS-IS_overload_wait-for-bgp "wikilink")
-   [Implement a BGP "overload" setting](/ER_BGP_overload "wikilink")
-   [Reduce IS-IS/PIM/Global restart-duration to more sensible values](/ER_ISIS_PIM_Restart_duration "wikilink")
-   [Optionally act as a stratum 10 local clock for NTP](/ER_Local_time_source "wikilink")
-   [Backup REs should be able to do sane NTP clock sync without fxp0](/ER_Backup_RE_NTP_sync "wikilink")
-   [Default-originate mechanism for BGP neighbors](/ER_BGP_Default_originate "wikilink")
-   [Prefix-specific policer support for multiple, non-contiguous prefixes](/ER_Prefix_specific_policers_with_noncontiguous "wikilink")
-   [Implement buffering of syslog messages if there is no route to destination, similarly to SNMP trap buffering](/ER_Syslog_buffering "wikilink")
-   [Ability to specify the filename for the automatic config upload feature](/ER_configuration_file_upload_filename "wikilink")
-   [Allow static routes to be set and configured via policy-statements or prefix-lists](/ER_static_routing_using_prefix_lists "wikilink")
-   [Option to set the channel numbering format](/ER_channel_numbering "wikilink")
-   [CLI command to clear policer counters](/ER_Clear_policer_counters "wikilink")
-   [Enhance BFD Support](/ER_Enhance_BFD_Support "wikilink")
-   [Configuration auditing (count/find portions of configs referenced inside other configs)](/ER_Configuration_audit_command "wikilink")
-   [Rollback or undelete command for sub-sections of configurations](/ER_Enhanced_rollback_or_undelete_command "wikilink")
-   [Auto synchronization of commit/op/event scripts between Routing Engines](/ER_Synchronize_scripts_command "wikilink")
-   [Native BGP configuration for GTSM/TTL Security](/ER_BGP_TTL_Security_support_enhancements "wikilink")
-   [Archival of configuration in xml format](/ER_ARCHIVAL_XML "wikilink")
-   [Enhance BGP extended community features/support](/ER_Extended_Community_Enhancements "wikilink")
-   [Configuration templating leveraging 'groups' using additional arguments](/ER_Groups_Arguments "wikilink")
-   [Detect AS-PATH prepending](/ER_Detect_AS-PATH_prepends "wikilink")

Possibly Hardware-affecting Feature Requests
--------------------------------------------

-   [Maintain counters for the dsc discard interface.](/ER_DSC_Interface_counters "wikilink")
-   [Add ability to record external power loss](/ER_log_external_power_failure "wikilink")
-   [Ability to skip over IPv6 extension headers/hop-by-hop options when looking for UDP/TCP port numbers, flags, etc.](/ER_skip_over_IPv6_extensions "wikilink")

Resolved Feature Requests
-------------------------

-   [Add length modifiers to prefix-lists (Implemented in JUNOS 7.4)](/ER_Prefix_List_Length_Modifiers "wikilink")
-   [Allow empty BGP peer groups to exist (Implemented)](/ER_Empty_bgp_groups "wikilink")
-   [Channelized DS3 config should be set separately from first CT3 channel](/ER_channelized_parent_config "wikilink")
-   [Ability to filter on IPv6 TCP flags (Implemented in JUNOS 7.4)](/ER_Filter_IPv6_TCP_flags "wikilink")
-   [Allow automatic inclusion of external files into configuration (Implemented in JUNOS 7.4)](/ER_Configuration_includes "wikilink")
-   [Add "show route" flag for displaying active routes only (Implemented in JUNOS 8.0: show route active-path)](/ER_Show_active_routes "wikilink")
-   [Show ARP cache entry ages and incomplete entries in "show arp" (Implemented in JUNOS 8.1)](/ER_Show_arp_enhancement "wikilink")
-   [Allow optional hiding of some configuration portions (Implemented in JUNOS 8.2)](/ER_Configuration_hiding "wikilink")
-   [Command to generate test SNMP interface and BGP traps](/ER_SNMP_trap_testing "wikilink") (Implemented in JUNOS 8.2)
-   [SNMP access filtering via prefix-list](/ER_SNMP_prefix_lists "wikilink") (Implemented in JunOS 8.5 with 'client-list-name')
-   [BGP prefix-limit on feasible routes, not just received routes](/ER_BGP_Prefix_limit_enhancements "wikilink") (Implemented in JunOS 9.2 with 'accepted-prefix-limit')
-   [Issue warnings on console before rolling back a "commit confirmed"](/ER_Warn_before_commit_confirmed_rollback "wikilink") (Implemented in JUNOS 8.x ?)
