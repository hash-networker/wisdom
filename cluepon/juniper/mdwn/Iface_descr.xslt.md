---
title: Iface descr.xslt
permalink: /Iface_descr.xslt/
---

<?xml version="1.0" standalone="yes"?>
<xsl:stylesheet version="1.0"
   xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
   xmlns:junos="http://xml.juniper.net/junos/*/junos"
   xmlns:xnm="http://xml.juniper.net/xnm/1.1/xnm"
   xmlns:jcs="http://xml.juniper.net/junos/commit-scripts/1.0">
`  `<xsl:import href="../import/junos.xsl"/>
`  `<xsl:template name="interface_comment">
`    `<xsl:param name="iface"/>
`    `<xsl:param name="comment"/>
`    `
`    `<xsl:for-each select="//protocols//interface[name = $iface]">
`      `
`      `<xsl:call-template name="jcs:emit-change">
`        `<xsl:with-param name="dot" select=".."/>
`        `<xsl:with-param name="content">
`          `<junos:comment replace="replace">
`            `<xsl:value-of select="$comment"/>
`          `</junos:comment>
`          `<interface>
`            `<name>
`              `<xsl:value-of select="$iface"/>
`            `</name>
`          `</interface>
`        `</xsl:with-param>
`      `</xsl:call-template>
`    `</xsl:for-each>
`  `</xsl:template>
`  `<xsl:template match="configuration">
`    `
`    `<xsl:for-each select="interfaces/interface">
`      `<xsl:variable name="int_comment" select="jcs:first-of(description, "No Description")"/>
`      `
`      `<xsl:call-template name="interface_comment">
`        `<xsl:with-param name="iface" select="name"/>
`        `<xsl:with-param name="comment" select="$int_comment"/>
`      `</xsl:call-template>
`      `
`      `<xsl:for-each select="unit">
`        `<xsl:variable name="iface" select="concat(../name, ".", name)"/>
`        `<xsl:variable name="comment" select="jcs:first-of(description, ../description)"/>
`        `<xsl:choose>
`          `<xsl:when test="string-length($comment)">
`            `<xsl:call-template name="interface_comment">
`              `<xsl:with-param name="iface" select="$iface"/>
`              `<xsl:with-param name="comment" select="$comment"/>
`            `</xsl:call-template>
`          `</xsl:when>
`          `<xsl:otherwise>
`            <xsl:if test="name != "0"">`
`              `
`              `<xsl:call-template name="interface_comment">
`                `<xsl:with-param name="iface" select="$iface"/>
`                `<xsl:with-param name="comment" select=""No Description""/>
`              `</xsl:call-template>
`            `</xsl:if>
`          `</xsl:otherwise>
`        `</xsl:choose>
`      `</xsl:for-each>
`    `</xsl:for-each>
`  `</xsl:template>
</xsl:stylesheet>