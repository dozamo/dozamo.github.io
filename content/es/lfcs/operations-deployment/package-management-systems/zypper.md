---
title: "Gestionando paquetes con zypper"
description: "Una guía práctica para usar zypper, el potente gestor de paquetes de línea de comandos para sistemas SUSE Linux Enterprise y openSUSE."
weight: 70
---

## ¿Qué es `zypper`?

`zypper` es la interfaz de línea de comandos para `libzypp`, el motor de gestión de paquetes utilizado en las distribuciones SUSE Linux Enterprise (SLES) y openSUSE. Es una herramienta de **alto nivel** muy potente, equiparable a `apt` y `dnf`, que gestiona repositorios y resuelve dependencias complejas.

### Enfoque de la Certificación LFCS

La certificación LFCS es agnóstica a la distribución, y aunque los sistemas SUSE son menos comunes que los derivados de Debian o Red Hat en algunos contextos, son un jugador importante en el mundo empresarial. Por lo tanto, se espera que un sysadmin certificado tenga un conocimiento básico de `zypper`.

**¿Qué evalúa la certificación LFCS con `zypper`?**

-   La capacidad de realizar operaciones básicas: instalar, eliminar y actualizar paquetes.
-   Actualizar el sistema.
-   Buscar paquetes.
-   Gestionar (refrescar) repositorios.

La sintaxis de `zypper` es un poco diferente, pero es lógica y fácil de aprender. Una característica notable es que muchos comandos tienen formas abreviadas (p. ej., `install` es `in`, `remove` es `rm`).

### Sintaxis y Casos de Uso Comunes

Casi todas las operaciones que modifican el sistema requieren privilegios de `sudo`.

#### 1. Refrescar Repositorios (`refresh` o `ref`)

Equivalente a `apt update`. Descarga los metadatos más recientes de todos los repositorios configurados.

**Sintaxis:**
```bash
sudo zypper refresh
# Forma corta:
sudo zypper ref
```

#### 2. Instalar un paquete (`install` o `in`)

Busca, descarga e instala un paquete y sus dependencias.

**Sintaxis:**
```bash
sudo zypper install nombre-del-paquete
# Forma corta:
sudo zypper in nombre-del-paquete
```

**Ejemplo:**
```bash
sudo zypper install apache2
```

#### 3. Actualizar Paquetes

-   **Actualizar todos los paquetes (patch):** `zypper patch` es el método recomendado para aplicar parches de seguridad y correcciones oficiales.
    ```bash
    sudo zypper patch
    ```

-   **Actualizar todos los paquetes a la última versión (update):** `zypper update` es más parecido a `apt upgrade` o `dnf update`.
    ```bash
    sudo zypper update
    # Forma corta:
    sudo zypper up
    ```

#### 4. Eliminar un paquete (`remove` o `rm`)

**Sintaxis:**
```bash
sudo zypper remove nombre-del-paquete
# Forma corta:
sudo zypper rm nombre-del-paquete
```

**Ejemplo:**
```bash
sudo zypper rm apache2
```

#### 5. Buscar un paquete (`search` o `se`)

Busca paquetes en los repositorios.

**Sintaxis:**
```bash
zypper search termino-de-busqueda
# Forma corta:
zypper se termino-de-busqueda
```

**Ejemplo:**
```bash
# Buscar paquetes relacionados con "firewall"
zypper se firewall
```
Por defecto, busca en nombres y resúmenes. Puedes añadir `-d` para buscar también en las descripciones.

#### 6. Obtener información de un paquete (`info`)

Muestra información detallada de un paquete.

**Sintaxis:**
```bash
zypper info nombre-del-paquete
```

**Ejemplo:**
```bash
zypper info systemd
```

#### 7. Gestionar Repositorios

-   **Listar repositorios (`repos` o `lr`):**
    ```bash
    zypper repos
    # Forma corta:
    zypper lr
    ```
-   **Añadir un repositorio (`addrepo` o `ar`):**
    ```bash
    sudo zypper addrepo URL_DEL_REPO ALIAS_DEL_REPO
    ```
-   **Eliminar un repositorio (`removerepo` o `rr`):**
    ```bash
    sudo zypper removerepo ALIAS_DEL_REPO
    ```
