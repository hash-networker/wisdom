---
title: Aspath.xslt
permalink: /Aspath.xslt/
---

<?xml version="1.0" standalone="yes"?>
<xsl:stylesheet version="1.0"
   xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
   xmlns:junos="http://xml.juniper.net/junos/*/junos"
   xmlns:xnm="http://xml.juniper.net/xnm/1.1/xnm"
   xmlns:jcs="http://xml.juniper.net/junos/commit-scripts/1.0">
`  `<xsl:import href="../import/junos.xsl"/>
`  `<xsl:template match="configuration">
`    <xsl:for-each select="//from/as-path[jcs:regex("^AS([0-9]+)$", .)]">`
`      `<transient-change>
`        `<policy-options>
`          `<as-path replace="replace">
`            `<name>
`              `<xsl:value-of select="."/>
`            `</name>
`            `<path>
`              `<xsl:value-of select="concat(".*", substring-after(., "AS"), ".*")"/>
`            `</path>
`          `</as-path>
`        `</policy-options>
`      `</transient-change>
`    `</xsl:for-each>
`  `</xsl:template>
</xsl:stylesheet>