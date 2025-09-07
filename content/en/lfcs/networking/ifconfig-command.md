---
title: "The ifconfig Command (Legacy)"
description: "A review of the ifconfig command for network management. Although deprecated, it's important to know for older systems and the LFCS certification."
weight: 30
categories: ["LFCS", "Linux"]
tags: ["Linux CLI"]
---

The `ifconfig` (interface configuration) command is the classic tool for network configuration on Unix-like systems. It belongs to the `net-tools` package, which has been **deprecated** on most modern Linux distributions in favor of `iproute2` (which provides the `ip` command).

Despite being a legacy tool, knowing its basic usage is crucial for the LFCS exam, as it may still be found on older systems or minimal installations that still include it.

### Important Note: Lack of Persistence

The main disadvantage of `ifconfig` is that **any changes made with it are temporary and will be lost after a system reboot or network service restart.** For permanent configuration, you must edit the appropriate network configuration files for your distribution.

### Common Use Cases

#### Viewing the Configuration of All Active Interfaces

Running `ifconfig` without arguments shows interfaces that are "up".

```bash
ifconfig
```

#### Viewing the Configuration of All Interfaces (Active and Inactive)

The `-a` flag shows all interfaces, even those that are "down".

```bash
ifconfig -a
```

#### Viewing the Configuration of a Specific Interface

```bash
ifconfig enp0s3
```

**Example Output:**

```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.10  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::a00:27ff:fea1:b2c3  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:a1:b2:c3  txqueuelen 1000  (Ethernet)
        ...
```

#### Assigning an IP Address and Netmask

This is a quick way to configure an IP for temporary testing.

```bash
# Syntax: ifconfig [INTERFACE] [IP_ADDRESS] netmask [NETMASK]
sudo ifconfig enp0s3 192.168.1.150 netmask 255.255.255.0
```

#### Bringing an Interface Up (Activating)

```bash
sudo ifconfig enp0s3 up
```

#### Bringing an Interface Down (Deactivating)

This disables the interface, stopping all traffic through it.

```bash
sudo ifconfig enp0s3 down
```
