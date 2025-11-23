---
order: 90
route: /usage/core-concepts/macros/
---

# Macros (etiquetas de reemplazo)

!!! Note
Esta lista puede estar incompleta o desactualizada. Usa el comando de barra `/help macros` en cualquier chat de SillyTavern para obtener la lista de macros que funcionan en tu instancia.
!!!

Las Macros se pueden usar en la descripción del personaje, notas del autor, información del mundo y muchos otros lugares y se reemplazan con los valores correspondientes al generar una respuesta. Se pueden usar para insertar contenido dinámico en el prompt, como el nombre del usuario, la descripción del personaje o la hora actual. Las Macros se encierran entre llaves dobles, p. ej. `{{user}}` y suelen ser insensibles a mayúsculas. **Tenga en cuenta que el anidamiento de macros no es compatible actualmente.**

Nota: algunas extensiones también pueden agregar macros especiales específicas del contexto que solo funcionan en ciertas áreas (es decir, marcadores de posición especiales para prompts de extensiones). Estos no serán documentados aquí a menos que la macro no esté vinculada a una funcionalidad específica.

## Macros Generales

| Macro | Descripción |
|-------|-------------|
| `{{pipe}}` | Solo para procesamiento de comandos de barra. Se reemplaza con el resultado devuelto del comando anterior. |
| `{{newline}}` | Inserta una nueva línea. |
| `{{trim}}` | Recorta nuevas líneas que rodean esta macro. |
| `{{noop}}` | Sin operación, solo una cadena vacía. |
| `{{user}}` o `<USER>` | Nombre del usuario. |
| `{{charPrompt}}` | Anulación del Prompt Principal del personaje. |
| `{{charJailbreak}}` | Anulación de Instrucciones Post-Historial del personaje. |
| `{{group}}` o `{{charIfNotGroup}}` | Lista separada por comas de nombres de miembros del grupo o nombre del personaje en chats individuales. |
| `{{groupNotMuted}}` | Igual a `{{group}}` pero excluye miembros silenciados. |
| `{{notChar}}` | Lista separada por comas de todos los participantes del chat excepto el hablante actual (`{{char}}`). En chats grupales esto aún incluye personajes silenciados, y cuando no se está generando ningún mensaje, lista cada personaje en el roster. |
| `{{char}}` o `<BOT>` | Nombre del personaje. |
| `{{description}}` | Descripción del personaje. |
| `{{scenario}}` | Escenario del personaje o anulación del escenario del chat (si está configurado). |
| `{{personality}}` | Personalidad del personaje. |
| `{{persona}}` | Descripción de la persona del usuario. |
| `{{mesExamples}}` | Ejemplos de diálogo del personaje (formato instruct). |
| `{{mesExamplesRaw}}`  | Ejemplos de diálogo del personaje (sin alterar y sin dividir). |
| `{{charVersion}}` | El número de versión del personaje. |
| `{{charDepthPrompt}}` | El prompt de profundidad del personaje. |
| `{{model}}` | Nombre del modelo de generación de texto para la API actualmente seleccionada. **¡Puede ser inexacto!** |
| `{{lastMessageId}}` | ID del último mensaje del chat. |
| `{{lastMessage}}` | Texto del último mensaje del chat. |
| `{{firstIncludedMessageId}}` | El ID del primer mensaje incluido en el contexto. Requiere que la generación se ejecute al menos una vez en la sesión actual. |
| `{{lastCharMessage}}` | Último mensaje de chat enviado por el personaje. |
| `{{lastUserMessage}}` | Último mensaje de chat enviado por el usuario. |
| `{{currentSwipeId}}` | ID basado en 1 del swipe del último mensaje actualmente mostrado. |
| `{{lastSwipeId}}` | Número de swipes en el último mensaje del chat. |
| `{{lastGenerationType}}` | Tipo de la última solicitud de generación en cola. Valores: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue". |
| `{{original}}` | Se puede usar en campos de Anulaciones de Prompt para incluir el prompt predeterminado de la configuración del sistema. Aplicado a APIs de Finalización de Chat e modo Instruct solamente. |
| `{{time}}` | Hora actual del sistema. |
| `{{time_UTC±X}}` | Hora actual en el desplazamiento UTC especificado (zona horaria), p. ej. para UTC+02:00 use `{{time_UTC+2}}`. |
| `{{timeDiff::(time1)::(time2)}}` | La diferencia de tiempo entre time1 y time2. Acepta macros de tiempo y fecha. |
| `{{date}}` | Fecha actual del sistema. |
| `{{input}}` | Contenido de la barra de entrada del usuario. |
| `{{weekday}}` | El día de la semana actual. |
| `{{isotime}}` | La hora ISO actual (reloj de 24 horas). |
| `{{isodate}}` | La fecha ISO actual (YYYY-MM-DD). |
| `{{datetimeformat ...}}` | Fecha/hora actual en formato especificado (p. ej. `{{datetimeformat DD.MM.YYYY HH:mm}}`). |
| `{{idle_duration}}` | Inserta una cadena humanizada del rango de tiempo desde que se envió el último mensaje del usuario (ejemplos: 4 horas, 1 día). |
| `{{random:(args)}}` | Devuelve un elemento aleatorio de la lista (p. ej. `{{random:1,2,3,4}}` devolverá 1 de los 4 números al azar). |
| `{{random::arg1::arg2}}` | Sintaxis alternativa para random que admite comas en sus argumentos. |
| `{{pick::(args)}}` | Alternativa a random, pero el argumento seleccionado es estable en evaluaciones posteriores en el chat actual si la cadena de origen permanece sin cambios. |
| `{{roll:(formula)}}` | Genera un valor aleatorio usando la sintaxis de dados D&D: XdY+Z (p. ej. `{{roll:d6}}` genera un valor 1-6). |
| `{{bias "text here"}}` | Establece un sesgo conductual para la IA hasta la siguiente entrada del usuario. Se requieren comillas alrededor del texto. |
| `{{// (note)}}` | Permite dejar una nota que será reemplazada con contenido en blanco. No visible para la IA. |
| `{{banned "text here"}}` | Agrega dinámicamente texto entrecomillado a secuencias de palabras prohibidas para backend de Text Generation WebUI. No hace nada para otros backends. Se requieren comillas. |
| `{{reverse:(content)}}` | Invierte el contenido de la macro. |
| `{{outlet::(name)}}` | Reemplazado con el contenido de la [salida de información del mundo](/Usage/worldinfo.md#outlet-name) nombrada, contendrá entradas activadas separadas por saltos de línea. |

## Macros de Modo Instruct y Plantilla de Contexto

(habilitado en la configuración de Formato Avanzado)

| Macro | Descripción |
|-------|-------------|
| `{{exampleSeparator}}` | Separador de diálogos de ejemplo de plantilla de contexto. |
| `{{chatStart}}` | Línea de inicio del chat de la plantilla de contexto. |
| `{{instructSystemPrompt}}` | Prompt del sistema Instruct. |
| `{{instructSystemPromptPrefix}}` | Secuencia de prefijo de prompt del sistema. |
| `{{instructSystemPromptSuffix}}` | Secuencia de sufijo de prompt del sistema. |
| `{{instructUserPrefix}}` | Secuencia de prefijo de mensaje del usuario. |
| `{{instructAssistantPrefix}}` | Secuencia de prefijo de mensaje del asistente. |
| `{{instructSystemPrefix}}` | Secuencia de prefijo de mensaje del sistema. |
| `{{instructUserSuffix}}` | Secuencia de sufijo de mensaje del usuario. |
| `{{instructAssistantSuffix}}` | Secuencia de sufijo de mensaje del asistente. |
| `{{instructSystemSuffix}}` | Secuencia de sufijo de mensaje del sistema. |
| `{{instructFirstAssistantPrefix}}` | Secuencia de primer resultado del asistente. |
| `{{instructLastAssistantPrefix}}` | Secuencia de último resultado del asistente. |
| `{{instructFirstUserPrefix}}` | Secuencia de primera entrada del usuario Instruct. |
| `{{instructLastUserPrefix}}` | Secuencia de última entrada del usuario Instruct. |
| `{{instructSystemInstructionPrefix}}` | Secuencia de prefijo de instrucción del sistema. |
| `{{instructUserFiller}}` | Texto del mensaje de relleno del usuario. |
| `{{instructStop}}` | Secuencia de parada Instruct. |
| `{{maxPrompt}}` | Tamaño máximo del prompt en tokens (longitud del contexto reducida por longitud de respuesta). |
| `{{systemPrompt}}` | Contenido del prompt del sistema, incluida la anulación del prompt del personaje si se permite y está disponible. |
| `{{defaultSystemPrompt}}` | Contenido del prompt del sistema (excluyendo la anulación del prompt del personaje). |

## Macros de Variables de Chat

- Variables locales = únicas para el chat actual
- Variables globales = funciona en cualquier chat para cualquier personaje

| Macro | Descripción |
|-------|-------------|
| `{{getvar::name}}` | Reemplazado con el valor de la variable local "name". |
| `{{setvar::name::value}}` | Reemplazado con cadena vacía, establece la variable local "name" a "value". Permite valores vacíos. |
| `{{addvar::name::increment}}` | Reemplazado con cadena vacía, agrega un valor numérico de "increment" a la variable local "name". |
| `{{incvar::name}}` | Reemplazado con el resultado de incrementar el valor de la variable "name" en 1. |
| `{{decvar::name}}` | Reemplazado con el resultado de decrementar el valor de la variable "name" en 1. |
| `{{getglobalvar::name}}` | Reemplazado con el valor de la variable global "name". |
| `{{setglobalvar::name::value}}` | Reemplazado con cadena vacía, establece la variable global "name" a "value". Permite valores vacíos. |
| `{{addglobalvar::name::value}}` | Reemplazado con cadena vacía, agrega un valor numérico de "increment" a la variable global "name". |
| `{{incglobalvar::name}}` | Reemplazado con el resultado de incrementar el valor de la variable global "name" por 1. |
| `{{decglobalvar::name}}` | Reemplazado con el resultado de decrementar el valor de la variable global "name" en 1. |
| `{{var::name}}` | Reemplazado con el valor de la variable con ámbito "name" (solo STscript). |
| `{{var::name::index}}` | Reemplazado con el valor en el índice de la variable con ámbito "name" (para arrays/objetos en STscript). |

## Macros Específicas de Extensión

Agregadas por extensiones y solo funcionan bajo ciertas condiciones.

| Macro | Descripción |
|-------|-------------|
| `{{summary}}` | Reemplazado con el resumen de la sesión de chat actual (si está disponible). |
| `{{authorsNote}}` | Reemplazado con el contenido de la Nota del Autor. |
| `{{charAuthorsNote}}` | Reemplazado con el contenido de la Nota del Autor del Personaje. |
| `{{defaultAuthorsNote}}` | Reemplazado con el contenido de la Nota del Autor predeterminada. |
| `{{charPrefix}}` | Reemplazado con un prefijo de prompt positivo de Generación de Imagen específico del personaje (si está disponible). |
| `{{charNegativePrefix}}` | Reemplazado con un prefijo de prompt negativo de Generación de Imagen específico del personaje (si está disponible). |
