---
route: /extensions/stable-diffusion/
templating: false
---

# Generación de imágenes

Utiliza APIs locales o basadas en la nube de Stable Diffusion, FLUX o DALL-E para generar imágenes.

Genera automáticamente imágenes como respuestas a tus mensajes para una inmersión total,
genera a partir del historial de chat e información del personaje desde el menú de varita mágica o comandos slash,
o usa el comando `/sd (cualquier_cosa_aqui)` en la barra de entrada del chat para crear una imagen con tu propio prompt.

La mayoría de configuraciones comunes de generación de Stable Diffusion se pueden personalizar dentro de la interfaz de SillyTavern.

- Compatible con [múltiples fuentes de generación de imágenes](#supported-sources), tanto locales como basadas en la nube
- Varios [modos de generación](#generation-modes) para personajes, escenas y prompts personalizados
- [Comandos slash](#how-to-generate-an-image) para generación fácil de imágenes dentro de chats
- [Modo interactivo](#use-interactive-mode) para activar la generación de imágenes basada en solicitudes en lenguaje natural
- Plantillas de prompt personalizables y [prefijos comunes](#common-prompt-prefix) para estilo y calidad consistentes
- [Prefijos de prompt específicos del personaje](#character-specific-prompt-prefix) para imágenes de personajes personalizadas
- [Presets de estilo](#styles) para cambiar rápidamente entre diferentes configuraciones de generación de imágenes
- Opciones de [visibilidad flexibles](#chat-message-visibility) para imágenes generadas en chat
- Integración avanzada de [ComfyUI](#comfyui-configuration) para flujos de trabajo altamente personalizables
- Capacidad de [ver todas las imágenes generadas](#view-all-generated-images) en una galería de personajes
- Función de [cambio de imágenes](#image-swipes) para regenerar imágenes manteniendo el mismo prompt
- Opciones para [editar prompts antes de la generación](#edit-prompts-before-generation) y [extender prompts de modo libre](#extend-free-mode-prompts)
- Integración con [llamada de funciones](#use-function-tool) de IA para detección automática de generación de imágenes

## Fuentes compatibles

| Fuente                                                                                            | Observaciones                                                                                         |
|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [AI.ML API](https://aimlapi.com/)                                                                 | Nube, pagado                                                                                     |
| [Black Forest Labs](https://bfl.ai/)                                                              | Nube, pagado                                                                                     |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)                                              | Local, código abierto (GPL3), gratuito, ver [ComfyUI Configuration](#comfyui-configuration). |
| [Draw Things](https://drawthings.ai/)                                                             | Local, Mac/iOS, gratuito                                                                  |
| [Electron Hub](https://electronhub.ai/)                                                           | Nube, pagado                                                                                     |
| [FAL.AI](https://fal.ai/)                                                                         | Nube, pagado                                                                                     |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | Nube, pagado. Series del modelo Imagen. AI Studio solo admite el modelo Imagen 3.0 002.         |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index)                         | Nube, gratuito                                                                           |
| [NanoGPT](https://nano-gpt.com/)                                                                  | Nube, pagado                                                                                     |
| [NovelAI Diffusion](https://novelai.net/)                                                         | Nube, requiere una suscripción activa                                                          |
| [OpenAI](https://platform.openai.com/)                                                            | Nube, pagado                                                                                     |
| [Pollinations](https://pollinations.ai/)                                                          | Nube, código abierto (MIT), gratuito                                                        |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic)                                   | Local, código abierto (AGPL3), gratuito                                                      |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)                           | Deprecated, no recomendado                                                                     |
| [Stability AI](https://platform.stability.ai/)                                                    | Nube, pagado                                                                                     |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | Local, código abierto (AGPL3), gratuito                                                      |
| [Stable Horde](https://stablehorde.net/)                                                          | Nube, código abierto (AGPL3), gratuito                                                       |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models)                        | Nube                                                                                           |
| [x.AI](https://x.ai/)                                                                             | Nube, pagado                                                                                     |

## Modos de generación

| Elemento del menú de varita | Argumento del comando slash | Descripción                                          | Observaciones                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | Un retrato de cuerpo completo del personaje actual. | -                                     |
| "Your Face"        | `face`                 | Un retrato de primer plano del personaje actual.  | Fuerza una relación de aspecto de retrato.       |
| "Me"               | `me`                   | Un retrato de la persona del usuario.                | -                                     |
| "The Whole Story"  | `scene`                | Un resumen visual de los eventos del chat.             | -                                     |
| "The Last Message" | `last`                 | Un resumen visual del último mensaje del chat.       | -                                     |
| "Raw Last Message" | `raw_last`             | Último mensaje utilizado como prompt literalmente.        | -                                     |
| "Background"       | `background`           | Un fondo de chat basado en el contexto de la historia.      | Fuerza una relación de aspecto de paisaje ancho. |

## Cómo generar una imagen

1. Utiliza el elemento "Image Generation" en el menú de contexto de extensiones (varita mágica).
2. Escribe un comando slash `/sd (argumento)` con un argumento de la tabla de modos de generación. Cualquier otra cosa activaría un "modo libre" para que SD genere lo que indicaste. Ejemplo: `/sd manzana árbol` generaría una imagen de un manzano.
3. Busca un ícono de pincel en las acciones de contexto para mensajes de chat. Esto forzará el modo "Raw Message" para el mensaje seleccionado.

Cada modo de generación excepto mensaje sin procesar y modo libre activará una generación de prompt usando tu API de generación principal actualmente seleccionada para convertir el contexto del chat en un prompt de SD.
Puedes configurar la plantilla de instrucciones para generar prompts para cada modo de generación usando el cajón de configuración "SD Prompt Templates" en el panel de extensiones.

### Consejos y trucos para el uso del comando `/sd`

#### Ver todas las imágenes generadas

Para ver todas las imágenes guardadas para un personaje (incluyendo otros chats), abre una galería desde un menú desplegable "More..." en un panel de información del personaje, o usa un comando slash `/show-gallery`.

#### Especifica un prompt negativo

Usa un argumento nombrado `negative` antes del prompt para aplicar un prompt negativo específico para esta generación.

```stscript
/sd negative="fries" cute tater farmer holding a tayto in a spud-field
```

#### Incluye un prefijo específico del personaje

Usa una macro especial `{{charPrefix}}` en modo de prompt libre para incluir prefijos de prompt positivos y negativos (si se definen) para el personaje actual.

```stscript
/sd {{charPrefix}}, riding a bike
```

#### Suprime un mensaje de chat

Puedes evitar publicar una imagen generada en el chat pasando un argumento nombrado `quiet=true`. La imagen aún se añadirá a la galería del personaje, y el comando producirá una URL relativa a la imagen que puede ser consumida por otros comandos.

El ejemplo a continuación enviará la imagen generada usando Markdown como una persona del usuario.

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### Cambio de imágenes

El cambio de imágenes permite regenerar la imagen manteniendo el mismo prompt. Si se establece una semilla fija, se aleatorizará para la próxima generación.

Para ciclar a través de imágenes, coloca el cursor del ratón (toca en dispositivo móvil) sobre una imagen generada para revelar botones de flecha y un contador de cambios. Al tocar la flecha derecha en la imagen más reciente, se generará una nueva.

*'Swipes' aquí es solo un nombre, no intentes el gesto de deslizamiento real, ya que esto regenerará el mensaje mismo, no la imagen adjunta.*

## Opciones

### Editar prompts antes de la generación

Permite editar manualmente los prompts generados automáticamente antes de enviarlos a la API de Stable Diffusion.

### Usar herramienta de función

Usa [llamada de funciones](/extensions/Stable-Diffusion.md) para detectar automáticamente la intención de generar una imagen.

**Requisitos:**

1. Debe tener la generación de imágenes configurada con una fuente compatible.
2. Debe usar un modelo de Chat Completion API compatible y tener habilitada la llamada a herramientas de función en la configuración de respuesta de IA.
3. La opción "Use function tool" debe estar habilitada en la configuración de Image Generation.
4. El usuario debe expresar una intención de generar una imagen en el mensaje de chat, p. ej. "Send me a picture of a cat".

!!!warning
El modo interactivo no se activará cuando la herramienta de función esté habilitada.
!!!

### Usar modo interactivo

Permite activar la generación de una imagen en lugar de texto como respuesta a un mensaje de usuario que sigue el patrón especial:

1. Contiene uno de los siguientes verbos: send, mail, imagine, generate, make, create, draw, paint, render
2. Seguido de uno de los siguientes sustantivos (no más de 10 caracteres de distancia): pic, picture, image, drawing, painting, photo, photograph
3. Seguido de un tema objetivo de generación de imágenes, que podría estar opcionalmente precedido por frases como "of a" u "of this".

Ejemplos de solicitudes válidas y temas capturados:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

Algunos temas especiales activan un modo de generación predefinido:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### Extender prompts de modo libre

Al usar el modo interactivo del comando slash, extiende automáticamente las descripciones del tema de generación en modo libre solicitándole a tu API principal.

### Ajustar resoluciones auto-ajustadas

Ajusta las solicitudes de generación de imágenes con una relación de aspecto forzada (retratos, fondos) a la resolución más cercana conocida, mientras intentas preservar los conteos de píxeles absolutos. Consulta el desplegable "Resolution" para ver la lista de opciones posibles.

**Recomendado para modelos SDXL**.

## Prefijo de prompt común

!!!tip Pro Tip
Usa la macro `{prompt}` para especificar exactamente dónde se insertará el prompt generado.
!!!

Se añade antes de cada prompt generado o en modo libre. Se usa comúnmente para establecer el estilo general de la imagen.

Ejemplo: `best quality, anime lineart`.

## Prompt negativo

Características de la imagen que no quieres que estén presentes en la salida.

Ejemplo: `bad quality, watermark`.

## Prefijo de prompt específico del personaje

!!!tip Pro Tip
Si la fuente de generación lo permite, también puedes usar LoRAs/embeddings aquí, por ejemplo: `<lora:DonaldDuck:1>`.
!!!

Cualquier característica que describa el personaje actualmente seleccionado. Se añadirá después de un prefijo común.

Ejemplo: `female, green eyes, brown hair, pink shirt`.

También puedes especificar un prefijo de prompt negativo para cualquier contenido no deseado. Se combinará con el prompt negativo general.

Limitaciones:
1. Funciona solo en chats uno a uno. No se usará en grupos.
2. No se usará para fondos y generaciones en modo libre.

!!! Note
Para forzar la inclusión de un prefijo de personaje en un prompt en modo libre, usa la macro `{{charPrefix}}` en cualquier lugar del prompt.
!!!

Si deseas compartir los prefijos con otros, marca la casilla "Shareable". Esto los guardará con los datos del personaje, en lugar de tus configuraciones locales.

## Estilos

Utiliza esto para guardar y restaurar rápidamente tus presets de estilo/calidad favoritos para usarlos después o al cambiar entre modelos. Lo siguiente se incluye en el preset de estilo:

1. Prefijo de Prompt Común
2. Prompt Negativo

También puedes cambiar entre estilos usando el comando `/imagine-style` (o `/sd-style` o `/img-style`).

## Visibilidad de Mensaje de Chat

Las imágenes generadas insertadas en el chat se ocultan en los prompts de API principales de forma predeterminada, pero esto puede anularse individualmente por iniciador de generación (ícono de "Magic wand", comando slash, modo interactivo). Esto se puede usar para hacer la experiencia más inmersiva permitiendo que los personajes "reconozcan" las imágenes. Los modelos multimodales en Chat Completions API también pueden 'ver' las imágenes si "Send inline images" está habilitado.

Un mensaje de texto se puede personalizar cambiando el "Chat Message Template" bajo Image Prompt Templates. Se pueden usar todos los macros regulares en esta plantilla, más una macro especial `{{prompt}}` para especificar dónde se añadirá el prompt de imagen.

## Configuración de ComfyUI

[ComfyUI](https://github.com/comfyanonymous/ComfyUI) es una opción rápida y muy flexible para la generación de imágenes.

Si estás familiarizado con ComfyUI, el resumen es: haz tu flujo de trabajo en ComfyUI, descárgalo **en formato API**, y pégalo en el Editor de Flujo de Trabajo de ComfyUI de SillyTavern. ST enviará tu flujo de trabajo a la API de ComfyUI y obtendrás una imagen en tu chat. Pero con gran poder viene gran responsabilidad, y la responsabilidad principal es insertar placeholders en tu JSON de flujo de trabajo para que puedas cambiar configuraciones desde SillyTavern.

Si no estás familiarizado con ComfyUI, aún puedes usar ComfyUI para generar imágenes en SillyTavern usando el flujo de trabajo predeterminado. Más tarde, cuando quieras gran poder, puedes aprender cómo usar ComfyUI...

### Controles

Este panel te permite configurar y administrar tu integración de ComfyUI con SillyTavern.

Ingresa la URL de tu servidor ComfyUI en el campo de entrada **ComfyUI URL**. El valor predeterminado es `http://127.0.0.1:8188`.
Si estás usando [SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI), el puerto predeterminado para el
[servidor ComfyUI administrado](https://github.com/mcmonkeyprojects/SwarmUI/blob/master/src/BuiltinExtensions/ComfyUIBackend/README.md) es `7821`,
20 puertos más alto que el puerto predeterminado para SwarmUI.

Después de ingresar la URL, elige <i class="fa-solid fa-check"></i> **Connect** para validar y establecer una conexión. El servidor ComfyUI debe ser accesible desde la máquina host de SillyTavern.

### Gestión de Flujo de Trabajo

Selecciona un flujo de trabajo de ComfyUI desde el menú desplegable. Se proporcionan dos flujos de trabajo predeterminados:

- Default_Comfy_Workflow.json: Un flujo de trabajo básico de texto a imagen que admite las configuraciones de generación de imágenes más comunes.
- Char_Avatar_Comfy_Workflow.json: Un flujo de trabajo de muestra de imagen a imagen que usa el avatar del personaje, más el prompt, para generar una imagen.

Utiliza los siguientes botones para administrar tus flujos de trabajo:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor** para ver y modificar el flujo de trabajo seleccionado.
- <i class="fa-solid fa-plus"></i> **Create new workflow** para crear un nuevo flujo de trabajo con un nombre personalizado.
- <i class="fa-solid fa-trash-can"></i> **Delete workflow** para eliminar el flujo de trabajo seleccionado.

### Editor de Flujo de Trabajo

El Editor de Flujo de Trabajo de ComfyUI te permite ver y modificar flujos de trabajo de ComfyUI para usar con SillyTavern.

El componente principal del editor es un área de texto grande donde puedes insertar o editar tu flujo de trabajo de ComfyUI en formato JSON.

Para añadir un flujo de trabajo de ComfyUI al editor, sigue estos pasos:

1. Habilita 'Dev Mode' en la configuración de ComfyUI.
2. Usa la opción 'Save (API Format)' en ComfyUI para descargar los datos JSON.
3. Crea un nuevo flujo de trabajo en SillyTavern y abre el editor.
4. Pega los datos JSON descargados en el área de texto.
5. Reemplaza valores específicos con placeholders según sea necesario para tu caso de uso.

!!!tip Tips
Puedes añadir el archivo JSON en formato API directamente al directorio `data/default-user/user/workflows` en tu instalación de SillyTavern. Esto te ahorrará los pasos 3 y 4.

Retén el archivo JSON original. Si necesitas abrir el flujo de trabajo de nuevo en ComfyUI para hacer cambios, es mucho más conveniente editar el archivo original que el que tiene todos los placeholders.
!!!

### Placeholders

El editor proporciona una lista de placeholders predefinidos que se pueden usar en tu JSON de flujo de trabajo. Estos placeholders se reemplazan con valores dinámicos cuando se ejecuta el flujo de trabajo en SillyTavern.

Los placeholders marcados con ✅ están presentes en tu JSON de flujo de trabajo. Los placeholders marcados con ❌ no están presentes en tu JSON de flujo de trabajo. Puedes añadir estos placeholders a tu JSON de flujo de trabajo según sea necesario. No necesitas añadir todos los placeholders, solo los que tu flujo de trabajo usa y quieres reemplazar dinámicamente.

#### Prompts

Los placeholders `%prompt%` y `%negative_prompt%` se usan para insertar los prompts de generación de imágenes en el flujo de trabajo. Estos contienen los prompts finales generados por SillyTavern, incluyendo el prompt generado para tu modo `/sd` elegido, el prefijo de prompt común, prompt negativo, y prefijo de prompt específico del personaje.

Por ejemplo, es posible que hayas probado tu flujo de trabajo con un prompt como "forest elf" en ComfyUI. Para usar este flujo de trabajo en SillyTavern, puedes reemplazar el prompt "forest elf" con el placeholder `%prompt%`:

+++ JSON con placeholder
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "%prompt%"
    }
}
```
+++ JSON Original
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "forest elf"
    }
}
```
+++

Ten en cuenta que el placeholder está envuelto en comillas dobles. Esto es importante para el formato JSON, y requerido por el sistema de reemplazo de placeholders de SillyTavern. Incluso para números, debes usar comillas dobles en el JSON de plantilla.

A veces el prompt (u otro valor) no aparece donde podrías esperarlo. ComfyUI eliminará nodos del flujo de trabajo de versión API si no son necesarios para que el flujo de trabajo funcione en modo API.

Por ejemplo, este flujo de trabajo usa un [nodo cargador de etiqueta LoRA](https://github.com/badjeff/comfyui_lora_tag_loader) con un primitivo de prompt para que el flujo de trabajo sea más claro en modo UI:

![Primitivo de prompt y cargador de LoRA](/static/extensions/sd-comfy-prompt-primitive.png)

El nodo primitivo de prompt se eliminará de la versión API del flujo de trabajo, por lo que insertas el placeholder en el nodo LoraTagLoader. Encuentra el texto "apple tree" en el flujo de trabajo y reemplázalo con el placeholder `%prompt%`:

+++ JSON con placeholder
```json
{
    "inputs": {
      "text": "%prompt%",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++ JSON Original
```json
{
    "inputs": {
      "text": "apple tree",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++

En algunos casos, es posible que necesites hacer varios reemplazos en el JSON del flujo de trabajo, incluso si el prompt aparece solo una vez en la UI.

#### Modelo

El placeholder `%model%` insertará el valor del modelo seleccionado en la configuración de generación de imágenes.

Un ejemplo del flujo de trabajo texto-a-imagen predeterminado:

+++ JSON con placeholder
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "%model%"
    }
}
```
+++ JSON Original
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "sd15.safetensors"
    }
}
```
+++

Para cargar UNets cuantizados con GGUF, usa un nodo [UNet Loader (GGUF)](https://github.com/city96/ComfyUI-GGUF) en tu flujo de trabajo,
elige un modelo `GGUF` en el desplegable de modelos de SillyTavern, y usa el placeholder `%model%` en la configuración del nodo así:

+++ JSON con placeholder
```json
{
    "inputs": {
      "unet_name": "%model%"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++ JSON Original
```json
{
    "inputs": {
      "unet_name": "flux1-dev-Q4_0.gguf"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++

!!!info Si tienes tipos de modelo distintos de los habituales en ComfyUI
Stable Diffusion checkpoints, SD UNets, y UNets cuantizados con GGUF todos aparecen en el desplegable de Modelo.
Los modelos de un tipo no funcionarán con flujos de trabajo/nodos cargadores que esperan otro tipo.
Si elige un tipo de modelo incompatible en ST, ComfyUI reportará un problema con el nodo cargador.
!!!

#### Imágenes de avatar

Usa los placeholders `%user_avatar%` y `%char_avatar%` para incluir los avatares del usuario y del personaje en el flujo de trabajo. Estos placeholders se reemplazan con los datos PNG de los avatares cuando se ejecuta el flujo de trabajo. Los datos de imagen se codifican en formato base64, por lo que debes decodificarlos en tu flujo de trabajo. Una opción popular para esta tarea es el nodo [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes).

En este ejemplo, el avatar del personaje se carga con un nodo `Load Image (Base64)`. También usa un nodo Image Resize para redimensionar la imagen al tamaño que se especifique en la configuración de generación de imágenes:

![Cargar imagen de cadena base64 y redimensionar](/static/extensions/sd-comfy-load-b64.png)

Inserta los placeholders `%char_avatar%`, `%width%`, y `%height%` en el JSON para los nodos Load Image (Base64) e Image Resize:

```json
{
    "97": {
        "inputs": {
            "image": "%char_avatar%"
        },
        "class_type": "ETN_LoadImageBase64",
        "_meta": {"title": "Load Image (Base64)"}
    },
    "98": {
        "inputs": {
            "mode": "resize",
            "resize_width": "%width%",
            "resize_height": "%height%",
            "image": ["97", 0]
        },
        "class_type": "Image Resize",
        "_meta": {"title": "Resize image"}
    }
}
```

Para obtener una cadena de imagen codificada en base64 para probar tu flujo de trabajo en ComfyUI, usa cualquier herramienta en línea que convierta imágenes a cadenas base64.
Aquí hay una cadena de ejemplo que puedes usar para pruebas iniciales: [sd-comfy-base64-test-string.txt](/static/extensions/sd-comfy-base64-test-string.txt).

#### Otros placeholders

La mayoría de otros placeholders usan los valores de los controles correspondientes en la configuración de generación de imágenes, o los valores que especificas con el comando `/sd`:

- `%vae%`, pero la mayoría de modelos SD incluyen un VAE por lo que los flujos de trabajo predeterminados no usan este placeholder. Úsalo con flujos de trabajo personalizados para cargar un VAE junto a un UNet, anular el VAE predeterminado, etc.
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`: para el flujo de trabajo de muestra de imagen a imagen, varía la cantidad de denoise entre aproximadamente 0.5 (cambios apenas perceptibles a la imagen de origen) y 1.0 (una imagen completamente diferente como si no se hubiera usado imagen de origen). No se usa en el flujo de trabajo predeterminado de texto a imagen porque no hay punto en usar un valor diferente a 1.0 para texto a imagen.
- `%clip_skip%`: no se usa por los flujos de trabajo predeterminados pero disponible para flujos de trabajo personalizados.

El placeholder `%seed%` insertará el valor de seed del control si has especificado uno. Si estableces la seed en `-1`, SillyTavern generará una nueva seed aleatoria para cada imagen en `%seed%`.

#### Placeholders personalizados

Puedes añadir placeholders personalizados a tu flujo de trabajo:

1. Busca la sección "Custom" debajo de los placeholders predefinidos.
2. Haz clic en el botón "+" para añadir un nuevo placeholder personalizado.
3. Ingresa un nombre para el placeholder en el campo `find`.
4. Ingresa el valor que quieres reemplazar el placeholder con en el campo `replace`.

Los placeholders personalizados aparecerán en una lista separada debajo de los predefinidos.

Por ejemplo, podrías reemplazar el prefijo "SillyTavern" para nombres de archivo de imagen guardada en el flujo de trabajo predeterminado con un placeholder personalizado. Añade un nuevo placeholder personalizado con `find` establecido en `filename_prefix` y `replace` establecido en `ServiceTesnor`. Inserta el nuevo placeholder `%filename_prefix%` en tu JSON de flujo de trabajo. Ahora puedes cambiar el prefijo de nombre de archivo de SillyTavern a ServiceTesnor cambiando el valor del placeholder personalizado.

+++ JSON con placeholder
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "%filename_prefix%",
        "images": ["8", 0]
    }
}
```
+++ JSON Original
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "SillyTavern",
        "images": ["8", 0]
    }
}
```
+++

### Trucos de Comfy

Lee toda la información general en esta página para que estés familiarizado con las opciones de generación de imágenes. Opciones como estilos intercambiables y prefijos de prompt comunes, cuando se combinan con la flexibilidad total de los flujos de trabajo de ComfyUI, te permiten crear una amplia variedad de configuraciones de generación de imágenes.

#### Cargar LoRAs

Usa un nodo cargador de etiqueta LoRA (como [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader)) para cargar cualquier LoRA especificada en el prompt.
Ahora puedes añadir tantos LoRAs como quieras a tu prompt con etiquetas como `<lora:CroissantStyle:0.8>`, y se cargarán en tu flujo de trabajo.
Esto también hará que el "pro-tip" de usar LoRAs en [prefijos de prompt específicos del personaje](#character-specific-prompt-prefix) funcione con ComfyUI.

#### Configurar valores de flujo de trabajo desde estilos o slash-commands

Puedes usar macros en valores de placeholder personalizado. Como ejemplo práctico,
digamos que a veces quieres generar imágenes sin un fondo, y te gustaría que esto fuera intercambiable con un
slash-command o estilo de imagen. Aquí está cómo podrías hacerlo:

1. Haz un flujo de trabajo de ComfyUI que elimine el fondo de la imagen, o no, dependiendo del valor de una entrada
2. Usa un placeholder personalizado para establecer el valor de esa entrada, pero usa `{{getvar::remove_background}}` como el valor de reemplazo
3. Ahora puedes establecer el valor de `remove_background` con `/setvar key=remove_background true` o `/setvar key=remove_background false` antes de generar una imagen
4. El flujo de trabajo usará el valor que estableciste para determinar si eliminar el fondo
5. Haz un estilo de imagen "No background" con prefijo de prompt común `{{setvar::remove_background::true}}`
6. Usa el control de estilo o `/imagine-style No background` para establecer el valor de `remove_background` en `true` antes de generar una imagen
