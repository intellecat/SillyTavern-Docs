---
route: /extensions/captioning/
templating: false
---

# Image Captioning

Image Captioning permite que SillyTavern genere automáticamente descripciones de texto para las imágenes utilizadas en chats.

Usa Image Captioning cuando quieras que tu personaje de IA "vea" y responda al contenido visual en tus conversaciones.

- Crear títulos para imágenes que subas o pegues en los mensajes
- Añadir contexto a imágenes existentes en el historial del chat
- Usar varias fuentes para la generación, incluyendo modelos locales, APIs en la nube, y redes colaborativas

Hay opciones que no requieren configuración, no cuestan dinero y no necesitan GPU. También hay opciones que requieren algunos o todos esos recursos. Elige la que se adapte a tus necesidades y recursos.

La extensión de Image Captioning está integrada en SillyTavern y no necesita ser instalada por separado.

## Inicio rápido

1. Configuración:
    - Abre el panel **Image Captioning** en el panel **<i class="fa-solid fa-cubes"></i> Extensiones**
    - Elige una fuente de captioning (lo más probable es "Local" o "Multimodal")
    - Para "Multimodal" asegúrate de haber configurado la conexión en la pestaña **<i class="fa-solid fa-plug"></i> Conexiones de API**
2. Genera un título:
    - Elige "**Generate Caption**" del menú emergente **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensiones**
    - Selecciona un archivo de imagen cuando se te pida
    - Espera a que se genere el título
3. Revisa y envía:
    - La imagen con título se insertará en tu mensaje
    - Ve el título usando el tooltip de la imagen
    - Haz clic en **<i class="fa-solid fa-paper-plane"></i> Enviar** para ver qué piensa tu personaje de la imagen

## Controles del panel

### Selección de fuente

Elige la fuente para Image Captioning. Opciones soportadas:

| Fuente                           | Descripción                                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **Cloud**: OpenAI, Anthropic, Google, MistralAI, y otros. <br>**Local**: Ollama, llama.cpp, KoboldCpp, Text Generation WebUI, y vLLM. <br>Soporta prompts personalizados para que puedas hacer preguntas a tus imágenes. |
| [Local](#local-source)           | Usa [transformers.js](https://huggingface.co/docs/transformers.js/en/index) ejecutándose localmente dentro de tu servidor SillyTavern. ¡Cero configuración!                                                                     |
| Horde                            | Usa la red [AI Horde](https://aihorde.net/), una red colaborativa distribuida de modelos de generación de imágenes. Nada que descargar, configurar o pagar. Tiempos de respuesta variables.                       |
| Extras                           | El proyecto Extras fue descontinuado en abril de 2024 y no se mantiene ni se soporta.                                                                                                                        |

### Configuración de Caption

- **Caption Prompt**: Introduce un prompt personalizado para captioning. El prompt por defecto es "¿Qué hay en esta imagen?"
- **Ask every time**: Activa para solicitar un prompt personalizado para cada título de imagen

### Plantilla de Mensaje

- **Message Template**: Personaliza la plantilla de mensaje de título. Usa la macro `{{caption}}` para insertar el título generado. La plantilla por defecto es `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### Auto-captioning

- **Automatically caption images**: Activa para habilitar el captioning automático de imágenes pegadas o adjuntas a mensajes
- **Edit captions before saving**: Activa para permitir editar títulos antes de guardarlos

## Captioning de imágenes

Todas las formas de hacer captioning de imágenes en SillyTavern:

* Elige "**Generate Caption**" del menú emergente **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensiones** y selecciona un archivo de imagen cuando se te pida
* Haz clic en el icono <i class="fa-solid fa-envelope-open-text"></i> **Caption** en la parte superior de una imagen ya en un mensaje
* Pega una imagen directamente en la entrada del chat con [auto-captioning](#auto-captioning) habilitado
* Adjunta un archivo de imagen a un mensaje usando el botón <i class="fa-solid fa-paperclip"></i> **Embed File or Image** en las acciones de un mensaje.
* Envía un mensaje con una imagen embebida
* Usa el comando [slash command](#slash-command-caption) `/caption`

## Auto-Captioning

La característica de auto-captioning te permite generar automáticamente títulos para imágenes conforme se añaden al chat, sin necesidad de activar manualmente el proceso de captioning cada vez.

Para habilitarlo, selecciona la casilla "Automatically caption images" en el panel Image Captioning. También puedes optar por editar títulos antes de guardarlos marcando la casilla "Edit captions before saving".

Una vez habilitado, el auto-captioning se activará en los siguientes escenarios:

- Cuando una imagen se pega directamente en la entrada del chat.
- Cuando un archivo de imagen se adjunta a un mensaje.
- Cuando se envía un mensaje con una imagen embebida.

El sistema usará tu fuente de captioning seleccionada (Local, Extras, Horde, o Multimodal) y la configuración establecida para generar un título para la imagen.

### Editar títulos antes de guardar (Modo Refine)

Si has habilitado la opción "Edit captions before saving":
1. Después de que se añada una imagen, aparecerá un popup con el título generado.
2. Puedes revisar y editar el título según sea necesario.
3. Haz clic en "OK" para aplicar el título, o "Cancel" para descartar el título sin guardar.

### Envío de Caption

El título generado (y opcionalmente editado) será insertado automáticamente en el prompt usando la Message Template que has configurado. Por defecto, será enviado en este formato:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## Slash Command: /caption

La extensión proporciona un comando slash `/caption` para usar en el chatbox o en scripts.

### Uso

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (opcional): Un prompt personalizado para el modelo de captioning. Solo soportado por fuentes multimodal.
- `quiet=true|false`: Si se establece a true, suprime el envío de un mensaje con caption al chat. Por defecto es false.
- `mesId=number`: Especifica un ID de mensaje para hacer caption a una imagen de un mensaje existente en lugar de subir una nueva.

Si no se proporciona `mesId`, el comando te pedirá que subas una imagen. Cuando `quiet` es false (por defecto), se enviará un nuevo mensaje con la imagen con caption al chat. El título generado puede usarse como entrada para otros comandos.

### Ejemplos

Hacer caption a una nueva imagen con la configuración por defecto:

```
/caption
```

Hacer caption a una nueva imagen con un prompt personalizado:

```
/caption Describe the main colours and shapes in this image
```

Hacer caption a una imagen del mensaje #5 sin enviar un nuevo mensaje:

```
/caption mesId=5 quiet=true
```

Hacer caption a una imagen del mensaje #10 con un prompt personalizado y luego [generar una nueva imagen](/extensions/Stable-Diffusion.md) basada en el título:

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine
```

## Fuente Local

Puedes cambiar el modelo en [config.yaml](/Administration/config-yaml.md#extensions-configuration). La clave se llama `extensions.models.captioning`. Introduce el ID del modelo de Hugging Face que quieras usar. El por defecto es `Xenova/vit-gpt2-image-captioning`.

Puedes usar cualquier modelo que soporte Image Captioning (`VisionEncoderDecoderModel` o pipeline "image-to-text"). El modelo necesita ser compatible con la librería transformers.js. Es decir, necesita pesos ONNX. Busca modelos con las etiquetas `ONNX` e `image-to-text`, o que tengan una carpeta llamada `onnx` llena de archivos `.onnx`.

## Fuente Multimodal

### Configuración general

- **Model**: Elige el modelo para Image Captioning. Las opciones varían según la API seleccionada.
- **Allow reverse proxy**: Activa para permitir usar un reverse proxy si está definido y es válido (OpenAI, Anthropic, Google, Mistral, xAI)

Las claves de API y URLs de endpoints para fuentes de captioning se gestionan en el panel [API Connections](/Usage/API_Connections/index.md). Configura la conexión primero en API Connections, luego selecciónala como tu fuente de captions en Captioning.

!!!warning Configúralo primero en el panel API Connections
Una última vez: configura la clave de API/dirección/puerto en **<i class="fa-solid fa-plug"></i> API Connections** y usa la conexión en Captioning.

Puedes seguir usando Claude para chats y Google AI Studio para Image Captioning, o lo que sea. Solo configúralos *ambos* en la pestaña 'API Connections' primero. Luego cambia tu fuente de Chat Completion a Claude y tu fuente de Captioning a Google AI Studio.
!!!

Para la mayoría de backends locales, necesitarás establecer algunas opciones en el backend del modelo en lugar de en SillyTavern. Si tu backend solo puede ejecutar un modelo a la vez y no soporta cambio automático, tienes varias opciones para usar diferentes modelos para chat y captioning:

1. **Secondary endpoints:** Usa la característica de secondary endpoint (ve la sección [Secondary endpoints](#secondary-endpoints) abajo) para conectar a un servidor de API diferente para captioning
2. **Multiple connection types:** Conecta a tu backend usando tanto Text Completion como Chat Completion en API Connections - esto te da dos conexiones separadas al mismo tipo de backend

### Fuentes

Para usar una de estas fuentes de caption, selecciona Multimodal en el dropdown Source.

* "Quiero el mejor captioning posible, y no me importa pagar por ello": Anthropic
* "No quiero pagar nada ni ejecutar nada": Google AI Studio free tier
* "Quiero hacer caption a imágenes localmente y que simplemente funcione": Ollama
* "Quiero mantener vivo el sueño de la IA local": [KoboldCpp](#koboldcpp)
* "Quiero quejarme cuando no funciona": ~~Extras~~

| Proveedor de API                  | Descripción                                                                                                                                                                   |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | Cloud, pagado, varios modelos GPT, Claude, y Gemini con capacidades de visión                                                                                                  |
| Claude                            | Cloud, pagado, todos los modelos Claude con capacidades de visión                                                                                                             |
| Cohere                            | Cloud, pagado, Aya Vision 8B / 32B                                                                                                                                          |
| Custom (OpenAI-compatible)        | Para APIs personalizadas compatibles con OpenAI, usa el modelo actualmente configurado en la pestaña API Connections                                                         |
| Electron Hub                      | Cloud, pagado, varios modelos con capacidades de visión.                                                                                                                     |
| Google AI Studio                  | Cloud, free tier luego pagado, Gemini Flash/Pro                                                                                                                             |
| Google Vertex AI                  | Cloud, free tier, Gemini Flash/Pro                                                                                                                                          |
| Groq                              | Cloud, llama-4 scout/maverick                                                                                                                                               |
| KoboldCpp                         | Local, debe configurarse el modelo en KoboldCpp                                                                                                                              |
| llama.cpp                         | Local, debe configurarse el modelo en llama.cpp                                                                                                                              |
| MistralAI                         | Cloud, pagado, pixtral-large, pixtral-12B, magistral, mistral-large, etc.                                                                                                   |
| Moonshot AI                       | Cloud, pagado, moonshot-vision                                                                                                                                              |
| NanoGPT                           | Cloud, pagado, varios modelos GPT/Claude/Google con capacidades de visión                                                                                                    |
| Ollama                            | Local, puede cambiar entre modelos disponibles y descargar [modelos de visión adicionales](https://ollama.com/search?c=vision) dentro de Captioning después de configurar en API Connections |
| OpenAI                            | Cloud, pagado, GPT-4 Vision, 4-turbo, 4o, 4o-mini                                                                                                                          |
| OpenRouter                        | Cloud, pagado (quizás opciones gratis), muchos modelos, elige de lo disponible dentro de Captioning después de configurar en API connections                                 |
| Pollinations                      | Cloud, gratis                                                                                                                                                                |
| Text Generation WebUI (oobabooga) | Local, debe configurarse el modelo en ooba                                                                                                                                   |
| vLLM                              | Local                                                                                                                                                                        |
| xAI (Grok)                        | Cloud, pagado, grok-vision                                                                                                                                                  |

### Secondary endpoints

Por defecto, la fuente Multimodal usa el endpoint primario configurado en la pestaña API Connections.
También puedes configurar un endpoint secundario específicamente para Image Captioning multimodal.

- Abre el panel **Image Captioning** en el panel **<i class="fa-solid fa-cubes"></i> Extensiones**.
- Selecciona "Multimodal" como la fuente de captioning y un proveedor de API preferido.
- Introduce una URL válida para el endpoint secundario en el campo "Secondary captioning endpoint URL".
- Marca la casilla "Use secondary URL" para habilitar el endpoint secundario.

!!!tip
No añadas `/v1` o `/chat/completions` al final de la URL. La extensión lo manejará automáticamente.
!!!

Esto solo está soportado por las siguientes APIs:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### Guías específicas de fuentes

#### KoboldCpp

Para información general sobre instalar y usar [KoboldCpp](https://github.com/LostRuins/koboldcpp), ve la [documentación de KoboldCpp](https://github.com/LostRuins/koboldcpp/wiki).

Para usar KoboldCpp para Image Captioning multimodal:

* obtén un modelo capaz de multimodal, entrenado para procesar prompts de texto e imagen al mismo tiempo.
* también obtén las proyecciones multimodal para el modelo. Estos pesos permiten que el modelo entienda cómo las partes de texto e imagen de la entrada se relacionan entre sí.
* carga el modelo y las proyecciones en la GUI de lanzamiento de KoboldCpp o en la interfaz de línea de comandos.

El modelo multimodal local original y clásico es LLaVA. Los archivos en formato GGUF para el modelo y las proyecciones están disponibles desde [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile). Para cargarlos desde la línea de comandos, establece el modelo y las proyecciones con las banderas `--model` y `--mmproj`. Por ejemplo:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

Algunos fine-tunes de LLaVA que puedes probar: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf), [xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf).

Puedes usar proyecciones multimodal para el modelo base del que tu particular fine-tune fue construido. Las proyecciones para algunos modelos base comunes están disponibles desde [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main).
