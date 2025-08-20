---
title: "Configuración de Red en Debian (/etc/network/interfaces)"
description: "Una guía práctica para la configuración de red persistente en Debian y derivados usando el archivo /etc/network/interfaces y el paquete ifupdown."
weight: 50
categories: ["Networking", "Implementación de Operaciones"]
tags: ["networking", "debian", "ifupdown", "interfaces"]
---

El método tradicional y aún muy común para configurar la red en Debian (y derivados como Raspbian) es a través del archivo `/etc/network/interfaces`. Este sistema utiliza el paquete `ifupdown` para activar (`ifup`) y desactivar (`ifdown`) las interfaces según la configuración definida.

**Nota:** En instalaciones de servidor de Debian más recientes, es posible que se ofrezca `systemd-networkd` o incluso `NetworkManager`. Sin embargo, `ifupdown` sigue siendo una competencia clave.

### Estructura del Archivo `/etc/network/interfaces`

El archivo tiene una sintaxis simple. Las directivas clave son:

-   `auto [interfaz]`: Indica que la interfaz debe ser activada automáticamente al arrancar el sistema.
-   `iface [interfaz] inet [método]`: Define una configuración para una interfaz.
    -   **`[interfaz]`**: El nombre de la interfaz (p. ej., `eth0`, `ens33`).
    -   **`[método]`**: Puede ser `loopback`, `dhcp` o `static`.

### Ejemplo 1: Configuración Básica (Loopback y DHCP)

Un archivo `/etc/network/interfaces` por defecto suele verse así:

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

-   La interfaz de loopback (`lo`) siempre está presente.
-   La interfaz `eth0` se configurará automáticamente al inicio (`auto eth0`) y obtendrá su configuración IP a través de DHCP (`iface eth0 inet dhcp`).

### Ejemplo 2: Configuración con Dirección IP Estática

Este es el escenario más común para un servidor.

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

-   `iface eth0 inet static`: Indica que la configuración es manual.
-   `address`: La dirección IP estática que se asignará a la interfaz.
-   `netmask`: La máscara de subred.
-   `gateway`: La puerta de enlace predeterminada.
-   `dns-nameservers`: Los servidores DNS a utilizar. Esta línea requiere que el paquete `resolvconf` esté instalado. Si no lo está, los DNS deben configurarse manualmente en `/etc/resolv.conf`.

### Aplicando los Cambios

Después de modificar `/etc/network/interfaces`, puedes aplicar la nueva configuración sin reiniciar usando los comandos `ifdown` e `ifup`.

```bash
# Bajar la interfaz (libera la IP actual)
sudo ifdown eth0

# Levantar la interfaz (aplica la nueva configuración)
sudo ifup eth0
```

Si tienes problemas, puedes reiniciar el servicio de red, aunque el método `ifdown`/`ifup` es más preciso.

```bash
sudo systemctl restart networking.service
```
