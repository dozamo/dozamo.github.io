---
title: "The ip Command"
description: "A practical guide to using the ip command for network management in Linux, essential for the LFCS certification."
weight: 10
categories: ["LFCS", "Linux"]
tags: ["Linux CLI"]
---

The `ip` command is the modern, unified tool for network management in Linux. It is part of the `iproute2` package and replaces classic tools like `ifconfig`, `route`, and `arp`. Its use is fundamental for any Linux system administrator.

### Basic Syntax

The general structure of the command is:

```bash
ip [OPTIONS] OBJECT {COMMAND | help}
```

-   **OBJECT**: The network element you want to manage. The most common are:
    -   `link`: Network interfaces (physical or virtual devices).
    -   `address` (or `addr`): IP addresses associated with interfaces.
    -   `route`: The kernel's routing table.

### Managing IP Addresses (`ip addr`)

This is one of the most frequently used subcommands. It allows you to view, add, and delete IP addresses.

#### Viewing IP Addresses

To display the IP addresses of all interfaces, use `show`.

```bash
ip addr show
# Or its shorthand form
ip a
```

**Example Output:**

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:a1:b2:c3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.10/24 brd 192.168.1.255 scope global dynamic enp0s3
       valid_lft 86354sec preferred_lft 86354sec
```

#### Adding an IP Address

To assign an IP to an interface. **Note:** This change is not persistent and will be lost upon reboot.

```bash
# Syntax: ip addr add [ADDRESS]/[MASK] dev [INTERFACE]
sudo ip addr add 192.168.1.100/24 dev enp0s3
```

#### Deleting an IP Address

To remove an IP from an interface.

```bash
# Syntax: ip addr del [ADDRESS]/[MASK] dev [INTERFACE]
sudo ip addr del 192.168.1.100/24 dev enp0s3
```

### Managing Network Interfaces (`ip link`)

Allows you to manage the state of network interfaces.

#### Viewing Interface Status

Shows Layer 2 (data link) information, such as the MAC address and state (UP/DOWN).

```bash
ip link show
```

#### Bringing an Interface Up (Activating)

```bash
sudo ip link set enp0s3 up
```

#### Bringing an Interface Down (Deactivating)

```bash
sudo ip link set enp0s3 down
```

### Managing Routes (`ip route`)

Allows you to view and manipulate the system's routing table.

#### Viewing the Routing Table

Shows how the system will direct traffic to different networks.

```bash
ip route show
# Or its shorthand form
ip r
```

**Example Output:**

```
default via 192.168.1.1 dev enp0s3 proto dhcp metric 100 
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.10 metric 100
```

#### Adding a Default Route (Gateway)

One of the most common tasks is defining the default gateway.

```bash
# Syntax: ip route add default via [GATEWAY_IP]
sudo ip route add default via 192.168.1.1
```

#### Deleting a Route

To remove a specific route.

```bash
# Delete the default route
sudo ip route del default

# Delete a route to a specific network
sudo ip route del 10.0.0.0/8
```
