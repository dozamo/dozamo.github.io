---
title: "Comando mount"
titleLink: "mount"
description: "Guía práctica para utilizar el comando mount en Linux para montar sistemas de archivos."
tags: ["mount", "filesystem", "storage", "linux-cli", "LFCS"]
categories: ["Storage"]
weight: 170
---

El comando `mount` en Linux se utiliza para montar sistemas de archivos y hacerlos accesibles a través de un punto de montaje en el árbol de directorios.

{{% alert %}}
`mount` — Monta un sistema de archivos en un punto de montaje.
{{% /alert %}}

## Synopsis

```bash
mount [-t tipo] dispositivo directorio
mount [-o opciones] dispositivo directorio
mount [-a]
```

## Opciones comunes

- `-t tipo` — Especifica el tipo de sistema de archivos (ext4, xfs, nfs, etc.).
- `-o opciones` — Monta con opciones específicas como `ro` (solo lectura), `rw` (lectura/escritura), `noexec`, `nosuid`, etc.
- `-a` — Monta todos los sistemas de archivos definidos en `/etc/fstab`.

## Casos de uso

1. Montar un dispositivo USB en `/mnt/usb`:
   
   ```bash
   sudo mount -t vfat /dev/sdb1 /mnt/usb
   ```

2. Montar un sistema de archivos NFS
   
   ```bash
   sudo mount -t nfs server:/path /mnt/nfs
   ```

3. Montar todos los sistemas definidos en `/etc/fstab`:

   ```bash
   sudo mount -a
   ```

## Archivos relacionados

- `/etc/fstab` - Tabla de sistemas de archivos para montaje automático.
- `/etc/mtab` - Lista de sistemas actualmente montados.
- `/proc/mounts` - Información del kernel sobre los montajes activos.


