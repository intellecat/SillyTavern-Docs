---
route: /
---

# ¿Qué es SillyTavern?

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern (o ST para abreviar) es una interfaz de usuario instalada localmente que te permite interactuar con LLMs de generación de texto, motores de generación de imágenes y modelos de voz TTS. Nuestro objetivo es empoderar a los usuarios con la mayor utilidad y control posible sobre sus prompts de LLM, abrazando la pronunciada curva de aprendizaje como parte de la diversión.

SillyTavern es un proyecto de pasión traído a ti por una comunidad dedicada de entusiastas de LLM y siempre será gratuito y de código abierto. Comenzando en febrero de 2023 como un fork de TavernAI 1.2.8, SillyTavern ahora cuenta con más de 200 contribuidores y 2 años de desarrollo independiente, y continúa sirviendo como un software líder para aficionados expertos de IA.

## Capturas de pantalla

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## Requisitos de instalación

Los requisitos de hardware son mínimos: funcionará en cualquier cosa que pueda ejecutar NodeJS 18 o superior. Si tienes la intención de hacer inferencia de LLM en tu máquina local, recomendamos una tarjeta gráfica NVIDIA de la serie 3000 con al menos 6GB de VRAM.

Sigue la guía de instalación para tu plataforma:

* [Windows](/Installation/Windows.md)
* [Linux y Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Ramas

SillyTavern se está desarrollando utilizando un sistema de dos ramas para garantizar una experiencia fluida para todos los usuarios.

* `release` -🌟 **Recomendado para la mayoría de usuarios.** Esta es la rama más estable y recomendada, actualizada solo cuando se lanzan versiones principales. Es adecuada para la mayoría de los usuarios. Típicamente actualizada una vez al mes.
* `staging` - ⚠️ **No recomendado para uso casual.** Esta rama tiene las últimas características, pero ten cuidado ya que puede fallar en cualquier momento. Solo para usuarios avanzados y entusiastas. Se actualiza varias veces al día.

## ¿Qué necesito además de SillyTavern?

Dado que SillyTavern es solo una interfaz, necesitarás acceso a un backend de LLM para proporcionar inferencia. Puedes usar AI Horde para chatear instantáneamente desde el primer momento. Aparte de eso, soportamos muchos otros backends de LLM locales y basados en la nube: API compatible con OpenAI, KoboldAI, Tabby, y muchos más. Puedes leer más sobre nuestras APIs soportadas en la sección [Conexiones de API](/Usage/API_Connections/index.md).

## Tarjetas de personaje

SillyTavern está construido alrededor del concepto de "tarjetas de personaje". Una tarjeta de personaje es una colección de prompts que establecen el comportamiento del LLM y es necesaria para tener conversaciones persistentes en SillyTavern. Funcionan de manera similar a los GPTs de ChatGPT o los bots de Poe. El contenido de una tarjeta de personaje puede ser cualquier cosa: un escenario abstracto, un asistente adaptado para una tarea específica, una personalidad famosa o un personaje ficticio.

Para tener una conversación rápida sin seleccionar una tarjeta de personaje o simplemente para probar la conexión del LLM, simplemente escribe tu prompt en la barra de entrada en la [Pantalla de bienvenida](/Usage/welcome-assistants.md) después de abrir SillyTavern. Esto creará una tarjeta de personaje "Asistente" vacía que puedes personalizar más tarde.

Para tener una idea general de cómo definir tarjetas de personaje, consulta el personaje predeterminado (Seraphina) o descarga tarjetas seleccionadas hechas por la comunidad desde el menú "Download Extensions & Assets".

También puedes crear tus propias tarjetas de personaje desde cero. Consulta la guía de [Diseño de personajes](/Usage/Characters/characterdesign.md) para más información.

## Características clave

* [Configuraciones avanzadas de generación de texto](/Usage/Prompts/advancedformatting.md) con muchos presets hechos por la comunidad
* [Soporte para World Info](Usage/worldinfo.md): crea un lore rico o ahorra tokens en tu tarjeta de personaje
* [Chats grupales](/Usage/Characters/groupchats.md): salas multi-bot para que los personajes hablen contigo y/o entre ellos
* [Opciones ricas de personalización de UI](/Usage/User_Settings/uicustomization.md): colores de tema, imágenes de fondo, CSS personalizado, y más
* [Personas de usuario](/Usage/personas.md): deja que la IA sepa un poco sobre ti para mayor inmersión
* [Soporte RAG incorporado](/Usage/Characters/data-bank.md): agrega documentos a tus chats para que la IA los referencie
* Amplio subsistema de [comandos de chat](/Usage/Chatting/slashcommands.md) y [motor de scripting](/For_Contributors/st-script.md) propio

## Extensiones

SillyTavern tiene soporte de extensibilidad.

* [Expresiones emocionales de personajes (sprites)](/extensions/Expression-Images.md)
* [Auto-resumen del historial de chat](/extensions/Summarize.md)
* Interfaz automática y [traducción de chat](extensions/Translation.md)
* [Generación de imágenes Stable Diffusion/FLUX/DALL-E](/extensions/Stable-Diffusion.md)
* [Texto a voz para mensajes de respuesta de IA (a través de ElevenLabs, Silero, o el TTS del sistema operativo)](/extensions/TTS.md)
* [Capacidades de búsqueda web para agregar contexto adicional del mundo real a tus prompts](/extensions/WebSearch.md)
* Muchas más están disponibles para descargar desde el menú "Download Extensions & Assets".

## ¿Cómo puedo ponerme en contacto con los desarrolladores directamente?

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [Publicar un issue en GitHub](https://github.com/SillyTavern/SillyTavern/issues)

## ¡Me gusta su proyecto! ¿Cómo puedo contribuir?

* ¡Damos la bienvenida a pull requests! Sigue las [Guías de contribución](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md) para comenzar.
* También damos la bienvenida a informes de errores útiles e informados que utilicen las plantillas proporcionadas en nuestro GitHub.
* No aceptamos donaciones monetarias para el proyecto en sí.

## Donaciones personales

Tu apoyo a los contribuidores individuales es apreciado, pero no influirá en la dirección general de desarrollo de SillyTavern.

* RossAscends tiene un [Patreon](https://www.patreon.com/RossAscends) y [Kofi](https://ko-fi.com/rossascends) personal

## Licencia

SillyTavern es un proyecto gratuito y de código abierto publicado bajo la [Licencia AGPL-3.0](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE).
