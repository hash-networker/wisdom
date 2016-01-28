---
title: MBUS
permalink: /MBUS/
---

**Maintenance BUS (MBUS):**

The MBUS provides out of band communications to line cards. It is a simple 1Mbps 2-wire serial interface based upon Controller Area Network (CAN) 2.0 spec (ISO 11898) - <http://www.can-cia.org/can/>

A daughter card on every line card has its own CPU, CAN controller, A/D converter, dual CAN interface, SRAM, Flash and serial EEPROM.

-   CSCs and BusBoards can proxy and or multiplex MBUS signals for power supplies.

Control pins reach into LED, Serial ID EEPROM, DC/DC power converter, clock select FPGA, temperature sensor and voltage sensors.

-   Logging messages can be enabled to be transmitted over MBUS rather than the fabric with IPC. This is done with the "logging method mbus" command.
