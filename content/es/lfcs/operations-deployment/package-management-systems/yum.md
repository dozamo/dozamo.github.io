---
title: "Gestionando paquetes con yum"
description: "Una guía sobre yum, el gestor de paquetes de alto nivel para sistemas RHEL y CentOS más antiguos, clave para la certificación LFCS."
weight: 50
---

## ¿Qué es `yum`?

`yum` (Yellowdog Updater, Modified) es el gestor de paquetes de **alto nivel** para distribuciones basadas en RPM como RHEL 7, CentOS 7 y versiones anteriores. Al igual que `apt`, `yum` trabaja con repositorios para **resolver dependencias automáticamente**, simplificando enormemente la instalación y gestión de software.

Aunque ha sido reemplazado por `dnf` en las versiones más nuevas, `yum` sigue siendo extremadamente relevante, ya que muchos sistemas empresariales aún ejecutan CentOS 7 o RHEL 7.

### Enfoque de la Certificación LFCS

El examen LFCS puede presentarte un entorno basado en CentOS 7/RHEL 7. Por lo tanto, es **obligatorio** saber utilizar `yum` con fluidez para:

-   Instalar, actualizar y eliminar paquetes desde repositorios.
-   Buscar software disponible.
-   Gestionar repositorios de paquetes.
-   Consultar el historial de transacciones y revertir cambios.

### Sintaxis y Casos de Uso Comunes

Todas las operaciones que modifican el sistema requieren privilegios de `sudo`.

#### 1. Instalar un paquete (`install`)

Busca, descarga e instala un paquete y todas sus dependencias.

**Sintaxis:**
```bash
sudo yum install nombre-del-paquete
```

**Ejemplo:**
```bash
# Instalar el servidor web Apache
sudo yum install httpd
```

#### 2. Actualizar paquetes (`update`)

Actualiza todos los paquetes instalados a su última versión disponible en los repositorios.

**Sintaxis:**
```bash
sudo yum update
```
Para actualizar un paquete específico:
```bash
sudo yum update nombre-del-paquete
```

#### 3. Eliminar un paquete (`remove` o `erase`)

Desinstala un paquete y las dependencias que ya no sean necesarias.

**Sintaxis:**
```bash
sudo yum remove nombre-del-paquete
```

**Ejemplo:**
```bash
sudo yum remove httpd
```

#### 4. Buscar un paquete (`search`)

Busca un término en los nombres y descripciones de los paquetes disponibles.

**Sintaxis:**
```bash
yum search termino-de-busqueda
```

**Ejemplo:**
```bash
yum search mariadb-server
```

#### 5. Obtener información de un paquete (`info`)

Muestra información detallada sobre un paquete.

**Sintaxis:**
```bash
yum info nombre-del-paquete
```

**Ejemplo:**
```bash
yum info cronie
```

#### 6. Listar paquetes (`list`)

Permite listar paquetes con diferentes filtros.

-   **Listar todos los paquetes instalados:**
    ```bash
    yum list installed
    ```
-   **Listar todos los paquetes disponibles:**
    ```bash
    yum list available
    ```
-   **Listar todos los paquetes:**
    ```bash
    yum list all
    ```

#### 7. Gestionar el historial (`history`)

`yum` mantiene un registro de todas las transacciones (instalaciones, eliminaciones). Esta es una característica muy potente.

-   **Ver el historial:**
    ```bash
    yum history
    ```
-   **Ver detalles de una transacción específica:**
    ```bash
    yum history info ID_TRANSACCION
    ```
-   **Revertir una transacción:**
    ```bash
    sudo yum history undo ID_TRANSACCION
    ```

**Ejemplo:**
```bash
# Supongamos que la instalación de httpd fue la transacción 25
sudo yum history undo 25
```

#### 8. Limpiar la caché (`clean`)

Elimina los datos almacenados en caché para forzar a `yum` a descargar información fresca la próxima vez.

**Sintaxis:**
```bash
sudo yum clean all
```
