---
title: SFM Power-off
permalink: /SFM_Power-off/
---

Sometimes a [SFM](/SFM "wikilink") will go bad, and the operator will disable it to stop errors and potential forwarding impact. However, whenever a new configuration is committed, the disabled SFM automatically is powered up again. The only way to make it stop doing this is to fully power off the SFM, using the hidden command:

`chassis {`
`    sfm 0 {`
`        disable-power;`
`    }`
`    sfm 1 {`
`        disable-power;`
`    }`
`    ...`
`}`

It is unknown why this command is hidden.

[Category:Stub](/Category:Stub "wikilink")