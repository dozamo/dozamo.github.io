---
title: Sidebar en Docsy
date: 2025-08-14
description: >
  Cómo funciona el sidebar en Docsy.
draft: true
---


Este post introduce a como funciona el sidebar en Docsy


## 1. Jerarquía automática basada en estructura de archivos

Estructura inicial de ejemplo
```bash
# Estructura de archivos que genera navegación automática
content/en/docs/
├── _index.md                    # "Documentation" (raíz)
│   weight: 10
├── getting-started/
│   ├── _index.md               # "Getting Started" (sección)
│   │   weight: 10
│   ├── installation.md        # "Installation" (página)
│   │   weight: 10
│   └── quickstart.md          # "Quick Start" (página)
│       weight: 20
├── lfcs/
│   ├── _index.md               # "LFCS Certification" (sección)
│   │   weight: 20
│   ├── lab-setup/
│   │   ├── _index.md           # "Lab Setup" (subsección)
│   │   │   weight: 10
│   │   ├── kvm-setup.md        # "KVM Setup" (página)
│   │   │   weight: 10
│   │   └── networking.md       # "Networking" (página)
│   │       weight: 20
│   └── system-admin/
│       ├── _index.md           # "System Administration" (subsección)
│       │   weight: 20
│       └── user-management.md  # "User Management" (página)
│           weight: 10
└── advanced/
    ├── _index.md               # "Advanced Topics" (sección)
    │   weight: 30
    └── optimization.md         # "Optimization" (página)
        weight: 10
```

## 2. Control de navegación con Front Matter

Ejemplos de uso, según el archivo donde se agrega.

```bash
# En _index.md de una sección
---
title: "LFCS Certification Guide"
linkTitle: "LFCS"              # Nombre corto para el sidebar
description: "Linux Foundation Certified System Administrator"
weight: 20                     # Orden en el sidebar
cascade:
  type: docs
---

# En una página individual
---
title: "KVM Virtual Machine Setup"
linkTitle: "KVM Setup"         # Nombre en sidebar (opcional)
weight: 10                     # Orden dentro de la sección
description: "Complete guide for KVM setup"
toc_hide: false               # Mostrar/ocultar TOC
hide_summary: true            # Ocultar resumen en listas
no_list: false                # Excluir de listados automáticos
---

# Parámetros avanzados para navegación
---
title: "Advanced Networking"
weight: 30
menu:
  docs:                       # Menú personalizado
    parent: "networking"      # Padre personalizado
    weight: 15
draft: false                  # false = visible, true = oculto
---
```

## 3. Personalización del sidebar template

Especificación del archivo `layouts/partials/lfcs/sidebar.html`

```html
<!-- Sidebar personalizado para sección LFCS -->
<div class="td-sidebar__inner">
  
  <!-- Header de la sección LFCS -->
  <div class="td-sidebar-section-header">
    <h5 class="td-sidebar-section-title">
      <i class="fas fa-certificate text-primary"></i>
      LFCS Study Guide
    </h5>
    <div class="progress mb-3" style="height: 5px;">
      <div class="progress-bar" role="progressbar" style="width: {{ .Params.progress | default "0" }}%"></div>
    </div>
  </div>

  <!-- Navegación personalizada por objetivos LFCS -->
  {{ $currentSection := .CurrentSection }}
  {{ $lfcsPages := where (where .Site.Pages "Type" "lfcs") ".IsPage" true }}
  
  <!-- Agrupar por objetivos LFCS -->
  {{ $objectives := slice "Essential Commands" "Operation of Running Systems" "User and Group Management" "Networking" "Service Configuration" "Storage Management" }}
  
  {{ range $objective := $objectives }}
  {{ $objectivePages := where $lfcsPages ".Params.lfcs_domain" $objective }}
  {{ if $objectivePages }}
  <div class="td-sidebar-nav-section">
    <h6 class="td-sidebar-nav-section-title">
      <span class="td-sidebar-nav-icon">
        {{ if eq $objective "Essential Commands" }}<i class="fas fa-terminal"></i>
        {{ else if eq $objective "Operation of Running Systems" }}<i class="fas fa-cogs"></i>
        {{ else if eq $objective "User and Group Management" }}<i class="fas fa-users"></i>
        {{ else if eq $objective "Networking" }}<i class="fas fa-network-wired"></i>
        {{ else if eq $objective "Service Configuration" }}<i class="fas fa-server"></i>
        {{ else if eq $objective "Storage Management" }}<i class="fas fa-hdd"></i>
        {{ end }}
      </span>
      {{ $objective }}
    </h6>
    
    <ul class="td-sidebar-nav-tree">
      {{ range $objectivePages.ByWeight }}
      <li class="td-sidebar-nav-tree-item{{ if eq .RelPermalink $.RelPermalink }} td-sidebar-nav-active-item{{ end }}">
        <a href="{{ .RelPermalink }}" class="td-sidebar-nav-link">
          {{ .LinkTitle | default .Title }}
          {{ with .Params.lfcs_objective }}
          <small class="text-muted d-block">{{ . }}</small>
          {{ end }}
        </a>
      </li>
      {{ end }}
    </ul>
  </div>
  {{ end }}
  {{ end }}

  <!-- Navegación tradicional jerárquica (fallback) -->
  {{ if not $lfcsPages }}
  <nav class="td-sidebar-nav">
    {{ template "section-tree-nav" (dict "sect" .FirstSection "currentpage" .) }}
  </nav>
  {{ end }}

</div>

<!-- Template recursivo para navegación jerárquica -->
{{ define "section-tree-nav" }}
{{ $s := .sect }}
{{ $p := .currentpage }}
{{ if $s.HasChildren }}
<ul class="td-sidebar-nav-tree">
  {{ range $s.Sections.ByWeight }}
  <li class="td-sidebar-nav-tree-item">
    <a href="{{ .RelPermalink }}" class="td-sidebar-nav-link{{ if eq .RelPermalink $p.RelPermalink }} td-sidebar-nav-active-item{{ end }}">
      {{ .LinkTitle | default .Title }}
    </a>
    {{ template "section-tree-nav" (dict "sect" . "currentpage" $p) }}
  </li>
  {{ end }}
  
  <!-- Páginas en esta sección -->
  {{ range where $s.Pages.ByWeight ".IsPage" true }}
  <li class="td-sidebar-nav-tree-item">
    <a href="{{ .RelPermalink }}" class="td-sidebar-nav-link{{ if eq .RelPermalink $p.RelPermalink }} td-sidebar-nav-active-item{{ end }}">
      {{ .LinkTitle | default .Title }}
    </a>
  </li>
  {{ end }}
</ul>
{{ end }}
{{ end }}
```

## 4. Configuración avanzada en hugo.yaml

```yaml
params:
  ui:
    # Configuración del sidebar
    sidebar_menu_compact: true          # Menú compacto
    sidebar_menu_foldable: true         # Secciones plegables
    sidebar_cache_limit: 1000           # Límite de cache
    
    # Configuración específica del sidebar
    sidebar_search_disable: false       # Habilitar búsqueda en sidebar
    breadcrumb_disable: false           # Mostrar breadcrumbs

  # Menús personalizados para el sidebar
  menu:
    docs:
      # Entrada manual al menú
      - name: "Quick Reference"
        url: "/docs/lfcs/quick-ref/"
        weight: 5
        pre: "<i class='fas fa-bookmark'></i>"
      
      - name: "Lab Environment"
        url: "/docs/lfcs/lab-setup/"
        weight: 10
        pre: "<i class='fas fa-flask'></i>"

  # Configuración de navegación por taxonomías
  taxonomy:
    # Usar tags para navegación alternativa
    taxonomyCloud: [tags, lfcs_domain]
    taxonomyCloudTitle: [Tags, "LFCS Domains"]

  # Parámetros específicos para LFCS
  lfcs:
    # Progreso del estudio (0-100)
    study_progress: 45
    
    # Dominios LFCS con pesos
    domains:
      - name: "Essential Commands"
        weight: 25
        icon: "fas fa-terminal"
      - name: "Operation of Running Systems" 
        weight: 20
        icon: "fas fa-cogs"
      - name: "User and Group Management"
        weight: 10
        icon: "fas fa-users"
      - name: "Networking"
        weight: 12
        icon: "fas fa-network-wired"
      - name: "Service Configuration"
        weight: 20
        icon: "fas fa-server"
      - name: "Storage Management"
        weight: 13
        icon: "fas fa-hdd"
```

## 5. Ejemplo práctico de front matter para LFCS

Ejemplo: `content/en/lfcs/essential-commands/file-operations.md`

```markdown
---
title: "File Operations and Permissions"
linkTitle: "File Operations"
description: "Master file operations, permissions, and ownership for LFCS"
weight: 15
date: 2024-01-15

# Clasificación LFCS
type: lfcs
lfcs_domain: "Essential Commands"
lfcs_objective: "1.3 Create, delete, copy, and move files and directories"
lfcs_weight: 25

# Control de navegación
toc_hide: false
hide_summary: false

# Taxonomías para navegación alternativa
tags: ["files", "permissions", "chmod", "chown"]
categories: ["system-administration"]

# Metadatos adicionales
difficulty: "intermediate"
estimated_time: "45 minutes"
lab_required: true

# Progreso del estudio (para barra de progreso)
progress: 75
---

# File Operations and Permissions

Este tema cubre...

## Objetivos de aprendizaje

- [ ] Comprender permisos de archivos
- [ ] Dominar chmod y chown
- [ ] Trabajar con enlaces simbólicos

## Comandos esenciales

```bash
# Permisos básicos
chmod 755 archivo
chown user:group archivo
```

## 6. Navegación con shortcodes personalizados

También se puede crear shortcodes para navegación especializada. A continuación se muestra un `layouts/shortcodes/lfcs-nav.html`, este es:

```html
<!-- Shortcode para navegación LFCS: \{\{ < lfcs-nav domain="Essential Commands" >}} -->
{{ $domain := .Get "domain" }}
{{ $lfcsPages := where (where .Page.Site.Pages "Type" "lfcs") ".Params.lfcs_domain" $domain }}

<div class="alert alert-info">
  <h5><i class="fas fa-list-ul"></i> {{ $domain }} - Topics</h5>
  <div class="row">
    {{ range $lfcsPages.ByWeight }}
    <div class="col-md-6 mb-2">
      <a href="{{ .RelPermalink }}" class="btn btn-outline-primary btn-sm btn-block text-left">
        <strong>{{ .LinkTitle | default .Title }}</strong>
        {{ with .Params.lfcs_objective }}
        <br><small class="text-muted">{{ . }}</small>
        {{ end }}
      </a>
    </div>
    {{ end }}
  </div>
</div>
```

## Estrategias de navegación para LFCS

### Opción 1: Por estructura jerárquica (tradicional)

```bash
docs/lfcs/
├── essential-commands/
├── system-operations/
└── user-management/
```

### Opción 2: Por dominios de examen

```bash
docs/lfcs/
├── domain-1-essential/
├── domain-2-operations/
└── domain-3-users/
```

### Opción 3: Por tipo de contenido

```bash
docs/lfcs/
├── tutorials/
├── commands-reference/
├── labs/
└── exam-tips/
```

## Debugging del sidebar

Para debuggear problemas de navegación:

```bash
# Ver la estructura de páginas
hugo server --verbose

# Inspeccionar variables en template
{{ printf "%#v" .CurrentSection }}
{{ printf "%#v" .Pages }}
```
