---
title: Noc interface description.xslt
permalink: /Noc_interface_description.xslt/
---

<?xml version="1.0" standalone="yes"?>
`  `<xsl:stylesheet version="1.0"
   xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
   xmlns:junos="http://xml.juniper.net/junos/*/junos"
   xmlns:xnm="http://xml.juniper.net/xnm/1.1/xnm"
   xmlns:jcs="http://xml.juniper.net/junos/commit-scripts/1.0">
`  `<xsl:import href="../import/junos.xsl"/>
`  `<xsl:template match="configuration">
`  `
`  `<xsl:variable name="ifler" select="apply-macro[name = 'noc_interface_description']/data[not(starts-with(name,      'noc_interface_descriptio'))]"/>
`  `<xsl:if test="$ifler/name">
`  `<xsl:for-each select="$ifler">
`       `<change>
`               `<interfaces>
`                       `<interface>
`                               `
`                               `<xsl:if test="contains(./name, '.')">
`                                       `<name><xsl:value-of select="substring-before(./name, '.')"/></name>
`                                               `<unit><xsl:value-of select="substring-after(./name, '.')"/>
`                                                       `<description><xsl:value-of select="./value"/></description>
`                                               `</unit>
`                               `</xsl:if>
`                               `<xsl:if test="not(contains(./name, '.'))">
`                                       `<name><xsl:value-of select="./name"/></name>
`                                               `<description><xsl:value-of select="./value"/></description>
`                               `</xsl:if>
`                       `</interface>
`               `</interfaces>
`       `</change>
` `</xsl:for-each>
` `<change action = "replace">
`       `<apply-macro replace="replace">
`               `<name>`noc_interface_description`</name>
`                       `<data>
`                               `<name>`noc_interface_description`</name>
`                                       `<value>`ok`</value>
`                               `</data>
`                       `</apply-macro>
` `</change>
` `</xsl:if>
` `</xsl:template>
` `</xsl:stylesheet>