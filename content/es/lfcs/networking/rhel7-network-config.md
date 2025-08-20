---
title: "Configuración de Red en RHEL 7 y Anteriores"
description: "Guía para la configuración de red persistente en distribuciones basadas en RHEL 7 (y anteriores) utilizando los archivos ifcfg."
weight: 70
categories: ["Networking", "Implementación de Operaciones"]
tags: ["networking", "rhel", "centos", "ifcfg", "sysconfig", "legacy"]
---

En Red Hat Enterprise Linux (RHEL) 7, CentOS 7 y versiones anteriores, el método tradicional para la configuración de red persistente es mediante la edición de archivos de script en el directorio `/etc/sysconfig/network-scripts/`.

Cada interfaz de red tiene su propio archivo de configuración, nombrado `ifcfg-<nombre-interfaz>` (p. ej., `ifcfg-eth0` o `ifcfg-enp0s3`). Aunque `NetworkManager` está presente y activado por defecto en RHEL 7, estos archivos son la fuente principal de configuración.

### Parámetros Clave en Archivos `ifcfg`

Estos archivos son scripts de shell que definen variables. Los parámetros más importantes son:

-   `DEVICE`: El nombre lógico de la interfaz (debe coincidir con el nombre del archivo sin el prefijo `ifcfg-`).
-   `ONBOOT`: Si se debe activar la interfaz al arrancar el sistema. Debe ser `yes` para que la red funcione al inicio.
-   `BOOTPROTO`: El método para obtener la configuración IP.
    -   `dhcp`: Usa el protocolo DHCP.
    -   `static` o `none`: Usa la configuración de IP estática definida en el archivo.
-   `IPADDR`: La dirección IP estática.
-   `NETMASK`: La máscara de subred (p. ej., `255.255.255.0`).
-   `PREFIX`: La longitud del prefijo de red (p. ej., `24`). Es una alternativa a `NETMASK`.
-   `GATEWAY`: La puerta de enlace predeterminada.
-   `DNS1`, `DNS2`: Los servidores DNS primario y secundario.
-   `NM_CONTROLLED`: Indica si NetworkManager debe gestionar este dispositivo. `yes` por defecto. Ponerlo en `no` hace que la interfaz sea gestionada por el servicio de red tradicional.

### Ejemplo 1: Configuración DHCP

Un archivo `ifcfg-enp0s3` para una configuración DHCP sería muy simple:

```ini
# /etc/sysconfig/network-scripts/ifcfg-enp0s3
DEVICE=enp0s3
BOOTPROTO=dhcp
ONBOOT=yes
NM_CONTROLLED=yes
```

### Ejemplo 2: Configuración con Dirección IP Estática

Este es el caso de uso típico para un servidor.

```ini
# /etc/sysconfig/network-scripts/ifcfg-enp0s3
DEVICE=enp0s3
BOOTPROTO=static
ONBOOT=yes
NM_CONTROLLED=yes

IPADDR=192.168.1.220
PREFIX=24
# O alternativamente: NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
DNS2=1.1.1.1
```

### Aplicando los Cambios

Después de crear o modificar un archivo `ifcfg`, debes reiniciar el servicio de red para que los cambios surtan efecto.

#### Usando `systemctl` (Método Preferido en RHEL 7)

```bash
sudo systemctl restart network.service
```

**Atención:** Reiniciar el servicio de red puede desconectarte si estás conectado por SSH.

#### Usando `ifdown` e `ifup`

También puedes reactivar una interfaz específica, lo cual es a menudo más seguro.

```bash
# Bajar la interfaz
sudo ifdown enp0s3

# Levantar la interfaz con la nueva configuración
sudo ifup enp0s3
```

Estos comandos leerán el archivo `ifcfg-enp0s3` y aplicarán su configuración.
