---
title: "Gestionando paquetes con dnf"
description: "Una guía moderna para dnf, el gestor de paquetes de nueva generación para sistemas Fedora, RHEL 8+, y CentOS 8+."
weight: 60
---

## ¿Qué es `dnf`?

`dnf` (Dandified YUM) es el sucesor de `yum` y el gestor de paquetes de **alto nivel** predeterminado en las distribuciones modernas basadas en RPM, como RHEL 8+, CentOS 8+, y Fedora. Fue diseñado para resolver algunas de las deficiencias de `yum`, ofreciendo un mejor rendimiento, un menor consumo de memoria y un algoritmo de resolución de dependencias más robusto.

Para facilitar la transición, `dnf` mantiene una alta compatibilidad de comandos con `yum`. De hecho, en muchos sistemas modernos, el comando `yum` es simplemente un enlace simbólico a `dnf`.

### Enfoque de la Certificación LFCS

Dado que la industria se está moviendo hacia RHEL 8/9 y sus derivados, es **imperativo** que domines `dnf` para la certificación LFCS. El examen evaluará tu capacidad para realizar las mismas tareas que con `yum`, pero en un sistema moderno. Afortunadamente, la sintaxis es casi idéntica.

**¿Qué evalúa la certificación LFCS con `dnf`?**

-   Uso de comandos básicos: `install`, `remove`, `update`.
-   Búsqueda y consulta de información de paquetes.
-   Gestión del historial de transacciones.
-   Instalación de paquetes desde un archivo `.rpm` local, permitiendo que `dnf` resuelva las dependencias.

### Sintaxis y Casos de Uso Comunes

Los comandos son prácticamente los mismos que los de `yum`, simplemente reemplazando `yum` por `dnf`.

#### 1. Instalar un paquete (`install`)

**Sintaxis:**
```bash
sudo dnf install nombre-del-paquete
```

**Ejemplo:**
```bash
# Instalar el servidor de base de datos MariaDB
sudo dnf install mariadb-server
```
Una gran ventaja de `dnf` sobre `rpm` es que puede instalar un archivo `.rpm` local y buscar sus dependencias en los repositorios:
```bash
sudo dnf install /ruta/al/paquete.rpm
```

#### 2. Actualizar paquetes (`update` o `upgrade`)

`update` es el alias recomendado. `upgrade` también funciona para mantener la compatibilidad con `yum`.

**Sintaxis:**
```bash
sudo dnf update
```
Para actualizar un paquete específico:
```bash
sudo dnf update nombre-del-paquete
```

#### 3. Eliminar un paquete (`remove`)

**Sintaxis:**
```bash
sudo dnf remove nombre-del-paquete
```

**Ejemplo:**
```bash
sudo dnf remove mariadb-server
```

#### 4. Eliminar dependencias no utilizadas (`autoremove`)

`dnf` tiene un comando explícito para esto, similar a `apt autoremove`.

**Sintaxis:**
```bash
sudo dnf autoremove
```

#### 5. Buscar y obtener información

-   **Buscar un paquete:**
    ```bash
    dnf search termino-de-busqueda
    ```
-   **Obtener información detallada:**
    ```bash
    dnf info nombre-del-paquete
    ```
-   **Encontrar qué paquete provee un comando o archivo:**
    ```bash
    dnf provides /ruta/al/archivo
    # Ejemplo: dnf provides /usr/sbin/semanage
    ```

#### 6. Gestionar el historial (`history`)

Funciona de la misma manera potente que en `yum`.

-   **Ver el historial:**
    ```bash
    dnf history
    ```
-   **Revertir una transacción:**
    ```bash
    sudo dnf history undo ID_TRANSACCION
    ```

#### 7. Listar paquetes y repositorios

-   **Listar paquetes instalados:**
    ```bash
    dnf list installed
    ```
-   **Listar repositorios habilitados:**
    ```bash
    dnf repolist
    ```