---
title: "Introduction to File Systems"
titleLink: "intro-filesystems"
description: "Basic concepts of Linux file systems, including types, partitions, and mount points."
tags: ["filesystem", "storage", "linux", "LFCS", "partitions", "mount-points"]
categories: ["Linux", "LFCS"]
---

In Linux, the file system organizes and manages how data is stored and retrieved. Every file and directory is located in a unique hierarchy starting at `/`.

## Types of file systems

- **ext4** — Standard in most modern distributions.
- **xfs** — Highly scalable, ideal for large volumes.
- **btrfs** — Offers advanced features like snapshots.
- **vfat/exFAT** — Compatible with Windows and removable media.
- **nfs** — For network file systems.

## Linux partitions

- Each storage device can be divided into partitions (`/dev/sda1`, `/dev/sda2`…).
- A partition can contain a single file system.
- Mount configuration can be defined in `/etc/fstab`.

## Mount points

- A mount point is a directory where the file system is made accessible.
- Example: mounting `/dev/sdb1` on `/mnt/data`.

## Practical example

```bash
sudo mkfs.ext4 /dev/sdb1
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
```