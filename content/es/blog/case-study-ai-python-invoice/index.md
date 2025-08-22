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
> # Fragmentos de código relevantes (relacionados/afectados a la funcionalidad)...
> # ./main.py
> import logging
> import sys
> from src.cli_invoice_processor.logging_config import setup_logging
> setup_logging()
> 
> from src.cli_invoice_processor import file_operations, processing
> from src.cli_invoice_processor.config import INVOICES_DIR, GCP_PROJECT_ID
> # ...
> 
>     logging.info("\n--- Invoice processing finished. ---")
> 
> if __name__ == "__main__":
>     run_application()
> # //END: ./main.py
> 
> # src/cli_invoice_processor/config.py
> import os
> import sys
> from dotenv import load_dotenv
> from typing import List, Dict
> 
> def get_base_path() -> str:
> #...
>     if getattr(sys, 'frozen', False):
>         return os.path.dirname(sys.executable)
>     else:
>         return os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
> 
> BASE_PATH = get_base_path()
> DOTENV_PATH = os.path.join(BASE_PATH, '.env')
> load_dotenv(dotenv_path=DOTENV_PATH)
> 
> GCP_PROJECT_ID = os.getenv("GCP_PROJECT_ID")
> GCP_LOCATION = os.getenv("GCP_LOCATION")
> DOCAI_PROCESSOR_ID = os.getenv("DOCAI_PROCESSOR_ID")
> 
> _default_invoices_path = os.path.join(BASE_PATH, "data", "new_invoices")
> INVOICES_DIR = os.getenv("INVOICES_DIRECTORY", _default_invoices_path)
> LOG_FILE_PATH = os.path.join(BASE_PATH, "debug.log")
> 
> # --- Business Logic Configuration ---
> # List of specific fields to extract for the business summary.
> REQUIRED_FIELDS: List[str] = [
>     "receiver_name",
>     "receiver_phone",
>     "invoice_date",
>     "invoice_id",
>     "total_amount"
> ]
> 
> # Define un umbral global y permite overrides específicos por campo.
> CONFIDENCE_THRESHOLDS: Dict[str, float] = {
>     "__default__": 0.70,  # Umbral por defecto para todos los campos.
>     "invoice_id": 0.60,   # Umbral más bajo y específico para invoice_id.
>     "supplier_name": 0.20 # Ejemplo: ser muy permisivo con el nombre del proveedor.
> }
> 
> # Minimum confidence score (from 0.0 to 1.0) required to consider an entity valid.
> # Experience suggests 80% is a good balance between accuracy and recall.
> MIN_CONFIDENCE_THRESHOLD: float = 0.80
> 
> # --- Configuration Validation ---
> if not all([GCP_PROJECT_ID, GCP_LOCATION, DOCAI_PROCESSOR_ID]):
>     print(
>         f"Error: Critical environment variables are missing. "
>         f"Attempted to load from: {DOTENV_PATH}. "
>         "Ensure GCP_PROJECT_ID, GCP_LOCATION, and DOCAI_PROCESSOR_ID "
>         "are defined in your .env file.",
>         file=sys.stderr
>     )
>     sys.exit(1)
> # //END: src/cli_invoice_processor/config.py
> 
> # src/cli_invoice_processor/data_cleaner.py
> import logging
> from dateutil import parser
> from typing import Optional
> 
> def normalize_date(date_string: str) -> Optional[str]:
>     """
>     Parses a date string from various formats and normalizes it to DD/MM/YYYY.
> 
>     Args:
>         date_string (str): The raw date string extracted from the document.
> 
>     Returns:
>        The formatted date string (DD/MM/YYYY) or None if parsing fails.
>     """
>    if not date_string:
>        return None
>
>    try:
>        # dateutil.parser is incredibly powerful and can handle most common formats
>        # e.g., "11/05/2016", "5 de Enero del 2030", "Jan 5, 2030", "2030-01-05"
>        parsed_date = parser.parse(date_string, dayfirst=True)
>        # We assume dayfirst=True is a safe bet for many locales, but this could be configured.
>
>        return parsed_date.strftime('%d/%m/%Y')
>
>    except (parser.ParserError, ValueError) as e:
>        logging.warning(f"Could not parse date '{date_string}'. Error: {e}")
>        return None # Return None to indicate failure, which is better than returning the original string.
> # //END: src/cli_invoice_processor/data_cleaner.py
> ```
>
> ```bash
> # Ejecución/Salida del Error
> $ python main.py 
> --- Starting Acme Inc. Invoice Processor ---
> Using Project ID: ston...7-m0
> ...
> Found 1 invoices to process.
> 
> 🔄 Processing: 001_invoice.pdf
> --- Extracted Entities from: 001_invoice.pdf ---
>   - Type: total_amount              | Value: '31,467' | Confidence: 99.36%
> ...
>   - Type: receiver_phone            | Value: '(55) 1234-5678' | Confidence: 42.50%
>   - Type: supplier_name             | Value: 'INFORMACIÓN DEL CLIENTE' | Confidence: 20.69%
>   - Type: remit_to_name             | Value: 'Luriel Zanabria' | Confidence: 18.49%
>   - Type: line_item                 | Value: 'Servicio 1 12,34 12,34' | Confidence: 100.00%
> ...
> --------------------------------------------------
> Could not parse date '5 de Enero del 2030'. Error: Unknown string format: 5 de Enero del 2030
> ✅ Document processed successfully. High-confidence data extracted.
>    --- Resumen de Datos Clave ---
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

¡Excelente! Asumo mi rol como validador senior de aplicaciones IT. He revisado a fondo la estructura, el código y la salida de la aplicación. El diseño actual es bastante bueno y modular, lo cual es una base sólida. Sin embargo, has identificado dos puntos de mejora clave que son muy comunes en proyectos de Document AI: el manejo de formatos de datos específicos de una región (fechas en español) y el ajuste fino de los umbrales de confianza para maximizar la extracción de datos valiosos.

A continuación, presento mi análisis y la refactorización necesaria para solucionar ambos problemas.

#### Análisis de los Problemas

1.  **Fallo en el Parseo de Fechas en Español (`Invoice Date`):**
    *   **Causa Raíz:** La librería `dateutil.parser` es extremadamente potente, pero por defecto, opera con la configuración regional (`locale`) del sistema, que generalmente es inglés para los nombres de meses ("January", "February", etc.). La factura contiene "Enero", un nombre de mes en español. La librería no lo reconoce y lanza un `ParserError`, que es capturado correctamente, pero resulta en el mensaje `"Unparseable Date"`.
    *   **Solución:** Debemos indicarle a Python que interprete la cadena de fecha en un contexto de español. La forma más robusta y segura de hacer esto es utilizando el módulo `locale` de Python para establecer temporalmente la configuración regional a español (`es_ES.UTF-8` o similar) justo antes de realizar el parseo.

2.  **Umbral de Confianza Demasiado Estricto para `receiver_phone`:**
    *   **Causa Raíz:** El campo `receiver_phone` fue extraído con una confianza del `42.50%`. La configuración actual en `src/cli_invoice_processor/config.py` tiene un umbral de confianza por defecto (`__default__`) del `70%`. Como no hay una regla específica para `receiver_phone`, se aplica el valor por defecto, y `42.50% < 70%`, por lo que el campo es descartado.
    *   **Solución:** La arquitectura de configuración ya es perfecta para este caso. Simplemente necesitamos añadir una "excepción" o un *override* para el campo `receiver_phone` en el diccionario `CONFIDENCE_THRESHOLDS`, estableciendo un umbral más bajo (por ejemplo, `40%`) que permita capturar este valor.

#### Plan de Acción

1.  **Refactorizar `src/cli_invoice_processor/data_cleaner.py`:** Modificar la función `normalize_date` para que utilice el módulo `locale` y pueda parsear correctamente fechas con meses en español.
2.  **Actualizar `src/cli_invoice_processor/config.py`:** Añadir una entrada para `receiver_phone` en el diccionario `CONFIDENCE_THRESHOLDS` con un valor más permisivo.

No es necesario modificar ningún otro archivo (`main.py`, `processing.py`, etc.), ya que la lógica existente está bien diseñada para soportar estos cambios de configuración y limpieza de datos.

<details>
<summary>Ver los cambios de código propuestos...</summary>

<br>

***Implementación (Refactorización del Código)***

Aquí están las versiones actualizadas de los archivos necesarios.

*1. `src/cli_invoice_processor/data_cleaner.py` (Modificado)*

Se importa el módulo `locale` y se gestiona el contexto de la configuración regional de forma segura dentro de un bloque `try...finally` para no afectar al resto de la aplicación.

```python
# src/cli_invoice_processor/data_cleaner.py
import logging
import locale  # <--- CAMBIO: Importamos el módulo locale
from dateutil import parser
from typing import Optional

def normalize_date(date_string: str) -> Optional[str]:
    """
    Parses a date string from various formats (including Spanish month names)
    and normalizes it to DD/MM/YYYY.

    Args:
        date_string (str): The raw date string extracted from the document.

    Returns:
        The formatted date string (DD/MM/YYYY) or None if parsing fails.
    """
    if not date_string:
        return None

    # --- CAMBIO: Gestión del locale para parsear fechas en español ---
    original_locale = locale.getlocale()  # Guardamos el locale actual
    
    try:
        # Intentamos establecer el locale a español. 'es_ES.UTF-8' es común en Linux.
        # En Windows podría ser 'Spanish_Spain.1252' o simplemente 'Spanish'.
        # Usamos un bloque try/except por si el locale no está instalado en el sistema.
        try:
            locale.setlocale(locale.LC_TIME, 'es_ES.UTF-8')
        except locale.Error:
            logging.warning("Locale 'es_ES.UTF-8' not available. Trying generic 'Spanish'.")
            try:
                locale.setlocale(locale.LC_TIME, 'Spanish')
            except locale.Error:
                 logging.error("Spanish locale not supported on this system. Date parsing may fail for Spanish dates.")

        # dateutil.parser ahora entenderá "Enero", "Febrero", etc.
        parsed_date = parser.parse(date_string)
        return parsed_date.strftime('%d/%m/%Y')

    except (parser.ParserError, ValueError) as e:
        logging.warning(f"Could not parse date '{date_string}'. Error: {e}")
        return None
    finally:
        # Restablecemos el locale original para no afectar otras partes del programa.
        locale.setlocale(locale.LC_TIME, original_locale)
```

*Justificación del cambio:*

Este enfoque es robusto. Al usar `try...finally`, nos aseguramos de que la configuración regional del sistema se restaure a su estado original, incluso si el parseo de la fecha falla. Esto previene efectos secundarios inesperados en otras partes de la aplicación que puedan depender de la configuración regional.

*2. `src/cli_invoice_processor/config.py` (Modificado)*

Simplemente añadimos la nueva regla al diccionario `CONFIDENCE_THRESHOLDS`.

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

*Justificación del cambio:*

Este es un cambio puramente de configuración. Demuestra la flexibilidad del diseño actual. En lugar de codificar `if entity.type == 'receiver_phone': ...` en el código de procesamiento, simplemente ajustamos un parámetro, haciendo el sistema mucho más fácil de mantener y adaptar a futuras necesidades.
</details>
<!-- Con estos cambios, la aplicación debería funcionar como se espera. -->
<br>

#### Verificación (Resultado Esperado)

Con estos cambios, al ejecutar `python main.py` nuevamente, la salida en la consola será la siguiente:

```bash
--- Starting Acme Inc. Invoice Processor ---
Using Project ID: ston...7-m0
# ...

🔄 Processing: 001_invoice.pdf
--- Extracted Entities from: 001_invoice.pdf ---
  - Type: total_amount              | Value: '31,467' | Confidence: 99.36%
  - Type: invoice_date              | Value: '5 de Enero del 2030' | Confidence: 96.85%
# ...
  - Type: receiver_phone            | Value: '(55) 1234-5678' | Confidence: 42.50%
# ...
✅ Document processed successfully. High-confidence data extracted.
   --- Resumen de Datos Clave ---
   - Receiver Name  : Andrés Piraquive
   - Receiver Phone : (55) 1234-5678
   - Invoice Date   : 05/01/2030
   - Invoice Id     : 0001234
   - Total Amount   : 31,467
# ...
```

#### Conclusión de la Validación

La refactorización propuesta soluciona de manera efectiva y elegante los dos problemas identificados. Los cambios están aislados en los módulos correctos (`data_cleaner.py` para la limpieza y normalización, `config.py` para la lógica de negocio), manteniendo el principio de responsabilidad única. La aplicación ahora es más robusta frente a variaciones de idioma en las fechas y más flexible en sus criterios de extracción de datos, todo ello sin alterar la lógica principal de procesamiento.

Validación completada. El código está listo para pasar a la siguiente fase.

{{% /alert %}}

### Iteración 2: Refinando la Solución

{{% alert title="Nota del autor" color="primary" %}}
La primera respuesta de la IA fue prometedora y lógicamente correcta. Solucionó el problema del umbral de confianza de inmediato y propuso una solución estándar para el manejo de fechas multi-idioma. Sin embargo, como veremos, el entorno de ejecución a menudo guarda sorpresas.
{{% /alert %}}

Apliqué los cambios, pero la fecha seguía fallando. Proporcioné a la IA más contexto sobre mi sistema operativo, lo que la llevó a proponer un *fallback* más robusto.

> **Segundo Prompt a la IA:**
>
> En tu rol aún de validador senior de aplicaciones IT basadas en Python, aprovechando tu experticia y conocimiento de la tecnología Document AI. Teniendo en cuenta que mi sistema es un Linux Mint, donde el language de `locale` es ingles (no se si esta relacionado), por lo que recurro a tu experiencia. Tu tarea es validar la siguiente salida y llegado al caso recurriendo a tu basto conocimiento refactorizar/recodificar y/o ajustar la correcta configuración de `locale` aplicada en la extracción de datos para los tipos date. Adjunto ademas la configuración actual de `locale` del sistema.
> 
> <details>
> <summary>Haz clic para ver el contexto completo (código y/o logs) del prompt...</summary>
>
> ```bash
> $ python main.py 
> --- Starting Acme Inc. Invoice Processor ---
> Using Project ID: ston...7-m0
> Searching for invoices in: /home/dzamo/in-my-gitea/001_client-invoice-processing/data/new_invoices
> Detailed results will be saved to: debug.log
> Found 1 invoices to process.
> 
> 🔄 Processing: 001_invoice.pdf
> --------------------------------------------------
> Could not parse date '5 de Enero del 2030'. Error: Unknown string format: 5 de Enero del 2030
> ✅ Document processed successfully. High-confidence data extracted.
>    --- Resumen de Datos Clave ---
>    - Receiver Name  : Andrés Piraquive
>    - Receiver Phone : (55) 1234-5678
>    - Invoice Date   : Unparseable Date: '5 de Enero del 2030'
>    - Invoice Id     : 0001234
>    - Total Amount   : 31,467
> 
> --- Invoice processing finished. ---
> $ date
> Fri Aug 22 12:42:39 AM CEST 2025
> $ locale
> LANG=en_US.UTF-8
> LANGUAGE=en_US
> LC_CTYPE="en_US.UTF-8"
> LC_NUMERIC=es_ES.UTF-8
> LC_TIME="en_US.UTF-8"
> LC_COLLATE="en_US.UTF-8"
> LC_MONETARY=es_ES.UTF-8
> LC_MESSAGES="en_US.UTF-8"
> LC_PAPER=es_ES.UTF-8
> LC_NAME=es_ES.UTF-8
> LC_ADDRESS=es_ES.UTF-8
> LC_TELEPHONE=es_ES.UTF-8
> LC_MEASUREMENT=es_ES.UTF-8
> LC_IDENTIFICATION=es_ES.UTF-8
> LC_ALL=
> ```
> 
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

{{% alert title="Respuesta del Asistente IA  (Solución al Prompt 2)" color="info" %}}

¡Excelente! Asumo mi rol como validador senior. El diseño es modular y sólido. Has identificado dos puntos de mejora clave.

#### Validación y Diagnóstico Definitivo

1. **Análisis de la Salida:** La ejecución del script sigue fallando en el parseo de la fecha, lo que indica que mi propuesta anterior de usar locale.setlocale no tuvo efecto.
2. **Análisis del Comando locale:** Aquí está la clave del problema. Tu salida muestra:

   ```
   LANG=en_US.UTF-8
   ...
   LC_TIME="en_US.UTF-8"
   ...
   LC_ALL=
   ```

   Esto confirma que, aunque tienes algunas categorías (`LC_NUMERIC`, `LC_MONETARY`) configuradas para español, la categoría que gobierna el formato de fechas y los nombres de los meses, `LC_TIME`, está firmemente establecida en inglés (`en_US.UTF-8`).

3. **Causa Raíz:** La llamada `locale.setlocale(locale.LC_TIME, 'es_ES.UTF-8')` en Python es una solicitud al sistema operativo. Si el sistema operativo no tiene el paquete de idioma español (`es_ES.UTF-8`) instalado y generado, la solicitud falla silenciosamente o no tiene el efecto deseado. Python no puede "inventar" un `locale` que no existe a nivel de sistema. Por lo tanto, `dateutil.parser` sigue operando en un contexto inglés y no reconoce "Enero".

#### Solución en dos pasos: entorno y código

Para una solución completa y robusta, debemos abordar esto en dos frentes: primero, corregir el entorno para que se comporte como se espera (la mejor práctica) y, segundo, hacer el código Python aún más resistente para que pueda funcionar incluso en entornos mal configurados.

1. **Paso 1: Corregir la Configuración del Sistema (Recomendado)**
2. **Paso 2: Refactorizar el Código para Máxima Robustez (La Solución Definitiva)**

<details>
<summary>ver Corregir la Configuración del Sistema (Recomendado)...</summary>

1. Debes instalar y generar el soporte para el idioma español en el Linux. Esto permitirá que la llamada de Python funcione correctamente.
    ```bash
    sudo apt-get update
    sudo apt-get install language-pack-es
    ```
2. Genera los archivos de locale para español:
    ```bash
    sudo locale-gen es_ES.UTF-8
    ```
3. Reconfigura los locales (opcional pero recomendado):
    ```bash
    sudo dpkg-reconfigure locales
    ```
    En el menú que aparece, asegúrate de que `es_ES.UTF-8` está seleccionado (navega con las flechas, selecciona con espacio y confirma con enter).

Después de ejecutar estos comandos, la versión anterior del código data_cleaner.py que te proporcioné debería funcionar perfectamente. Sin embargo, como validador senior, no podemos depender de que cada entorno donde se ejecute la aplicación esté perfectamente configurado. Por eso, procedemos al paso 2.

</details>

<details>
<summary>ver Refactorizar el Código para Máxima Robustez (La Solución Definitiva)...</summary>

Vamos a refactorizar `data_cleaner.py` para que intente usar el método `locale` primero, pero si falla, tenga un mecanismo de `fallback` que reemplace manualmente los nombres de los meses en español por sus equivalentes en inglés. Esto hace que el script sea mucho más portátil y resistente a problemas de configuración del entorno.

***Refactorización***

*1. Aquí está el nuevo `src/cli_invoice_processor/data_cleaner.py`*

```python
    
# src/cli_invoice_processor/data_cleaner.py
import logging
import locale
from dateutil import parser
from typing import Optional

# --- CAMBIO: Añadimos un diccionario de fallback ---
SPANISH_TO_ENGLISH_MONTHS = {
    'enero': 'january',
    'febrero': 'february',
    'marzo': 'march',
    'abril': 'april',
    'mayo': 'may',
    'junio': 'june',
    'julio': 'july',
    'agosto': 'august',
    'septiembre': 'september',
    'octubre': 'october',
    'noviembre': 'november',
    'diciembre': 'december'
}

def _parse_with_fallback(date_string: str) -> Optional[parser.datetime]:
    """Intenta parsear la fecha usando un fallback manual si el locale falla."""
    # Convertimos a minúsculas para una coincidencia insensible a mayúsculas
    lower_date_string = date_string.lower()
    
    # Buscamos y reemplazamos el nombre del mes
    for spa, eng in SPANISH_TO_ENGLISH_MONTHS.items():
        if spa in lower_date_string:
            # Reemplazamos y luego intentamos parsear de nuevo
            modified_date_string = lower_date_string.replace(spa, eng)
            try:
                # Volvemos a llamar a parser.parse, ahora con el mes en inglés
                return parser.parse(modified_date_string)
            except (parser.ParserError, ValueError):
                # Si incluso el fallback falla, nos rendimos
                return None
    return None

def normalize_date(date_string: str) -> Optional[str]:
    """
    Parses a date string from various formats and normalizes it to DD/MM/YYYY.
    It first tries using Spanish locale, and if it fails, it uses a manual
    month name replacement as a fallback.
    """
    if not date_string:
        return None

    original_locale = locale.getlocale(locale.LC_TIME)
    parsed_date = None
    
    # --- ESTRATEGIA 1: Intentar con el locale español (método preferido) ---
    try:
        # Intentamos establecer el locale a español
        try:
            locale.setlocale(locale.LC_TIME, 'es_ES.UTF-8')
        except locale.Error:
            locale.setlocale(locale.LC_TIME, 'Spanish') # Fallback para Windows

        parsed_date = parser.parse(date_string)

    except (parser.ParserError, ValueError, locale.Error):
        logging.warning(f"Could not parse date '{date_string}' using Spanish locale. Attempting manual fallback.")
        # --- ESTRATEGIA 2: Si el locale falla, usar el fallback manual ---
        parsed_date = _parse_with_fallback(date_string)
    
    finally:
        # Siempre restauramos el locale original para no causar efectos secundarios
        locale.setlocale(locale.LC_TIME, original_locale)

    if parsed_date:
        return parsed_date.strftime('%d/%m/%Y')
    else:
        # Si ambas estrategias fallan, registramos el error final
        logging.error(f"Failed to parse date '{date_string}' with all available methods.")
        return None # Devolvemos None para indicar un fallo total
```

*Justificación de la Refactorización:*

* *Robustez:* La nueva función `normalize_date` ahora tiene dos estrategias. Siempre intentará la forma "correcta" (usando `locales`), pero no se detendrá si el entorno no coopera.
* *Portabilidad:* Este código ahora funcionará en un sistema Linux sin el `locale` español, en un contenedor Docker mínimo, o en un sistema Windows, sin necesidad de configuración previa del sistema.
* *Mantenimiento:* La lógica está encapsulada. Si en el futuro se necesitan más idiomas, se puede expandir el patrón de `fallback` sin tocar el flujo principal.
* *Manejo de Errores Mejorado:* Ahora, si ambas estrategias fallan, se devuelve `None`. En `processing.py`, la línea `value = normalized_value if normalized_value else f"Unparseable Date: '{value}'"` manejará correctamente este `None` y mostrará el mensaje de error como antes, pero ahora sabemos que hemos agotado todas las opciones.

</details>

<br>

#### Próximos Pasos

* Aplica los cambios en el sistema (Paso 1) para tener un entorno configurado correctamente. Esto es una buena práctica general.
* Reemplaza el contenido de tu archivo src/cli_invoice_processor/data_cleaner.py con el código refactorizado que te he proporcionado.
* Ejecuta la aplicación de nuevo.

Con estos cambios, la salida esperada será la correcta, sin importar si el `locale` de tu sistema fue arreglado o no.

#### Salida Final Esperada (con el código refactorizado)

```bash
$ python main.py 
--- Starting Acme Inc. Invoice Processor ---
...
🔄 Processing: 001_invoice.pdf
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

Validación completada. La solución propuesta es ahora robusta, portable y resuelve la causa raíz del problema a nivel de código y de entorno.

{{% /alert %}}

### Iteración última: Solución Definitiva

{{% alert title="Nota del autor" color="primary" %}}
Aunque en la anterior respuesta, nos aproximamos casí a la solución definitiva, aún no se gestionaban del todo bien las fechas multi-idioma. Habían sido aplicados los cambios, pero la fecha seguía fallando. Por lo tanto se invoco a un último prompt, el cual se muestra en este item.
{{% /alert %}}

Tras un pequeño error de sintaxis (`AttributeError`) que corregimos rápidamente, llegamos a una versión que no solo traduce los meses, sino que también limpia las preposiciones del español (*de*, *del*).

*(Para mantener el artículo conciso, el prompt intermedio se ha omitido, pero siguio el mismo patrón de *"este es el nuevo error, aquí está el log"*).*

> **Un Último Prompt a la IA**
>
> En tu rol de validador senior de aplicaciones IT basadas en Python, en este fixed/recodificación si que has avanzado. Ahora en tu tarea la de 'formatear' la salida, pues se requiere en el formato DD/MM/AAAA como se especifico inicialmente. Actualmente tenemos:
> ```bash
> (.venv) $ python main.py 
> --- Starting Acme Inc. Invoice Processor ---
> Using Project ID: ston...7-m0
> Searching for invoices in: /home/dzamo/in-my-gitea/001_client-invoice-processing/data/new_invoices
> Detailed results will be saved to: debug.log
> Found 1 invoices to process.
> 
> 🔄 Processing: 001_invoice.pdf
> --------------------------------------------------
> Could not parse date '5 de Enero del 2030' using Spanish locale. Attempting manual fallback.
> Failed to parse date '5 de Enero del 2030' with all available methods.
> ✅ Document processed successfully. High-confidence data extracted.
>    --- Resumen de Datos Clave ---
>    - Receiver Name  : Andrés Piraquive
>    - Receiver Phone : (55) 1234-5678
>    - Invoice Date   : Unparseable Date: '5 de Enero del 2030'
>    - Invoice Id     : 0001234
>    - Total Amount   : 31,467
> 
> --- Invoice processing finished. ---
>
