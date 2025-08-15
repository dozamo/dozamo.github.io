---
title: "El directorio /dev"
titleLink: "dev-directory"
description: "Descripción y uso del directorio /dev en Linux."
tags: ["/dev", "device-files", "LFCS"]
categories: ["Storage"]
---

{{% alert title="/dev" %}}
  `/dev` contiene archivos de dispositivos que representan hardware del sistema.
{{% /alert %}}

## Tipos

- **Bloque** (discos, USB): `/dev/sda`, `/dev/sdb`
- **Carácter** (terminales, puertos): `/dev/tty`, `/dev/null`

## Herramientas útiles

```bash
ls -l /dev
udevadm info /dev/sda
```