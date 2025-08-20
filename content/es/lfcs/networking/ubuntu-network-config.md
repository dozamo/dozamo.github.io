---
title: "Configuración de Red en Ubuntu (/etc/netplan)"
description: "Guía para la configuración de red en versiones modernas de Ubuntu Server utilizando Netplan y su sintaxis YAML."
weight: 60
categories: ["Networking", "Implementación de Operaciones"]
tags: ["networking", "ubuntu", "netplan", "yaml", "systemd-networkd", "networkmanager"]
---

A partir de Ubuntu 17.10, `Netplan` es la herramienta por defecto para la configuración de red. Netplan actúa como una capa de abstracción que genera la configuración para un servicio de backend, que puede ser `systemd-networkd` (usado por defecto en Ubuntu Server) o `NetworkManager` (usado por defecto en Ubuntu Desktop).

La configuración se define en archivos con formato **YAML** ubicados en `/etc/netplan/`.

### Sintaxis YAML y Estructura de Netplan

YAML es un lenguaje de serialización de datos legible por humanos. **La indentación (espacios, no tabuladores) es crucial y sintácticamente significativa.**

La estructura básica de un archivo de Netplan es:

```yaml
network:
  version: 2
  renderer: [backend]
  ethernets:
    [nombre_interfaz]:
      [configuración]
```

-   `renderer`: Especifica el backend (`systemd-networkd` o `NetworkManager`).
-   `ethernets`: Clave para definir configuraciones de interfaces Ethernet.

### Ejemplo 1: Configuración DHCP (Por Defecto)

Un archivo de configuración típico para DHCP, como el que se encuentra en `/etc/netplan/00-installer-config.yaml`, se ve así:

```yaml
network:
  ethernets:
    enp0s3:
      dhcp4: true
  version: 2
```

-   `enp0s3`: Es el nombre del dispositivo de red. Debes reemplazarlo por el nombre correcto en tu sistema (verifícalo con `ip a`).
-   `dhcp4: true`: Habilita la obtención de una dirección IPv4 a través de DHCP.

### Ejemplo 2: Configuración con Dirección IP Estática

Para un servidor, la configuración estática es la más común.

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

-   `dhcp4: no`: Es importante deshabilitar DHCP explícitamente.
-   `addresses`: Una lista de direcciones IP en notación CIDR. Puedes listar más de una.
-   `gateway4`: La puerta de enlace predeterminada para IPv4.
-   `nameservers`: Contiene una lista de `addresses` para los servidores DNS.

### Aplicando la Configuración de Netplan

El flujo de trabajo con Netplan es siempre el mismo:

1.  **Editar el archivo YAML:**
    Modifica el archivo `.yaml` en `/etc/netplan/` con `sudo`.

    ```bash
    sudo nano /etc/netplan/00-installer-config.yaml
    ```

2.  **Probar la configuración (opcional pero recomendado):**
    El comando `try` aplica la configuración temporalmente (por 120 segundos) y la revierte si no confirmas. Si pierdes la conexión SSH, se restaurará automáticamente.

    ```bash
    sudo netplan try
    ```

3.  **Aplicar la configuración de forma permanente:**
    Si la prueba fue exitosa o estás seguro de los cambios, aplícalos permanentemente.

    ```bash
    sudo netplan apply
    ```

Este comando leerá los archivos YAML, generará la configuración para el backend (ej. `systemd-networkd`) y la aplicará.
