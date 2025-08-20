---
title: "Gestionando paquetes con dpkg"
description: "Una guía práctica para usar el comando dpkg para la gestión de paquetes de bajo nivel en sistemas Debian y derivados."
weight: 20
---

`dpkg` (Debian Package) es la herramienta de software fundamental que subyace al sistema de gestión de paquetes en Debian y sus derivados como Ubuntu. Es una utilidad de **bajo nivel**, lo que significa que opera directamente sobre archivos de paquete (`.deb`) pero no gestiona repositorios ni resuelve dependencias automáticamente.

### Enfoque de la Certificación LFCS

Para el examen LFCS, se espera que sepas usar `dpkg` para tareas que no involucran repositorios remotos. La certificación evalúa principalmente tu capacidad para:

-   Instalar un paquete desde un archivo `.deb` local.
-   Listar los paquetes instalados y verificar su estado.
-   Determinar qué archivos fueron instalados por un paquete.
-   Identificar a qué paquete pertenece un archivo específico del sistema.
-   Eliminar y purgar paquetes.

### Sintaxis y Casos de Uso Comunes

La mayoría de las operaciones con `dpkg` requieren privilegios de superusuario (`sudo`).

#### 1. Instalar un paquete (`-i` o `--install`)

Esta es la tarea más común. Se utiliza para instalar un paquete que ya has descargado.

**Sintaxis:**
```bash
sudo dpkg -i /ruta/al/paquete.deb
```

**Ejemplo:**
```bash
# Descargamos el paquete de Google Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

# Lo instalamos con dpkg
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

> **¡Importante!** Si este comando falla por dependencias incumplidas, el sistema queda en un estado inconsistente. La solución habitual es ejecutar `sudo apt-get -f install`, que le pide a `apt` que busque y solucione las dependencias que `dpkg` no pudo resolver.

#### 2. Eliminar un paquete (`-r` o `--remove`)

Esto desinstala el paquete, pero **mantiene sus archivos de configuración**.

**Sintaxis:**
```bash
sudo dpkg -r nombre-del-paquete
```

**Ejemplo:**
```bash
sudo dpkg -r google-chrome-stable
```

#### 3. Purgar un paquete (`-P` o `--purge`)

Esto desinstala el paquete **Y** elimina sus archivos de configuración.

**Sintaxis:**
```bash
sudo dpkg -P nombre-del-paquete
```

**Ejemplo:**
```bash
sudo dpkg -P apache2
```

#### 4. Listar paquetes instalados (`-l` o `--list`)

Muestra una lista de todos los paquetes conocidos por el sistema. Es muy útil combinarlo con `grep` para buscar algo específico.

**Sintaxis:**
```bash
dpkg -l [patrón-de-búsqueda]
```

**Ejemplo:**
```bash
# Listar todos los paquetes que contienen "nginx" en su nombre
dpkg -l | grep nginx
```

#### 5. Consultar el estado de un paquete (`-s` o `--status`)

Proporciona información detallada sobre un paquete específico, incluyendo si está instalado, su versión y su descripción.

**Sintaxis:**
```bash
dpkg -s nombre-del-paquete
```

**Ejemplo:**
```bash
dpkg -s ufw
```

#### 6. Listar archivos de un paquete (`-L` o `--listfiles`)

Muestra todos los archivos que un paquete ha instalado en el sistema.

**Sintaxis:**
```bash
dpkg -L nombre-del-paquete
```

**Ejemplo:**
```bash
# Ver todos los archivos instalados por el paquete curl
dpkg -L curl
```

#### 7. Averiguar a qué paquete pertenece un archivo (`-S` o `--search`)

Si encuentras un archivo en el sistema y quieres saber qué paquete lo instaló, este es el comando que necesitas.

**Sintaxis:**
```bash
dpkg -S /ruta/completa/al/archivo
```

**Ejemplo:**
```bash
# Averiguar qué paquete instaló el comando /bin/ls
dpkg -S /bin/ls
```
