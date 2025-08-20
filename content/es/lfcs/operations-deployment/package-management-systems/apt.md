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

--- FIN DEL ARTÍCULO ---
---
title: "Gestionando paquetes con rpm"
description: "Una guía práctica para usar rpm en la gestión de paquetes de bajo nivel en sistemas Red Hat, CentOS y Fedora."
weight: 40
---

## ¿Qué es `rpm`?

`rpm` (RPM Package Manager) es la herramienta de gestión de paquetes de **bajo nivel** para el ecosistema Red Hat (RHEL, CentOS, Fedora, SUSE, etc.). Al igual que `dpkg` en el mundo Debian, `rpm` trabaja directamente con archivos de paquete (`.rpm`), pero **no resuelve dependencias automáticamente**.

### Enfoque de la Certificación LFCS

El conocimiento de `rpm` es esencial para el examen LFCS, ya que te enfrentarás a escenarios en sistemas basados en Red Hat. La certificación se centra en tu habilidad para:
-   Instalar, actualizar y eliminar paquetes desde un archivo `.rpm` local.
-   Consultar la base de datos de RPM para obtener información sobre paquetes.
-   Listar los archivos que pertenecen a un paquete.
-   Identificar qué paquete instaló un archivo en el sistema.
-   Verificar la integridad de los archivos de un paquete.

### Sintaxis y Casos de Uso Comunes

Las operaciones de `rpm` se agrupan por modos, siendo los más comunes la instalación (`-i`), la consulta (`-q`) y la eliminación (`-e`).

#### 1. Instalar un paquete (`-i` o `-ivh`)

Se utiliza para instalar un paquete desde un archivo `.rpm`. Es una práctica común usar las opciones `-ivh`:
-   `-i`: Instalar.
-   `-v`: Verbose (mostrar más información).
-   `-h`: Hash (mostrar una barra de progreso).

**Sintaxis:**
```bash
sudo rpm -ivh /ruta/al/paquete.rpm
```

**Ejemplo:**
```bash
# Instalar un paquete descargado manualmente
sudo rpm -ivh epel-release-latest-8.noarch.rpm
```
> **¡Importante!** Si `rpm` reporta un error de dependencias, deberás encontrar e instalar esas dependencias manualmente, o, de forma más realista, usar una herramienta de alto nivel como `yum` o `dnf` para instalar el paquete local, ya que estas sí pueden resolver las dependencias (`sudo dnf install paquete.rpm`).

#### 2. Actualizar un paquete (`-U` o `-Uvh`)

Si un paquete ya está instalado, `-U` lo actualiza. Si no está instalado, lo instala. Es generalmente más seguro que `-i`.

**Sintaxis:**
```bash
sudo rpm -Uvh /ruta/al/paquete_nuevo.rpm
```

#### 3. Eliminar un paquete (`-e`)

Desinstala un paquete. Se utiliza el nombre del paquete, no el nombre del archivo `.rpm`.

**Sintaxis:**
```bash
sudo rpm -e nombre-del-paquete
```

**Ejemplo:**
```bash
sudo rpm -e httpd-tools
```

#### 4. Consultar paquetes (`-q` para Query)

El modo de consulta es extremadamente potente y versátil.

-   **Verificar si un paquete está instalado:**
    ```bash
    rpm -q nombre-del-paquete
    # Ejemplo: rpm -q httpd
    ```

-   **Listar todos los paquetes instalados:** (`-a` para all)
    ```bash
    rpm -qa | grep -i [termino]
    # Ejemplo: rpm -qa | grep -i kernel
    ```

-   **Obtener información detallada de un paquete:** (`-i` para info)
    ```bash
    rpm -qi nombre-del-paquete
    # Ejemplo: rpm -qi bash
    ```

-   **Listar los archivos de un paquete instalado:** (`-l` para list)
    ```bash
    rpm -ql nombre-del-paquete
    # Ejemplo: rpm -ql coreutils
    ```

-   **Averiguar a qué paquete pertenece un archivo:** (`-f` para file)
    ```bash
    rpm -qf /ruta/completa/al/archivo
    # Ejemplo: rpm -qf /etc/passwd
    ```

#### 5. Verificar la integridad de un paquete (`-V`)

Compara el estado actual de los archivos instalados por un paquete con la información almacenada en la base de datos de RPM (tamaño, permisos, hash, etc.). Si no devuelve nada, todo está correcto.

**Sintaxis:**
```bash
rpm -V nombre-del-paquete
```

**Ejemplo:**
```bash
# Verificar el paquete openssh. Si hay cambios, mostrará una línea por cada archivo modificado.
rpm -V openssh
```

--- FIN DEL ARTÍCULO ---
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

--- FIN DEL ARTÍCULO ---
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

--- FIN DEL ARTÍCULO ---
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

--- FIN DEL ARTÍCULO ---