---
title: "Práctica de laboratorio: Explorando Sistemas de Archivos Montados"
titleLink: "lab-mounted-filesystems"
description: "Ejercicio práctico para identificar y analizar sistemas de archivos montados en Linux."
tags: ["filesystem", "mount", "umount", "storage", "LFCS", "lab"]
categories: ["Storage"]
---

Este laboratorio te ayudará a explorar los sistemas de archivos actualmente montados y su configuración.

{{% alert title="Objetivo" %}}
Familiarizarse con la información y opciones de montaje/desmontaje.
{{% /alert %}}

## Pasos

1. Listar montajes activos
   ```bash
   mount
   ```

2. Consultar `/proc/mounts`
   ```bash
   cat /proc/mounts
   ```

3. Usar `df` para ver espacio disponible
   ```bash
   df -h
   ```

4. Montar un dispositivo de prueba y verificarlo
   ```bash
   sudo mkdir /mnt/test
   sudo mount /dev/sdb1 /mnt/test
   mount | grep test
   ```


