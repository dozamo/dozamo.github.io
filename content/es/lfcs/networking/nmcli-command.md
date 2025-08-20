---
title: "Comando nmcli"
description: "Guía sobre nmcli, la herramienta de línea de comandos para NetworkManager, fundamental para la gestión de redes en sistemas modernos."
weight: 20
categories: ["Networking", "Comandos Esenciales"]
tags: ["networking", "networkmanager", "nmcli", "linux", "rhel", "ubuntu"]
---

`nmcli` es la utilidad de línea de comandos para controlar `NetworkManager`, el servicio estándar para la gestión de redes en la mayoría de las distribuciones de escritorio y servidor modernas (incluyendo RHEL/CentOS, Ubuntu, etc.). A diferencia de `ip`, los cambios realizados con `nmcli` son **persistentes** por defecto.

### Estructura y Conceptos Clave

`nmcli` organiza la configuración en dos objetos principales:

-   **Device (Dispositivo):** La interfaz de red física o virtual (`enp0s3`, `wlan0`).
-   **Connection (Conexión):** Un perfil de configuración que se puede aplicar a un dispositivo. Contiene la configuración de IP, DNS, gateway, etc. Un dispositivo puede tener múltiples perfiles de conexión, pero solo uno puede estar activo a la vez.

### Gestión General y de Dispositivos

#### Ver el estado general de NetworkManager

```bash
nmcli general status
```

#### Listar dispositivos de red y su estado

```bash
nmcli device status
# O su forma abreviada
nmcli dev status
```

**Ejemplo de Salida:**

```
DEVICE   TYPE      STATE      CONNECTION         
enp0s3   ethernet  connected  Wired connection 1 
lo       loopback  unmanaged  --
```

#### Desconectar o conectar una interfaz

Esto desactiva la conexión activa en un dispositivo, pero no lo "baja" (sigue `UP` a nivel de `ip link`).

```bash
# Desconectar
sudo nmcli device disconnect enp0s3

# Conectar (activa un perfil de conexión adecuado para el dispositivo)
sudo nmcli device connect enp0s3
```

### Gestión de Conexiones

Esta es la parte más poderosa de `nmcli`, ya que define la configuración persistente.

#### Listar perfiles de conexión existentes

```bash
nmcli connection show
# O su forma abreviada
nmcli con show
```

#### Ver los detalles de una conexión específica

```bash
# Usa el NOMBRE o el UUID de la conexión
nmcli connection show "Wired connection 1"
```

#### Añadir una nueva conexión Ethernet con IP estática

Este es un caso de uso fundamental para servidores.

```bash
# Sintaxis: nmcli con add type ethernet con-name [NOMBRE] ifname [INTERFAZ] ip4 [IP/MASCARA] gw4 [GATEWAY]
sudo nmcli con add type ethernet con-name 'static-prod' ifname enp0s3 ip4 192.168.1.50/24 gw4 192.168.1.1
```

Después de añadir la conexión, es común configurar los servidores DNS.

```bash
# Modificar la conexión recién creada para añadir DNS
sudo nmcli con mod 'static-prod' ipv4.dns "8.8.8.8 8.8.4.4"

# Asegurarse de que la configuración IP no sea DHCP
sudo nmcli con mod 'static-prod' ipv4.method manual
```

Para aplicar la nueva conexión:

```bash
sudo nmcli con up 'static-prod'
```

#### Modificar una conexión para usar DHCP

Si necesitas cambiar una conexión estática a dinámica (DHCP).

```bash
# Cambiar el método de obtención de IP a automático (DHCP)
sudo nmcli con mod 'static-prod' ipv4.method auto

# Opcional: limpiar la configuración estática
sudo nmcli con mod 'static-prod' ipv4.addresses "" ipv4.gateway "" ipv4.dns ""

# Reactivar la conexión para aplicar los cambios
sudo nmcli con down 'static-prod' && sudo nmcli con up 'static-prod'
```

#### Eliminar una conexión

```bash
sudo nmcli connection delete 'static-prod'
```
