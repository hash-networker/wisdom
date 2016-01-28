---
title: Dom.slax
permalink: /Dom.slax/
---

    /*
     * An op script to table optical power readings.
     *
     * 2010.03.12 Pierfrancesco Caci
     * License: public domain
     *
     * 20100408 - added check for different output xml from ge- interfaces.
     *
     * 20100505 - collect all interface descriptions with one command invoke
     *            and show physical interface description if unit 0 description
     *            does not exist - < cougar @ random . ee >
     *
     */
    version 1.0;


    ns junos = "http://xml.juniper.net/junos/*/junos";
    ns xnm = "http://xml.juniper.net/xnm/1.1/xnm";
    ns jcs = "http://xml.juniper.net/junos/commit-scripts/1.0";


    import "../import/junos.xsl";

    var $arguments = {
      <argument> {
      }
    }

    template resolve_ifdescr($ifdescrdb, $if)
    {
      if ($if) {
        if ($ifdescrdb/logical-interface[name == concat($if, ".0")]/description) {
          expr $ifdescrdb/logical-interface[name == concat($if, ".0")]/description;
        } else {
          if ($ifdescrdb/physical-interface[name == $if]/description) {
            expr $ifdescrdb/physical-interface[name == $if]/description;
          } else {
            expr "";        /* no descripion */
          }
        }
      } else {
        expr "?";           /* should never happen (ASSERT ?)*/
      }
    }

    match / {
      <op-script-results> {
          var $rpc-optics = <get-interface-optics-diagnostics-information> {
          }
          var $optics = jcs:invoke($rpc-optics);

          var $rpc-ifdecr = { <command> 'show interface descriptions'; }
          var $ifdescrdb = jcs:invoke($rpc-ifdecr);

          <output> jcs:printf("Interface    In mW  In dBm Out mW Out dBm Temp C Description\n");

          for-each ($optics/physical-interface) {
          var $unavail = optics-diagnostics/optic-diagnostics-not-available;
          var $tx-power = optics-diagnostics/laser-output-power;
          var $tx-dbm = optics-diagnostics/laser-output-power-dbm;
          var $rx-power = {
            if (optics-diagnostics/laser-rx-optical-power) { /* xe interfaces */
              expr optics-diagnostics/laser-rx-optical-power;
            }
            else if (optics-diagnostics/rx-signal-avg-optical-power) { /* ge */
              expr optics-diagnostics/rx-signal-avg-optical-power;
            }
          }
          var $rx-dbm = {
            if (optics-diagnostics/laser-rx-optical-power-dbm) {
              expr optics-diagnostics/laser-rx-optical-power-dbm;
            }
            else if (optics-diagnostics/rx-signal-avg-optical-power-dbm) {
              expr optics-diagnostics/rx-signal-avg-optical-power-dbm;
            }
          }
          var $temp = optics-diagnostics/module-temperature/@junos:celsius;

          if (not ($unavail)  ) {
            var $if = { expr name; }
            var $descr = { call resolve_ifdescr($ifdescrdb, $if); }
            <output> jcs:printf("%-12s%7s%7s%7s%8s%7s %s\n",
                    name,
                    $rx-power,$rx-dbm,
                    $tx-power,$tx-dbm,
                    $temp,
                    $descr);
          }
        }
      }
    }