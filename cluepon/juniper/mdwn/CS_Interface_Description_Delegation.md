---
title: CS Interface Description Delegation
permalink: /CS_Interface_Description_Delegation/
---

Information
-----------

With this script, you can delegate interface description rights to operators without the delegation of "interfaces" rights. So this way operator will not have access to any interface configuration but can write or edit interface descriptions. Perhaps this is a safe way of having operators take care of interface descriptions and being safe about operator misconfigurations.

Usage
-----

apply-macro noc_interface_description {

`   ds-1/2/1:2.0 desctiption_here;`
`   noc_interface_description ok;`

}

After you commit this configuration, interface statements under this macro will disappear and descriptions will be written to referenced interfaces.

Bugs
----

Please let me know...

ChangeLog
---------

1.0 - Initial Version