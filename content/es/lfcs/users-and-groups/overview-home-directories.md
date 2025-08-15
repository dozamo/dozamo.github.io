---
title: "Directorio de inicio del usuario"
titleLink: "overview-home-directories"
description: "Descripción general de los directorios home de los usuarios en Linux."
tags: ["FHS-linux", "users", "permissions", "LFCS"]
categories: ["Users and Groups"]
---

{{% alert title="Directorio home" %}}
  En Linux, cada usuario tiene un **directorio home** que sirve como espacio de trabajo personal y almacenamiento de archivos.
{{% /alert %}}

## Características

- Ubicación predeterminada: `/home/username`
- Contiene configuraciones personales y archivos.
- Permisos restringidos para proteger la privacidad.
- Configurado en `/etc/passwd` junto con la shell predeterminada.

## Ejemplo de ver el home de un usuario

```bash
grep username /etc/passwd
```

## Buenas prácticas

- Mantener el home del usuario con permisos `700` o `750`.
- Usar `/etc/skel` para establecer archivos de inicio predeterminados.
