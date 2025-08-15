# 🔄 Migración desde otro SSG a MkDocs

Este documento recoge los pasos y ajustes para migrar desde un generador de sitios estáticos (SSG) como **Jekyll**, **Hugo**, **mdBook**, etc., hacia **MkDocs**.

---

## 1. Limpiar el repositorio

Si vienes de otro SSG:
- Elimina carpetas de salida como `_site/` (Jekyll) o `public/` (Hugo).
- Asegúrate de agregar un `.gitignore` adaptado a MkDocs, algo tal como:

```gitignore
site/
__pycache__/
*.py[cod]
.venv/
```

---

## 2. Mantener los Markdown

MkDocs soporta Markdown puro (`.md`) y, opcionalmente, front matter YAML.
En la mayoría de los casos, basta con mover tus .md a la carpeta `docs/`.

Ejemplo:

```
docs/
  index.md
  yoga/
    asanas.md
    respiracion.md

```

## 3. Crear mkdocs.yml

Ejemplo mínimo:

```
site_name: "Mi Documentación LFCS"
theme:
  name: material
nav:
  - Inicio: index.md
  - Linux:
      - Comandos básicos: linux/comandos-basicos.md
      - Gestión de usuarios: linux/gestion-usuarios.md
```

## 4. Probar en local
```bash
mkdocs serve
```

Abrir en `http://127.0.0.1:8000/`.

## 5. Desplegar

Con GitHub Pages:
```
mkdocs gh-deploy
```

Esto crea (o actualiza) la rama `gh-pages` y la usa como fuente de Pages.

## 💡 Notas de compatibilidad (respecto a otros SSG)

- _Front matter:_ MkDocs lo ignora salvo que uses plugins (ej. `mkdocs-meta`).
- _Taxonomías/tags:_ No son nativas; se añaden con plugins como `mkdocs-tags-plugin`.
- _Extensiones Markdown:_ MkDocs usa Python-Markdown; muchas funciones de Hugo/Jekyll requieren ajustes.
