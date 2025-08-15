---
title: "Filesystem /proc"
titleLink: "proc-filesystem"
description: "Uso del sistema de archivos virtual /proc en Linux."
tags: ["FHS-linux", "virtual-filesystem", "processes", "LFCS"]
categories: ["Essential Commands"]
---

{{% alert title="/proc" %}}
  `/proc` es un sistema de archivos virtual que proporciona información del kernel y procesos en tiempo real.
{{% /alert %}}

## Características

- Contiene directorios numerados por `PID`.
- Archivos especiales como `/proc/cpuinfo`, `/proc/meminfo`, `/proc/mounts`.
- Permite ajustar parámetros del kernel (ej. `/proc/sys`).

## Ejemplo

```bash
cat /proc/cpuinfo
```