---
title: JunOS 4.2 kernel config
permalink: /JunOS_4.2_kernel_config/
---

    #
    # GENERIC -- Generic machine with WD/AHx/NCR/BTx family disks
    #
    # For more information read the handbook part System Administration ->
    # Configuring the FreeBSD Kernel -> The Configuration File.
    # The handbook is available in /usr/share/doc/handbook or online as
    # latest version from the FreeBSD World Wide Web server
    # <URL:http://www.FreeBSD.ORG/>
    #
    # An exhaustive list of options and more detailed explanations of the
    # device lines is present in the ./LINT configuration file. If you are
    # in doubt as to the purpose or necessity of a line, check first in LINT.
    #
    #   GENERIC,v 1.77.2.22 1998/03/24 01:20:14 jkh Exp

    machine     "i386"
    cpu     "I586_CPU"
    cpu     "I686_CPU"
    ident       GENERIC
    maxusers    64

    options     PFE         #Juniper PFE support

    options     FFS         #Berkeley Fast Filesystem
    options     NFS         #Network Filesystem
    options     MFS         #Memory Filesystem
    options     MSDOSFS         #MSDOS Filesystem
    options     "CD9660"        #ISO 9660 Filesystem
    options     PROCFS          #Process filesystem
    options     "COMPAT_43"     #Compatible with BSD 4.3 [KEEP THIS!]
    #options    BOUNCE_BUFFERS      #include support for DMA bounce buffers
    options     UCONSOLE        #Allow users to grab the console
    options     FAILSAFE        #Be conservative
    options     USERCONFIG      #boot -c editor
    options     VISUAL_USERCONFIG   #visual boot -c editor
    options     "MAXMEM=0x100000"   #Memory size, in units of 1024 bytes
    options     NMBCLUSTERS=0x2000  #number of clusters and mbufs
    options     VM_KMEM_SIZE=128    # size in MB. used to size kmem_map.

    options     CCC         #Connection Cross Connect
    options     TNP         #Trivial Network Protocol
    options     INET            #InterNETworking
    options     ISO         #IS-IS/ES-IS support (only)
    options         TAG                     #InterNETworking
    options     RAW_IF          #RAW_IF support
    options     "MD5"           #MD5 authentication

    options     KTRACE
    options     SYSVSHM
    options     SYSVSEM
    options     SYSVMSG
    options     INCLUDE_CONFIG_FILE #include a copy of generated config

    options     DDB         #include support for GDB/DDB
    options     DDB_UNATTENDED      #don't enter debugger on panic
    #options    TRAPTRACE       #local trap hacks
    config      kernel  root on wd0

    controller  isa0
    controller  pci0

    # PCCARD (PCMCIA) support
    device      pcic0   at isa? port 0x3e0

    controller  fdc0    at isa? port "IO_FD1" bio irq 6 drq 2 vector fdintr
    disk        fd0 at fdc0 drive 0
    disk        fd1 at fdc0 drive 1

    options     "CMD640"    # work around CMD640 chip deficiency

    controller  wdc0    at isa? port "IO_WD1" bio irq 14 vector wdintr
    disk        wd0 at wdc0 drive 0
    disk        wd1 at wdc0 drive 1

    controller      wdc1    at isa? port "IO_WD2" bio irq 15 vector wdintr
    disk            wd2     at wdc1 drive 0
    disk            wd3     at wdc1 drive 1

    #
    # The following 2 line are unique to this controller because it has a
    # PCMCIA slot. Address (0x180), controller number (2) are defined in
    # pccard/card.h . Any changes to these *must* be reflected there too.
    #
    # The "flags 0x200" is also unique to this controller -- it is used to
    # set the alternate status register differently.
    #

    controller  wdc2    at isa? port "0x180" bio flags 0x200
    disk            wd4     at wdc2 drive 0

    options     ATAPI           #Enable ATAPI support for IDE bus
    options     ATAPI_STATIC        #Don't do it as an LKM
    device      wcd0            #IDE CD-ROM
    device      wfd0            #IDE floppy (LS-120)

    controller  ahc0            # Adaptec controller
    controller  scbus0          # Generic SCSI
    device  sd0             # SCSI disk
    device  st0             # SCSI tape
    device  cd0             # SCSI cd

    device  sc0 at isa? port "IO_KBD" tty irq 1 vector scintr
    device  npx0    at isa? port "IO_NPX" flags 0x1 irq 13 vector npxintr
    #device lpt0    at isa? port? tty irq 7 vector lptintr
    device  psm0    at isa? port "IO_KBD" conflicts tty irq 12 vector psmintr

    # sio -- serial ttys for ns16550 uart generic driver
    #
    device  sio0    at isa? port "IO_COM1" conflicts tty flags 0x15 irq 4 vector siointr
    device  sio1    at isa? port "IO_COM2" conflicts tty irq 3 vector siointr
    #
    # sio -- serial ttys for ns16550 uart driver on teknor cpci board
    #
    device  sio0    at isa? port "IO_COM1" conflicts tty flags 0x20010 irq 4 vector siointr
    device  sio1    at isa? port "IO_COM3" conflicts tty flags 0x20000 irq 5 vector siointr
    device  sio2    at isa? port "IO_COM2" conflicts tty flags 0x20000 irq 3 vector siointr

    device  mcs0                # Juniper MCS device
    device  smb0                # smb, i2c
    device  de0             # DEC 10/100 Ethernet
    device  fxp0                # Intel 10/100 Ethernet
    device  fpa0                # DEC FDDI
    device  sr0             # SDL DS1 sync serial
    device  mps0                # NTT SONET
    device  ccti3               # SDL DS3 sync serial
    device  en0             # Efficient/Midway ATM
    # device    hfa0                # Fore ATM

    device  ed0 at isa? port 0x300 net irq 10 iomem 0xd8000 vector edintr
    device  ed1 at isa? port 0x280 net irq 12 iomem 0xd4000 vector edintr
    device  ed2 at isa? port 0x340 net irq  5 iomem 0xd0000 vector edintr
    device  ep0 at isa? port 0x300 net irq  5 vector epintr

    pseudo-device   loop
    pseudo-device   ether
    pseudo-device   fddi
    pseudo-device   sppp
    pseudo-device   fr
    pseudo-device   atm     # ATM support
    pseudo-device   tencap      # tunnel encapsulations support
    pseudo-device   log
    pseudo-device   vn  1
    #pseudo-device  tun 1
    pseudo-device   pty 32
    pseudo-device   gzip        # Exec gzipped a.out's
    pseudo-device   bpfilter 4
    pseudo-device   snp 4
    pseudo-device   key     # Key management support