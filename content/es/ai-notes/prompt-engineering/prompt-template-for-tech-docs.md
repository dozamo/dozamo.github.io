---
title: "Plantilla de Prompt para un Artículo Técnico Individual"
linkTitle: "Prompt 001 - TechDoc Individual"
description: "Una plantilla de prompt reutilizable y robusta para generar un artículo técnico único y de alta calidad usando modelos de lenguaje de IA."
weight: 100
categories: ["ia", "tecnología"]
tags: ["ingeniería-prompts", "plantilla-individual", "documentación-técnica", "llm", "hugo", "markdown", "automatización-contenido"]
---

Tener una plantilla de prompt bien diseñada es un superpoder para generar contenido técnico con Modelos de Lenguaje Grandes (LLMs). Asegura consistencia, refuerza estándares de calidad y acelera dramáticamente el proceso de creación.

Este artículo documenta la plantilla maestra para generar un artículo técnico completo y autónomo. Es la base ideal para crear documentación detallada sobre un tema específico.

## La Plantilla Maestra

Esta plantilla está diseñada con marcadores de posición (`[... ]`) para ser universalmente adaptable. Simplemente reemplaza el texto dentro de los corchetes para ajustarlo a tus necesidades.

```markdown
Actúa como un escritor técnico experto en [ÁREA_DE_EXPERTISE], especializado en crear contenido claro y conciso para profesionales y estudiantes que se preparan para la certificación [NOMBRE_DE_LA_CERTIFICACIÓN_O_CONTEXTO].

Tu tarea es generar un artículo de documentación completo en formato Markdown estricto, optimizado para Hugo con el theme Docsy, sobre el siguiente tema: **[TEMA_ESPECÍFICO_AQUÍ]**

### Requisitos del Contenido:
1.  **Formato:** Markdown estricto.
2.  **Front Matter:** Incluye un bloque de front matter YAML completo al inicio del artículo con los siguientes campos:
    - `title`: Un título claro y descriptivo para el artículo.
    - `linkTitle`: Una versión más corta para menús, si es aplicable (ej. "Comando [TEMA]").
    - `description`: Un resumen de 1-2 frases sobre el contenido del artículo.
    - `weight`: Usa el valor `10`.
    - `categories`: Asigna una o más categorías relevantes de la lista: [LISTA_DE_CATEGORÍAS_VÁLIDAS].
    - `tags`: Asigna al menos tres etiquetas relevantes y específicas en minúsculas (ej. `[tag1]`, `[tag2]`).
3.  **Enfoque:** El contenido debe ser práctico y centrarse en los aspectos más importantes para un profesional que utiliza esta tecnología. Evita la historia del tema o detalles excesivamente esotéricos.
4.  **Estructura:** Organiza el artículo con encabezados lógicos (##, ###). Comienza con una breve introducción, explica la sintaxis principal, y luego detalla los casos de uso más comunes con ejemplos.
5.  **Ejemplos de Código:** Incluye ejemplos claros y funcionales dentro de bloques de código con el lenguaje especificado (ej. ```bash).
6.  **Tono y Estilo:** El tono debe ser profesional, autoritativo y didáctico. No incluyas opiniones personales, saludos o texto conversacional. Ve directo al punto.
7.  **Idioma:** La salida final debe estar en [IDIOMA_DESEADO].

### Requisitos de Salida:
Asegúrate de que TODA la salida, incluyendo el front matter, esté envuelta en un único bloque de código markdown (```markdown) para facilitar el copiado y pegado sin errores de formato.
```

## Ejemplo de Uso: Artículo sobre `ip` para LFCS

Para generar un artículo sobre el comando `ip` en el contexto de la certificación LFCS, rellenaríamos la plantilla así:

-   **[ÁREA_DE_EXPERTISE]:** `Redes en Linux y DevOps`
-   **[NOMBRE_DE_LA_CERTIFICACIÓN_O_CONTEXTO]:** `LFCS (Linux Foundation Certified Sysadmin)`
-   **[TEMA_ESPECÍFICO_AQUÍ]:** `El comando ip para la gestión de redes`
-   **[LISTA_DE_CATEGORÍAS_VÁLIDAS]:** `["Networking", "Essential Commands"]`
-   **[IDIOMA_DESEADO]:** `Español`

## Deconstruyendo el Prompt

Este prompt es efectivo porque es un conjunto de instrucciones precisas:

1.  **Establece la Persona:** `Actúa como...` define el contexto, tono y nivel de conocimiento esperado.
2.  **Define la Tarea:** `Tu tarea es generar...` establece el objetivo final de forma inequívoca.
3.  **Establece Restricciones Claras:** La sección de `Requisitos del Contenido` actúa como una guía de estilo detallada, controlando desde la estructura del archivo hasta el tono del contenido.
4.  **Controla el Formato de Salida:** La instrucción final de `Requisitos de Salida` es una meta-instrucción crucial que garantiza que la respuesta del modelo sea 100% utilizable de inmediato.

> **Siguiente Paso:** Para escalar la producción, aprende a generar múltiples artículos en un solo lote con la [Plantilla de Pipeline de Contenido]({{< relref "prompt-template-multiples-topic-individual.md" >}}).
