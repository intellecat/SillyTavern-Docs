---
label: Extensiones
icon: plug
expanded: true
order: 35
route: /extensions/
---

# Extensiones

SillyTavern viene con muchas extensiones que se pueden habilitar o deshabilitar en el panel de Extensiones. Las extensiones pueden agregar nuevas características, cambiar el comportamiento de características existentes o proporcionar contenido adicional para que su IA utilice. Se pueden instalar más extensiones desde el menú "Descargar Extensiones y Activos" en el panel de Extensiones.

## Panel de extensiones

Para abrir o cerrar el panel de Extensiones, elija **<i class="fa-solid fa-cubes fa-fw"></i> Extensions** en la barra superior.

- **<i class="fa-solid fa-cubes"></i> Administrar extensiones**: Activar, desactivar y actualizar extensiones
- **Download Extensions & Assets**: Instale [más extensiones](#installable-extensions), personajes, sonidos y fondos del repositorio de SillyTavern
- **Notificar sobre actualizaciones de extensiones**: Marque para ser notificado cuando hay actualizaciones disponibles para extensiones instaladas
- **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**: Importe una [extensión de terceros](#third-party-extensions) desde una URL de repositorio de Git

## Extensiones integradas

Estas extensiones están integradas en SillyTavern y no necesitan ser instaladas. Se pueden habilitar o deshabilitar en el panel de Extensiones.

:::callout
**[Chat Translation](Translation.md)**

Traducir mensajes de chat a un idioma diferente
:::

:::callout
**[Image Captioning](captioning.md)**

Genera texto a partir de imágenes para que su IA pueda "ver" y responder al contenido visual en sus conversaciones
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

Utilice API de Stable Diffusion, FLUX o DALL-E locales o basadas en la nube para generar imágenes
:::

:::callout
**[Expression Images](Expression-Images.md)**

Imágenes (también llamadas 'sprites') de su personaje de IA, mostradas junto o detrás de la ventana de chat
:::

:::callout
**[Summarize](Summarize.md)**

Resumen automático del historial de chat
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

Busca mensajes relevantes del historial de chat y los agrega al contexto
:::

:::callout
**[Text To Speech](TTS.md)**

Narración de voz para sus mensajes de chat a través de ElevenLabs, Silero, su sistema TTS, **[AllTalk](AllTalk.md)**, **[XTTS](XTTS.md)** y más
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

Responder a mensajes de chat con un solo clic, ejecutar comandos y STscripts, y más
:::

:::callout
**Token Counter**

Convierte texto en tokens y cuenta el número de tokens
:::

---

## Extensiones instalables

!!!tip
Debe tener git instalado para descargar extensiones. Siga las instrucciones en la [página de instalación de Git](https://git-scm.com/downloads) si no tiene instalado.
!!!

Puede ver una lista de todas las extensiones disponibles directamente desde la aplicación yendo al menú **<i class="fa-solid fa-cubes"></i> Extensions** => **Download Extensions & Assets** y haciendo clic en el botón **<i class="fa-solid fa-plug-circle-exclamation"></i> Load Asset List**. Para instalar una extensión, haga clic en el botón **<i class="fa-solid fa-download"></i> Download**. Para leer más sobre una extensión, haga clic en el botón **<i class="fa-solid fa-arrow-up-right-from-square"></i> Link** junto a su nombre para abrir su página de GitHub.

!!!info Extensiones no son Extras
El proyecto Extras fue descontinuado en abril de 2024. No necesita instalar Extras para usar extensiones.
!!!

:::callout
**[Blip](Blip.md)**

Anime el texto de los mensajes de caracteres con velocidad variable y reproduzca sonido junto a la animación.
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

Agrega música de fondo envolvente y sonidos ambientales a sus chats.
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

Juegue juegos de consola retro directamente en chats de SillyTavern.
:::

:::callout
**[Live2d](Live2d.md)**

Agrega soporte para modelos live2d. Expresiones, animaciones e interacciones personalizables.
:::

:::callout
**[Objective](Objective.md)**

Establezca un Objetivo para que la IA apunte durante el chat.
:::

:::callout
**[RVC](RVC.md)**

Agrega capacidades de Clonación de Voz en Tiempo Real al módulo de Texto a Voz.
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

Convierta su voz en texto usando navegador o extras.
:::

:::callout
**[VRM](VRM.md)**

Agrega soporte para modelos VRM. Expresiones, animaciones e interacciones personalizables.
:::

:::callout
**[Web Search](WebSearch.md)**

Agrega resultados de búsqueda web a indicaciones de LLM.
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

Proporciona información meteorológica usando la API de AccuWeather como comando de barra o herramienta de función.
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

Agrega una barra superior a la ventana de chat con accesos directos a acciones rápidas.
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

Juegue ajedrez con el LLM.
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

Permite ejecutar código JavaScript y STscript desde bloques de código en chat.
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

Un conjunto de 7 dados D&D clásicos para todas sus necesidades de lanzamiento de dados.
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

Agrega la capacidad de agrupar caracteres por grupos de similitud para encontrar fácilmente duplicados.
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

Agrega un botón para insertar rápidamente emojis en un mensaje de chat.
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

Permite establecer saludos alternativos específicos para chats grupales.
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

Agrega un botón para insertar rápidamente una plantilla de comando /sendas para el miembro del grupo seleccionado.
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

Muestre sugerencias personalizadas basadas en sus chats recientes utilizando el motor HypeBot de NovelAI. Requiere una suscripción activa de NovelAI.
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

Agrega "indicaciones de inactividad" después de que el usuario ha estado inactivo durante algún tiempo para continuar orgánicamente la conversación.
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

Ver metadatos de imágenes ampliadas adjuntas a un chat.
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

Renderizar fórmulas LaTeX y AsciiMath en mensajes de chat.
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

Agrega renderización de diagramas Mermaid y diagramas de flujo a chats de SillyTavern.
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

Agrega un lugar para almacenar sus notas. Soporta formato de texto enriquecido.
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

Agrega la capacidad de aleatorizar controles deslizantes de configuración de API con cada generación.
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

Mejora la experiencia actual de la novela visual con más características (Modo Enfoque, Modo Letterbox y más)!
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

Agrega una opción para inspeccionar y editar indicaciones de salida antes de enviarlas al servidor.
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

Permite recibir notificaciones push para mensajes de chat entrantes.
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

Agrega un menú desplegable para seleccionar personas de usuario desde la barra de chat.
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

Obtiene las últimas noticias de fuentes RSS como comando de barra o herramienta de función.
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

Proporciona la imagen de la pantalla para modelos multimodales cuando envía un mensaje.
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

Agrega un reproductor de audio de silencio al menú de extensiones. Puede ayudar si la pestaña del navegador se está cerrando en segundo plano.
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

Agrega navegación de línea de tiempo al historial de chat.
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

Forma fácil de ver y modificar variables.
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

Proporciona una interfaz para que las extensiones usen modelos de lenguaje directamente en el navegador.
:::

## Extensiones de terceros

!!!danger
El uso de extensiones de terceros puede tener efectos secundarios no deseados y puede presentar riesgos de seguridad. Asegúrese siempre de confiar en la fuente antes de importar una extensión a través de **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**. No somos responsables de ningún daño causado por extensiones de terceros.
!!!

Para instalar una extensión de terceros, vaya al menú **<i class="fa-solid fa-cubes"></i> Extensions** => **<i class="fa-solid fa-cloud-arrow-down"></i> Install Extension** y pegue la URL del repositorio de extensiones. Opcionalmente, especifique la rama y (en escenarios de [múltiples usuarios](../Administration/multi-user.md)) el destino de instalación: todos los usuarios o solo el usuario actual. La extensión se descargará y cargará automáticamente.
