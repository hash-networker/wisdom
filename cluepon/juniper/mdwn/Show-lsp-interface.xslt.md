---
title: Show-lsp-interface.xslt
permalink: /Show-lsp-interface.xslt/
---

<?xml version="1.0" standalone="yes"?>
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:junos="http://xml.juniper.net/junos/*/junos" xmlns:xnm="http://xml.juniper.net/xnm/1.1/xnm" xmlns:jcs="http://xml.juniper.net/junos/commit-scripts/1.0" version="1.0">
`  `
`  `<xsl:import href="../import/junos.xsl"/>
`  `<xsl:variable name="arguments">
`    `<argument>
`      `<name>`interface`</name>
`      `<description>`interface name`</description>
`    `</argument>
`    `<argument>
`      `<name>`srcif`</name>
`      `<description>`source interface name`</description>
`    `</argument>
`    `<argument>
`      `<name>`dstif`</name>
`      `<description>`destination interface name`</description>
`    `</argument>
`    `<argument>
`      `<name>`showbypass`</name>
`      `<description>`1 = show Bypass LSPs also`</description>
`    `</argument>
`  `</xsl:variable>
`  `<xsl:param name="interface"/>
`  `<xsl:param name="srcif"/>
`  `<xsl:param name="dstif"/>
`  `<xsl:param name="showbypass"/>
`  `<xsl:template name="resolve_ifdescr">
`    `<xsl:param name="ifdescrdb"/>
`    `<xsl:param name="if"/>
`    `<xsl:choose>
`      `<xsl:when test="$if">
`        `<xsl:choose>
`          `<xsl:when test="$ifdescrdb/logical-interface[name = $if]/description">
`            `<xsl:value-of select="$ifdescrdb/logical-interface[name = $if]/description"/>
`          `</xsl:when>
`          `<xsl:otherwise>`-` </xsl:otherwise>
`        `</xsl:choose>
`      `</xsl:when>
`      `<xsl:otherwise>`?` </xsl:otherwise>
`    `</xsl:choose>
`  `</xsl:template>
`  `<xsl:template name="resolve_nh_ifdescr">
`    `<xsl:param name="ifdescrdb"/>
`    `<xsl:param name="nhif"/>
`    `<xsl:variable name="if" select="$nhif"/>
`    `<xsl:call-template name="resolve_ifdescr">
`      `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`      `<xsl:with-param name="if" select="$if"/>
`    `</xsl:call-template>
`  `</xsl:template>
`  `<xsl:template name="resolve_ph_ifdescr">
`    `<xsl:param name="ifdescrdb"/>
`    `<xsl:param name="phif"/>
`    `<xsl:variable name="if" select="$phif"/>
`    `<xsl:call-template name="resolve_ifdescr">
`      `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`      `<xsl:with-param name="if" select="$if"/>
`    `</xsl:call-template>
`  `</xsl:template>
`  `<xsl:template name="show_lsp_list">
`    `<xsl:variable name="q1">
`      `<command>`show mpls lsp detail`</command>
`    `</xsl:variable>
`    `<xsl:variable name="lspdb" select="jcs:invoke($q1)"/>
`    `<xsl:variable name="q2">
`      `<command>`show interface descriptions`</command>
`    `</xsl:variable>
`    `<xsl:variable name="ifdescrdb" select="jcs:invoke($q2)"/>
`    <xsl:if test="$srcif = """>`
`      `<xsl:value-of select="jcs:output("------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------")"/>
`      `<xsl:value-of select="jcs:output("t name                                                                                                                              dst             next-hop        dst-if        dst-ifdescr
")"/>
`      <xsl:for-each select="$lspdb/rsvp-session-data[session-type = "Ingress"]/rsvp-session/mpls-lsp[lsp-state = "Up"]">`
`        `<xsl:variable name="name" select="name"/>
`        `<xsl:variable name="dst" select="destination-address"/>
`        `<xsl:variable name="nhip" select="substring-before(substring-after(mpls-lsp-path/explicit-route, "
") , "
")"/>
`        `<xsl:variable name="routequery">
`          `<command>
`            `<xsl:value-of select="concat("show route table inet.0 protocol direct ", $nhip)"/>
`          `</command>
`        `</xsl:variable>
`        `<xsl:variable name="routeresult" select="jcs:invoke($routequery)"/>
`        `<xsl:variable name="nhif" select="$routeresult/route-table/rt/rt-entry/nh/via"/>
`        <xsl:if test="(($interface = "") and($dstif = "")) or($interface = $nhif) or($dstif = $nhif)">`
`          `<xsl:variable name="ndescr">
`            `<xsl:call-template name="resolve_nh_ifdescr">
`              `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`              `<xsl:with-param name="nhif" select="$nhif"/>
`            `</xsl:call-template>
`          `</xsl:variable>
`          `<xsl:variable name="outline">
`            `<xsl:value-of select="jcs:printf("I %-32s                                                                                                  %-15s %-15s %-13s %-50s
", $name, $dst, $nhip, $nhif, $ndescr)"/>
`          `</xsl:variable>
`          `<xsl:value-of select="jcs:output($outline)"/>
`        `</xsl:if>
`      `</xsl:for-each>
`    `</xsl:if>
`    <xsl:if test="$dstif = """>`
`      `<xsl:value-of select="jcs:output("------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------")"/>
`      `<xsl:value-of select="jcs:output("t name                             src             prev-hop        src-if        src-ifdescr
")"/>
`      <xsl:for-each select="$lspdb/rsvp-session-data[session-type = "Egress"]/rsvp-session[lsp-state = "Up" and(not(starts-with(name, "Bypass")) or($showbypass = 1))]">`
`        `<xsl:variable name="name" select="name"/>
`        `<xsl:variable name="src" select="source-address"/>
`        `<xsl:variable name="phip" select="packet-information/previous-hop"/>
`        `<xsl:variable name="routequery">
`          `<command>
`            `<xsl:value-of select="concat("show route table inet.0 protocol direct ", $phip)"/>
`          `</command>
`        `</xsl:variable>
`        `<xsl:variable name="routeresult" select="jcs:invoke($routequery)"/>
`        `<xsl:variable name="phif" select="$routeresult/route-table/rt/rt-entry/nh/via"/>
`        <xsl:if test="(($interface = "") and($srcif = "")) or($interface = $phif) or($srcif = $phif)">`
`          `<xsl:variable name="pdescr">
`            `<xsl:call-template name="resolve_ph_ifdescr">
`              `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`              `<xsl:with-param name="phif" select="$phif"/>
`            `</xsl:call-template>
`          `</xsl:variable>
`          `<xsl:variable name="outline">
`            `<xsl:value-of select="jcs:printf("E %-32s %-15s %-15s %-13s %-50s
", $name, $src, $phip, $phif, $pdescr)"/>
`          `</xsl:variable>
`          `<xsl:value-of select="jcs:output($outline)"/>
`        `</xsl:if>
`      `</xsl:for-each>
`    `</xsl:if>
`    `<xsl:value-of select="jcs:output("------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------")"/>
`    `<xsl:value-of select="jcs:output("t name                             src             prev-hop        src-if        src-ifdescr                                        dst             next-hop        dst-if        dst-ifdescr
")"/>
`    <xsl:for-each select="$lspdb/rsvp-session-data[session-type = "Transit"]/rsvp-session[lsp-state = "Up" and(not(starts-with(name, "Bypass")) or($showbypass = 1))]">`
`      `<xsl:variable name="name" select="name"/>
`      `<xsl:variable name="src" select="source-address"/>
`      `<xsl:variable name="dst" select="destination-address"/>
`      <xsl:if test="packet-information[@heading = "  PATH"]/previous-hop">`
`        `<xsl:variable name="phip" select="packet-information[@heading = "  PATH"]/previous-hop"/>
`        `<xsl:variable name="phif" select="packet-information[previous-hop = $phip]/interface-name"/>
`        `<xsl:variable name="nhip" select="packet-information[@heading = "  PATH"]/next-hop"/>
`        `<xsl:variable name="nhif" select="packet-information[next-hop = $nhip]/interface-name"/>
`        <xsl:if test="(($interface = "") and($srcif = "") and($dstif = "")) or($interface = $phif) or($interface = $nhif) or(($srcif = $phif) and($dstif = "")) or(($dstif = $nhif) and($srcif = "")) or(($srcif = $phif) and($dstif = $nhif))">`
`          `<xsl:variable name="pdescr">
`            `<xsl:call-template name="resolve_ph_ifdescr">
`              `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`              `<xsl:with-param name="phif" select="$phif"/>
`            `</xsl:call-template>
`          `</xsl:variable>
`          `<xsl:variable name="ndescr">
`            `<xsl:call-template name="resolve_nh_ifdescr">
`              `<xsl:with-param name="ifdescrdb" select="$ifdescrdb"/>
`              `<xsl:with-param name="nhif" select="$nhif"/>
`            `</xsl:call-template>
`          `</xsl:variable>
`          `<xsl:variable name="outline">
`            `<xsl:value-of select="jcs:printf("T %-32s %-15s %-15s %-13s %-50s %-15s %-15s %-13s %-50s
", $name, $src, $phip, $phif, $pdescr, $dst, $nhip, $nhif, $ndescr)"/>
`          `</xsl:variable>
`          `<xsl:value-of select="jcs:output($outline)"/>
`        `</xsl:if>
`      `</xsl:if>
`    `</xsl:for-each>
`  `</xsl:template>
`  `<xsl:template match="/">
`    `<op-script-results>
`      `<xsl:call-template name="show_lsp_list"/>
`    `</op-script-results>
`  `</xsl:template>
</xsl:stylesheet>