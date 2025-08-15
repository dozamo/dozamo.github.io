---
title: "Sistemas de Archivos"
titleLink: "intro-filesystems"
description: "Conceptos básicos de sistemas de archivos en Linux, incluyendo variedades, particiones y puntos de montaje."
tags: ["filesystem", "storage", "linux", "LFCS", "partitions", "mount-points"]
categories: ["Storage"]
weight: 150
---

En Linux, el sistema de archivos organiza y gestiona cómo se almacenan y recuperan los datos. Cada archivo y directorio se ubica en una jerarquía única, comenzando en `/`.

<img src="/images/lfcs/filesystem_partitions_mount-points.png" alt="Filesystems | partitions | mount points" class="img-fluid rounded shadow-lg">
<small class="d-block mt-2">Filesystems | partitions | mount points.</small>

## Variedades de sistemas de archivos

- **ext4** — Estándar en la mayoría de distribuciones modernas.
- **xfs** — Altamente escalable, ideal para grandes volúmenes.
- **btrfs** — Incluye funciones avanzadas como snapshots.
- **vfat/exFAT** — Compatible con Windows y dispositivos extraíbles.
- **nfs** — Para sistemas de archivos de red.

## Particiones de Linux

- Cada dispositivo de almacenamiento se puede dividir en particiones (`/dev/sda1`, `/dev/sda2`…).
- Una partición puede contener un sistema de archivos único.
- La configuración de montaje puede definirse en `/etc/fstab`.

## Puntos de montaje

- Un punto de montaje es un directorio donde se hace accesible el sistema de archivos.
- Ejemplo: montar `/dev/sdb1` en `/mnt/data`.

## Ejemplo práctico

```bash
sudo mkfs.ext4 /dev/sdb1
sudo mkdir /mnt/data
sudo mount /dev/sdb1 /mnt/data
```