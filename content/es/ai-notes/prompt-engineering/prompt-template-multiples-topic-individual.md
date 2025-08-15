---
title: "Una Plantilla para Generar Múltiples Artículos Individuales"
linkTitle: "Prompt 002 - Plantilla TechDoc temas múltiples"
description: "Una plantilla de prompt avanzada que instruye a una IA para actuar como un pipeline de contenido, generando múltiples artículos separados a partir de una lista de temas en una sola ejecución."
weight: 120
categories: ["ia", "tecnología"]
tags: ["ingeniería-prompts", "plantilla-múltiple", "pipeline-contenido", "automatización", "batch-generation", "documentación-técnica", "llm", "escalabilidad"]
---

Mientras que [Prompt 001 - TechDoc Individual]({{< relref "prompt-template-for-tech-docs.md" >}}) es perfecto para crear un artículo a la vez, podemos evolucionar el concepto para generar contenido en lotes. Esto es increíblemente eficiente cuando necesitas crear varias páginas de documentación relacionadas pero separadas.

Este artículo documenta un prompt de "pipeline" avanzado. El objetivo es proporcionar a la IA una lista de temas y hacer que genere un artículo Markdown completo e individual para **cada** tema en esa lista, todo dentro de una sola respuesta.

## El Prompt de Pipeline Maestro

Aquí está la plantilla completa. Funciona dándole a la IA una lista de temas y una sub-plantilla para usar en cada uno.

```markdown
Actúa como un pipeline de generación de contenido. Tu tarea es tomar la siguiente lista de temas y, para cada tema, generar un artículo de documentación completo e individual en formato Markdown estricto.

**Lista de Temas:**
- crontab
- at
- telinit
- systemctl resource limits

**Plantilla de Artículo a usar para CADA tema:**
"""
---
title: "Gestionando [TÍTULO_DEL_TEMA_AQUÍ]"
description: "Una guía práctica para usar el comando [TÍTULO_DEL_TEMA_AQUÍ] para programación de tareas y gestión del sistema."
weight: [UN_NÚMERO_ÚNICO_AQUÍ]
---

(Genera un artículo conciso pero completo sobre [TÍTULO_DEL_TEMA_AQUÍ] aquí, siguiendo la estructura y enfoque de una guía de certificación técnica. Incluye sintaxis, casos de uso comunes, y ejemplos claros.)
"""

**Instrucciones:**
1.  Itera a través de cada elemento en la "Lista de Temas".
2.  Para cada tema, usa la "Plantilla de Artículo" para generar un artículo completo. Reemplaza los marcadores de posición como `[TÍTULO_DEL_TEMA_AQUÍ]`.
3.  Asegúrate de que cada artículo generado esté completo y se sostenga por sí mismo.
4.  Separa cada artículo completo en tu salida con una línea separadora clara, como esta:
    `--- FIN DEL ARTÍCULO ---`
```

## Deconstruyendo el Prompt: Por Qué Funciona

Este prompt es más complejo porque define un flujo de trabajo programático para la IA.

{{% alert title="1. Estableciendo la Persona: 'Actúa como un pipeline de generación de contenido'" color="info" %}}
Hemos cambiado la persona de un simple "experto" a un "pipeline". Esto enmarca la tarea como un proceso repetible y automatizado, que es exactamente lo que queremos.
{{% /alert %}}

{{% alert title="2. Proporcionando Datos Claros y una Plantilla" color="info" %}}
En lugar de un solo tema, proporcionamos dos entradas distintas:
- Una **Lista de Temas**: Los datos que el pipeline procesará.
- Una **Plantilla de Artículo**: El "molde" que se usará para cada pieza de datos. Esto nos da un control increíble sobre la estructura final de cada artículo.
{{% /alert %}}

{{% alert title="3. Instrucciones de Bucle Explícitas" color="info" %}}
El núcleo del prompt reside en las instrucciones explícitas: "Itera a través de cada elemento...", "Para cada tema, usa la 'Plantilla de Artículo'...". Esto elimina toda ambigüedad y le dice a la IA que realice un bucle, que es un concepto que entiende bien.
{{% /alert %}}

{{% alert title="4. Definiendo el Formato de Salida" color="info" %}}
Solicitar un separador claro (`--- FIN DEL ARTÍCULO ---`) es crucial. Hace que la respuesta única y larga de la IA sea fácil de analizar para un humano. Simplemente puedes copiar y pegar cada sección en su propio archivo `.md`.
{{% /alert %}}

## Flujo de Trabajo

El flujo de trabajo previsto es:

1.  Envía este prompt completo a la IA.
2.  Recibe la respuesta única conteniendo todos los artículos generados.
3.  Copia el contenido para el artículo de "crontab" y guárdalo como `crontab.md`.
4.  Copia el contenido para el artículo de "at" y guárdalo como `at.md`.
5.  Y así sucesivamente para todos los temas.