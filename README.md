# IT Notes! 🚀

![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Hugo Version](https://img.shields.io/badge/Hugo-0.126.1+-blue.svg)
![Theme](https://img.shields.io/badge/Theme-Docsy-blueviolet.svg)
![Hosted on](https://img.shields.io/badge/Hosted_on-GitHub_Pages-black?logo=github&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?logo=linux&logoColor=black)

¡Bienvenido a mi repositorio de notas de IT! Este es el código fuente de mi sitio personal de documentación, alojado en [**dozamo.github.io**](https://dozamo.github.io).

![Página de Inicio de IT Notes!](https://dozamo.github.io/images/screenshot.png)

## 🎯 Propósito del Sitio

Este espacio nace como una colección personal de guías, comandos y recursos de estudio. Mi objetivo es documentar mi aprendizaje, principalmente enfocado en la preparación para la certificación **LFCS (Linux Foundation Certified Sysadmin)**.

Como bien dice la frase que inspira el sitio:

> To teach is to learn twice — Protégé Effect!

## 🛠️ Stack Tecnológico

*   **Generador de Sitio Estático:** [Hugo](https://gohugo.io/) (versión extendida).
*   **Tema:** [Docsy](https://www.docsy.dev/).
*   **Alojamiento:** [GitHub Pages](https://pages.github.com/).

## 🔧 Desarrollo y Visualización en Local

Si quieres clonar el proyecto para experimentar o contribuir, sigue estos pasos.

### 1. Prerrequisitos

Necesitarás tener instalada una versión reciente y **extendida** de [Hugo](https://gohugo.io/installation/) y el lenguaje [Go](https://go.dev/doc/install).

### 2. Clonar el Repositorio

```bash
git clone git@github.com:dozamo/dozamo.github.io.git
cd dozamo.github.io
```

3. Instalar Dependencias del Tema

El tema Docsy y sus componentes se gestionan como módulos de Hugo. Ejecuta el siguiente comando para descargarlos:

```bash
hugo mod tidy
```

4. Levantar el Servidor Local

Una vez clonado y con las dependencias listas, levanta el servidor de desarrollo de Hugo:

```bash
hugo server
```

Ahora puedes abrir tu navegador y visitar http://localhost:1313 para ver el sitio en tiempo real. Cualquier cambio que guardes en los ficheros se reflejará automáticamente.

## 📚 Contenido Principal

Actualmente, el contenido se está centrando en:

- ✅ Notas para la certificación LFCS.
- 🐧 Comandos y utilidades para la administración de sistemas Linux.
- 💡 Guías rápidas y "cheatsheets" sobre diversas herramientas de IT.

## 📙 Notas para con el sitio

Estas notas son de soporte para el autor/desarrollador del sitio

###  Categorias/Tags

Se listan aquí las categorias (`categories`) y tags (`tags`) actuales utilizados para los artículos/entradas.

### En Ingles

```markdown
# Definiciones en el front matter (metadatos de los markdown)
...
categories: ["AI", "Technical Documentation", "LFCS", "Laboratory", "Linux",  "Windows", "PowerShell7"]
tags: ["Linux CLI", "Prompt Engineering"]
...
```

### En Español

## 📄 Licencia

El contenido de este proyecto está distribuido bajo la Licencia MIT. Puedes ver los detalles completos en el fichero LICENSE del repositorio.