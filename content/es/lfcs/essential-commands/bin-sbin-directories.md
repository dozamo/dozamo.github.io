---
title: "El /bin y el /sbin"
titleLink: "bin-sbin-directories"
description: "Función de los directorios /bin y /sbin en Linux."
tags: ["/bin", "/sbin", "essential-commands", "LFCS"]
categories: ["Essential Commands"]
---

{{% alert title="/bin y /sbin" %}}
  - `/bin` contiene comandos esenciales para todos los usuarios.
  - `/sbin` contiene comandos administrativos y de mantenimiento, generalmente para el superusuario.
{{% /alert %}}

## Ejemplos

- `/bin/ls`, `/bin/cp`, `/bin/mv`
- `/sbin/reboot`, `/sbin/mkfs`

## Nota

En distribuciones modernas, `/bin` y `/sbin` pueden estar enlazados a `/usr/bin` y `/usr/sbin`.
