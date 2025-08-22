---
title: "Plantilla de Pipeline para Generar Múltiples Artículos"
linkTitle: "Prompt 002 - Plantilla TechDoc temas múltiples"
description: "Una plantilla de prompt avanzada que instruye a una IA para actuar como un pipeline, generando múltiples artículos Markdown separados a partir de una lista en una sola ejecución."
weight: 20
categories: ["ia", "tecnología"]
tags: ["ingeniería-prompts", "plantilla-múltiple", "pipeline-contenido", "automatización", "batch-generation", "documentación-técnica", "llm", "escalabilidad"]
---

Mientras que el [Prompt 001]({{< relref "prompt-template-for-tech-docs.md" >}}) es perfecto para un artículo a la vez, podemos escalar el proceso para generar contenido en lotes. Esto es increíblemente eficiente cuando necesitas crear múltiples páginas de documentación relacionadas.

Este artículo documenta un prompt de "pipeline" avanzado. El objetivo es dar a la IA una lista de temas y hacer que genere un artículo Markdown completo e individual para **cada** tema, todo dentro de una única respuesta.

## El Prompt de Pipeline Maestro

Esta plantilla le da a la IA una lista de temas y una sub-plantilla dinámica para aplicar a cada uno.

```markdown
Actúa como un pipeline de generación de contenido automatizado. Eres un experto en [ÁREA_DE_EXPERTISE], especializado en crear documentación técnica para la certificación [NOMBRE_DE_LA_CERTIFICACIÓN_O_CONTEXTO].

Tu tarea es tomar la siguiente "Lista de Temas" y, para cada tema, generar un artículo de documentación completo e individual en formato Markdown estricto, optimizado para Hugo+Docsy.

### Requisitos Generales:
1.  **Formato:** Markdown estricto para cada artículo.
2.  **Enfoque:** El contenido debe ser conciso, práctico y relevante para un profesional en el campo.
3.  **Ejemplos:** Incluye ejemplos de código claros en bloques ```bash.
4.  **Idioma:** La salida final debe estar en [IDIOMA_DESEADO].

### Lista de Temas a Procesar:
```
[LISTA_DE_TEMAS_SEPARADOS_POR_LÍNEA]
```

### Plantilla de Artículo (A usar para CADA tema):
Debes usar esta plantilla como base para cada artículo. Rellena los marcadores de posición dinámicamente.
"""
---
title: "[TÍTULO_GENERADO_A_PARTIR_DEL_TEMA]"
description: "[DESCRIPCIÓN_DE_1-2_FRASES_GENERADA_PARA_EL_TEMA]"
weight: [NÚMERO_INCREMENTAL_EMPEZANDO_EN_10_Y_SUMANDO_10_POR_CADA_ARTÍCULO]
categories: [[LISTA_DE_CATEGORÍAS_VÁLIDAS]]
tags: [[LISTA_DE_3_TAGS_RELEVANTES_GENERADOS_PARA_EL_TEMA]]
---

(Aquí, genera un artículo conciso pero completo sobre el tema actual, siguiendo todos los requisitos generales. La estructura debe incluir una introducción, sintaxis/uso principal, y 2-3 ejemplos prácticos con encabezados claros.)
"""

### Instrucciones del Pipeline:
1.  Itera sobre cada ítem en la "Lista de Temas".
2.  Para cada tema, aplica la "Plantilla de Artículo". Genera dinámicamente el contenido para los marcadores de posición (`title`, `description`, `weight`, `tags`).
3.  Asegúrate de que cada artículo generado sea autónomo y completo.
4.  Si un tema en la lista es ambiguo o no tienes suficiente información, omítelo y añade una nota al final de tu respuesta en la sección de "Notas".
5.  Separa cada artículo completo en tu salida con un separador claro y único en una nueva línea:
    `<!-- ARTICLE_SEPARATOR: [TEMA_ORIGINAL_DE_LA_LISTA] -->`
6.  Envuelve TODA la salida, incluyendo todos los artículos y separadores, en un único bloque de código markdown (```markdown).
```

## Deconstruyendo el Prompt de Pipeline

1.  **Persona de Pipeline:** `Actúa como un pipeline...` cambia el marco mental de la IA de un "escritor" a un "procesador automatizado", ideal para tareas repetitivas.
2.  **Entradas Separadas:** El prompt distingue claramente entre los **Datos** (`Lista de Temas`) y la **Lógica** (`Plantilla de Artículo` e `Instrucciones`).
3.  **Plantilla Dinámica:** Instruir a la IA para que genere `description` o `tags` dinámicamente produce resultados mucho más relevantes que una plantilla estática. Pedir un `weight` incremental asegura un ordenamiento por defecto correcto en Hugo.
4.  **Instrucciones de Bucle Explícitas:** Las instrucciones `Itera...` y `Para cada tema...` convierten el prompt en un pseudo-algoritmo que la IA puede seguir de manera fiable.
5.  **Separador Robusto:** Usar un comentario HTML como separador es una práctica recomendada, ya que es invisible en el renderizado final y perfecto para el parsing automático o manual.

## Flujo de Trabajo Sugerido

1.  Rellena los marcadores de posición en la plantilla maestra (`[ÁREA_DE_EXPERTISE]`, `[LISTA_DE_TEMAS]`, etc.).
2.  Envía el prompt completo a la IA.
3.  Recibe la respuesta única que contiene todos los artículos.
4.  Divide la respuesta usando el separador `<!-- ARTICLE_SEPARATOR: ... -->`.
5.  Guarda cada sección en su propio archivo `.md`. Se recomienda un nombre de archivo basado en el tema, por ejemplo, `comando-ip.md`, `comando-nmcli.md`, etc.
