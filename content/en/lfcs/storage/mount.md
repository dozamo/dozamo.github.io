---
title: "mount command"
titleLink: "mount"
description: "Practical guide to using the mount command in Linux to mount file systems."
weight: 170
categories: ["LFCS", "Linux"]
tags: ["Linux CLI"]
---

The `mount` command in Linux is used to attach file systems and make them accessible through a mount point in the directory tree.

## mount

{{% alert %}}
`mount` — Mounts a file system to a mount point.
{{% /alert %}}

### Synopsis

```bash
mount [-t type] device directory
mount [-o options] device directory
mount [-a]
```

### Common options

- `-t type` — Specifies the file system type (ext4, xfs, nfs, etc.).
- `-o options` — Mounts with specific options such as `ro` (read-only), `rw` (read/write), `noexec`, `nosuid`, etc.
- `-a` — Mounts all file systems defined in `/etc/fstab`.

## Use cases

1. Mount a USB device to `/mnt/usb`:
   
   ```bash
   sudo mount -t vfat /dev/sdb1 /mnt/usb
   ```

2. Mount an NFS file system:

   ```bash
   sudo mount -t nfs server:/path /mnt/nfs
   ```

3. Mount all systems defined in /etc/fstab:

   ```bash
   sudo mount -a
   ```

## Related files

- `/etc/fstab` - File system table for automatic mounting.
- `/etc/mtab` - List of currently mounted systems.
- `/proc/mounts` - Kernel information about active mounts.

