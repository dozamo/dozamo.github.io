---
title: "Refactorizando un Parser de Fechas en Python con Ayuda de IA"
description: "Caso de Estudio: Un diálogo iterativo con un asistente IA para resolver problemas de 'locale' y umbrales de confianza en un script de Document AI."
weight: 1
tags: ["Python", "Inteligencia Artificial", "Debugging", "Document AI", "Hugo"]
resources:
- src: '001_invoice.pdf'
  title: 'Factura de Ejemplo (PDF)'
---

## Introducción: El Problema

Recientemente, mientras desarrollaba una aplicación en Python para procesar facturas con **Google Document AI**, me enfrenté a un desafío común pero persistente:

1.  **Fechas en español**: El parser no reconocía fechas en formato textual como `'5 de Enero del 2030'`.
2.  **Umbrales de confianza**: Ciertos campos con baja confianza, aunque correctos, eran descartados por una configuración demasiado estricta.

Decidí abordar este problema utilizando un asistente de IA como mi compañero de *pair programming*. Este artículo documenta el diálogo iterativo que tuvimos, mostrando cómo un buen "prompt engineering" y un enfoque paso a paso pueden llevar a una solución robusta y elegante.

## El Diálogo Iterativo

El proceso de debugging rara vez es una línea recta. A continuación, se muestra la conversación completa, con mis prompts y las respuestas de la IA, que nos llevaron a la solución final.

### Iteración 1: Diagnóstico y Primera Solución

Mi primer paso fue presentarle a la IA el estado completo del proyecto: la estructura de archivos, el código relevante y la salida de error exacta.

> **Mi Prompt Inicial a la IA:**
>
> En tu rol de validador senior de aplicaciones IT basadas en Python, y con tu experticia en Document AI, tu tarea es validar y refactorizar la siguiente aplicación. Los problemas clave son:
>
> 1.  Formatear `Invoice Date` al formato `DD/MM/AAAA`.
> 2.  Ajustar el umbral de confianza para que `receiver_phone` sea capturado.
>
> <details>
> <summary>Haz clic para ver el contexto completo del proyecto (código y logs)...</summary>
>
> ```bash
> # Estructura del Proyecto
> $ tree
> .
> ├── data
> │   └── new_invoices
> │       └── 001_invoice.pdf
> ├── main.py
> ├── requirements.txt
> └── src
>     └── cli_invoice_processor
>         ├── config.py
>         ├── data_cleaner.py
>         └── ...
> ```
> ```python
> # Fragmentos de código relevantes...
> # main.py, config.py, data_cleaner.py
> ```
> ```bash
> # Salida del Error
> $ python main.py
> ...
> --- Resumen de Datos Clave ---
>    - Receiver Name  : Andrés Piraquive
>    - Receiver Phone : Not found or low confidence
>    - Invoice Date   : Unparseable Date: '5 de Enero del 2030'
>    - Invoice Id     : 0001234
>    - Total Amount   : 31,467
> ...
> ```
> </details>

---

{{% alert title="Respuesta del Asistente IA (Solución 1)" color="info" %}}

¡Excelente! Asumo mi rol como validador senior. El diseño es modular y sólido. Has identificado dos puntos de mejora clave.

#### Análisis de los Problemas

1.  **Fallo en Parseo de Fechas:** La librería `dateutil.parser` opera por defecto en inglés y no reconoce "Enero". La solución es usar el módulo `locale` de Python para establecer temporalmente el contexto a español.
2.  **Umbral de Confianza Estricto:** `receiver_phone` tiene `42.50%` de confianza, por debajo del `70%` por defecto. La solución es añadir un *override* específico para este campo en el diccionario `CONFIDENCE_THRESHOLDS`.

#### Plan de Acción

1.  **Refactorizar `data_cleaner.py`** para usar `locale`.
2.  **Actualizar `config.py`** con el nuevo umbral para `receiver_phone`.

<details>
<summary>Ver los cambios de código propuestos...</summary>

**1. `src/cli_invoice_processor/data_cleaner.py` (Modificado)**
```python
# ... (código modificado con la lógica de locale.setlocale)
```

**2. `src/cli_invoice_processor/config.py` (Modificado)**
```python
# ...
CONFIDENCE_THRESHOLDS: Dict[str, float] = {
    "__default__": 0.70,
    "invoice_id": 0.60,
    "supplier_name": 0.20,
    "receiver_phone": 0.40  # <--- CAMBIO
}
# ...
```
</details>

Con estos cambios, la aplicación debería funcionar como se espera.
{{% /alert %}}

{{% alert title="Nota del autor" color="primary" %}}
La primera respuesta de la IA fue prometedora y lógicamente correcta. Solucionó el problema del umbral de confianza de inmediato y propuso una solución estándar para el manejo de fechas multi-idioma. Sin embargo, como veremos, el entorno de ejecución a menudo guarda sorpresas.
{{% /alert %}}

### Iteración 2 y 3: Refinando la Solución

Apliqué los cambios, pero la fecha seguía fallando. Proporcioné a la IA más contexto sobre mi sistema operativo, lo que la llevó a proponer un "fallback" más robusto. Tras un pequeño error de sintaxis (`AttributeError`) que corregimos rápidamente, llegamos a una versión que no solo traducía los meses, sino que también limpiaba las preposiciones del español (`de`, `del`).

*(Para mantener el artículo conciso, los prompts intermedios se han omitido, pero siguieron el mismo patrón de "este es el nuevo error, aquí está el log").*

### La Solución Final y Consolidada

Después del proceso iterativo, el código final quedó de la siguiente manera, resolviendo ambos problemas de forma robusta e independiente del entorno.

**`src/cli_invoice_processor/config.py` (Cambio final)**
```python
# ...
CONFIDENCE_THRESHOLDS: Dict[str, float] = {
    "__default__": 0.70,
    "invoice_id": 0.60,
    "supplier_name": 0.20,
    "receiver_phone": 0.40  # Umbral más permisivo para el teléfono
}
# ...
```

**`src/cli_invoice_processor/data_cleaner.py` (Versión final y robusta)**
```python
import logging
import locale
from dateutil import parser
from typing import Optional
from datetime import datetime

SPANISH_TO_ENGLISH_MONTHS = {
    'enero': 'january', 'febrero': 'february', 'marzo': 'march',
    'abril': 'april', 'mayo': 'may', 'junio': 'june',
    'julio': 'july', 'agosto': 'august', 'septiembre': 'september',
    'octubre': 'october', 'noviembre': 'november', 'diciembre': 'december'
}

def _parse_with_fallback(date_string: str) -> Optional[datetime]:
    temp_string = date_string.lower()
    for spa, eng in SPANISH_TO_ENGLISH_MONTHS.items():
        if spa in temp_string:
            temp_string = temp_string.replace(spa, eng)
            break
    
    temp_string = temp_string.replace(' de ', ' ').replace(' del ', ' ')
    
    try:
        return parser.parse(temp_string)
    except (parser.ParserError, ValueError):
        return None

def normalize_date(date_string: str) -> Optional[str]:
    # ... (lógica completa con intento de locale y fallback)
    # ...
    if parsed_date:
        return parsed_date.strftime('%d/%m/%Y')
    return None
```

Con estos cambios, la ejecución fue un éxito:

```bash
$ python main.py 
--- Starting Acme Inc. Invoice Processor ---
...
✅ Document processed successfully. High-confidence data extracted.
   --- Resumen de Datos Clave ---
   - Receiver Name  : Andrés Piraquive
   - Receiver Phone : (55) 1234-5678
   - Invoice Date   : 05/01/2030
   - Invoice Id     : 0001234
   - Total Amount   : 31,467

--- Invoice processing finished. ---
```

## Aprendizajes Clave

Este ejercicio me dejó varias lecciones valiosas sobre cómo colaborar eficazmente con una IA para la programación:

*   **El Contexto es Rey:** Proporcionar la estructura del proyecto, el código y los logs exactos desde el principio elimina la ambigüedad y conduce a respuestas de alta calidad.
*   **El Poder de la Iteración:** No esperes la solución perfecta al primer intento. El verdadero valor reside en el diálogo: probar una solución, informar del resultado y refinar el enfoque.
*   **Código Robusto vs. Dependencias del Entorno:** La solución final con un "fallback" manual es superior porque no depende de la configuración `locale` del sistema operativo, haciendo la aplicación más portable.
*   **Sé el Piloto, no el Pasajero:** La IA es una herramienta increíble, pero tú eres el desarrollador. Corregir sus pequeños errores (como el `AttributeError`) es parte del proceso de colaboración.

## Recursos del Proyecto

A continuación, puedes descargar la factura utilizada en este análisis.

- 📄 **[Factura de Ejemplo (001_invoice.pdf)](001_invoice.pdf)**
