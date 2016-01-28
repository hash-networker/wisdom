---
title: Using a USB drive to transfer files between router and PC
permalink: /Using_a_USB_drive_to_transfer_files_between_router_and_PC/
---

By default, USB thumb drives formated in your windows PC, are not readable on your Juniper router. To mount a FAT/VFAT formatted USB drive on your Juniper router to copy files to/from a router, follow these steps:

    user@router> start shell
    % sudo
    Password:
    root@router% mkdir -p /mnt/usb
    root@router% mount_msdosfs /dev/da0s1 /mnt/usb/
    root@router% cp /mnt/usb/jinstall-10.4R3.4-domestic-signed.tgz /var/tmp
    root@router% cp /var/log/messages /mnt/usb/
    root@router% umount /mnt/usb
    root@router% exit
    % exit
    user@router>

*NOTE: /dev/da0s1 is the device name when using a MX series router. You may find different device name is correct on other series.*

Here we have created an empty directory in /mnt/usb which can serve as a mount point for the drive, then used mount_msdosfs to mount the drive, then copied a copy files to and from the usb drive, then unmounted the drive so it is safe to remove.