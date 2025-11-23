---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# Plantilla de Contexto

!!! Se aplica a: Text Completion APIs
Para configuraciones equivalentes en Chat Completion APIs, use [Prompt Manager](prompt-manager.md).
!!!

Normalmente, los modelos de IA requieren que proporciones los datos del personaje de una manera específica. SillyTavern incluye una lista de reglas de conversión predefinidas para diferentes modelos, pero puedes personalizarlas como desees.

Edita estas configuraciones en el panel "[Formato Avanzado](advancedformatting.md)".

## Cadena de Historia

Este campo es una plantilla para el preámbulo del prompt (conocido internamente como cadena de historia). Esta es la forma principal de agregar la información definida en [Tarjetas de Personaje](/Usage/Characters/index.md) para finalización de texto e instrucción de modelos.

La plantilla admite sintaxis Handlebars, inyecciones de texto personalizado o formato, y cualquier otra [macro](/Usage/Characters/macros.md). Consulta la referencia del idioma aquí: <https://handlebarsjs.com/guide/>

Proporcionamos los siguientes parámetros al evaluador Handlebars (entre llaves dobles):

1. `{{anchorBefore}}`: Prompts configurados para usar la posición "Antes de la Cadena de Historia".
2. `{{anchorAfter}}`: Prompts configurados para usar la posición "Después de la Cadena de Historia".
3. `{{description}}`: La [Descripción](/Usage/Characters/characterdesign.md#character-description) del personaje.
4. `{{scenario}}`: El [Escenario](/Usage/Characters/characterdesign.md#scenario) del personaje.
5. `{{personality}}`: El [Resumen de Personalidad](/Usage/Characters/characterdesign.md#personality-summary) del personaje.
6. `{{system}}`: El [prompt del sistema](advancedformatting.md#system-prompt) O la anulación de [prompt principal](/Usage/Characters/characterdesign.md#prompt-overrides) del personaje (si existe y "Preferir Prompt del Personaje" está habilitado en Configuración de Usuario).
7. `{{persona}}`: La [descripción de la persona seleccionada](/Usage/personas.md#persona-description).
8. `{{char}}`: El nombre del personaje.
9. `{{user}}`: El nombre de la persona seleccionada.
10. `{{wiBefore}}` o `{{loreBefore}}`: Entradas de [World Info](/Usage/worldinfo.md) activadas combinadas con Posición configurada en "Antes de Definiciones de Personaje".
11. `{{wiAfter}}` o `{{loreAfter}}`: Entradas de [World Info](/Usage/worldinfo.md) activadas combinadas con Posición configurada en "Después de Definiciones de Personaje".
12. `{{mesExamples}}`: (Opcional) Los [Diálogos de Ejemplo](/Usage/Characters/characterdesign.md#examples-of-dialogue) del personaje, formateados con instrucciones y un separador.
13. `{{mesExamplesRaw}}`: Los [Diálogos de Ejemplo](/Usage/Characters/characterdesign.md#examples-of-dialogue) del personaje en formato bruto, sin ningún formato.

!!!tip **Importante**
Cuando uses `{{mesExamples}}` en la Cadena de Historia, configura **"Comportamiento de Mensajes de Ejemplo"** en el panel **<i class="fa-solid fa-user-cog"></i> Configuración de Usuario** en **"Nunca incluir ejemplos"** para evitar duplicar mensajes de ejemplo en el prompt.
!!!

Se admite una macro especial `{{trim}}` para eliminar cualquier salto de línea que la rodee. Úsala si deseas que una parte del texto no se separe de la línea anterior por un salto de línea (_los espacios **no** se recortan_).

**ADVERTENCIA**: Si falta alguno de los parámetros anteriores en la plantilla de cadena de historia, no se enviarán en absoluto en el prompt.

### Anclajes de Prompt

`{{anchorBefore}}` y `{{anchorAfter}}` son marcadores de posición genéricos para prompts agregados por varias extensiones y características varias en una posición estática elegida, por ejemplo:

* [Nota del Autor](/Usage/Characters/Author's-Note.md)
* [Resúmenes](/extensions/Summarize.md)
* [Vectorización de Chat](/extensions/Chat-vectorization.md) / [Banco de Datos](/Usage/Characters/data-bank.md)
* [Inyecciones STscript](/For_Contributors/st-script.md#prompt-injections)
* [Búsqueda Web](/extensions/WebSearch.md)

### Posición de la Cadena de Historia

De forma predeterminada, la cadena de historia renderizada (con todos los marcadores de posición reemplazados) se coloca al principio del prompt, seguida de mensajes de ejemplo e historial de chat visible.

Alternativamente, puedes moverla a una posición dinámica eligiendo la opción "En el chat @ Profundidad", que coloca la cadena de historia a una profundidad específica en el contexto del chat.

!!!warning **Atención**
Si la plantilla contiene elementos de prompt estáticos (prefijos o sufijos específicos del modelo) para envolver la cadena de historia, usar la posición "En el Chat @ Profundidad" hará que se envuelva incorrectamente con secuencias duplicadas, lo que puede llevar a resultados inesperados.

En este caso, puedes solucionar el problema de una de las siguientes maneras:

1. **Plantillas integradas**: Restablece las plantillas a sus valores predeterminados siguiendo los pasos descritos en [Formato Avanzado](/Usage/Prompts/advancedformatting.md#resetting-templates).
2. **Plantillas personalizadas**: Mueve los elementos estáticos de la plantilla de cadena de historia a [Secuencias de Cadena de Historia](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
!!!

### Envolvimiento de Cadena de Historia

!!!
Esta sección solo se aplica cuando **Modo Instrucciones** está ACTIVADO.
!!!

* **Posición predeterminada**: La Cadena de Historia renderizada se envolverá usando las secuencias definidas en [Secuencias de Cadena de Historia](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
* **Posición en el chat @ Profundidad**: La Cadena de Historia renderizada se envolverá usando las secuencias definidas en [Secuencias de Mensajes de Chat](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) para un rol elegido (predeterminado: Sistema).

## Separador de Ejemplo

Se utiliza como encabezado de bloque y separador entre bloques de diálogos de ejemplo. Cualquier instancia de etiquetas `<START>` en los diálogos de ejemplo será reemplazada por el contenido de este campo.

## Inicio de Chat

Se inserta como separador después de la cadena de historia renderizada y después de los bloques de diálogos de ejemplo, pero antes del primer mensaje en contexto.

## Separadores como Cadenas de Parada

Agrega "Separador de Ejemplo" e "Inicio de Chat" a la lista de cadenas de parada.

Útil si el modelo tiende a alucinar o filtrar bloques completos de diálogos de ejemplo precedidos por el separador.

## Nombres como Cadenas de Parada

Agrega los nombres del Personaje y la Persona del Usuario a la lista de cadenas de parada.

Se recomienda mantenerlo activado para evitar la suplantación de identidad del modelo.

## Siempre agregar el nombre del personaje al prompt

!!!info
Esta configuración no tiene efecto cuando el Modo Instrucciones está ACTIVADO. El comportamiento del nombre se define en su lugar por la opción [Incluir Nombres](/Usage/Prompts/instructmode.md#include-names) seleccionada.
!!!

Añade el nombre del personaje al prompt para forzar al modelo a completar el mensaje como el personaje:

```txt
** OTRO CONTEXTO AQUÍ **
Personaje:
```
