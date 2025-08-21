---
title: "Configuración de Red en RHEL 8+"
description: "Gestión de la configuración de red persistente en distribuciones basadas en RHEL 8 (y superiores) utilizando NetworkManager, nmcli y nmtui."
weight: 40
categories: ["Networking", "Implementación de Operaciones"]
tags: ["rhel y derivados", "networkmanager", "nmcli", "nmtui"]
---

En Red Hat Enterprise Linux (RHEL) 8 y sus derivados (CentOS Stream, AlmaLinux, Rocky Linux), `NetworkManager` es el servicio de red por defecto y la única herramienta soportada para la gestión de la configuración de red. La interacción se realiza principalmente a través de las utilidades `nmcli` (línea de comandos) y `nmtui` (interfaz de texto).

### 1. Usando `nmcli` (Método Recomendado)

`nmcli` es la herramienta más potente y versátil, ideal para scripts y automatización. Los cambios son persistentes.

#### Ejemplo: Configurar una Dirección IP Estática

1.  **Crear un nuevo perfil de conexión:**

    ```bash
    # Se crea un perfil llamado 'static-ens192' para la interfaz 'ens192'
    # Se asigna IP, máscara (en notación CIDR) y gateway
    sudo nmcli connection add type ethernet con-name 'static-ens192' ifname ens192 ip4 10.0.0.100/24 gw4 10.0.0.1
    ```

2.  **Configurar los servidores DNS:**

    ```bash
    sudo nmcli connection modify 'static-ens192' ipv4.dns "8.8.8.8 1.1.1.1"
    ```

3.  **Asegurarse de que el método sea manual (no DHCP):**

    ```bash
    sudo nmcli connection modify 'static-ens192' ipv4.method manual
    ```

4.  **Activar la conexión:**
    Si existía una conexión previa para esa interfaz, es posible que deba desactivarse primero.

    ```bash
    sudo nmcli connection up 'static-ens192'
    ```

### 2. Usando `nmtui` (Interfaz Gráfica en Texto)

`nmtui` (NetworkManager Text User Interface) proporciona un menú sencillo en la terminal para configurar la red de forma interactiva. Es ideal para quienes prefieren una guía visual sin una GUI completa.

1.  **Inicia la herramienta:**

    ```bash
    sudo nmtui
    ```

2.  **Navega por el menú:**
    -   Usa las flechas para seleccionar `Edit a connection` y presiona Enter.
    -   Selecciona la interfaz de red que deseas configurar y elige `<Edit...>`.
    -   En la pantalla de edición:
        -   Cambia `IPv4 CONFIGURATION` de `<Automatic>` a `<Manual>`.
        -   Selecciona `<Show>` para ver los campos de configuración.
        -   Añade la **dirección IP** (con notación CIDR, ej. `10.0.0.100/24`), **Gateway** y **DNS servers**.
        -   Navega hasta `<OK>` y presiona Enter.
    -   Regresa al menú principal con `<Back>`.

3.  **Activa la conexión:**
    -   Selecciona `Activate a connection`.
    -   Busca tu perfil de conexión, selecciónalo y presiona Enter para (re)activarlo.
    -   Sal de `nmtui` seleccionando `<Quit>`.

### 3. Archivos de Configuración (`ifcfg`)

Aunque `nmcli` y `nmtui` son los métodos recomendados, NetworkManager todavía lee y escribe en los tradicionales archivos `ifcfg` ubicados en `/etc/sysconfig/network-scripts/`.

**No se recomienda editar estos archivos directamente en RHEL 8+**, ya que NetworkManager podría sobrescribir los cambios. Sin embargo, es útil saber leerlos para diagnosticar problemas.

**Ejemplo de archivo `/etc/sysconfig/network-scripts/ifcfg-static-ens192`:**

```ini
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none  # 'none' o 'static' para IP estática
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

Si por alguna razón editas un archivo `ifcfg` manualmente, debes recargar las configuraciones en NetworkManager:

```bash
sudo nmcli connection reload
sudo nmcli connection up 'static-ens192'
```
