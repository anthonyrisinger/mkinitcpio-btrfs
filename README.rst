mkinitcpio-btrfs
================

This hook provides volatile btrfs support during bootup.

To enable support, a snapshot of your system's current state must be
made to /__active; this snapshot will subsequently be used as the primary boot
device.

The original / will remain intact and unused; it is up to you to rm -rf the
stagnant files from your old / (var, usr, lib, etc), and reclaim what would in
time become dead space.  DO NOT remove /__active, /__rollback, or /__snapshot.

The following command will be executed:

# btrfs subvolume snapshot / /__active

If a rollback snapshot is booted, you can easily copy this snaptshot to __active
and make it the default again.

# btrfs subvolume delete /__active
# btrfs subvolume snapshot / /__active
