---
title: "Network config. on Debian (and Ubuntu older)"
titleLink: "Network Configuration on Debian (and Ubuntu < 17.10) - /etc/network/interfaces)"
description: "A practical guide for persistent network configuration on Debian and its derivatives using the /etc/network/interfaces file and the ifupdown package."
weight: 50
categories: ["LFCS", "Linux"]
tags: ["Linux CLI", "Debian", "Ubuntu < 17.10"]
---

The traditional and still very common method for configuring the network on Debian (and derivatives like Raspbian) is through the `/etc/network/interfaces` file. This system uses the `ifupdown` package to bring interfaces up (`ifup`) and down (`ifdown`) according to the defined configuration.

**Note:** On newer Debian server installations, `systemd-networkd` or even `NetworkManager` may be offered. However, `ifupdown` remains a key competency.

### `/etc/network/interfaces` File Structure

The file has a simple syntax. The key directives are:

-   `auto [interface]`: Indicates that the interface should be brought up automatically on boot.
-   `iface [interface] inet [method]`: Defines a configuration stanza for an interface.
    -   **`[interface]`**: The name of the interface (e.g., `eth0`, `ens33`).
    -   **`[method]`**: Can be `loopback`, `dhcp`, or `static`.

### Example 1: Basic Configuration (Loopback and DHCP)

A default `/etc/network/interfaces` file often looks like this:

```ini
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet dhcp
```

-   The loopback interface (`lo`) is always present.
-   The `eth0` interface will be configured automatically on boot (`auto eth0`) and will obtain its IP configuration via DHCP (`iface eth0 inet dhcp`).

### Example 2: Configuration with a Static IP Address

This is the most common scenario for a server.

```ini
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface with a static IP
auto eth0
iface eth0 inet static
    address 192.168.1.200
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

-   `iface eth0 inet static`: Indicates that the configuration is manual.
-   `address`: The static IP address to be assigned to the interface.
-   `netmask`: The subnet mask.
-   `gateway`: The default gateway.
-   `dns-nameservers`: The DNS servers to use. This line requires the `resolvconf` package to be installed. If it is not, DNS must be configured manually in `/etc/resolv.conf`.

### Applying the Changes

After modifying `/etc/network/interfaces`, you can apply the new configuration without rebooting using the `ifdown` and `ifup` commands.

```bash
# Bring the interface down (releases the current IP)
sudo ifdown eth0

# Bring the interface up (applies the new configuration)
sudo ifup eth0
```

If you encounter issues, you can restart the networking service, although the `ifdown`/`ifup` method is more precise.

```bash
sudo systemctl restart networking.service
```
