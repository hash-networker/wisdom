---
title: Shbgpsum.slax
permalink: /Shbgpsum.slax/
---

    /*
     * An op script to restrict "show bgp summary" to a single AS Number
     *
     * 2009.11.11 Pierfrancesco Caci
     * License: public domain
     *
     * Changelog:
     * 2010.11.05 - Added the possibility to match the description in addition to the AS number
     *            - Added a match on peer-state
     *            - Merged functionality of peerdown.slax
     * 2011.06.17 - Make neighbor name match case insensitive
     *
     */
    version 1.0;


    ns junos = "http://xml.juniper.net/junos/*/junos";
    ns xnm = "http://xml.juniper.net/xnm/1.1/xnm";
    ns jcs = "http://xml.juniper.net/junos/commit-scripts/1.0";


    import "../import/junos.xsl";

    /* FIXME: find a way to do the same without needing to write 'asn' */
    var $arguments = {
      <argument> {
        <name> "asn";
        <description> "Autonomous System Number";
      }
      <argument> {
        <name> "name";
        <description> "Neighbor name (regexp against the description)";
      }
      <argument> {
        <name> "state";
        <description> "(up|down) Neighbor state (down matches anything other than Established)";
      }
    }

      param $asn;
    param $name;
    param $state;

    var $string = {
      if ( $name ) {
        expr translate($name,"ABCDEFGHIJKLMNOPQRSTUVWXYZ","abcdefghijklmnopqrstuvwxyz");
     } else {
      expr "^$";
     }
    }

    var $status = {
      if ( $state == "up" ) {
        expr "Established";
      } else if ( $state == "down") {
        expr "(Active|Connect|Idle)";
      } else {
        expr ".*";
      }
    }

    match / {

      <op-script-results> {
        var $as = jcs:regex("(^[0-9]+$)", $asn);
        if ( ($as[1] > 1 and $as[1] < 4294967296) or $name ) {
          var $rpc = <get-bgp-summary-information> {
          }
          var $bgpsumm = jcs:invoke($rpc);

          <output> jcs:printf("Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dw\
    n State|#Active/Received/Accepted/Damped...\n");

          for-each ($bgpsumm/bgp-peer) {
              /*<output> jcs:printf("[%s %s - %s %s - %s %s - %s]\n",$asn,peer-as,$string,description,$stat\
    e,peer-state,$status);*/

              if ( (peer-as == $asn or
                    jcs:regex($string,translate(description,"ABCDEFGHIJKLMNOPQRSTUVWXYZ",
                                                "abcdefghijklmnopqrstuvwxyz"))) and
                   jcs:regex($status,peer-state) ) {
                <output> jcs:printf("%s\n",description);
                <output> jcs:printf("%-17s%10s%11s%11s%8s%8s%12s %s\n",
                                    peer-address,
                                    peer-as,
                                    input-messages,
                                    output-messages,
                                    route-queue-count,
                                    flap-count,
                                    elapsed-time,
                                    peer-state
                                    );


                for-each (bgp-rib) {
                    <output> jcs:printf("  %s: %s/%s/%s/%s\n",
                                        name,
                                        active-prefix-count,
                                        received-prefix-count,
                                        accepted-prefix-count,
                                        suppressed-prefix-count);

                  }

                <output> jcs:printf("----------\n");

              }

            }
        } else if ( $state =="down" ) {
          var $rpc = <get-bgp-summary-information> {
          }
          var $bgpsumm = jcs:invoke($rpc);

          /* This portions is from peerdown.slax by Teun Vink */
           <output> jcs:printf("%-34s %10s   %15s   %s", "peer IP", "peer ASN", "downtime", "peer descripti\
    on");
           <output> jcs:printf("===========================================================================\
    =======");

           /* search for the bgp information tag */
           <bgp-information> {
               /* print each peer which's state is not Established */
               for-each($bgpsumm/bgp-peer[peer-state!='Established']) {
                    <output> jcs:printf("%-35s%10s   %15s   %s", peer-address, peer-as, elapsed-time, descr\
    iption);
               }
           }

        } else {
          <xsl:message terminate="yes"> "Usage:\n" _
            "op shbgpsum asn <AS Number in the range 1-4294967295>\n\twill show only that ASN\n" _
            "op shbgpsum name <regular expression>\n\twill show neighbors whose description matches\n" _
            "op shbgpsum state down\n\twill show only neighbors not in Established state\n" _
            "op shbgpsum asn <AS Number> name <regex>\n\twill match both conditions (logical or)\n" _
            "op shbgpsum (asn <AS Number>|name <regex>) state (up|down)\n\twill show only selected peers th\
    at are either up or down\n";
        }
      }
    }