---
title: "Comando umount"
titleLink: "umount"
description: "Guía para desmontar sistemas de archivos en Linux usando el comando umount."
tags: ["umount", "filesystem", "storage", "linux-cli", "LFCS"]
categories: ["Storage"]
weight: 173
---

El comando `umount` en Linux se utiliza para desmontar sistemas de archivos, desconectándolos del punto de montaje.

## umount

{{% alert %}}
`umount` — Desmonta un sistema de archivos.
{{% /alert %}}

### Synopsis

`umount [opciones] directorio|dispositivo`


### Opciones comunes

- `-l` — Desmontaje perezoso: desvincula el sistema de archivos inmediatamente pero finaliza operaciones pendientes antes de liberarlo.
- `-f` — Forzar el desmontaje, útil cuando un recurso de red no responde.
- `-r` — Remonta el sistema como solo lectura si no se puede desmontar normalmente.

## Casos de uso

1. Desmontar un USB montado en `/mnt/usb`:
   ```bash
   sudo umount /mnt/usb
   ```

2. Desmontar por dispositivo

  ```bash
  sudo umount /dev/sdb1
  ```

3. Desmontaje perezoso de un recurso NFS
  
  ```bash
  sudo umount -l /mnt/nfs
  ```

## Buenas prácticas

- Siempre asegurarse de que no existan procesos usando el punto de montaje (`lsof` o `fuser` pueden ayudar).
- Evitar extraer físicamente un dispositivo sin desmontarlo para prevenir corrupción de datos.