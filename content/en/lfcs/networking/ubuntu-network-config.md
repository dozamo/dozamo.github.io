---
title: "Network config. on Ubuntu"
titleLink: "Network config. on Ubuntu (/etc/netplan)"
description: "A guide to network configuration on modern Ubuntu Server versions using Netplan and its YAML syntax."
weight: 60
categories: ["LFCS", "Linux"]
tags: ["Linux CLI", "Ubuntu"]
---

Since Ubuntu 17.10, `Netplan` is the default tool for network configuration. Netplan acts as an abstraction layer that generates the configuration for a backend service, which can be `systemd-networkd` (default on Ubuntu Server) or `NetworkManager` (default on Ubuntu Desktop).

The configuration is defined in **YAML** formatted files located in `/etc/netplan/`.

### YAML Syntax and Netplan Structure

YAML is a human-readable data serialization language. **Indentation (spaces, not tabs) is crucial and syntactically significant.**

The basic structure of a Netplan file is:

```yaml
network:
  version: 2
  renderer: [backend]
  ethernets:
    [interface_name]:
      [configuration]
```

-   `renderer`: Specifies the backend (`systemd-networkd` or `NetworkManager`).
-   `ethernets`: A key for defining Ethernet interface configurations.

### Example 1: DHCP Configuration (Default)

A typical configuration file for DHCP, like the one found at `/etc/netplan/00-installer-config.yaml`, looks like this:

```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
```

-   `enp0s3`: This is the name of the network device. You must replace it with the correct name on your system (check with `ip a`).
-   `dhcp4: true`: Enables obtaining an IPv4 address via DHCP.

### Example 2: Configuration with a Static IP Address

For a server, static configuration is the most common.

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.210/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

-   `dhcp4: no`: It's important to explicitly disable DHCP.
-   `addresses`: A list of IP addresses in CIDR notation. You can list more than one.
-   `gateway4`: The default gateway for IPv4.
-   `nameservers`: Contains a list of `addresses` for DNS servers.

### Applying Netplan Configuration

The workflow with Netplan is always the same:

1.  **Edit the YAML file:**
    Modify the `.yaml` file in `/etc/netplan/` with `sudo`.

    ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
    ```

2.  **Test the configuration (optional but recommended):**
    The `try` command applies the configuration temporarily (for 120 seconds) and reverts it if you don't confirm. If you lose your SSH connection, it will automatically be restored.

    ```bash
    sudo netplan try
    ```

3.  **Apply the configuration permanently:**
    If the test was successful or you are sure of the changes, apply them permanently.

    ```bash
    sudo netplan apply
    ```

This command will read the YAML files, generate the configuration for the backend (e.g., `systemd-networkd`), and apply it.
