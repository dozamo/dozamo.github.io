---
title: "El /var"
titleLink: "var-directory"
description: "Propósito del directorio /var en Linux."
tags: ["/var", "logs", "LFCS"]
categories: ["Storage"]
---

{{% alert title="/var" %}}
  `/var` almacena datos variables
{{% /alert %}}

Algunos de esos datos son:

- Archivos de log (`/var/log`)
- Buzones de correo
- Cachés y archivos temporales persistentes
- Datos de aplicaciones

## Ejemplo

```bash
ls /var/log
```