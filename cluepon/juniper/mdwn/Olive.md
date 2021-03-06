---
title: Olive
permalink: /Olive/
---

[thumb|right|The Olive](/Image:Olive.jpg "wikilink")

What is an Olive?
-----------------

The Olive (Olea europaea) is a species of small tree in the family Oleaceae, native to coastal areas of the eastern Mediterranean region, who . The natural wild Olive is a small tree or shrub to 8 m tall with rather straggling growth and thorny branches. The leaves are opposite, oblong pointed, 4-10 cm long and 1-3 cm broad, dark greyish-green above and, in the young state, hoary beneath with whitish scales. The small white flowers, with four-cleft calyx and corolla, two stamens and bifid stigma, are borne generally on the last year's wood, in racemes springing from the axils of the leaves. The fruit in the wild plant is small drupe 1-2 cm long, and the fleshy pericarp, which gives the cultivated olive its economic value, is comparatively thin.

No really, what is it?
----------------------

Olive is also the [codename](/Codenames "wikilink") name given to [JUNOS](/JUNOS "wikilink") software running on an PC rather than a Juniper router. A common misconception is that Olive is some sort of "special software", but it is actually ordinary JUNOS software running on a PC of similar specifications to a Routing Engine, with no forwarding hardware (or [PFE](/PFE "wikilink")) attached. If you took a Routing Engine out of a Juniper router and booted it in a blade server chassis, it would effectively be an Olive.

Juniper originally developed Olive functionality as a software development platform, before its hardware product was fully implemented. It is not intended as a "router simulator", and has never been a supported product, or intended for use by the general public in any way. At one point it was used by Juniper internally for lab work, but has largely been phased out of this role with the availability of low-end hardware based platforms such as the [M5](/M5 "wikilink").

The most common use of the Olive platform is for creative and unix-competent hackers to learn the JUNOS CLI on a low-cost platform. It is capable of forwarding a small amount of traffic, but does not support many of the features found on real Juniper routers. Essentially the forwarding on an Olive is the same as routing traffic via your fxp0 or em0 management interface on a real Routing Engine.

Ok so why all the secrecy?
--------------------------

Juniper's official position is that Olive does not exist. Considering that Olive is an unsupported and unsupportable platform using "free" (aka illegally licensed) software, this is not an unreasonable official position. Olive is essentially a hackers platform, with absolutely no support of any kind, and it is not suitable for any type of commercial use. If you are in any doubt, or if you are not able to figure it out, you should invest in a low-cost platform such as [J-Series](/J-Series "wikilink") instead.

It is also important to remember that Olive exists because Juniper allows it to exist, and is a testament to the mutual respect between the extremely knowledgeable developer and user bases. If the Olive platform became widely abused, Juniper could easily add additional software checks to prevent it from working. Please do not abuse this feature by doing stupid things like contacting JTAC for support on an Olive, or selling illegal copies of the software as "router simulators". This type of activity is likely to have serious legal consequences and/or provoke a justified response from Juniper, so just don't do it.

Installation instructions
-------------------------

Beware: There Are No Olives!

### "True" Olive : Hardware requirements

You need a generic PC with at least one Intel EtherExpress Pro/100B network card, an IDE hard disk and at least 128MB memory. JunOS 5.x and 6.x install fine on a Pentium. JunOS 7.x seems to require a Pentium II and a minimum 196MB to install (*All together now...Somebody please give me some more memory!* - aborting the memoryhungry installation means you'll have to start the FreeBSD install all over again). Following installation though, will run fine with just 48MB. Note that for JunOS 8.5, 256MB is required. Otherwise, the install will fail due to the hardware check done by the JunOS installation process.

Note that for JunOS 9.0 and later, 512MB is required. Otherwise, the install will fail due to the hardware check done by the JunOS installation process.

#### Network Cards

[thumb|right|You must have the right network cards!](/Image:nc3120.jpg "wikilink") It's crucially important to have the right network cards. You can't just use any old one. You can use the [Intel support website](http://support.intel.com/support/network/sb/cs-012904.htm) to identify their adapters.

**Network cards that do work:**

-   Intel EtherExpress Pro/100 and Pro/100B (82558 / 82558B chipset)
-   Intel EtherExpress Pro/100+ Management Adapter (82559 chipset)
-   Intel Pro/1000MT Desktop Gigabit Adapter
-   Intel Pro/1000MT Dual Port Server Adapter (FW82546EB chipset)
-   Intel ICH3 Onboard Controller (82801CAM)
-   Compaq NC3120 (82258 chipset)
-   Compaq NC3121 (82558B chipset)
-   Compaq NC3134 (82559 chipset - 1496 MTU when using Dot1q)
-   Matrox QS-NIC Quad FE NIC (8255x chipset)
-   Radisys Quad Ethernet' FE NIC (82559ER chipset - 1496 MTU when using Dot1q)
-   Intel Pro/1000 MT (82545GM chipset) PCI-X133 tested in 32-bit slot.
-   Intel Pro/1000 PT server adapter EXPI9400PT PCI Express 1x

*JunOS 4.x had support for DEC 21x40 network cards so you could install it under VMware.*

**Network cards that don't work:**

-   Intel 82557 chipset based cards (detected as fxp interfaces, but link won't come up) \[edit by Networker: they work just fine with me\]
-   Intel 82574 (Intel Desktop CT, some Atom-based Board) Interfaces not detected
-   3Com 905B-TX

**Tip:** If you get an error similar to "fxp0: Multicast addr command failed", and find your network cards don't actually pass traffic, try enabling "PCI Bus Mastering" in the BIOS. Some machines have this disabled by default.

### Virtual Machine Configuration

#### VMware

Allthough some people say you can't, you actually can run JunOS on VMware:

-   Create a new virtual machine
-   Edit your \*.vmx file and add *ethernet0.virtualDev = "e1000"*. With this line, VMware will emulate an Intel network card, and FreeBSD/JunOS will detect it as em interface.
-   Go to your VM settings -&gt; hardware and add a serial port
-   Install it as usual.
-   Unfortunately IP Multicast doesn't work with the virtual Intel driver of VMware. So it is not possible to use neatly protocols like OSPF, IS-IS, ... . [rwayan](/User:rwayan "wikilink") has written a patch for Olive in VMWare [1](http://www.netemu.cn/bbs/thread-7417-1-1.html) (in Chinese only). **The below instructions are only valid for JunOS 8.5 - They do not work on earlier or later installations**
    1.  Downgrade your olive into JunOS 8.5 R1.14
    2.  download the attachment in <http://www.netemu.cn/bbs/thread-7417-1-1.html>
    3.  put it into olive's /boot/modules
    4.  boot Olive with single mode
               input <space> then input 'boot -s' at bootloader

    5.  load the patch
               input 'kldload syscall' or 'kldload ./syscall.ko' in single mode shell where you put the patch

    6.  back to multiuser mode
               input "Ctrl+D" in shell

    7.  login and active the patch

               input 'sysctl dev.em.0.fix_em_multicast=1' after login

               if you have more network card active them all

               'sysctl dev.em.1.fix_em_multicast=1'

               'sysctl dev.em.2.fix_em_multicast=1'

               .....

    8.  Play with it.


    If you are confused by the steps above, you can download a virtual machine (running in Vmware）from [2](http://www.one-tom.com/bbs/viewtopic.php?f=87&t=5002) that does not need any installation or configuration. OSPF, ISIS, and RIP are working. The patch is upgraded with Logical Router support.

#### Qemu

##### Virtual Olive : JEMU

Why JEMU ? Because we use Qemu to emulate JUNOS and the emulation of the PIX (which use Qemu too) is called PEMU --&gt; JEMU ;-)

##### Details

What is working :

|       | YES | NO  |
|-------|-----|-----|
| RIP   | X   |     |
| OSPF  | X   |
| IS-IS | X   |
| BGP   | X   |
| MPLS  | X   |
| LDP   | X   |
| RSVP  | X   |

##### Procedure of installation:

-   Download last version of Qemu for Windows (~VMWare) on this site : [3](http://fabrice.bellard.free.fr/qemu/)
-   Download OpenVPN to create TAP interface : [4](http://openvpn.net/)
-   Download FreeBSD 4.4 mini : [5](ftp://ftp-archive.freebsd.org/pub/FreeBSD-Archive/old-releases/i386/ISO-IMAGES/4.4/4.4-mini.iso)
-   Download a version of JUNOS &lt; 7.4
-   Download modify version of qemu (version with the good Intel driver : i82559er) = JQEMU : [6](http://www.netemu.cn/bbs/thread-5225-1-1.html) (it's necessary to subscribe to the forum to download the file). Take the second one.

*Installation of Qemu :*

Unzip the official package of Qemu. Copy into the Qemu directory the JQEMU executable (jqemu.exe). In old versions of Qemu, a source code patch was needed to enable ip multicast packets to be received. See workarounds.

*Creation of the Olive :*

Use the tool : "qemu-img.exe" to create an image.

`qemu-img.exe create olive.img 2G`

`jqemu.exe -L . -m 256 -hda olive.img -cdrom FreeBSD4.4-mini.iso -boot d -localtime`

Install FreeBSD and JUNOS like it is explain below.

*Creation of the TAP interface :*

Install OpenVPN. During the installation install only "TAP-Win32 Virtual Ethernet Adapter" and "Add Shortcuts to Start Menu". To create an interface click on the menu : "Add a new TAP-Win32 virtual ethernet adapter". Rename the TAP interface on this way : "tap1", "tap2", ...

###### Workarounds

### qemu 1.0

In qemu 1.0, the below fix for multicast on i8255x devices is already incorporated; just build with vde support and run out of the box.

Since previous entries in other wikis, qemu's main tree has all of the support on the ethernet adapter side, multicast side, etc, to work natively out of the box.

### qemu 0.9.1 and earlier

In Qemu version 0.9.1, Stefan's multicast code has a check which i'm still decoding to solve properly that exits nic_receive before multicast frames make it to the CPU. I'm no developer, but this simple workaround will enable native qemu without any patch files, shady chinese translated forums, or modifications of jemu code.

Obtain the qemu source from the project website. The file hw/eepro100.c as of 0.9.1 has this line in the function nic_receive, comment the 'return' out.

`       int mcast_idx = compute_mcast_idx(buf);`
`       if (!(s->mult[mcast_idx >> 3] & (1 << (mcast_idx & 7)))) {`
`               //Commented out by JP Senior (sartan) Wed June 23 2008, this needs to be fixed`
`           //return;`
`       }`
`       rfd_status |= 0x0002;`

there is a 'return'.

Comment this out, compile, and multicast will work properly. Stefan from this qemu-devel mailing list entry is already working on the fix, <http://www.mail-archive.com/qemu-devel@nongnu.org/msg11306.html>

To run this, simply run qemu normally:

`sudo ./qemu-system-x86_64 -hda /opt/kvm/JunOS/olive.img -cdrom /home/storage/apps/bsd/4.4-mini.iso -net nic,vlan=0,model=i82551 -net tap,vlan=0,ifname=tap0,script=no -serial `[`telnet::1001,server,nowait`](telnet::1001,server,nowait)` -localtime -m 196 -L /usr/share/qemu`

--[Sartan](/User:Sartan "wikilink") 21:05, 23 June 2008 (UTC)

**Note: Some operating systems will strip multicast traffic from tap interfaces and bridges** According to <http://www.linuxfoundation.org/en/Net:Bridge#No_traffic_gets_trough_.28except_ARP_and_STP.29>, no traffic gets through except ARP and Spanning Tree Protocol traffic. Your kernel might have ethernet filtering (ebtables, bridge-nf, arptables) enabled, and traffic gets filtered. The easiest way to disable this is to go to /proc/sys/net/bridge. Check if the bridge-nf-\* entries in there are set to 1; in that case, set them to zero and try again.

`# cd /proc/sys/net/bridge`
`# ls`
`bridge-nf-call-arptables  bridge-nf-call-iptables`
`bridge-nf-call-ip6tables  bridge-nf-filter-vlan-tagged`
`# for f in bridge-nf-*; do echo 0 > $f; done`

--[Sartan](/User:Sartan "wikilink") 21:05, 23 June 2008 (UTC)

[Use of JEMU :](/Use_of_JEMU_: "wikilink")

Example with 2 interfaces:

`jqemu.exe -L . -m 48 -hda Olive.img -serial `[`telnet::1001,server`](telnet::1001,server)` -localtime `
`-net nic,vlan=0,macaddr=00:aa:00:00:01:01,model=i82559er -net tap,vlan=0,ifname=tap1 `
`-net nic,vlan=0,macaddr=00:aa:00:00:01:02,model=i82559er -net tap,vlan=0,ifname=tap2`

JEMU will start when you do a telnet : "telnet 127.0.0.1 1001"

To install JEMU on Linux, there is some interesting information here : <http://www.internetworkpro.org/wiki/Using_QEMU_with_Olive_to_emulate_Juniper_Routers>.

### Operating System Installation instructions

You need a copy of the jinstall tgz package for the version you want to install. It's often rumoured that the Olive jinstall is a "special" jinstall file, but no, it's the same jinstall as you'd load on your M&T (and now MX) series routers. Obviously, installing this somewhere else than in a Juniper router you have a support contract for is not allowed.

Install [FreeBSD 4.x](ftp://ftp-archive.freebsd.org/pub/FreeBSD-Archive/old-releases/) on it according to the instructions below:

Boot the CD, and accept the default options (e.g. don't customise the kernel) until the main menu is presented.

Then select "Standard Installation".

Create one "fdisk" partition on ad0 using the whole disk, and make it bootable.

Choose the 'Standard MBR (no boot manager)' option.

Create the FreeBSD slices according to the following layout:

#### Partition layout

`ad0a    /             500M`
`ad0b    swap          500M`
`ad0e    /config       100M`
`ad0f    /var          `<rest of disk>

For modern versions of JunOS it might be worth creating a larger swap partition, assuming you have a modern relatively large hard drive available.

Select to perform a 'Minimal' install, and that you are installing from a FreeBSD CD/DVD.

Once installation has completed, configure the first Ethernet interface, and the timezone.

Don't adjust anything else, and say no to all the network server / service options.

Reboot the unit, and if all has gone well you will get to a login prompt.

Login as "root", and ftp / scp your jinstall file into /var/tmp.

Then, to install JunOS, do the following:

#### JunOS install commands

`rm /dev/wd0c ; ln -s /dev/ad0c /dev/wd0c`
`mkdir /var/etc ; cd /var/etc`
`touch master.passwd inetd.conf group`
`pkg_add /var/tmp/jinstall-xxx.tgz`
`reboot`

If you switch to the serial console at this point, you will see JunOS installing.

On the first boot, you may get lots of things failing, but if you start the CLI and commit a basic configuration then it should start normally if you reboot again.

You can now configure your Olive from the console as per any normal Juniper. Once your IP is up and running, you can manage the router remotely using telnet or SSH.

#### Dual Booting JunOS and FreeBSD

Create at least two separate partitions, or use two separate hard drives. The first partition should be labeled with the layout above -- this will be the JunOS installation target. The second partition can be laid out however you wish and will be the installation target for FreeBSD. Install a fresh installation of FreeBSD on both partitions. After this is complete, boot the instance of FreeBSD that is on the second partition. Keep the FreeBSD install CD handy, as you'll need it again later.

Edit /etc/fstab and add the following entries to allow you to access the JunOS partitions from the FreeBSD instance:

`/dev/ad0s1a             /JunOS          ufs     rw              2       2`
`/dev/ad0s1e             /JunOS/config   ufs     rw              2       2`
`/dev/ad0s1f             /JunOS/var      ufs     rw              2       2`

Compile or install the GNU GRUB boot manager. Install the GRUB stage-1 and stage-2 boot loader images int /JunOS/boot/grub. Install the below snippet (edit as needed) as menu.list in /JunOS/boot/grub. This instace has JunOS installed on the first hard drive, and FreeBSD on the second.

`# Boot automatically after 10 secs.`
`timeout 10`
`# By default, boot the first entry.`
`default 0`
`# Fallback to the second entry.`
`fallback 1`
`# JUNOS ( bootloader )`
`title JunOS`
`root (hd0,0,a)`
`kernel /boot/loader`
`# FREEBSD`
`title FreeBSD`
`root (hd1,0,a)`
`kernel /boot/loader`

Install grub to the main boot sector.

`# grub`
`grub> find /boot/grub/stage2`
`grub> root (hd0,0,a)`
`grub> setup (hd0)`

Once Grub has successfully installed, ensure you can boot from both instances. Save the contents of /JunOS/boot/grub in a tar file for later.

`tar cvzpf /root/Grub-backup.tar.gz /JunOS/boot/grub`

Install the GNU GRUB boot manager from source or the ports collection. Install the jinstall package as above, and allow the Olive instance to fully boot. Once this is working, re-insert the FreeBSD install CD. As the FreeBSD install CD is loading the FreeBSD kernel, hit the spacebar repeatedly to bring up the boot loader from the CD. You should be at the OK prompt at this point. Enter lsdev to find the disk and partition that still has FreeBSD on it. Enter

`set currdev=[boot disk and partition] `

to boot back into your FreeBSD install. From the root directory, untar your Grub backup, and then go through the Grub install process again. At this point you should have a dual booting Olive/FreeBSD machine.

##### Default BootMGR is already dual-boot capable

Assign two disks to your operating system (VMWare, Qemu, etc), and install your JunOS freebsd first, build it out, and complete the installation. Second, install FreeBSD again on the second partition. The default BootMGR that the mini 4.4 installer users already recognizes both installations and will present a menu "F1: FreeBSD F2: Disk5" if you install BootMBR to the second partition, and mark the FreeBSD install to boot before JunOS does. This is a no-configuration option, but the grub option listed earlier is far more graceful. If you build out your operating system in this way, you can set up mount points to copy files into and out of JunOS (considering things like SSH, FTP, and CD-Roms may be locked out).

Create a new file /mnt/mount.csh

`#!/bin/csh`
`mount -t ufs /dev/ad0a /mnt/JunOS/root`
`mount -t ufs /dev/ad0e /mnt/JunOS/configs`

Set the attributes chmod a+x mount.csh

Run mount.csh

You are now able to access JunOS disks from FreeBSD.

Alternatively, from JunOS to BSD, the opposite is true (With a simpler partition scheme on FreeBSD than what is listed above by mounting ad1s1a

#### Issues with JunOS post release 7.4

Versions of JunOS newer than 7.4 will fail using the above method with an "ELF binary type "0" not known" error. This is due to a binary in the distribution called checkpic which will not run and will cause the pkg_add to bomb out. In order to allow newer versions to install you can replace the checkpic binary with /usr/bin/true. checkpic is located in the pkgtools.tgz archive inside the jinstall archive. If you untar the jinstall archive, do the the same to the pkgtools archive and replace the file then tar them all back up again you will be able to complete the installation following the steps above with your modified jinstall package.

Another alternative is to install JunOS 7.4 or prior on your olive and then upgrade to a post release 7.4 installation from the CLI using **request system software add <package name>**.

Tested and working
------------------

-   Versions:
    -   5.1R4.3 (install)
    -   5.2R1.4 (install)
    -   5.2R3.4 (as upgraded from 5.2R1.4)
    -   5.4R1.3 (install)
    -   6.4R1.6 (install)
    -   7.2R1.7 (install)
    -   7.2R2.4 (install)
    -   7.2R3.3 (install)
    -   7.3R2.9 (upgraded)
    -   7.6R1.9 (install)
    -   7.6R2.6 (upgraded)
    -   8.0R1.9 (install)
    -   8.1R1.5 (install)
    -   8.2R3.5 (upgraded)
    -   8.3R4.3 (upgraded)
    -   8.4R1.13 (upgraded)
    -   8.5R1.13 (upgraded/install)
    -   8.5R1.14 (upgraded)
    -   8.5R2.10 (upgraded)
    -   8.5R3.4 (upgraded)
    -   8.5R4.3 (upgraded)
    -   9.0R1.10 (upgraded from 8.5R2.10) Note: The 9.0 JunOS kernel hangs on VMWare
    -   9.3R3.8 (install)
    -   9.5R3.7 (upgraded)
    -   10.0R1.8 (install)
    -   10.0R3.10 (upgraded)
    -   10.4R4.5 (install)
    -   11.1R2.2 (upgraded)

<!-- -->

-   IPv6
-   Telnet, FTP, SSH
-   Syslog
-   NTP
-   RIP
-   OSPF (on fxp0.0 too)
-   IS-IS
-   BGP
-   MP-BGP
-   MPLS L3-VPN (seems to require "vrf-table-label" configured to actually pass traffic)
-   IGMP (v1, v2, v3)
-   DVMRP
-   PIM (Dense, Sparse, Sparse-Dense)
-   PIM RPs (Static, Auto-RP, Bootstrap)
-   PIM Encapsulator (and Decapsulator) - i.e. no Tunnel Services PIC required
-   MSDP
-   SAP
-   SNMP
-   GRE tunnels
-   GRE tunnels with MTU requiring fragmentation and reassembly
-   VLAN tagging (fxp0 interfaces support unit 0 only though...) Note: When using NC3134 NIC's & JunOS9.3R2.8 any unit number is supported when using Dot1Q.
-   Routing between tunnels
-   Routing between VLANs
-   Routing through fxp0
-   Firewall filters
-   Filter Based Forwarding (only ingress filters work, cannot filter on egress)
-   Multiple fxp interfaces (tested with up to 4 single, 4 dual and a quad etherexpress pro 100 -- note you need a pci bridge and as many ethernet controllers as there are ports; if you have 2 ports but only one controlling chip they won't work)
-   em (intel pro 1000) interfaces (tested with up to 6 e1000 on 7.5. 7.2R2.4 works as well, but only for one NIC I found so far, described as "Intel Pro/1000MT Desktop Gigabit Adapter")

Tested and apparently not working
---------------------------------

-   Policer Filters (fxp interfaces don't measure realtime traffic rates)
-   Modifying MTU (missing commands)
-   MTU larger than 1500 on fxp0 (including any additional headers like 802.1q, MPLS etc.).


Rumours say that reason for this is that The Vendor's fxp0 driver is from the time of first-generation eepro cards which had issues with oversized frames, and there has been no need for an update.

-   VLANs on em0
-   Multiple VLANs on em interfaces (only unit 0 supported)
-   "show chassis" commands. Returns error: "Unrecognized command"
-   Aggregated Ethernet (missing commands)
-   Traffic sampling on fxp interfaces (Doesn't generate any errors, simply doesn't work)
-   Port mirroring (requires traffic to transit a PFE).


When issuing "set forwarding-options sampling output interface fxp0", returns: "error: interface: 'fxp0': Must be a monitoring or service interface"

-   MPLS L2 VPN and L2 Circuits (Draft Kompella and Draft Martini) - cannot set the required physical encapsulation type on an fxp interface (e.g. "set interfaces fxp0 encapsulation vlan-ccc" command missing)
-   Class of Service (CoS) configuration on fxp interfaces.


Adding any configuration under the "class-of-service interfaces" stanza for fxp interfaces gives the error:


COSD_PARSE_ERROR: \[edit class-of-service interfaces fxp0\]

interfaces fxp0

Cannot configure class-of-service on interface fxp0.

-   VPLS - Cannot use an FXP interface in a VPLS routing instance
-   VPLS - Cannot use an em interface in a VPLS routing instance
-   FlowSpec - Whilst inetflow.0 is populated with routes, no actual actions are performed (9.1R3.5)
-   Most (if not all) "show chassis" commands, including "show chassis routing-engine", which would have been useful to see RAM etc :-)

[Category:Unsupported activities](/Category:Unsupported_activities "wikilink")

Tips and Tricks
===============

Olive Installation Error
------------------------

`WARNING: The /tmp/preinstall filesystem is low on free space.`
`WARNING: This package requires 202800k free, but there`
`WARNING: is only 162132k available.`

When installing a later version of Junos as Olive you might want to try out a different partition table when installing the initial FreeBSD OS.

`ad0a    /             500M`
`ad0b    swap          500M`
`ad0d    /config       100M`
`ad0e    /tmp          2000M`
`ad0f    /var          `<rest of disk>

If you do not set the /tmp as a partition the Olive installation process will allocate a TMPFS memorybased /tmp file system and if you only allocate 512M RAM the /tmp will grab something like 160M which is unsufficient. If there is a /tmp partition the Olive installation process will use that during the setup. Later on in the process the disk will be completely repartitioned to comply to Junos standards.

Use video console for kernel bootup and login sessions
------------------------------------------------------

By default, **Olive** writes its output to a console serial port instead of a video console. We can make **Olive** easier to use by redirecting the output to a video display, just as was the case during the install of the FreeBSD operating system. This means COM port redirection or a serial loopback cable are not needed, and makes it much easier to host multiple Olive-based routers on one machine.

Although we cannot directly edit the /etc/ttys file since it's overwritten by **Olive's** *jbase* package on each and every boot, we can try to change this file automatically using another part of the initialization procedure. Make the following changes:

Add a few simple lines to /etc/rc.local. The end of the file will be fine. An alternative would be to put this into /etc/rc.custom. Be sure that /etc/rc.custom has been marked as executable (e.g. chmod +x /etc/rc.custom) or /etc/rc.local will not run it at boot.

`echo Forcing virtual console -USER MODIFICATION-`
`echo 'ttyv0 "/usr/libexec/getty Pc" cons25 on secure' >>/etc/ttys`
`kill -HUP 1`

(See the manual pages for the *init* command for more details). What these commands will do is cause the init process to run again, re-reading /etc/ttys. We need to add this line to the /etc/ttys file at every boot since it is always overwritten when the *jbase* image is loaded.

To tell the boot loader to send kernel loading messages to the video console is pretty easy. Edit the file /boot/loader.conf and change the line

`console="comconsole"`

to

`console="vidconsole"`

These changes will make it so you do not ever need a serial port for Olive and you're able to work on the system directly on the video console. If you still want a serial port, it's still available (just not for booting).

(Coming up; SSH/Telnet access - work in progress)

--[Sartan](/User:Sartan "wikilink") 05:15, 13 July 2008 (UTC)

Use serial port with VMWare Server 2.x and \*nix hosts
------------------------------------------------------

*These instructions may work on other Vmware products*

1) Add a serial device to VMWare, with the following settings:

`Named pipe: /home/storage/virtual/vmware/Olive1/Olive1-pipe `
`Near end: Is a server`
`Far end: Is a virtual machine`

This will create a named pipe which will connect a file on your hard drive to the actual serial port on the

2) Use the multipurpose relay *socat* binary Open up a PTY so you can use programs such as minicom to work with your serial ports. What this does is translate the named pipe to a usable PTY for serial console usage.

`[root@arcane Olive1]# socat /home/storage/virtual/vmware/Olive1/Olive1-pipe pty:,link=/home/storage/virtual/vmware/Olive1/Olive1-pty`

3) Connect minicom to the new PTY socket As root, issue the 'minicom -s' command (Edit minicom settings). Go to the serial port setup option, and change the serial device noted to /home/storage/virtual/vmware/Olive1/Olive1-pty. Save the configuration as Olive1, and exit. Now, just run 'minicom Olive1' once your settings are saved, and you should get a fully functional serial console.<span class="plainlinks">[<span style="color:black;font-weight:normal; text-decoration:none!important; background:none!important; text-decoration:none;">big bang theory quotes</span>](http://www.thefunnyquotessayings.com/dr-sheldon-cooper-quotes-the-big-bang-theory/)

(Alternatively) 4) Use screen to connect to the PTY As root (or any user with r/w access to the pty), issue screen /home/storage/virtual/vmware/Olive1/Olive-pty for a far easier method of working with the console than minicom.