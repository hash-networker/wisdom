---
title: Creating USB Install Media
permalink: /Creating_USB_Install_Media/
---

USB Install Media
-----------------

Juniper documentation shows how to create a USB install flash drive from the USB port on a router's RE. While it is not supported, it can also be done from a PC.

Below are some methods that have been known to work for MX series routers (and will most likely work on others)

BSD/MacOS
---------

1. Download **Install media** on juniper.net/support

Example: For 9.2R2

-   go to the 9.2 code download: <http://www.juniper.net/support/csc/swdist-domestic/9.2/>
    -   On the software tab, click on the: “M-series and T-series Install Media”

Note: the code download site doesn’t seem to like Firefox. IE is recommended.

2. Find out what the device name of your USB device is:

-   Insert USB drive.

\`\`\`MAC\`\`\`:

`open a terminal.  `
`Run ‘diskutil list’, this will list your hard drive and any other mounted drives `
`(such as a CD ROM and USB drive), take note of the name of the USB drive.  `
`Mine was /dev/drive2 (drive1=dvd, and drive0=hard drive) on my MacBook Pro.`

\`\`\`BSD\`\`\`:

3. Get ready to write:

-   unmount the USB drive

MAC:

`diskutil unmountDisk /dev/drive2`

BSD:

`umount /dev/drive2`

Replace ‘drive2’ with the name of your drive.

**NOTE: THE DD command will overwrite whatever is there. If you have important stuff on your USB drive, you’ll lose it. If you accidentally put in your hard drive’s device name, you’ll overwrite the hard drive!!!**

-   erase the USB Device with the following command

`sudo dd if=/dev/zero of=/dev/disk2 count=20`

Make sure to replace ‘disk2’ with the name of your USB drive!

-   Change directories to where you downloaded the install-media file (using the cd command)
-   Write the install media to the drive with this command:

`sudo dd if=install-media-9.2R3.5-domestic of=/dev/disk2 bs=64k`

Make sure to replace the drive name and the install-media filename with the values you are using.

-   -   MAC:

`diskutil eject /dev/drive2`

-   -   BSD:

`umount /dev/drive2`

ejects the USB drive so it can safely be removed.

Windows
-------

1. Downloads

-   Download [<http://www.chrysocome.net/dd> dd for Windows](/http://www.chrysocome.net/dd_dd_for_Windows "wikilink")
-   Download install media image from Juniper (as mentioned in the BSD example)

2. Erase your USB drive:

`dd if=/dev/zero of=\\.\`<DRIVE LETTER>`: count=20`

**WARNING: This will wipe the drive. Replace <DRIVER LETTER> with the drive letter of your usb drive. If you get this wrong, it will permanently erase whatever is on that drive!!!**

3. write the install media image to the usb drive

`dd if=install-media-9.2R3.5-domestic of=\\.\`<DRIVE LETTER>`: bs=64k `

replace <DRIVE LETTER> with your USB drive letter, and make sure to use the name of the install media image file instead of the one in the example.

4. Stop the drive by selecting the eject hardware icon in your systray, and eject.

------------------------------------------------------------------------

Remove the USB drive and take it to your Juniper router.