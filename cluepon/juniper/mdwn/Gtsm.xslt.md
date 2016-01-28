---
title: Gtsm.xslt
permalink: /Gtsm.xslt/
---

<?xml version="1.0" standalone="yes"?>
<xsl:stylesheet version="1.0"
   xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
   xmlns:junos="http://xml.juniper.net/junos/*/junos"
   xmlns:xnm="http://xml.juniper.net/xnm/1.1/xnm"
   xmlns:jcs="http://xml.juniper.net/junos/commit-scripts/1.0">
`  `<xsl:import href="../import/junos.xsl"/>
`  `<xsl:template match="configuration">
`    `
`    `<xsl:variable name="ttl" select="protocols/bgp/group/neighbor/apply-macro[name = "ttl-security"]"/>
`    `
`    `<xsl:for-each select="$ttl">
`      `<xsl:call-template name="jcs:emit-change">
`        `<xsl:with-param name="tag" select=""transient-change""/>
`        `<xsl:with-param name="dot" select=".."/>
`        `<xsl:with-param name="content">
`          `<multihop>
`            `<ttl>
`              `<xsl:value-of select="255"/>
`            `</ttl>
`          `</multihop>
`        `</xsl:with-param>
`      `</xsl:call-template>
`    `</xsl:for-each>
`    `
`    `<transient-change>
`      `<policy-options>
`        `<prefix-list replace="replace">
`          `<name>`GTSM-IPV4-NEIGHBORS`</name>
`          `<xsl:for-each select="$ttl">
`            <xsl:if test="contains(../name, ".")">`
`              `<prefix-list-item>
`                `<name>
`                  `<xsl:value-of select="../name"/>
`                `</name>
`              `</prefix-list-item>
`            `</xsl:if>
`          `</xsl:for-each>
`        `</prefix-list>
`        `<prefix-list replace="replace">
`          `<name>`GTSM-IPV6-NEIGHBORS`</name>
`          `<xsl:for-each select="$ttl">
`            <xsl:if test="contains(../name, ":")">`
`              `<prefix-list-item>
`                `<name>
`                  `<xsl:value-of select="../name"/>
`                `</name>
`              `</prefix-list-item>
`            `</xsl:if>
`          `</xsl:for-each>
`        `</prefix-list>
`      `</policy-options>
`    `</transient-change>
`  `</xsl:template>
</xsl:stylesheet>