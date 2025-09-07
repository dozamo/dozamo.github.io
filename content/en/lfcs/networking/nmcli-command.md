---
title: "The nmcli Command"
description: "A guide to nmcli, the command-line tool for NetworkManager, essential for network management on modern systems."
weight: 20
categories: ["Linux", "LFCS"]
tags: ["networking", "networking", "networkmanager", "nmcli", "linux", "rhel", "ubuntu"]
---

`nmcli` is the command-line utility for controlling `NetworkManager`, the standard service for network management on most modern desktop and server distributions (including RHEL/CentOS, Ubuntu, etc.). Unlike `ip`, changes made with `nmcli` are **persistent** by default.

### Structure and Key Concepts

`nmcli` organizes configuration into two main objects:

-   **Device:** The physical or virtual network interface (`enp0s3`, `wlan0`).
-   **Connection:** A configuration profile that can be applied to a device. It contains IP settings, DNS, gateway, etc. A device can have multiple connection profiles, but only one can be active at a time.

### General and Device Management

#### View the General Status of NetworkManager

```bash
nmcli general status
```

#### List Network Devices and Their Status

```bash
nmcli device status
# Or its shorthand form
nmcli dev status
```

**Example Output:**

```
DEVICE   TYPE      STATE      CONNECTION         
enp0s3   ethernet  connected  Wired connection 1 
lo       loopback  unmanaged  --
```

#### Disconnecting or Connecting an Interface

This deactivates the active connection on a device but does not bring it "down" (it remains `UP` at the `ip link` level).

```bash
# Disconnect
sudo nmcli device disconnect enp0s3

# Connect (activates a suitable connection profile for the device)
sudo nmcli device connect enp0s3
```

### Connection Management

This is the most powerful part of `nmcli`, as it defines the persistent configuration.

#### List Existing Connection Profiles

```bash
nmcli connection show
# Or its shorthand form
nmcli con show
```

#### View the Details of a Specific Connection

```bash
# Use the connection's NAME or UUID
nmcli connection show "Wired connection 1"
```

#### Adding a New Ethernet Connection with a Static IP

This is a fundamental use case for servers.

```bash
# Syntax: nmcli con add type ethernet con-name [NAME] ifname [INTERFACE] ip4 [IP/MASK] gw4 [GATEWAY]
sudo nmcli con add type ethernet con-name 'static-prod' ifname enp0s3 ip4 192.168.1.50/24 gw4 192.168.1.1
```

After adding the connection, it's common to configure DNS servers.

```bash
# Modify the newly created connection to add DNS
sudo nmcli con mod 'static-prod' ipv4.dns "8.8.8.8 8.8.4.4"

# Ensure the IP configuration method is manual, not DHCP
sudo nmcli con mod 'static-prod' ipv4.method manual
```

To apply the new connection:

```bash
sudo nmcli con up 'static-prod'
```

#### Modifying a Connection to Use DHCP

If you need to change a static connection to dynamic (DHCP).

```bash
# Change the IP acquisition method to automatic (DHCP)
sudo nmcli con mod 'static-prod' ipv4.method auto

# Optional: clear the static configuration
sudo nmcli con mod 'static-prod' ipv4.addresses "" ipv4.gateway "" ipv4.dns ""

# Reactivate the connection to apply the changes
sudo nmcli con down 'static-prod' && sudo nmcli con up 'static-prod'
```

#### Deleting a Connection

```bash
sudo nmcli connection delete 'static-prod'
```
