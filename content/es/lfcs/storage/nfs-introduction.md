---
title: "Introducción a NFS y Sistemas de Archivos de Red"
titleLink: "intro-nfs"
description: "Conceptos básicos de NFS y otros sistemas de archivos de red en Linux."
tags: ["nfs", "network-filesystem", "linux", "storage", "LFCS"]
categories: ["Networking", "Storage"]
---

NFS (Network File System) permite que sistemas Linux compartan directorios y archivos a través de una red como si estuvieran en el sistema local.

## Características

- Protocolo cliente-servidor.
- Montaje transparente de recursos remotos.
- Control de acceso configurable.

## Otros sistemas de archivos de red

- **SMB/CIFS** — Integración con sistemas Windows.
- **SSHFS** — Montaje seguro vía SSH.
- **GlusterFS** — Almacenamiento distribuido.

## Ejemplo de montaje NFS

```bash
sudo mount -t nfs srv-nfs01:/shared/data/rrhh /var/srv-nfs01/rrhh
```

## Archivo /etc/fstab para NFS

```bash
srv-nfs01:/shared/data /var/srv-nfs01/rrhh nfs defaults 0 0
```