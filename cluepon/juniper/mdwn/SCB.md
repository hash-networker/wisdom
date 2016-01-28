---
title: SCB
permalink: /SCB/
---

[thumb|right|System Control Board (SCB)](/Image:SCB_Front.jpg "wikilink")

More stuff here.

[Category:Hardware](/Category:Hardware "wikilink") [Category:Components](/Category:Components "wikilink") [Category:Switch Boards](/Category:Switch_Boards "wikilink")

<graphviz> graph G { label = "M40 SCB"; size= "10,16"; fontsize="10"; node \[fontsize = "6"\]; edge \[fontsize = "4"\];

`        subgraph cluster_0 {`
`               label = "CPU Complex";`
`       "603e" -- "Motorola MPC106" [ label = "Data" ];`
`               "603e" -- "Motorola MPC106" [ label = "Addr" ];`
`               "603e" -- "Motorola MPC106" [ label = "Ctl" ];`
`               "Motorola MPC106" -- DRAM;`
`               "Motorola MPC106" -- DUART;`
`               "603e" -- "L2 Cache 256k" [ label = "Data" ];`
`               "603e" -- "L2 Cache 256k" [ label = "Addr" ];`
`               "603e" -- "L2 Cache 256k" [ label = "Ctl" ];`
`               "Motorola MPC106" -- "PCI Bus";`
`               "603e" -- DRAM;`
`               "603e" -- DUART;`
`              `
`               DUART -- "RS-232 interface";`
`      }`

`       subgraph cluster_1 {`
`       label = "Backplane";`
`       "Host Interface";`
`       "Link to A1/A2";`
`       "CPU I2C Bus";`
`       "Link to N1";`
`       "SRAM I2C Bus";`
`       "SONET Clock out";`
`       "Primary I2C Bus";`
`       "I2C Control";`
`       "From SONET ref";`
`       "125mHz clock Bus";`
`       "JTAG Bus";      `
`       }`

DRAM -- "CPU I2C Bus"; "PCI Bus" -- "100mbit interface" -- "Host Interface"; "PCI Bus" -- "C Chip" -- "Link to A1/A2"; "C Chip" -- "Routing table SRAM"; "C Chip" \[URL="C Chip"\]; "Routing table SRAM" -- "SRAM I2C Bus"; "PCI Bus" -- "PCI bridge FPGA" -- "ISA Bus"; "ISA Bus" -- "I2C controller" -- "Primary I2C Bus"; "ISA Bus" -- "Hub Controller" -- "10mbit hub"; "10mbit hub" -- "Link to N1" \[ label = "8 links" \]; "PCI Bus" -- "10mbit interface AMD" -- "10mbit hub"; "PCI Bus" -- "JTAG Interface" -- "JTAG Bus"; "ISA Bus" -- "Other FPGA" -- "I2C Control"; "Other FPGA" -- "125mHz clock" -- "125mHz clock Bus"; "Other FPGA" -- "Led"; "Other FPGA" -- "SONET Clock"; "From SONET ref" -- "SONET Clock" -- "SONET Clock out";

`      } `

} </graphviz> [Category:Image needed](/Category:Image_needed "wikilink") [Category:Stub](/Category:Stub "wikilink")