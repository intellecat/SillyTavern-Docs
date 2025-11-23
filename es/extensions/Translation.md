---
route: /extensions/translation/
templating: false
---

# Traducción de Chat

## Descripción General

La extensión Chat Translation permite la traducción en tiempo real de mensajes de chat entre diferentes idiomas utilizando varios proveedores de traducción. Admite tanto modos de traducción manual como automática.

![Mensaje de personaje traducido del inglés al chino usando el botón de acción de mensaje 'Translate Message/翻譯訊息'](../static/extensions/translation/sensei.png)

+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## Uso

Todas las formas de traducir mensajes de chat:

**<i class="fa-solid fa-language"></i> Translate Chat** botón en el menú **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions**

- Traduce todo el historial de chat a la vez

**<i class="fa-solid fa-keyboard"></i> Translate Input** botón en el menú **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions**

- Traduce solo el texto de entrada actual
- Útil antes de enviar un mensaje

**<i class="fa-solid fa-language"></i> Translate Message** icono en la barra de herramientas **<i class="fa-solid fa-ellipsis"></i> Message Actions** de cualquier mensaje

- Haz clic para traducir solo ese mensaje
- Haz clic de nuevo para revertir al texto original

**Modo automático** configuración en el panel **Chat Translation** del panel **<i class="fa-solid fa-cubes"></i> Extensions**

- Traduce automáticamente las entradas de usuario, respuestas de IA, o ambas

Comando **/translate**

- Usa `/translate [target=language_code] text` para traducir texto

## Configuración

Las opciones de configuración están disponibles en el panel **Chat Translation** del panel **<i class="fa-solid fa-cubes"></i> Extensions**.

#### Proveedor

- Elige tu servicio de traducción preferido [translation service](#translation-providers)
- Haz clic en el icono **<i class="fa-solid fa-key"></i> API Key**, si aparece, para introducir una clave API
- Haz clic en el icono **<i class="fa-solid fa-link"></i> Custom URL**, si aparece, para introducir una URL API personalizada

#### Idioma de Destino

Selecciona el idioma en el que deseas escribir tus mensajes, o leer respuestas de IA.

#### Modo Automático

Configura el comportamiento de traducción automática.

- **Ninguno**: Sin traducción automática
- **Translate responses**: Traduce automáticamente las respuestas de IA al idioma de destino
- **Translate inputs**: Traduce automáticamente las entradas de usuario al inglés
- **Translate both**: Traduce tanto las entradas de usuario como las respuestas de IA

#### Limpiar Traducciones

El botón **<i class="fa-solid fa-trash-can"></i> Clear Translations** elimina todas las traducciones de los mensajes en el chat actual. Los mensajes originales se conservan.

### Ejemplo de Configuración: Chat de Chino a Inglés

Para configurar un flujo de trabajo donde un usuario que habla chino pueda chatear en chino con una IA que opera en inglés:

1. Establece el Modo Automático en "Translate both"
2. Establece el Idioma de Destino en "Chinese (Simplified)" o "Chinese (Traditional)"
3. Elige un proveedor de traducción con buena detección automática de idioma (por ejemplo, Google o DeepL)

Esta configuración:

- Traduce la entrada en chino del usuario al inglés para la IA
- Traduce las respuestas en inglés de la IA de vuelta al chino para el usuario

Esta configuración se basa en la detección automática del idioma para la entrada. Para un control más preciso, futuras actualizaciones pueden incluir la selección explícita del idioma de origen.

## Proveedores de Traducción

**:icon-cloud:** Basado en la nube
**<i class="fa-solid fa-link"></i>** Local, URL personalizada
**<i class="fa-solid fa-key"></i>** Requiere clave API

| Proveedor                                                           | Ubicación                                                                     | Características                                                                                               |
|---------------------------------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| [LibreTranslate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | Alternativa auto-hospedada (AGPL-3.0) a servicios de traducción propietarios, con nivel Pro alojado en la nube     |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                  | Ampliamente utilizado, compatible con muchos idiomas, buena precisión                                                    |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                              | Front-end alternativo para Google Translate, código abierto (AGPL-3.0), enfocado en privacidad                    |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                  | Traducciones de alta calidad, especialmente para idiomas europeos                                           |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                              | Proxy DeepL auto-hospedado, código abierto (MIT), gratuito pero el proxying de DeepL Pro requiere clave API de DeepL         |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                  | Servicio de traducción de Microsoft, se integra con servicios de Azure                                        |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                              | Front-end auto-hospedado para Google Translate y otros proveedores, enfocado en privacidad, código abierto (AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                  | Bueno para ruso e idiomas de Europa del Este                                                        |

### Configuración específica de DeepL

- Niveles de formalidad disponibles para German, French, Italian, Spanish, Dutch, Japanese, y Russian
- Configura a través de `deepl.formality` en [config.yaml](/Administration/config-yaml.md#deepl-configuration)

## Comandos de Barra

Utiliza el comando `/translate` para traducciones rápidas. Sintaxis: `/translate [target=language_code] text`. Si no se proporciona el idioma de destino, se utilizará el valor de la configuración de la extensión.

### Uso Básico

Traduce texto al idioma de destino actual y muéstralo en una ventana emergente:

```
/translate Welcome to the Tavern | /echo
```

![Ventana emergente en chino (Simplificado), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

Traduce el texto al español y añádelo al chat:

```
/translate target=es Hello world | /send
```

![Mensaje de usuario en español, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### Pruebas, traducción en cadena, localización

Solicita al usuario un mensaje e idioma, traduce el mensaje a ese idioma, luego retradúcelo al idioma de destino configurado y muestra ambas traducciones en una ventana emergente. Este ejemplo utiliza los comandos `/input` y `/buttons` para recopilar entrada del usuario:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> |
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> |
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target |
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

Esto es útil para verificar la calidad de una traducción a un idioma que no hablas, antes de escribirla en un lugar importante.

![Ventana emergente, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Ventana emergente, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

Los controles de la interfaz se muestran en la configuración regional actual, independientemente del idioma de destino configurado.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Diálogo de entrada, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Diálogo de botones, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Ventana emergente, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

La detección del idioma de entrada es relativamente efectiva en los siguientes ejemplos:

![Ventana emergente, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Ventana emergente, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Ventana emergente, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## Notas Técnicas

- Se admite codificación UTF-8, caracteres especiales y emojis
- Maneja mensajes grandes dividiéndolos en fragmentos cuando es necesario
- Preserva el formato e imágenes incrustadas en los mensajes
- Almacena en caché las traducciones para evitar llamadas API redundantes

### Idioma de entrada de IA

`internal_language` controla el idioma en el cual los mensajes de usuario se traducen automáticamente antes de ser enviados a la IA. Está codificado como 'en' en la configuración predeterminada y no se puede cambiar a través de la interfaz. Por lo tanto, el idioma de destino de la traducción para mensajes *a la IA* es siempre inglés. Las pruebas anteriores mostraron que el rendimiento de la IA era mejor al recibir mensajes en inglés, pero esto puede cambiar a medida que se entrenan más LLM en datos de idiomas más variados. Supongo que uno podría cambiar `internal_language` en `settings.json` y averiguarlo.

### Manejo de variantes de chino

La extensión admite tanto chino simplificado como tradicional, pero no todos los proveedores de traducción lo hacen. La interfaz presenta estos como 'Chinese (Simplified)' y 'Chinese (Traditional)' respectivamente, con códigos de idioma 'zh-CN' y 'zh-TW'. Se asignan a los siguientes códigos de idioma para proveedores de traducción:

* LibreTranslate: 'zh-CN' a 'zh' y 'zh-TW' a 'zt'.
* DeepL y DeepLX: ambas variantes a 'ZH'.
* Bing: 'zh-CN' a 'zh-Hans', 'zh-TW' tal cual.
* Otros proveedores utilizan 'zh-CN' y 'zh-TW' como se proporcionan.

### Límites de Longitud de Texto

Algunos proveedores tienen límites de caracteres por solicitud:

- Yandex: 5000 caracteres
- DeepLX: 1500 caracteres
- Bing: 1000 caracteres
- Google: 5000 caracteres

Los textos más largos se dividen automáticamente en fragmentos para la traducción.
