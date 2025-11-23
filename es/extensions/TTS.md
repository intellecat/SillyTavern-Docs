---
order: tts
route: /extensions/tts/
---

# TTS

SillyTavern tiene una amplia variedad de opciones de TTS (síntesis de voz) que se utilizan para que una voz narre partes de tu chat. Esta página explica la configuración y el uso.

## Configurar TTS

### Selección de Proveedor TTS

Se utiliza para seleccionar qué servicio de TTS deseas utilizar. Algunas opciones son gratuitas, algunas requieren una suscripción paga, y algunas se ejecutan localmente en tu PC.

Opciones disponibles (la lista puede cambiar con el tiempo):

- **AllTalk** - gratuito, instalación local de código abierto, ofrece una variedad de motores de TTS. Consulta la página [AllTalk](./AllTalk.md) para obtener instrucciones de configuración.
- **Azure TTS** - las mismas voces que Microsoft Edge. Requiere una cuenta de Azure y una suscripción paga.
- **Coqui-TTS** (deprecado) - gratuito, requiere la API de Extras para ejecutarse. Modelos Text2Speech de alto rendimiento (Tacotron, Tacotron2, Glow-TTS, SpeedySpeech) así como Bark.
- **Edge** - gratuito, se ejecuta a través de Azure. Cuando se ejecuta con "Plugin" seleccionado como proveedor, también necesitas instalar [este complemento de servidor](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin). La otra opción requiere la API de Extras (deprecada) para ejecutarse.
- **Electron Hub** - reutiliza tu clave de API de [Electron Hub](https://electronhub.ai/) para acceder a voces en la nube (GPT-4o Mini TTS, voces neurales de Microsoft, etc.) con controles por modelo.
- **ElevenLabs** - se requiere suscripción paga. Obtén una clave de API de [ElevenLabs](https://elevenlabs.io/).
- **Google Translate** - una voz gratuita proporcionada por Google, una por idioma, la calidad puede variar mucho.
- **Google Gemini TTS** - requiere una clave de API de [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) o [AI Studio](/Usage/API_Connections/google.md#google-ai-studio), utiliza modelos de [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts).
- **Kokoro** - gratuito, utiliza [kokoro.js](https://www.npmjs.com/package/kokoro-js) para ejecutar el modelo localmente en tu navegador. Sin embargo, [algunos navegadores](https://caniuse.com/webgpu) pueden no ser compatibles con WebGPU para la opción de dispositivo.
- **MiniMax** - requiere una clave de API de [MiniMax](https://www.minimax.io/). Consulta la página [MiniMax TTS](./MiniMaxTTS.md) para obtener instrucciones de configuración.
- **Novel** - requiere una suscripción paga de NovelAI, generado por el motor de TTS de NovelAI
- **OpenAI** - se requiere clave de API paga, utiliza los modelos de TTS de OpenAI.
- **Pollinations** - acceso gratuito a los modelos de TTS de OpenAI, pero con límite de velocidad. [Sitio web](https://pollinations.ai/).
- **Silero** - gratuito, se ejecuta en tu PC, la calidad puede variar mucho. Requiere una [instalación de servidor de API dedicado](https://github.com/ouoertheo/silero-api-server) o API de Extras (deprecada).
- **System** - utiliza el motor de TTS de tu SO, si existe uno. La calidad puede variar mucho dependiendo del SO.
- **XTTS** - gratuito, requiere una instalación de servidor de API dedicado. Consulta la página [XTTS](./XTTS.md) para obtener instrucciones de configuración.

### Casillas de Verificación

- **Habilitado** - activa/desactiva la reproducción de TTS
- **Generación Automática** - permite que TTS comience a reproducirse automáticamente cuando un nuevo mensaje entra en el chat
- **Solo narrar "comillas"** - Limita la reproducción de TTS para incluir solo el texto dentro de `"comillas"`. Esto `*incluirá "comillas" dentro de líneas con asteriscos*` (nombre de variable interno = `narrate_quoted_only`)
- **Ignorar \*texto, incluso "comillas", dentro de asteriscos\*** - TTS no reproducirá ningún texto dentro de `*asteriscos*`, ni siquiera "comillas" (nombre de variable interno = `narrate_dialogues_only`)
- *tener ambas casillas de verificación "solo narrar comillas" e "ignorar asteriscos" marcadas resultará en que TTS solo lea "comillas" que no estén en asteriscos e ignore todo lo demás.*
- **Narrar solo el texto traducido** - esto hará que TTS solo narre el texto traducido.

Dado el texto de ejemplo: `*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
Aquí hay una tabla mostrando cómo se modificará el texto según los estados booleanos de **Ignorar \*texto, incluso "comillas", dentro de asteriscos\*** e **Solo narrar "comillas"**:

| **Ignorar \*texto, incluso "comillas", dentro de asteriscos\*** 	 | **Solo narrar "comillas"**	 | **Resultado**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------|
| Deshabilitado                                               | 	Deshabilitado	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Deshabilitado                                               | Habilitado	                   | "nya"... "Good evening, senpai"                                           |
| Habilitado	                                               | Deshabilitado	                  | "Good evening, senpai", she says.                                         |
| Habilitado	                                               | Habilitado	                   | "Good evening, senpai"                                                    |

### Deslizadores

Estos cambiarán dependiendo de la API que selecciones.

### Botones

- **Aplicar** - esto debe hacerse clic después de establecer una API de TTS y después de editar el mapa de voces.
- **Actualizar** - recarga la lista de voces del servicio de TTS seleccionado.
- **Voces disponibles** - carga una ventana emergente con todas las voces disponibles para tu API seleccionada, y te permite previsualizarlas con diálogos de muestra.

## Usar TTS

1. Haz clic en la casilla de verificación "Habilitar", o nada sucederá.
2. Haz clic en la casilla de verificación "Generación automática" si deseas que TTS comience automáticamente cada vez que llega un nuevo mensaje en el chat.
3. Opcionalmente, haz clic en el icono de megáfono dentro de la esquina superior derecha de cualquier mensaje para reproducir bajo demanda.
4. Haz clic en el botón "Detener" de la esquina inferior derecha (encontrado dentro del menú de varita mágica) para detener cualquier reproducción.

### Mapa de Voces

Debes proporcionar un mapa de voces para que TTS lo use, de lo contrario, no sabrá qué voces usar para cada personaje. Para configurar el mapa de voces, primero abre un chat con un personaje al que te gustaría asignar una voz y/o selecciona una persona de usuario a la que asignar una voz, luego selecciona una voz listada por un proveedor de TTS en el menú desplegable. Si no ves una lista de voces y/o personajes, asegúrate de que tu proveedor de TTS esté configurado correctamente y haz clic en "Actualizar". Algunos proveedores (como OpenAI-compatible o NovelAI) requieren que llenes la lista de voces manualmente.
