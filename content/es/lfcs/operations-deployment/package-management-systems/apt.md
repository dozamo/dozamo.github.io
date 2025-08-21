---
title: "Gestionando paquetes con apt"
description: "Una guía completa sobre el uso de apt para la gestión de paquetes de alto nivel, repositorios y dependencias en sistemas Debian/Ubuntu."
weight: 30
---

`apt` (Advanced Package Tool) es la herramienta de **alto nivel** para la gestión de paquetes en sistemas Debian, Ubuntu y derivados. A diferencia de `dpkg`, `apt` trabaja con repositorios remotos, gestiona listas de paquetes y, lo más importante, **resuelve dependencias automáticamente**. El comando moderno `apt` es una interfaz más amigable que combina las funcionalidades de herramientas más antiguas como `apt-get` y `apt-cache`.

### Enfoque de la Certificación LFCS

`apt` es una herramienta fundamental en el ecosistema Debian. Para la certificación LFCS, es absolutamente crucial dominar las siguientes tareas:

-   Actualizar la lista de paquetes de los repositorios.
-   Instalar, actualizar y eliminar software de los repositorios.
-   Buscar paquetes disponibles.
-   Obtener información detallada sobre un paquete.
-   Gestionar el sistema y mantenerlo actualizado.

### Sintaxis y Casos de Uso Comunes

La mayoría de las operaciones con `apt` que modifican el sistema requieren privilegios de superusuario (`sudo`).

#### 1. Actualizar la lista de paquetes (`update`)

Este comando no actualiza el software. En su lugar, descarga la información más reciente sobre los paquetes disponibles desde los repositorios configurados en `/etc/apt/sources.list` y `/etc/apt/sources.list.d/`. **Este es siempre el primer paso antes de instalar o actualizar software.**

**Sintaxis:**
```bash
sudo apt update
```

#### 2. Actualizar los paquetes instalados (`upgrade`)

Una vez actualizada la lista de paquetes, este comando descarga e instala las versiones más nuevas de todos los paquetes instalados en el sistema.

**Sintaxis:**
```bash
sudo apt upgrade
```
> También existe `sudo apt full-upgrade`, que además de actualizar, puede eliminar paquetes instalados si es necesario para resolver conflictos de dependencias en una actualización mayor.

#### 3. Instalar un paquete (`install`)

Busca un paquete en los repositorios, resuelve todas sus dependencias, y los descarga e instala.

**Sintaxis:**
```bash
sudo apt install nombre-del-paquete
```

**Ejemplo:**
```bash
# Instalar el servidor web Nginx
sudo apt install nginx
```

#### 4. Eliminar un paquete (`remove` y `purge`)

-   `remove`: Desinstala el paquete pero deja sus archivos de configuración.
-   `purge`: Desinstala el paquete Y elimina sus archivos de configuración.

**Sintaxis:**
```bash
sudo apt remove nombre-del-paquete
sudo apt purge nombre-del-paquete
```

**Ejemplo:**
```bash
# Eliminar Nginx pero conservar la configuración
sudo apt remove nginx

# Eliminar Nginx y toda su configuración
sudo apt purge nginx
```

#### 5. Buscar un paquete (`search`)

Busca en los nombres y descripciones de los paquetes disponibles en los repositorios.

**Sintaxis:**
```bash
apt search termino-de-busqueda
```

**Ejemplo:**
```bash
# Buscar herramientas relacionadas con redis
apt search redis-tools
```

#### 6. Mostrar información de un paquete (`show`)

Muestra información detallada de un paquete, como su versión, dependencias, tamaño y descripción.

**Sintaxis:**
```bash
apt show nombre-del-paquete
```

**Ejemplo:**
```bash
apt show docker-ce
```

#### 7. Limpiar el sistema (`autoremove` y `clean`)

-   `autoremove`: Elimina paquetes que fueron instalados como dependencias de otros paquetes pero que ya no son necesarios.
-   `clean`: Borra la caché de paquetes descargados (`.deb`) de `/var/cache/apt/archives/` para liberar espacio en disco.

**Sintaxis:**
```bash
sudo apt autoremove
sudo apt clean
```
