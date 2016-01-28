---
title: Show-bgp-sum.slax
permalink: /Show-bgp-sum.slax/
---

    /*
     *  show_bgp_sum.slax
     *
     *  Author: Pavel Gulchouck <gul@gul.kiev.ua>
     *
     *  Version History
     *  ===============
     *  v0.1         Initial release.
     *  v0.2         Process groups inheritance, support route-instances
     *
     */

    version 1.0;

    ns junos = "http://xml.juniper.net/junos/*/junos";
    ns xnm = "http://xml.juniper.net/xnm/1.1/xnm";
    ns jcs = "http://xml.juniper.net/junos/commit-scripts/1.0";

    import "../import/junos.xsl";

    match / {
        <op-script-results> {

            var $reqconf = <get-configuration database="commited" inherit="inherit" format="xml"> {
                                    <configuration> {
                                            <protocols> {
                                                    <bgp>;
                                            }
                                    }
                            }
            var $bgpconf = jcs:invoke($reqconf);
            var $reqinst = <get-instance-information format="brief">;
            var $instres = jcs:invoke($reqinst);
            var $bgpinstconf := {
                    for-each ($instres/instance-core) {
                            if (not(jcs:regex("^__", instance-name)) and not(instance-name = "master")) {
                                    var $reqinstconf = <get-configuration database="commited" inherit="inherit" format="xml"> {
                                            <configuration> {
                                                    <routing-instances> {
                                                            <instance> {
                                                                    <name> instance-name;
                                                                    <protocols> {
                                                                            <bgp>;
                                                                    }
                                                            }
                                                    }
                                            }
                                    }
                                    var $inst = jcs:invoke($reqinstconf)/routing-instances;
                                    if ($inst/instance/protocols/bgp/group/neighbor) {
                                            <instance> {
                                                    <name> $inst/instance/name;
                                                    <protocols> {
                                                            <bgp> {
                                                                    for-each ($inst/instance/protocols/bgp/group) {
                                                                            <group> {
                                                                                    <name> name;
                                                                                    for-each (neighbor) {
                                                                                            <neighbor> {
                                                                                                    <name> name;
                                                                                            }
                                                                                    }
                                                                            }
                                                                    }
                                                            }
                                                    }
                                            }
                                    }
                            }
                    }
            }
            var $req = <get-bgp-summary-information format="brief">;
            var $results = jcs:invoke($req);

            <output> jcs:printf("%-22s %-7s %11s %-6s %-20s %-12s %s", "   Peer", "    AS", "Last Up/Dwn", "State", "       Act/Rcv/Acc", "[Inst/]Group", " Descr");
            for-each ($results/bgp-peer) {
                    var $state = jcs:regex(".....?.?", peer-state);
                    var $prefixes = {
                            if ($state[1] = "Establ") {
                                    expr jcs:printf("%d/%d/%d", bgp-rib/active-prefix-count, bgp-rib/received-prefix-count, bgp-rib/accepted-prefix-count);
                            }
                    }
                    var $peer = peer-address;
                    var $group = $bgpconf/protocols/bgp/group[not(@inactive) and neighbor[not(@inactive)]/name=$peer]/name;
                    var $grname = {
                            if ($group) {
                                    copy-of $group;
                            } else {
                                    var $inst = $bgpinstconf/instance/protocols/bgp/group[not(@inactive) and neighbor[not(@inactive)]/name=$peer];
                                    expr jcs:printf("%s/%s", $inst/../../../name, $inst/name);
                            }
                    }
                    <output> jcs:printf("%-22s %7s %11s %-6s %20s %-12s %s", $peer, peer-as, elapsed-time, $state[1], $prefixes, $grname, description);
            }
        }
    }