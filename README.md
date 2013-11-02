mkinitcpio-btrfs
================

This hook provides advanced support for single and multi device btrfs
partitions, including rollback operations at boot time.

### How to Setup:

Use the official installation guide as reference.
https://wiki.archlinux.org/index.php/Official_Arch_Linux_Install_Guide

Before you start your installation update btrfs-progs and install git.

    $ pacman -Sy btrfs-progs git

Partition your drive as you prefer and use mkfs.btrfs to create your root
partition. Setting up RAID 1 (or higher) is recommended for btrfs
selfhealing features. If you don't have two drives, setup two partitions
of the same size on one drive.
https://btrfs.wiki.kernel.org/index.php/UseCases#RAID

    $ mkfs.btrfs [YOUR_ROOT_PARTITION]

Mount your root partition on /mnt (don't use subvol/subvolid options).

    $ mount [YOUR_ROOT_PARTITION] /mnt

Prepare your root partition for rollback support:
First clone the btrfs root to an active subvolume.

    $ cd /mnt
    $ btrfs subvolume snapshot . __active

Then make the new active subvolume mounted by default.

    $ btrfs subvolume list .
    ID 256 gen 98 top level 5 path __active
    $ btrfs subvolume set-default 256 .

And create a subfolder to hold the future snapshots.

    $ mkdir __snapshot

You can also use different subvolume and folder names and specify them in
/etc/default/btrfs_advanced later.

Remount your root partition (don't use subvol/subvolid options).

    $ cd
    $ umount /mnt
    $ mount [YOUR_ROOT_PARTITION] /mnt

Proceed with bootstrap. Add dependencies of mkinitcpio-btrfs too.

    $ pacstrap /mnt base base-devel btrfs-progs kexec-tools [PREFERED_BOOTLOADER]

Clone mkinitcpio-btrfs to your new root.

    $ cd /mnt/root
    $ git clone https://github.com/xtfxme/mkinitcpio-btrfs

Create your /etc/fstab and chroot into your installation.

    $ genfstab -p /mnt >> /mnt/etc/fstab
    $ arch-chroot /mnt /bin/bash

Create a place where you mount the btrfs root to create snapshots later.

    $ mkdir /var/lib/btrfs

Add an additional line to /etc/fstab for it. Here you'll need the subvolid=0
option to mount the real root.

    [YOUR_ROOT_PARTITION]    /var/lib/btrfs    rw,relatime,space_cache,subvolid=0    0 0

Follow the official installation guide until you need to edit mkinitcpio.conf.
Before doing that, install mkinitcpio-btrfs.

    $ cd /root/mkinitcpio-btrfs
    $ makepkg -i --asroot

Add btrfs_advanced to your HOOKS variable.

    $ vi /etc/mkinitcpio.conf

Modify /etc/default/btrfs_advanced to your needs.

    $ vi /etc/default/btrfs_advanced

Generate your initial RAM disk.

    $ mkinitcpio -p linux

Install your bootloader and reboot.

Note: In multi device RAID setups, install your bootloader to multible
drives too.

### Usage Hints:

Create a snapshot of your root filesystem.

    $ cd /var/lib/btrfs
    $ sudo btrfs subvolume snapshot __active __snapshot/[snapshot name]

Use "btrfs scrub" for periodic online filesystem checks.

    $ sudo btrfs scrub start /
    $ sudo btrfs scrub status /

If you use a multi device RAID setup, make sure you rebalance after system
upgrades, to prevent kernel panic on device failure.

    $ sudo btrfs balance /
