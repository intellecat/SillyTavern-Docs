---
order: 50
templating: false
route: /usage/prompts/prompt-manager/
---

# Administrador de Prompts

El Administrador de Prompts es un sistema que proporciona más control sobre la estrategia de [construcción de prompts](index.md) para Chat Completion APIs.

!!! Se aplica a: Chat Completion APIs
Para configuraciones equivalentes en Text Completion APIs, utiliza [Advanced Formatting](advancedformatting.md).
!!!

!!!tip Nomenclatura de Presets
Si un preset comparte el mismo nombre con una de tus tarjetas de personaje, será seleccionado automáticamente al iniciar un chat con ese personaje. Nombra los presets de manera única para evitar este comportamiento.
!!!

Accede al Administrador de Prompts haciendo clic en el botón "AI Response Configuration" en la barra de navegación. El Administrador de Prompts se encuentra debajo del panel de [configuración común](/Usage/Common-Settings.md).

## Edición Rápida de Prompts

Proporciona espacio para editar rápidamente secciones de prompts comunes, como **Main Prompt**, **Auxiliary Prompt**, y **Post-History Instructions**. Puedes encontrar más información sobre estos prompts en la página de [construcción de prompts](index.md).

## Utility Prompts

Estos prompts se envían al modelo Chat Completion para ayudarlo a entender la información que se le está enviando, o para instruirlo a actuar de formas específicas durante ciertos tipos de interacciones.

### Format Templates

!!!tip
Si el formato template no está configurado, la información será enviada tal cual, sin ningún envoltorio.
!!!

Estas son plantillas de cadenas utilizadas para envolver la información extraída de [World Info](/Usage/worldinfo.md) y [Character Cards](/Usage/Characters/characterdesign.md).

Se utiliza un marcador especial para indicar dónde debe insertarse la información:

- `{0}` para la plantilla de formato World Info.
- `{{scenario}}` para la plantilla de formato Scenario.
- `{{personality}}` para la plantilla de formato Personality.

### Group Nudge Prompt Template

Se utiliza solo en chats grupales. Se coloca al final del prompt para forzar una respuesta de un personaje específico.

Déjalo vacío para desactivar la funcionalidad de Group Nudge.

### New Chat, New Group Chat, New Example Chat

Estos se envían antes del historial de chat y antes de cada bloque de [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) para informar al modelo dónde termina la información de fondo e inicia el historial de chat.

- **New Chat:** Se utiliza para chats individuales.
- **New Group Chat:** Se utiliza para chats grupales.
- **New Example Chat:** Se utiliza para bloques de diálogo de ejemplo.

Déjalos vacíos para desactivar esta funcionalidad.

### Continue Nudge

Se envía al final del prompt para instruir al modelo sobre qué hacer cuando se activa Continue, como cuando se presiona el botón Continue o cuando se activa por STScript.

!!! Chat Completion 'Continues'
Ten en cuenta que los modelos Chat Completion manejan Continues de manera diferente que los modelos **Text Completion**, y puede que no siempre entreguen resultados fluidos independientemente de tu Continue Nudge.
!!!

### Replace Empty Message

Envía el contenido de este campo en lugar de un mensaje en blanco cuando la caja de texto está vacía y se presiona **Send a message**.

## Character Names Behavior

Proporciona diferentes estrategias para instruir al modelo sobre cómo asociar mensajes con personajes. Si un modelo Chat Completion tiene dificultades para determinar qué mensajes pertenecen a qué personaje, puede ser que necesite una estrategia diferente seleccionada.

## Continue Postfix

Cuando se activa Continue, el mensaje 'continued' devuelto por el modelo tendrá el Continue Postfix seleccionado antepuesto al principio. Por ejemplo, puede agregar un espacio antes del texto continuado.

## Additional Settings

### Wrap in Quotes

!!!warning
Opción deprecada. Prefiere [Regex scripts](/extensions/Regex.md) en su lugar.
!!!

Envuelve el mensaje completo del usuario en comillas ocultas antes de enviarlo. Esto es útil para sesiones donde los personajes no usan comillas para indicar diálogo. Si tu sesión usa comillas para indicar diálogo, déjalo sin marcar.

### Continue Prefill

!!!warning
Puede que no funcione con todas las fuentes Chat Completion.
!!!

Envía el Continue Nudge como un mensaje de rol Assistant en lugar de un mensaje System. Si esto está habilitado, el prompt Continue Nudge no será utilizado.

### Squash system messages

!!!warning
Opción deprecada. Prefiere [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) en su lugar.
!!!

Combina mensajes System consecutivos en un solo mensaje combinado (excluyendo Example Dialogue).

### Enable web search

!!!
No confundas esto con la [Web Search extension](/extensions/WebSearch.md).
!!!

Habilita capacidades de búsqueda web proporcionadas por el backend Chat Completion. El prompt generalmente se enriquece con resultados de búsqueda por el proveedor del modelo y puede incurrir en costos adicionales.

### Enable function calling

Ver [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
No confundas esto con la [Image Captioning extension](/extensions/captioning.md).
!!!

Si el modelo Chat Completion tiene capacidades multimodales para procesar imágenes y videos enviados, esto alterna su capacidad para hacerlo. Para añadir medios al prompt, utiliza la opción **Attach A File** en el menú "Magic Wand".

### Request inline images

!!!
No confundas esto con la [Image Generation extension](/extensions/Stable-Diffusion.md).
!!!

Permite que el modelo devuelva adjuntos de imagen.

### Use system prompt

!!!
Solo soportado por los backends Google Gemini y Anthropic Claude.

A pesar de tener configuraciones muy similares para estos dos, son técnicamente opciones separadas, por lo que pueden ser configuradas de forma independiente.
!!!

Combina todos los mensajes del sistema hasta el primer mensaje con un rol no-sistema (User/Assistant) y los envía como un campo de instrucción del sistema separado.

## Reasoning Settings

Si el modelo Chat Completion utiliza reasoning, estas configuraciones afectan su visibilidad y funcionalidad.

### Request model reasoning

Ver [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend).

### Reasoning Effort

Ver [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort).

## "Prompts"

El Administrador de Prompts forma la base del prompt enviado al modelo Chat Completion. Controla qué se envía así como el *orden* en que se envía.

### The 'Prompts' Dropdown

Contiene una lista desplegable de todos los prompts (no predeterminados) que el preset Chat Completion actual incluye. Para que uno de estos prompts sea añadido al mensaje saliente, necesita ser seleccionado de la lista desplegable y luego agregado al Administrador de Prompts presionando el botón **Insert prompt**. Para crear un nuevo prompt a añadir a esta lista desplegable, presiona el botón **New prompt**. Una vez que el nuevo prompt está escrito y guardado, se añade a la lista desplegable y puede entonces ser insertado.

### Prompts List

Esta es una interfaz de arrastrar y soltar que lista los prompts seleccionados para ser potencialmente enviados al modelo Chat Completion. Los prompts colocados más cerca de la **parte superior** de la interfaz se envían primero. La **parte inferior** de la lista es lo **último** enviado al modelo (típicamente, esto sería tu **Post-History Instructions**).

!!! Prompts 'Pinned' = Prompts Predeterminados
Los prompts predeterminados no pueden ser removidos de la lista de prompts seleccionados. Esto incluye Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History, y Post-History Instructions. Si estos no son deseados, pueden ser **alternados a 'OFF'**, pero no removidos o eliminados completamente.
!!!

## Editing a Prompt

Hacer clic en el **botón de lápiz** en un prompt te llevará a la **Edit interface**. Aquí, puedes editar el prompt directamente.

!!! Asegúrate de guardar tus cambios
Para guardar permanentemente los cambios en estos prompts en tu preset Chat Completion, debes hacer clic en el botón **Save** en la esquina inferior derecha de la **Edit interface**, así como guardar el preset mismo usando el botón **Save** ubicado en la parte superior de la sección **AI Response Configuration**. De lo contrario, los cambios realizados se perderán cuando el preset Chat Completion sea cambiado a uno diferente.
!!!

### Name

El nombre del prompt. Esto no se envía al modelo Chat Completion; es solo para tu referencia dentro del Administrador de Prompts.

### Role

Qué rol envía el prompt. Puedes elegir entre System, AI Assistant, o User.

### Triggers

Los tipos de generación para los cuales este prompt se envía. Si nada está seleccionado, el prompt se enviará para todos los tipos de generación. Si uno o más están seleccionados, el prompt se enviará solo para esos tipos de generación específicos:

- **Normal:** Solicitud de generación de mensajes regulares.
- **Continue:** Cuando se presiona el botón Continue.
- **Impersonate:** Cuando se presiona el botón Impersonate.
- **Swipe:** Cuando la generación se activa por deslizamiento.
- **Regenerate:** Cuando se presiona el botón Regenerate en chats individuales.
- **Quiet:** Solicitudes de generación en segundo plano, generalmente activadas por comandos de [extensions](/extensions/index.md) o [STscript](/For_Contributors/st-script.md).

!!!
El trigger "Regenerate" no está disponible en chats grupales ya que utiliza una lógica de regeneración diferente: todos los mensajes de la última respuesta se eliminan, y los mensajes se ponen en cola usando el tipo de generación "Normal" según la [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) elegida.
!!!

### Position

Cuando Position está configurado a **Relative**, este prompt se envía donde está ubicado en la interfaz de arrastrar y soltar con todos los otros prompts. Cuando está configurado a **In-Chat** y se le asigna una **Depth**, en su lugar se envía **dentro del Chat History** como el Role seleccionado, e **ignora** el orden de la interfaz de arrastrar y soltar.

### Depth

Cuando Position está configurado a **In-Chat**, esto define cuán profundo se envía el prompt dentro del historial de chat. Cuanto mayor sea el número, más profundo se envía. Por ejemplo, una Depth de 0 se enviará después del último mensaje del chat, una Depth de 1 se enviará antes del último mensaje del chat, una Depth de 2 se enviará antes del segundo-al-último mensaje del chat, y así sucesivamente.

### Order

!!!
Los prompts que tienen el mismo Role y Depth serán agrupados juntos y ordenados por su valor Order.
El orden es el siguiente (de arriba a abajo): User, AI Assistant, System.
!!!

Cuando Position está configurado a **In-Chat**, esto define el orden en que se envía el prompt dentro del historial de chat. Cuanto menor sea el número, más temprano se envía.

## Building Your Prompt: Tips and Tricks

Visita la sección [construcción de prompts](index.md) de la documentación de SillyTavern para más información sobre cómo escribir prompts efectivos. La información puede ser ampliamente aplicada a presets Chat Completion.
