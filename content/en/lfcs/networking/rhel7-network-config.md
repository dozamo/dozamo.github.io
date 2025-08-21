---
titleLink: "Network config. on RHEL 7 and Older" 
title: "Network config. on RHEL 7"
description: "A guide to persistent network configuration on RHEL 7-based distributions (and older) using ifcfg files."
weight: 70
categories: ["Networking", "Operations Implementation"]
tags: ["networking", "rhel", "centos", "ifcfg", "sysconfig", "legacy"]
---

On Red Hat Enterprise Linux (RHEL) 7, CentOS 7, and older versions, the traditional method for persistent network configuration is by editing script files in the `/etc/sysconfig/network-scripts/` directory.

Each network interface has its own configuration file, named `ifcfg-<interface-name>` (e.g., `ifcfg-eth0` or `ifcfg-enp0s3`). Although `NetworkManager` is present and enabled by default in RHEL 7, these files are the primary source of configuration.

### Key Parameters in `ifcfg` Files

These files are shell scripts that define variables. The most important parameters are:

-   `DEVICE`: The logical name of the interface (must match the filename without the `ifcfg-` prefix).
-   `ONBOOT`: Whether to activate the interface on system boot. Must be `yes` for the network to work on startup.
-   `BOOTPROTO`: The method for obtaining the IP configuration.
    -   `dhcp`: Use the DHCP protocol.
    -   `static` or `none`: Use the static IP configuration defined in the file.
-   `IPADDR`: The static IP address.
-   `NETMASK`: The subnet mask (e.g., `255.255.255.0`).
-   `PREFIX`: The network prefix length (e.g., `24`). It's an alternative to `NETMASK`.
-   `GATEWAY`: The default gateway.
-   `DNS1`, `DNS2`: The primary and secondary DNS servers.
-   `NM_CONTROLLED`: Indicates if NetworkManager should manage this device. `yes` by default. Setting it to `no` causes the interface to be managed by the traditional network service.

### Example 1: DHCP Configuration

An `ifcfg-enp0s3` file for a DHCP configuration would be very simple:

```ini
# /etc/sysconfig/network-scripts/ifcfg-enp0s3
DEVICE=enp0s3
BOOTPROTO=dhcp
ONBOOT=yes
NM_CONTROLLED=yes
```

### Example 2: Configuration with a Static IP Address

This is the typical use case for a server.

```ini
# /etc/sysconfig/network-scripts/ifcfg-enp0s3
DEVICE=enp0s3
BOOTPROTO=static
ONBOOT=yes
NM_CONTROLLED=yes

IPADDR=192.168.1.220
PREFIX=24
# Or alternatively: NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=1.1.1.1
```

### Applying the Changes

After creating or modifying an `ifcfg` file, you must restart the network service for the changes to take effect.

#### Using `systemctl` (Preferred Method in RHEL 7)

```bash
sudo systemctl restart network.service
```

**Warning:** Restarting the network service may disconnect you if you are connected via SSH.

#### Using `ifdown` and `ifup`

You can also reactivate a specific interface, which is often safer.

```bash
# Bring the interface down
sudo ifdown enp0s3

# Bring the interface up with the new configuration
sudo ifup enp0s3
```

These commands will read the `ifcfg-enp0s3` file and apply its configuration.
