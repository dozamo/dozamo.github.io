---
title: "Una Plantilla para Generar un Artículo Técnico Individual"
linkTitle: "Prompt 001 - TechDoc Individual"
description: "Una plantilla de prompt reutilizable y efectiva para generar un artículo técnico único y de alta calidad usando modelos de lenguaje de IA."
weight: 10
categories: ["ia", "tecnología"]
tags: ["ingeniería-prompts", "plantilla-individual", "documentación-técnica", "llm", "hugo", "markdown", "lfcs", "automatización-contenido"]
---

Tener una plantilla de prompt bien diseñada es un superpoder cuando trabajas con Modelos de Lenguaje Grande (LLM) para generar contenido técnico. Asegura consistencia, refuerza estándares de calidad, y acelera dramáticamente el proceso de creación de contenido.

Este artículo documenta el "prompt maestro" fundamental para generar un artículo técnico completo y autónomo. Esta es la plantilla ideal para crear documentación en profundidad sobre un tema específico.

## La Plantilla de Prompt Maestro

Aquí está la plantilla completa. Está diseñada para ser alimentada directamente a un modelo de IA para producir un artículo técnico estructurado, enfocado y correctamente formateado.

```markdown
Actúa como un experto en Linux y DevOps con certificación LFCS de la Linux Foundation, especializado en crear contenido técnico para certificaciones. Tu tarea es generar un artículo de documentación en formato Markdown estricto, optimizado para Hugo+Docsy.

El artículo debe cubrir el siguiente tema: [Insertar Comando o Tema Tecnológico Aquí]

Requisitos del Contenido:
1.  **Formato:** Markdown estricto. Debe incluir una sección de front matter de Hugo para el theme Docsy al inicio con 'title', 'description', 'weight', 'tags', y 'categories'.
1.1 **categories:** Considerando que las cinco dominios y competencias de la LCFS son: Implementación de Operaciones; Networking; Almacenamiento; Comandos Esenciales y  Usuarios y Grupos, asignales la que sén el tema corresponda, si ademas consideras que puede pertenecer a otra categoría también la agregas.
1.2 **tags:** Asigna al menos un valor para este atributo, que sea relacionado al contexto de la tecnología IT relacionada.
2.  **Enfoque:** El contenido debe ser conciso, práctico, y enfocarse exclusivamente en los aspectos más relevantes para un examen de certificación como el LFCS (Linux Foundation Certified Sysadmin). Evita detalles excesivamente esotéricos.
3.  **Estructura:** Organiza el artículo con encabezados claros. Explica la sintaxis básica y luego las sub-operaciones más importantes con ejemplos.
4.  **Ejemplos:** Incluye ejemplos de código claros y breves dentro de bloques de código bash (```bash).
5.  **Idioma:** La salida final debe estar en [Idioma Deseado].

Asegúrate de que toda la salida final esté envuelta en un solo bloque de código delimitado (```markdown) para evitar problemas de formateo al copiar y pegar.
```

## Deconstruyendo el Prompt: Por Qué Funciona

Este prompt es efectivo porque deja muy poco al azar. Cada instrucción sirve un propósito específico.

{{% alert title="1. Estableciendo la Persona: 'Actúa como...'" color="success" %}}
Esta es la parte más crítica. Al decirle a la IA "Actúa como un experto en Linux y DevOps...", estamos estableciendo el **contexto, tono, y base de conocimiento**. El modelo adoptará una voz profesional y técnica y extraerá de sus datos de entrenamiento relacionados con ese dominio específico.
{{% /alert %}}

{{% alert title="2. Definiendo la Tarea Principal: 'Tu tarea es generar...'" color="success" %}}
Esto establece claramente el entregable final: un artículo de documentación. Inmediatamente define el alcance de la salida, previniendo una respuesta conversacional o una simple lista de hechos.
{{% /alert %}}

{{% alert title="3. Estableciendo Restricciones Claras: 'Requisitos del Contenido'" color="success" %}}
Esta sección actúa como un conjunto de "barandillas" que guían a la IA para producir exactamente la salida necesaria.

- **Formato**: Especificar "Markdown estricto" y los campos exactos del "front matter" es esencial para la compatibilidad con Hugo.
- **Enfoque**: Definir la audiencia ("para un examen de certificación") y el alcance ("evita detalles excesivamente esotéricos") previene que la IA genere un artículo excesivamente largo o irrelevante.
- **Estructura**: Solicitar encabezados claros y un flujo lógico asegura que el artículo esté bien organizado y sea fácil de leer para un humano.
{{% /alert %}}

{{% alert title="4. La Instrucción Final 'Envolvente': 'Asegúrate de que...'" color="success" %}}
Esta es una meta-instrucción para interfaces de chat. Le pide al modelo que envuelva su respuesta en un bloque de código, asegurando que el texto crudo pueda copiarse perfectamente, preservando todo el formateo.
{{% /alert %}}

## Conclusión

Al usar un prompt detallado y estructurado como este, pasas de "pedirle" información a la IA a "instruirla" para realizar una tarea específica y repetible. Esta plantilla puede adaptarse fácilmente para diferentes temas.

> **Siguiente Paso:** Para generar múltiples artículos en un solo lote, ve la evolución de esta plantilla en [Prompt 002 - Plantilla TechDoc temas múltiples]({{< relref "prompt-template-multiples-topic-individual.md" >}}).
