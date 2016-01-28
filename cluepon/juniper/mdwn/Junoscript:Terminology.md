---
title: Junoscript:Terminology
permalink: /Junoscript:Terminology/
---

Junoscript Terminology
----------------------

When discussing any technical topic, it really helps to be speaking the same language. Below is a list of junoscript terms and an explanation of each.

-   **Attribute**: name and value pair that is stored w/in the opening tag, written with the name to the left of an equals sign, and value in double quotes to the right. Any number of attributes can be assigned to an element, each separated by a space. ie. <configuration junos:commit-user="josh">
-   **Conditionally Set Variable** - Is a variable who's value is set by the output of subroutine including 'expr'. ie. var $foo = { if( string-length( $interface ) &gt; 0 ) { expr "empty"; } else { expr $interface; } }
-   **Element**: similar to a 'tag' in HTML.
-   **Element Data**: everything between start and end tags (both text as well as child elements)
-   **Function**: Coded procedures within the script engine. Not to be confused with the definable 'template', functions are part of the JUNOS operating system and return results unlike templates. Examples are substring(), jcs:printf(), and contains().
-   **Location Path**: a method of extracting data from an XML structure. This is done by referring to a 'node-set' variable, followed by one or more combinations of a / and a child name in the node-set.
    -   *Multiple Steps* - A pair of slashs, //, can be used as a kind of wildcard in the middle of the path to skip over node levels. ie. $top/interfaces/interface\[name == "ge-0/0/0"\]//family (skips over unit)
    -   *Parent Axis* - A pair of dots, .., can be used to move up a node. If $int=/interfaces/interface\[name == "ge-0/0/0"\], then $int/../interface\[name == "ge-0/0/1"\] would indicate you moved up one node and looked for a new 'interface' child node named ge-0/0/1.
    -   *Attribute Axis* - Allows you to pull an attribute from a particular node. if $results = jcs:invoke( "get-system-uptime-information" ), then $results/system-booted-time/date-time/@junos:seconds will be the junos:seconds attribute found on the date-time node.
    -   *Wildcard Match* - The wildcard matches all nodes at a given node level (by default the child axis), ie. $top/system/login/user/\* will match all configured local user accounts.
    -   *Context Node* - A single period, . can be used to reference the current node level in a function found in a predicate. ie. in $ge-interface = $configuration/interfaces/interface/name\[ starts- with(., “ge”) \] the <name> element node itself is evaluated by the starts-with() function since the . is used as an argument. This can also be used in a foreach loop to reference the current node.
    -   Less frequently used axes can be found here: <http://www.w3.org/TR/xpath#axes/>
-   **Namespace**: similar to a library of functions referenced in an include file or url. A namespace is a proper http url, but for ease of use, a 'shortcut' that points to the namespace can be defined by specifying the attribute xmlns:shortcutname="http://fqdn.com/url". This attribute is set in the <rpc-reply> element and impacts all children of the rpc-reply element. In slax, a shortcut can ALSO be set by calling 'ns shortcutname = "http://fqdn.com/url";'
-   **Node**: Combination of element, attribute. Text data is a node with a parent node which is an element. Useful to think about in a hierarchical tree. Each branch or leaf is a 'node', including the root.
-   **Parameters**: Similar to variables, except their value is assigned by an external source. Some examples of global parameters are $product, $user, $hostname, $script, and $localtime. Op scripts pass parameters via the command used to call the script, they are also used to pass information when calling a template.
-   **Predicate**: Predicates are filters that prevent non-matching nodes from being included in the location path result. If there are multiple occurrences of a node at a given level (a good example is <interface>), a predicate can be used to select a specific occurrence. Predicate is enclosed in brackets and includes the textual data that makes that node unique, ie, /interfaces/interface/\[name == "ge-0/0/0"\]. Multiple predicates can be referenced at one node level, in which case all must match in order to return a result.
-   **Slax data structure**: Just like XML, except opening tags are followed by { and closing tag IS }. Text data is displayed on the same line as its parent element. If the element contains child elements then curly braces { } contain the child elements (the same method used to indicate hierarchy in Junos). If the element contains data then the data is written within quotation marks (" ") and the line is terminated with a semi-colon (;) (similar to Junos configurations)  A single start tag terminated by a semi-colon (;) represents empty elements with no children or text data.
-   **Template**: similar to a 'function' in most languages. The 'main template' is different depending on script type. For op scripts it is 'match /'. All templates besides the 'main template' are called 'named templates' and can be called from anywhere in the script, or another script that imports the one the named template is within.
-   **Variable**: A reference to an assigned value. Junoscript variables are *immutable*, they cannot be changed. This is fundamentally different than most scripting languages, and important to note. Data-type is automatically determined when the variable is set based on the assigned value. Variables assigned within a template can be used only within that template. Variables assigned within a code block such as an 'if' or 'for-each' has a scope confined to that code block as well.
