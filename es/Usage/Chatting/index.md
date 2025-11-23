---
icon: report
order: 170
expanded: false
route: /usage/chatting/
---

# Chat

Cuando estés [conectado a una API](/Usage/API_Connections/index.md), envía mensajes a la IA escribiendo en la barra de chat en la parte inferior de la pantalla. Luego haz clic en <i class="fa-solid fa-paper-plane"></i> **Enviar** o presiona Enter.
![Chat bar](/static/chatbox.png)

La IA responderá con un mensaje que continúa la conversación.

![Chat message](/static/chatmessage.png)

Ahora puedes:

* **Enviar otro mensaje**
* **Deslizar la respuesta**: Haz clic en el botón <i class="fa-solid fa-chevron-right"></i> **Deslizar** en el mensaje para generar una respuesta diferente.
* **Editar el mensaje**: Haz clic en el botón <i class="fa-solid fa-pencil"></i> **Editar** en cualquier mensaje para [editar el contenido del mensaje](#edit-message-content).
* **Acciones de mensaje**: Haz clic en el botón <i class="fa-solid fa-ellipsis"></i> **Acciones de mensaje** en un mensaje para más [opciones de mensaje](#message-actions-panel) como [traducción](../../extensions/Translation.md), generación de imágenes y ramificación de historias.
* **Opciones de chat**: Haz clic en el botón <i class="fa-solid fa-bars"></i> **Opciones** al lado de la barra de chat para más [opciones de chat](#chat-options-panel) como notas del autor y gestión de archivos de chat.

!!! Editar y deslizar
Si deseas haber dicho algo diferente, puedes editar tu mensaje y luego deslizar la respuesta de la IA para obtener una nueva.
!!!

!!! Atajos de teclado
También puedes usar la tecla **flecha derecha** para deslizar, y la tecla **flecha arriba** para editar el último mensaje en el chat. Para más atajos de teclado, usa el comando [slash command](/Usage/Chatting/slashcommands.md) `/help hotkeys` en el chat o consulta la página [Atajos de teclado](/Usage/Chatting/hotkeys.md).
!!!

## Panel de acciones de mensaje

Gestiona mensajes de chat individuales a través del botón de elipsis (•••) en el mensaje.

Para mostrar estas opciones en todos los mensajes de tus chats, activa la configuración [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) en tu configuración de usuario.

### Funciones principales

* <i class="fa-solid fa-language"></i> **Traducir**: Convierte el mensaje a un idioma diferente
* <i class="fa-solid fa-paintbrush"></i> **Generar Imagen**: [Crea una imagen](/extensions/Stable-Diffusion.md) a partir del contenido del mensaje
* <i class="fa-solid fa-bullhorn"></i> **Narrar**: Conversión [texto a voz](/extensions/TTS.md)
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: Ver el prompt de generación y el uso de tokens

### Visibilidad del mensaje

* <i class="fa-solid fa-eye"></i> **Incluido**: La IA ve este mensaje; haz clic para excluirlo
* <i class="fa-solid fa-eye-slash"></i> **Excluido**: La IA no ve este mensaje; haz clic para incluirlo

### Gestión de contenido

* <i class="fa-solid fa-paperclip"></i> **Incrustar**: [Adjunta archivos o imágenes](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: Crear punto de control de historia
* <i class="fa-solid fa-flag"></i> **Navegación de Checkpoint**: Haz clic para abrir chat de punto de control, Shift+Clic para actualizar punto de control existente
* <i class="fa-solid fa-code-branch"></i> **Rama**: Comenzar ruta de historia alternativa
* <i class="fa-solid fa-copy"></i> **Copiar**: Copiar texto del mensaje
* <i class="fa-solid fa-pencil"></i> **Editar**: Editar contenido del mensaje

## Editar contenido del mensaje

Un panel compacto de herramientas de manipulación de mensajes que aparece cuando <i class="fa-solid fa-pencil"></i> **Editas** un mensaje de chat.

### Acciones principales

* <i class="fa-solid fa-check"></i> **Confirmar**: Guardar cambios del mensaje
* <i class="fa-solid fa-xmark"></i> **Cancelar**: Descartar cambios del mensaje

### Operaciones de mensaje

* <i class="fa-solid fa-copy"></i> **Copiar**: Duplicar contenido del mensaje
* <i class="fa-solid fa-trash-can"></i> **Eliminar**: Eliminar mensaje

### Posición del mensaje

* <i class="fa-solid fa-chevron-up"></i> **Mover arriba**: Subir mensaje en el chat
* <i class="fa-solid fa-chevron-down"></i> **Mover abajo**: Bajar mensaje en el chat

Nota: Los controles de movimiento pueden desactivarse según la posición del mensaje en el historial de chat.

## Panel de opciones de chat

Gestiona la configuración y operaciones del chat a través del botón <i class="fa-solid fa-bars"></i> **Opciones** en la parte inferior izquierda de la interfaz de chat.

### Controles de visualización

* <i class="fa-lg fa-solid fa-times"></i> **Cerrar chat**: Salir de la sesión de chat actual
* <i class="fa-lg fa-solid fa-cog"></i> **Alternar Paneles**: Mostrar/ocultar [paneles de interfaz](/Usage/index.md#control-panels)

### Configuración de generación

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Nota del Autor](/Usage/Characters/Author's-Note.md)**: Instrucciones de contexto personalizadas
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[Escala CFG](/Usage/Prompts/CFG.md)**: Ajusta la creatividad de la respuesta
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Probabilidades de Token](#token-probabilities-panel)**: Ver estadísticas de generación de tokens

### Navegación de chat

* <i class="fa-lg fa-solid fa-left-long"></i> **Volver al chat principal**: Retornar a la conversación principal
* <i class="fa-lg fa-solid fa-flag"></i> **Guardar punto de control**: Crear punto de control de historia
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convertir a grupo**: Transformar en [chat grupal](/Usage/Characters/groupchats.md)

### Gestión de chat

* <i class="fa-lg fa-solid fa-comments"></i> **Iniciar nuevo chat**: Comenzar una conversación nueva
* <i class="fa-lg fa-solid fa-address-book"></i> **Gestionar archivos de chat**: [Operaciones de archivos de chat](/Usage/Characters/chatfilemanagement.md) como importar, exportar y renombrar

### Controles de mensaje

* <i class="fa-lg fa-solid fa-trash-can"></i> **Eliminar mensajes**: Seleccionar y eliminar varios mensajes
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerar**: Crear nueva respuesta
* <i class="fa-lg fa-solid fa-user-secret"></i> **Suplantar**: La IA escribe mensaje como usuario
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continuar**: Extender último mensaje

Nota: Algunas opciones pueden estar ocultas dependiendo del contexto y estado del chat.

## Panel de Probabilidades de Token

El panel de Probabilidades de Token te permite ver el proceso de muestreo de la IA para la generación de texto. Te muestra no solo lo que la IA escribió, sino qué otras opciones consideró en cada punto del texto.

Para abrirlo, haz clic en el botón <i class="fa-solid fa-pie-chart"></i> **Probabilidades de Token** en el panel <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Opciones de Chat**.

![Example message](/static/token-probs/fling-msg.png){ width=500}

![Token probabilities display for example message](/static/token-probs/fling-probs.png){ width=500}

Cuando hagas clic en cualquier token (palabra, puntuación o carácter de formato) en el texto generado, el panel muestra tokens alternativos que la IA consideró en esa posición, junto con sus puntuaciones de probabilidad. Esto te da una idea del "proceso de pensamiento" de la IA y muestra otras direcciones que la respuesta podría haber tomado. Ver estas alternativas puede ayudarte a entender si había varias opciones probables o una sola opción clara.

![Alternative tokens and probabilities](/static/token-probs/fling-probs-logprob.png){ width=500}

Si ves un token que crees que la IA debería haber elegido diferente, elige una alternativa y el mensaje se regenerará desde ese punto en adelante, potencialmente dándote una respuesta diferente.

### Rerolling

Si cambias un token específico y regeneras la respuesta, la parte de la nueva respuesta antes del token cambiado será la misma que la respuesta original. Esta parte se muestra en gris. Como no fue generada, no hay información de probabilidad para esta parte.

Puede que desees ver otras respuestas que podrían haber sido generadas basadas en tu token alternativo.

Puedes hacer clic en la porción gris para "reroll" (rehacer) la generación, dándote una nueva variación del texto. Hacer clic en cualquier parte de la porción gris mantendrá toda la porción gris y regenerará toda la porción blanca/teñida.

Mantener presionado Ctrl mientras haces clic en un token en la porción gris retendrá la porción gris hasta el token en el que hiciste clic y regenerará el resto del texto. Tu elección de token alternativo no puede ser mantenida en este caso.

### Controles

**Visualización de Token**:

* El texto generado se divide en tokens individuales
* Cada token es interactivo, haz clic en un token para ver alternativas consideradas por la IA
* Los tokens están teñidos como una ayuda visual pero esto no indica probabilidad
* Los caracteres especiales (espacios, saltos de línea) están visiblemente marcados

**Selección de Token**:

* Haz clic en un token para ver alternativas
* Haz clic en una alternativa para reemplazar el token y regenerar la respuesta
* Pasa el cursor sobre un token para ver su puntuación de log-probabilidad sin procesar

**Controles de Ventana**:

* <i class="fa-solid fa-grip"></i> Asa de arrastre para reposicionamiento de panel (MovingUI solamente)
* <i class="fa-solid fa-window-maximize"></i> Maximizar/restaurar tamaño del panel
* <i class="fa-solid fa-circle-chevron-up"></i> Expandir/contraer contenido del panel
* <i class="fa-solid fa-circle-xmark"></i> Cerrar panel

### Disponibilidad

Debes seleccionar **Solicitar probabilidades de token** en [Configuración de Usuario](/Usage/User_Settings/index.md#chatmessage-handling) para habilitar esta función.

Las probabilidades de token solo están disponibles para el mensaje más reciente, y no se guardan en el chat. Si la información de probabilidad de token ya no está disponible para un mensaje, el panel mostrará un mensaje indicando esto.

Las probabilidades de token no están disponibles cuando se usa Smooth Streaming.

Las probabilidades de token no están disponibles de todos los APIs. Si estás usando una API que no admite probabilidades de token, el panel se abrirá pero no mostrará ninguna información.

#### Finalización de texto
* **LlamaCPP**: Disponible
* **Text Generation WebUI** (oobabooga): Disponible
* **TabbyAPI**: Disponible
* **NovelAI**: Disponible
* **KoboldCPP**: Disponible
* **Ollama**: Parece no estar disponible
* **OpenRouter Text**: Parece no estar disponible

#### Finalización de chat
* **OpenAI** o **Custom**: Disponible, pero rerolling no está soportado
* **Anthropic**: Parece no estar disponible
* **Google AI Studio**: Parece no estar disponible
* **OpenRouter Chat**: Parece no estar disponible
