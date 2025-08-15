---
title: "Directorios de medios removibles"
#Removable Media: /media, /run, and /mnt"
titleLink: "removable-media-directories"
description: "Uso de directorios para medios removibles y montaje temporal."
tags: ["FHS-linux", "mount", "LFCS"]
categories: ["Storage"]
---

{{% alert title="Directorios medios removibles" %}}
  Directorios: `/media`, `/mnt`, `/run` 
{{% /alert %}}

## Objetivo de los directorios

- `/media` — Montaje automático de dispositivos externos.
- `/mnt` — Punto genérico para montajes manuales temporales.
- `/run` — Información y montajes de tiempo de ejecución (tmpfs).
