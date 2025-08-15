---
title: Linux Filesystems and Mount Points
description: >
  Overview of Linux filesystems, partitions, and mount points for LFCS preparation.
categories: [Study, lfcs, linux]
tags: [linux, filesystem, mount, partitions, nfs]
weight: 100
---

This article provides a concise overview of the key concepts about filesystems in Linux, focusing on LFCS exam preparation.  
It covers filesystem hierarchy, types, partitions, mount points, and network filesystems (NFS).

---

![Linux file systems scheme](/mydocs/images/lfcs/filesystem-map.png)

---

{{< figure src="/mydocs/images/lfcs/filesystem-map.png" alt="Esquema de sistemas de archivos" class="img-fluid" >}}

## Linux Filesystem Basics

### 1. "Everything is a file"
In Linux, both data files and devices (printers, sound cards, etc.) are managed using the same input/output operations (open, read, write). This simplifies interaction with the system.

---

### 2. Filesystem Hierarchy
- Structured as an **inverted tree** starting at the **root directory `/`**.
- The root directory `/` is **not** the same as the root user.
- Path elements are separated by `/` (e.g., `/usr/bin/emacs`).

---

### 3. Filesystem Varieties
- **Native:** `ext3`, `ext4`, `squashfs`, `btrfs`
- **From other OS:** `ntfs`, `vfat`, `exfat`, `xfs`, `jfs`, `hfs+`
- **Journaling (safer, more resilient):** `ext4`, `xfs`, `btrfs`, `jfs`
- **Network/Distributed:** `NFS`, `Ceph`, `Lustre`, `OpenAFS`

---

### 4. Linux Partitions
- Each filesystem usually resides in its **own disk partition**.
- Common separation:
  - `/` → system files
  - `/home` → user data
  - `/var` → variable or temporary data
- Advantages: isolation, preventing full-disk failures, limiting corruption.

---

### 5. Mount Points
- **Definition:** Directory where a filesystem is attached to the main filesystem tree.
- **Important:** Mounting on a non-empty directory hides its original contents until unmounted.
- **Commands:**
  ```bash
  sudo mount /dev/sda5 /home
  sudo umount /home
  ```

---

### 6. tmpfs

- Special in-memory filesystem for temporary data.

### 7. Network Filesystems (NFS)

- _On the server:_

  ```bash
  sudo systemctl start nfs-server
  ```

  Configure /etc/exports:
  ```bash
  /projects *.example.com(rw)
  ```

  Apply changes:
  
  ```bash
  sudo exportfs -av
  ```

- _On the client:_

  Temporary mount:

  ```bash
  sudo mount server:/projects /mnt/nfs/projects
  ```

  Persistent mount → add entry in `/etc/fstab`.

---

## 📜 Summary for LFCS

- Know how to mount/unmount filesystems.
- Understand `/etc/fstab`.
- Identify different filesystem types and their purposes.
- Configure NFS shares and clients.
