---
title: Converted jnpr xml conf to simple data struct
permalink: /Converted_jnpr_xml_conf_to_simple_data_struct/
---

Here is a quick demo of a simple way to build python data structures from Junos xml format config. Most of the code in the xml2data module is taken and adpated from the python activestate recipe site. The pastebin link includes the xml2data module imported in the example. Obviously pythonic ninjas will be comfortable with the full complex power of xml and python. The below is intended as a helpfule hint for network engineers capable of reading and writing pigeon python and looking to make quick, dirty and useful script for configuration parsing. :-) <http://pastebin.com/9Q32HU2Q>

` joob-mac:Desktop Phill$ python`
`Python 2.7.1 (r271:86832, Jun 16 2011, 16:59:05) `
`[GCC 4.2.1 (Based on Apple Inc. build 5658) (LLVM build 2335.15.00)] on darwin`
`Type "help", "copyright", "credits" or "license" for more information.`
`>>>`
`# Make sure you have the xml module install and the xml2data module in`
`# your python path. You can check you python path like this.`
`>>> import sys`
`>>> sys.path`
`# if all is good import the xml2data module`
`>>> import xml2data`
`# path to the xml format file. You should trim the file so the outermost tags`
`# is `<configuration>` <\configuration>. So basically remove the rpc tags.`
`# might be best to create this with 'show conf | disp inher | disp xml'`
`>>> cfg_file = '/Users/Phill/Dev/tools/some_rtrs_jnpr_cfg.xml'`
`>>> `
`# pass the xml file to the xml_jcfg2data method.`
`>>> jcfg_data = xml2data.xml_jcfg2data(cfg_file)`
`>>> `
`# jcfg_data is an object containing the cfg as a data structure. You can access`
`# it with OOB syntax. For example jcfg_data.bridge_domains.domain would return`
`# an array for all bridge-domains.`
`#`
`# More useful might be a for loop like 'for bd in bridge_domains.domain'`
`# This would make a bd object for each iteration in the loop.`
`# Each step of the loop is an individual bridge domains object.`
`# We could then do something like 'print bd.name' in the loop to print each`
`# bridge domains name.`
`#`
`# for for bd in bridge_domains.domain:`
`#    print bd.name`
`#`
`# Here we can do something similar for interfaces but only for one element in`
`# an array of interfaces`
`>>> jcfg_data.interfaces.interface[0]`
`{apply_groups:u'MTU-GROUP', description:u'linknet ae1-RFAD07_43-RFAD06_43 GEF1',`
`gigether_options:{ieee_802_3ad:{bundle:u'ae1'}, no_auto_negotiation:''},`
`name:u'ge-0/0/0'}`
`>>> `
`# if we omitted the [0], element index, it would return a whole mess of`
`# interfaces. the entire array`
`# for ifd in jcfg_data.interfaces.interface:`
`#    if 'description' is in ifd:`
`#        print '%s has description %s' % (ifd.name, ifd.description)`
`#    else:`
`#        print 'WTF? %s has no description' % ifd.name`
`# The above would print the description of a major interface if it has one,`
`# or complain if it doesn't have one.`
</nowiki>