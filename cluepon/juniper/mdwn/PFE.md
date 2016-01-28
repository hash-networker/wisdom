---
title: PFE
permalink: /PFE/
---

The PFE is the most important component of a Juniper Networks router. On the Packet Forwarding Engine, the actual forwarding of traffic is done. The PFE has its own copy of the forwarding-table, which is being stored by the RE (routing-engine). Technically, if the RE dies, the PFE is still able to forward packets. A Juniper router will do so for 5 minutes (current time-out) after the PFE lost contact with the RE and will then stop forwarding traffic. It is designed to do so, to prevent problems due to loss of adjancency of routing-protocols, which are being handled by the RE.

The PFE-RE communication goes via an internal network. On the RE, the interface used is FXP1.