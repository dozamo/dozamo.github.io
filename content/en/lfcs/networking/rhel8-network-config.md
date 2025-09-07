---
title: "Network config. on RHEL 8+"
description: "Managing persistent network configuration on RHEL 8-based distributions (and newer) using NetworkManager, nmcli, and nmtui."
weight: 40
categories: ["LFCS", "Linux"]
tags: ["Linux CLI", "RHEL"]
---

On Red Hat Enterprise Linux (RHEL) 8 and its derivatives (CentOS Stream, AlmaLinux, Rocky Linux), `NetworkManager` is the default networking service and the only supported tool for managing network configurations. Interaction is primarily done through the `nmcli` (command-line) and `nmtui` (text user interface) utilities.

### 1. Using `nmcli` (Recommended Method)

`nmcli` is the most powerful and versatile tool, ideal for scripting and automation. Changes are persistent.

#### Example: Configure a Static IP Address

1.  **Create a new connection profile:**

    ```bash
    # A profile named 'static-ens192' is created for the 'ens192' interface
    # IP, mask (in CIDR notation), and gateway are assigned
    sudo nmcli connection add type ethernet con-name 'static-ens192' ifname ens192 ip4 10.0.0.100/24 gw4 10.0.0.1
    ```

2.  **Configure DNS servers:**

    ```bash
    sudo nmcli connection modify 'static-ens192' ipv4.dns "8.8.8.8 1.1.1.1"
    ```

3.  **Ensure the method is manual (not DHCP):**

    ```bash
    sudo nmcli connection modify 'static-ens192' ipv4.method manual
    ```

4.  **Activate the connection:**
    If a previous connection for that interface existed, it might need to be deactivated first.

    ```bash
    sudo nmcli connection up 'static-ens192'
    ```

### 2. Using `nmtui` (Text User Interface)

`nmtui` (NetworkManager Text User Interface) provides a simple, terminal-based menu for interactively configuring the network. It's ideal for those who prefer a visual guide without a full GUI.

1.  **Launch the tool:**

    ```bash
    sudo nmtui
    ```

2.  **Navigate the menu:**
    -   Use the arrow keys to select `Edit a connection` and press Enter.
    -   Select the network interface you wish to configure and choose `<Edit...>`.
    -   On the edit screen:
        -   Change `IPv4 CONFIGURATION` from `<Automatic>` to `<Manual>`.
        -   Select `<Show>` to reveal the configuration fields.
        -   Add the **Address** (with CIDR notation, e.g., `10.0.0.100/24`), **Gateway**, and **DNS servers**.
        -   Navigate to `<OK>` and press Enter.
    -   Return to the main menu with `<Back>`.

3.  **Activate the connection:**
    -   Select `Activate a connection`.
    -   Find your connection profile, select it, and press Enter to (re)activate it.
    -   Exit `nmtui` by selecting `<Quit>`.

### 3. Configuration Files (`ifcfg`)

Although `nmcli` and `nmtui` are the recommended methods, NetworkManager still reads from and writes to the traditional `ifcfg` files located in `/etc/sysconfig/network-scripts/`.

**Editing these files directly is not recommended on RHEL 8+**, as NetworkManager may overwrite the changes. However, knowing how to read them is useful for troubleshooting.

**Example of `/etc/sysconfig/network-scripts/ifcfg-static-ens192`:**

```ini
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none  # 'none' or 'static' for a static IP
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=static-ens192
UUID=...
DEVICE=ens192
ONBOOT=yes
IPADDR=10.0.0.100
PREFIX=24
GATEWAY=10.0.0.1
DNS1=8.8.8.8
DNS2=1.1.1.1
```

If for some reason you edit an `ifcfg` file manually, you must reload the configurations into NetworkManager:

```bash
sudo nmcli connection reload
sudo nmcli connection up 'static-ens192'
```
