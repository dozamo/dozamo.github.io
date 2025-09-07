---
title: Provisioning Ubuntu 22.04 LTS
date: 2017-01-05
description: >
  Guide for an automated, text-based installation of an Ubuntu 22.04 LTS server for our lab.
categories: ["Linux", "LFCS"]
tags: ["Virtualization", "Debian", "Ubuntu", "kvm"]
weight: 200
---

{{< alert color="success" >}}
This guide covers an automated, text-based installation of Ubuntu Server 22.04 using `virt-install` and KVM storage pools.
{{< /alert >}}

## 1. Initial Specifications

This VM will be the first Debian-based guest domain for our LFCS lab environment.

- **Name:** `lfcs-ubuntu-n01`
- **RAM:** 2048 MB
- **Disk Path:** `/var/lib/libvirt/images/vms_pool/lfcs-ubuntu-n01.qcow2` (managed by the pool)
- **Disk Size:** 16 GB
- **vCPUs:** 2
- **Network Bridge:** `br0`

## 2. Prerequisites

- A running KVM host.
- An active storage pool named `vms_pool`.
- The Ubuntu Server 22.04 ISO is downloaded to `/var/extra/kvm/isos/ubuntu-22.04.5-live-server-amd64.iso`.

## 3. Creating the Virtual Disk via Storage Pool

This ensures proper management of the virtual disk by libvirt.

```bash
sudo virsh vol-create-as vms_pool lfcs-ubuntu-n01.qcow2 16G --format qcow2
```

Verify its creation:

```bash
sudo virsh vol-list vms_pool
```

## 4. The virt-install Command for Automated Install

This command uses --location to extract the kernel and initrd, enabling a console-based, automated installation.

```bash
sudo virt-install --name lfcs-ubuntu-n01 \
   --ram 2048 \
   --vcpus 2 \
   --disk vol=vms_pool/lfcs-ubuntu-n01.qcow2 \
   --os-variant ubuntu22.04 \
   --network bridge=br0 \
   --graphics none \
   --console pty,target_type=serial \
   --location /var/extra/kvm/isos/ubuntu-22.04.5-live-server-amd64.iso,kernel=casper/vmlinuz,initrd=casper/initrd \
   --extra-args 'console=ttyS0,115200n8 autoinstall'
```

Note: The `--extra-args '... autoinstall'` part triggers Ubuntu's automated installation process. You may need a cloud-init user-data file for a fully unattended setup. For a standard text-based install, you can follow the prompts directly in the console after connecting.

To connect to the serial console:
```bash
sudo virsh console lfcs-ubuntu-n01
```

## 5. Benefits of Using a Storage Pool

- Integrated Management: The disk is registered within the KVM pool.
- Better Visibility: Easily list all volumes with `virsh vol-list <pool_name>`.
- Portability: If you export/move the pool, all its volumes are included.
- Security: Permissions are managed by libvirt.
