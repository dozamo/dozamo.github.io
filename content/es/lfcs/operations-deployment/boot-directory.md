---
title: "El directorio /boot"
titleLink: "boot-directory"
description: "Uso del directorio /boot en Linux."
tags: ["/boot", "bootloader", "kernel", "LFCS"]
categories: ["Operations Deployment"]
---

{{% alert="/boot" %}}
  `/boot` contiene los archivos necesarios para iniciar el sistema
{{% /alert %}}

En este directorio se encuentran los archivos:
- Kernel (`vmlinuz`)
- Imágenes initramfs
- Archivos de GRUB

**Ejemplo:**
```bash
ls /boot
```
