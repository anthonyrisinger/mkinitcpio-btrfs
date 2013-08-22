mkinitcpio-btrfs
================

This hook provides advanced support for single and multi device btrfs
partitions, including rollback operations at boot time.

### How to Setup:

Before you start your instalaltion update btrfs-progs and install git

    pacman -Sy btrfs-progs git

Partition your drive as you prefer and use mkfs.btrfs to create your root
partition.

    mkfs.btrfs

Mount your root partition on /mnt

    mount [YOUR_ROOT_PARTITION] /mnt

Prepare your root partition for rollback support

    cd /mnt
    btrfs subvolume snapshot . __active
    btrfs subvolume list .
    btrfs subvolume set-default [ID_OF__ACTIVE] .
    mkdir __snapshot

Remount your root partition.

    umount /mnt
    mount [YOUR_ROOT_PARTITION] /mnt

Proceed with bootstrap

    pacstrap /mnt base base-devel btrfs-progs kexec-tools [PREFERED_BOOTLOADER]

Clone mkinitcpio-btrfs to your new root.

    cd /mnt/root
    git clone https://github.com/visit1985/mkinitcpio-btrfs

Create your /etc/fstab and chroot into your installation.

    genfstab -p /mnt >> /mnt/etc/fstab
    arch-chroot /mnt /bin/bash

Create a place where you mount the btrfs root to create snapshots later.

    mkdir /var/lib/btrfs

Add an additional line to /etc/fstab for it.

    [YOUR_ROOT_PARTITION]    /var/lib/btrfs    rw,relatime,space_cache,subvolid=0    0 0

Follow the normal Installation Guide until you need to edit mkinitcpio.conf.
Before doing that, install mkinitcpio-btrfs.

    cd /root/mkinitcpio-btrfs
    makepkg -i --asroot

Add btrfs_advanced to your HOOKS variable and generate your initial RAM disk.

Install your bootloader and reboot.


