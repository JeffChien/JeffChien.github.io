+++
date = "2016-01-04T12:35:21+08:00"
title = "backup"

+++

In my company, we have some data across servers. Data create/update/delete every day, so we want to have
a daily snapshot of them in case server failure, and it would be great if we have multiple version of them.

The naively way is to create a new copy from previous snapshot and issue rsync with `rsync --delete`. It works, but it is not efficent, and cost too
much disk space. `hardlink` can't be use here, since all files just reference to the same copy.

Thanks to the copy-on-write filesystem available today. the problem can be solve in a elegant way. In my case, Btrfs save my day.

# create btrfs filesystem
I use file as a filesystem.
``` shell
dd if=/dev/zero of=backupfs bs=1M count=10000
dev=$(losetup -f)
losetup "$dev" backupfs
mkfs -t btrfs ${dev}
mount -o rw,user_subvol_rm_allowed backupfs /mnt
```

If you allow normal user to remove volume, `user_subvol_rm_allowed` should be enable.

# backup
the flow is simple

1. create snapshot from previous snapshot.
    ``` shell
    btrfs subvolume create <new> #if it is the first sync
    btrfs subvolume snapshot <old> <new>
    ```

2. sync data into new snapshot.
    ``` shell
    rsync -az --delete <remote> <new>
    ```

3. remove the oldest snapshot.
    ``` shell
    btrfs subvolume delete <oldest>
    ```

The result is if a file doesn't change, all snapshot have the same reference.
If cheanged, btrfs will create a new copy of it and keep old one untouched. That is how we archive incremental and multiple version backup.
