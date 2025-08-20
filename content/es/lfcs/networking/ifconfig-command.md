---
title: "Comando ifconfig (Legacy)"
description: "Revisión del comando ifconfig para la gestión de redes. Aunque obsoleto, es importante conocerlo para sistemas antiguos y para la certificación LFCS."
weight: 30
categories: ["Networking", "Comandos Esenciales"]
tags: ["networking", "ifconfig", "legacy", "net-tools", "sysadmin"]
---

El comando `ifconfig` (interface configuration) es la herramienta clásica para la configuración de redes en sistemas tipo Unix. Pertenece al paquete `net-tools`, que ha sido **declarado obsoleto** en la mayoría de las distribuciones modernas de Linux en favor de `iproute2` (que provee el comando `ip`).

A pesar de ser una herramienta legada, es crucial conocer su uso básico para el examen LFCS, ya que aún puede encontrarse en sistemas más antiguos o en instalaciones mínimas que todavía lo incluyen.

### Nota Importante: Falta de Persistencia

La principal desventaja de `ifconfig` es que **cualquier cambio realizado con él es temporal y se perderá tras un reinicio del sistema o del servicio de red.** Para una configuración permanente, se deben editar los archivos de configuración de red correspondientes a la distribución.

### Casos de Uso Comunes

#### Ver la configuración de todas las interfaces activas

Ejecutar `ifconfig` sin argumentos muestra las interfaces que están "levantadas" (UP).

```bash
ifconfig
```

#### Ver la configuración de todas las interfaces (activas e inactivas)

El flag `-a` muestra todas las interfaces, incluso las que están "bajadas" (DOWN).

```bash
ifconfig -a
```

#### Ver la configuración de una interfaz específica

```bash
ifconfig enp0s3
```

**Ejemplo de Salida:**

```
enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.10  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::a00:27ff:fea1:b2c3  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:a1:b2:c3  txqueuelen 1000  (Ethernet)
        ...
```

#### Asignar una dirección IP y máscara de red

Esta es una forma rápida de configurar una IP para una prueba temporal.

```bash
# Sintaxis: ifconfig [INTERFAZ] [DIRECCIÓN_IP] netmask [MÁSCARA_RED]
sudo ifconfig enp0s3 192.168.1.150 netmask 255.255.255.0
```

#### Levantar (Activar) una interfaz

```bash
sudo ifconfig enp0s3 up
```

#### Bajar (Desactivar) una interfaz

Esto deshabilita la interfaz, deteniendo todo el tráfico a través de ella.

```bash
sudo ifconfig enp0s3 down
```
