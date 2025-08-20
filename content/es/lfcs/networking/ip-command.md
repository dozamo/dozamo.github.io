---
title: "Comando ip"
description: "Una guía práctica sobre el comando ip para la gestión de redes en Linux, esencial para la certificación LFCS."
weight: 10
categories: ["Networking", "Comandos Esenciales"]
tags: ["networking", "iproute2", "linux", "sysadmin"]
---

El comando `ip` es la herramienta moderna y unificada para la gestión de redes en Linux. Forma parte del paquete `iproute2` y reemplaza a herramientas clásicas como `ifconfig`, `route` y `arp`. Su uso es fundamental para cualquier administrador de sistemas Linux.

### Sintaxis Básica

La estructura general del comando es:

```bash
ip [OPCIONES] OBJETO {COMANDO | help}
```

-   **OBJETO**: Es el elemento de red que se desea gestionar. Los más comunes son:
    -   `link`: Interfaces de red (dispositivos físicos o virtuales).
    -   `address` (o `addr`): Direcciones IP asociadas a las interfaces.
    -   `route`: Tabla de enrutamiento del kernel.

### Gestión de Direcciones IP (`ip addr`)

Este es uno de los subcomandos más utilizados. Permite ver, añadir y eliminar direcciones IP.

#### Ver direcciones IP

Para mostrar las direcciones IP de todas las interfaces, utiliza `show`.

```bash
ip addr show
# O su forma abreviada
ip a
```

**Ejemplo de Salida:**

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

#### Añadir una dirección IP

Para asignar una IP a una interfaz. **Nota:** Este cambio no es persistente y se perderá al reiniciar.

```bash
# Sintaxis: ip addr add [DIRECCIÓN]/[MÁSCARA] dev [INTERFAZ]
sudo ip addr add 192.168.1.100/24 dev enp0s3
```

#### Eliminar una dirección IP

Para quitar una IP de una interfaz.

```bash
# Sintaxis: ip addr del [DIRECCIÓN]/[MÁSCARA] dev [INTERFAZ]
sudo ip addr del 192.168.1.100/24 dev enp0s3
```

### Gestión de Interfaces de Red (`ip link`)

Permite gestionar el estado de las interfaces de red.

#### Ver estado de las interfaces

Muestra información de la capa 2 (enlace de datos), como la dirección MAC y el estado (UP/DOWN).

```bash
ip link show
```

#### Levantar (Activar) una interfaz

```bash
sudo ip link set enp0s3 up
```

#### Bajar (Desactivar) una interfaz

```bash
sudo ip link set enp0s3 down
```

### Gestión de Rutas (`ip route`)

Permite visualizar y manipular la tabla de enrutamiento del sistema.

#### Ver la tabla de enrutamiento

Muestra cómo el sistema dirigirá el tráfico a diferentes redes.

```bash
ip route show
# O su forma abreviada
ip r
```

**Ejemplo de Salida:**

```
default via 192.168.1.1 dev enp0s3 proto dhcp metric 100 
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.10 metric 100
```

#### Añadir una ruta por defecto (Gateway)

Una de las tareas más comunes es definir la puerta de enlace predeterminada.

```bash
# Sintaxis: ip route add default via [IP_GATEWAY]
sudo ip route add default via 192.168.1.1
```

#### Eliminar una ruta

Para eliminar una ruta específica.

```bash
# Eliminar la ruta por defecto
sudo ip route del default

# Eliminar una ruta a una red específica
sudo ip route del 10.0.0.0/8
```
