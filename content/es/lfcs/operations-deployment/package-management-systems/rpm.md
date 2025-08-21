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
