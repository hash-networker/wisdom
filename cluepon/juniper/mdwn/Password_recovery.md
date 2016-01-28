---
title: Password recovery
permalink: /Password_recovery/
---

1.  From console, interrupt the boot routine:

`        Hit [Enter] to boot immediately, or any other key for command prompt.`
`        Booting [kernel] in 9 seconds...`

`        < Press the space bar at this point >`

1.  Enter into single-user mode:

`        Type '?' for a list of commands, 'help' for more detailed help.`
`        ok boot -s`

1.  Enter the shell:

<!-- -->

1.  For new JunOS releases, the system will prompt:

`        "Enter full pathname of shell or 'recovery' for root password recovery or RETURN for /bin/sh: "`

If you enter "recovery" at this point, it will do the next two steps for you, and leave you in the JunOS CLI, from where you can edit the root password.

1.  Mount the virtual file systems (for JUNOS 5.4 and above, it is not necessary to mount the jbase (or jcrypto) package, however the other packages still need to be mounted):

`        NOTE: to go to multi-user operation, exit the single-user shell`

(with ^D)

`        # cd /packages`
`        # ./mount.jbase`
`        Mounted jbase package on /dev/vn1...`
`        # ./mount.jkernel`
`        Mounted jkernel package on /dev/vn2...`
`        # ./mount.jroute`
`        Mounted jroute package on /dev/vn3...`

1.  Enter recovery mode:

`        # /usr/libexec/ui/recovery-mode`

1.  Enter configuration mode and either delete or change the root

authentication password:

`        root> configure`
`        Entering configuration mode`

`        [edit]`
`        root# delete system root-authentication`

1.  Commit the changes, and exit configuration mode

`        [edit]`
`        root # commit`
`        commit complete`

`        [edit]`
`        root@router# exit`
`        Exiting configuration mode`

`        root@router> exit`

Exit recovery mode and enter "y" when prompted to reboot the system:

`        Reboot the system? [y/n] y`
`        Terminated`

The system now reboots and changes made to root authentication are activated.