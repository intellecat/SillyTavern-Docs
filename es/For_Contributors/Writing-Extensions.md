---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# Extensiones de UI

Las extensiones de UI amplían la funcionalidad de SillyTavern al conectarse a sus eventos y API. Se ejecutan en un contexto de navegador y tienen acceso prácticamente sin restricciones al DOM, APIs de JavaScript y el contexto de SillyTavern. Las extensiones pueden modificar la UI, llamar a APIs internas e interactuar con datos de chat. Esta guía explica cómo crear tus propias extensiones (se requiere conocimiento de JavaScript).

!!!tip ¿Solo quieres instalar extensiones?
Ve aquí: [Extensiones](../extensions/index.md).
!!!

Para extender la funcionalidad del servidor Node.js, consulta la página [Server Plugins](./Server-Plugins.md).

**¿No puedes escribir JavaScript?**

* Considera [STscript](./st-script.md) como una alternativa más simple a escribir una extensión completa.
* Sigue el [Curso de MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) y vuelve cuando termines.

## Envío de extensiones

¿Quieres contribuir tus extensiones al [repositorio oficial de contenido](https://github.com/SillyTavern/SillyTavern-Content)? ¡Contáctanos!

Para asegurar que todas las extensiones sean seguras y fáciles de usar, tenemos algunos requisitos:

1. Tu extensión debe ser de código abierto y tener una licencia libre (ver [Choose a License](https://choosealicense.com/licenses/)). Si no estás seguro, AGPLv3 es una buena opción.
2. Las extensiones deben ser compatibles con la última versión de lanzamiento de SillyTavern. Por favor, prepárate para actualizar tu extensión si algo en el núcleo cambia.
3. Las extensiones deben estar bien documentadas. Esto incluye un archivo README con instrucciones de instalación, ejemplos de uso y una lista de características.
4. Las extensiones que requieren un complemento de servidor para funcionar no serán aceptadas.

## Ejemplos

Consulta ejemplos en vivo de extensiones simples de SillyTavern:

* <https://github.com/city-unit/st-extension-example> - plantilla de extensión básica. Muestra creación de manifest, importación de scripts locales, adición de un panel de UI de configuración y uso de configuración persistente de extensiones.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - una lista de todas las extensiones oficiales de SillyTavern en GitHub.

## Agrupamiento

Las extensiones también pueden utilizar agrupamiento para aislarse del resto de módulos y usar cualquier dependencia de NPM, incluidos marcos de UI como Vue, React, etc.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - repositorio plantilla de una extensión usando TypeScript y Webpack (sin React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - repositorio plantilla de una extensión básica usando React y Webpack.

Para usar importaciones relativas del paquete, es posible que necesites crear un envoltorio de importación. Aquí hay un ejemplo para Webpack:

```js
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// Import a function from 'script.js' module
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

Cada extensión debe tener una carpeta en `data/<user-handle>/extensions` y un archivo `manifest.json`, que contiene metadatos sobre la extensión y una ruta a un archivo de script JS que es el punto de entrada de la extensión.

Las extensiones descargables se montan en la carpeta `/scripts/extensions/third-party` cuando se sirven sobre HTTP, por lo que se deben usar importaciones relativas basadas en eso. Para facilitar el desarrollo local, considera colocar tu repositorio de extensión en la carpeta `/scripts/extensions/third-party` (la opción "Install for all users").

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    }
}
```

### Campos de manifest

* `display_name` es requerido. Se muestra en el menú "Manage Extensions".
* `loading_order` es opcional. Un número más alto se carga después.
* `js` es la referencia del archivo JS principal y es requerida.
* `css` es una referencia de archivo de estilo opcional.
* `author` es requerido. Debe contener el nombre o información de contacto del autor(es).
* `auto_update` se establece en `true` si la extensión debe actualizarse automáticamente cuando cambia la versión del paquete ST.
* `i18n` es un objeto opcional que especifica los idiomas admitidos y sus archivos JSON correspondientes (ver abajo).
* `dependencies` es una matriz opcional de cadenas que especifican otras **extensiones** de las que depende esta extensión.
* `generate_interceptor` es una cadena opcional que especifica el nombre de una función global llamada en solicitudes de generación de texto.
* `minimum_client_version` es una cadena opcional que especifica la versión mínima de SillyTavern requerida para que esta extensión funcione.

### Dependencias

Las extensiones también pueden depender de otras extensiones de SillyTavern. La extensión no se cargará si falta alguna de estas dependencias o está deshabilitada.

Las dependencias se especifican por su **nombre de carpeta** tal como aparece en el directorio `public/extensions`.

Ejemplos:

* Extensiones incorporadas: `"vectors"`, `"caption"`
* Extensiones de terceros: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### Campos deprecados

* `requires` es una matriz opcional de cadenas que especifican **módulos Extras** requeridos. La extensión no se cargará si la API de Extras conectada no proporciona todos los módulos enumerados.
* `optional` es una matriz opcional de cadenas que especifican **módulos Extras** opcionales. La extensión seguirá cargándose si faltan estos, y la extensión debe manejar su ausencia con gracia.

Para verificar qué módulos proporciona actualmente la API de Extras conectada, importa la matriz `modules` de `scripts/extensions.js`.

## Scripting

### Usando getContext

La función `getContext()` en el objeto global de `SillyTavern` te da acceso al contexto de SillyTavern, que es una colección de todos los objetos de estado de la aplicación principal, funciones útiles y utilidades.

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
context.groupId; // ID of the current group
// And many more...
```

Puedes encontrar la lista completa de propiedades y funciones disponibles en el [código fuente de SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js).

!!!
Si te faltan algunas de las funciones/propiedades en `getContext`, ¡por favor comunícate con los desarrolladores o envíanos un pull request!
!!!

### Bibliotecas compartidas

La mayoría de las bibliotecas npm utilizadas internamente por el frontend de SillyTavern se comparten en la propiedad `libs` del objeto global de `SillyTavern`.

* `lodash` - Librería de utilidades. [Docs](https://lodash.com/).
* `localforage` - Librería de almacenamiento del navegador. [Docs](https://localforage.github.io/localForage/).
* `Fuse` - Librería de búsqueda difusa. [Docs](https://www.fusejs.io/).
* `DOMPurify` - Librería de desinfección de HTML. [Docs](https://github.com/cure53/DOMPurify).
* `Handlebars` - Librería de plantillas. [Docs](https://handlebarsjs.com/).
* `moment` - Librería de manipulación de fecha/hora. [Docs](http://momentjs.com/).
* `showdown` - Librería convertidora de Markdown. [Docs](https://showdownjs.com/).

Puedes encontrar la lista completa de bibliotecas exportadas en el [código fuente de SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js).

**Ejemplo:** Usando la librería DOMPurify.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### Aviso de TypeScript

Si quieres acceso a autocompletar para todos los métodos en el objeto global de `SillyTavern` (y probablemente lo desees), incluidos `getContext()` y `libs`, debes agregar una declaración de módulo `.d.ts` de TypeScript. Esta declaración debe importar tipos globales del código fuente de SillyTavern, dependiendo de la ubicación de tu extensión. A continuación se muestra un ejemplo que funciona para ambos tipos de instalación: "all users" y "current user".

**global.d.ts** - coloca este archivo en la raíz de tu directorio de extensión (junto a `manifest.json`):

```ts
export {};

// 1. Import for user-scoped extensions
import '../../../../public/global';
// 2. Import for server-scoped extensions
import '../../../../global';

// Define additional types if needed...
declare global {
    // Add global type declarations here
}
```

### Importación desde otros archivos

!!!warning
Usar importaciones del código de SillyTavern no es confiable y puede romperse en cualquier momento si la estructura interna de los módulos de ST cambia. `getContext` proporciona una API más estable.
!!!

A menos que estés construyendo una extensión agrupada, puedes importar variables y funciones desde otros archivos JS.

Por ejemplo, este fragmento de código generará una respuesta desde la API seleccionada actualmente en segundo plano:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## Gestión de estado

### Configuración persistente

Cuando una extensión necesita persistir su estado, puede usar el objeto `extensionSettings` de la función `getContext()` para almacenar y recuperar datos. Una extensión puede almacenar cualquier dato serializable en JSON en el objeto de configuración y debe usar una clave única para evitar conflictos con otras extensiones.

Para persistir la configuración, usa la función `saveSettingsDebounced()`, que guardará la configuración en el servidor.

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// Define a unique identifier for your extension
const MODULE_NAME = 'my_extension';

// Define default settings
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

### Metadatos de chat

Para vincular algunos datos a un chat específico, puedes usar el objeto `chatMetadata` de la función `getContext()`. Este objeto te permite almacenar datos arbitrarios asociados con un chat, lo que puede ser útil para almacenar estado específico de extensión.

Para persistir los metadatos, usa la función `saveMetadata()`, que guardará los metadatos en el servidor.

!!!warning
No guardes la referencia a `chatMetadata` en una variable de larga duración, ya que la referencia cambiará cuando se cambiar el chat. Siempre usa `SillyTavern.getContext().chatMetadata` para acceder a los metadatos del chat actual.
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// Set some metadata for the current chat
chatMetadata['my_key'] = 'my_value';

// Get the metadata for the current chat
const value = chatMetadata['my_key'];

// Save the metadata to the server
await saveMetadata();
```

!!!tip
El evento `CHAT_CHANGED` se emite cuando se cambia el chat, por lo que puedes escuchar este evento para actualizar el estado de tu extensión en consecuencia. Ver más en la sección [Escuchar eventos](#escuchar-eventos).
!!!

### Tarjetas de caracteres

SillyTavern es totalmente compatible con la [Especificación de Tarjeta de Caracteres V2](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md), que permite almacenar datos arbitrarios en los datos JSON de la tarjeta de caracteres.

Esto es útil para extensiones que necesitan almacenar datos adicionales asociados con el personaje y hacerlo compartible al exportar la tarjeta de caracteres.

Para escribir datos en el campo de datos de [extensiones](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) de la tarjeta de caracteres, usa la función `writeExtensionField` de la función `getContext()`. Esta función toma un ID de carácter, una clave de cadena y un valor a escribir. El valor debe ser serializable en JSON.

!!!warning Rareza Adelante
A pesar de ser llamado `characterId`, no es un identificador único "real" sino más bien un índice del personaje en la matriz de `characters`.

El índice del carácter actual se proporciona mediante la propiedad `characterId` en el contexto. Si deseas escribir datos en el carácter seleccionado actualmente, usa `SillyTavern.getContext().characterId`. Si necesitas almacenar datos para otro carácter, encuentra el índice buscando el carácter en la matriz de `characters`.

**Precaución: `characterId` es `undefined` en chats de grupo o cuando no hay ningún carácter seleccionado!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// Write some data to the character card
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// Read the data back from the character card
const character = SillyTavern.getContext().characters[characterId];
// The data is stored in the `extensions` object of the character's data
const myData = character.data?.extensions?.my_extension_key;
```

### Presets de configuración

Los datos JSON arbitrarios pueden almacenarse en los presets de configuración de los tipos de API principales. Se exportarán e importarán junto con el JSON preestablecido, por lo que puedes usarlo para almacenar configuración específica de extensión para el preset. Los siguientes tipos de API admiten extensiones de datos en presets de configuración:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

Para leer o escribir los datos, primero debes obtener la instancia de PresetManager del contexto:

```js
const { getPresetManager } = SillyTavern.getContext();

// Get the preset manager for the current API type
const pm = getPresetManager();

// Write data to the preset extension field:
// - path: the path to the field in the preset data
// - value: the value to write
// - name (optional): the name of the preset to write to, defaults to the currently selected preset
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// Read data from the preset extension field:
// - path: the path to the field in the preset data
// - name (optional): the name of the preset to read from, defaults to the currently selected preset
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
Los eventos `PRESET_CHANGED` y `MAIN_API_CHANGED` se emiten cuando cambia el preset o se cambia la API principal, por lo que puedes escuchar estos eventos para actualizar el estado de tu extensión en consecuencia. Ver más en la sección [Escuchar eventos](#escuchar-eventos).
!!!

## Internacionalización

!!!
Para información general sobre proporcionar traducciones, consulta la página [Internacionalización](/For_Contributors/i18n.md).
!!!

Las extensiones pueden proporcionar cadenas localizadas adicionales para usar con las funciones `t`, `translate` y el atributo `data-i18n` en plantillas HTML.

Ver la lista de idiomas admitidos aquí (clave `lang`): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### Llamada directa a `addLocaleData`

Pasa un código de idioma y un objeto con las traducciones a la función `addLocaleData`. Las anulaciones de claves existentes *NO* están permitidas. Si el código de idioma pasado no es un idioma elegido actualmente, los datos se ignorarán silenciosamente.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### Vía el manifest de extensión

Agrega un objeto i18n con una lista de idiomas admitidos y sus rutas de archivos JSON correspondientes (relativas al directorio de tu extensión) al manifest.

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // rest of the fields
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## Registrar comandos slash (nueva forma)

Aunque `registerSlashCommand` aún existe para compatibilidad hacia atrás, los nuevos comandos slash ahora deben ser registrados a través de `SlashCommandParser.addCommandObject()` para proporcionar detalles extendidos sobre el comando y sus parámetros al analizador (y, a su vez, al autocompletar y la ayuda del comando).

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

Todos los comandos registrados pueden ser usados en [STscript](/For_Contributors/st-script.md) de cualquier forma posible.

## Eventos

### Escuchar eventos

Usa `eventSource.on(eventType, eventHandler)` para escuchar eventos:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

Los tipos de eventos principales son:

* `APP_READY`: la aplicación está completamente cargada y lista para usar. Se activará automáticamente cada vez que se adjunte un nuevo oyente después de que la aplicación esté lista.
* `MESSAGE_RECEIVED`: el mensaje LLM se genera y se registra en el objeto `chat` pero aún no se representa en la UI.
* `MESSAGE_SENT`: el mensaje es enviado por el usuario y registrado en el objeto `chat` pero aún no se representa en la UI.
* `USER_MESSAGE_RENDERED`: el mensaje enviado por un usuario se representa en la UI.
* `CHARACTER_MESSAGE_RENDERED`: el mensaje LLM generado se representa en la UI.
* `CHAT_CHANGED`: el chat ha sido cambiado (por ejemplo, se cambió a otro personaje, u otro chat fue cargado).
* `GENERATION_AFTER_COMMANDS`: la generación está a punto de comenzar después de procesar comandos slash.
* `GENERATION_STOPPED`: la generación fue detenida por el usuario.
* `GENERATION_ENDED`: la generación ha sido completada o ha tenido un error.
* `SETTINGS_UPDATED`: la configuración de la aplicación ha sido actualizada.

El resto se puede encontrar [en el código fuente](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

!!!info Datos de eventos
La forma en que cada evento pasa sus datos al oyente no es uniforme. Algunos eventos no emiten ningún dato; algunos pasan un objeto o un valor primitivo. Por favor, consulta el código fuente donde se emite el evento para ver qué datos pasa, o verifica con el depurador.
!!!

### Emitir eventos

Puedes producir cualquier evento de aplicación desde extensiones, incluidos eventos personalizados, llamando a `eventSource.emit(eventType, ...eventData)`:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Interceptores de Prompt

Los Interceptores de Prompt proporcionan una manera para que las extensiones realicen cualquier actividad como modificar los datos del chat, agregar inyecciones o abortar la generación antes de que se realice una solicitud de generación de texto.

Los interceptores de diferentes extensiones se ejecutan secuencialmente. El orden se determina por el campo `loading_order` en sus respectivos archivos `manifest.json`. Las extensiones con valores más bajos de `loading_order` se ejecutan primero. Si `loading_order` no se especifica, `display_name` se usa como alternativa. Si ninguno se especifica, el orden es indefinido.

### Registrando un Interceptor

Para definir un interceptor de prompt, agrega un campo `generate_interceptor` al archivo `manifest.json` de tu extensión. El valor debe ser el nombre de una función global que será llamada por SillyTavern.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Función Interceptor

La función `generate_interceptor` es una función global que será llamada en solicitudes de generación que no son ejecuciones secas. Debe ser definida en el alcance global (por ejemplo, `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) y puede retornar una `Promise` si necesita realizar cualquier operación asincrónica.

La función interceptor recibe los siguientes argumentos:

* `chat`: Una matriz de objetos de mensaje que representa el historial de chat que se utilizará para la construcción de prompt. Puedes modificar esta matriz directamente (por ejemplo, agregar, eliminar o alterar mensajes). Ten en cuenta que los mensajes son mutables, por lo que cualquier cambio que hagas en la matriz se reflejará en el historial de chat real. Si deseas que los cambios sean efímeros, usa `structuredClone` para crear una copia profunda del objeto del mensaje.
* `contextSize`: Un número que indica el tamaño de contexto actual (en tokens) calculado para la generación próxima.
* `abort`: Una función que, cuando se llama, señalizará para prevenir que la generación de texto continúe. Acepta un parámetro booleano que previene que cualquier interceptor subsecuente se ejecute si es `true`.
* `type`: Una cadena que indica el tipo o disparador de la generación (por ejemplo, `'quiet'`, `'regenerate'`, `'impersonate'`, `'swipe'`, etc.). Esto ayuda al interceptor a aplicar lógica condicionalmente basada en cómo fue iniciada la generación.

**Implementación de Ejemplo:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // Example: Add a system note before the last user message
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // Insert before the last message
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## Generación de texto

SillyTavern proporciona varias funciones para generar texto en diferentes contextos usando la API LLM seleccionada actualmente. Estas funciones te permiten generar texto en el contexto de un chat, generación sin contexto o con salidas estructuradas.

### Dentro del contexto de chat

La función `generateQuietPrompt()` se usa para generar texto en el contexto de un chat con un "quiet" prompt agregado (instrucción post-historial) en segundo plano (la salida no se representa en la UI). Esto es útil para generar texto sin interrumpir la experiencia del usuario mientras se mantiene el chat relevante y los datos del personaje intactos, como generar un resumen o un prompt de imagen.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### Generación sin contexto

La función `generateRaw()` se usa para generar texto sin ningún contexto de chat. Es útil cuando deseas tener control total sobre el proceso de construcción de prompt.

Acepta un `prompt` como una cadena de Completación de Texto o una matriz de objetos de Completación de Chat, construyendo la solicitud con un formato apropiado dependiendo del tipo de API seleccionado, por ejemplo, convirtiendo entre modos de chat/texto, aplicando formato instruct, etc. También puedes pasar un `systemPrompt` adicional y un `prefill` a la función para incluso más control sobre el proceso de generación.

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
In Chat Completion mode, will produce a prompt like this:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
In Text Completion mode (no instruct), will produce a prompt like this:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### Salidas Estructuradas

!!!info
Actualmente solo soportado por la API de Completación de Chat. La disponibilidad varía según la fuente seleccionada y el modelo. Si el modelo seleccionado no soporta salidas estructuradas, la generación fallará o devolverá un objeto vacío (`'{}'`). Verifica la documentación de la API específica que estés usando para ver si se soportan salidas estructuradas.
!!!

Puedes usar la función de salidas estructuradas para asegurar que el modelo produzca un objeto JSON válido que se adhiera a un [JSON Schema](https://json-schema.org/learn) proporcionado. Esto es útil para extensiones que requieren datos estructurados, como seguimiento de estado, clasificación de datos, etc.

Para usar salidas estructuradas, debes pasar un esquema JSON a `generateRaw()` o `generateQuietPrompt()`. El modelo entonces generará una respuesta que coincida con el esquema, y será devuelta como un objeto JSON stringificado.

!!!warning
Las salidas no se validan contra el esquema, debes manejar el análisis y validación de la salida generada tú mismo. Si el modelo no genera un objeto JSON válido, la función devolverá un objeto vacío (`'{}'`).

[Zod](https://zod.dev/json-schema) es una librería popular para generar y validar esquemas JSON. Su uso no será cubierto aquí.
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// Define a JSON schema for the expected output
const jsonSchema = {
    // Required: a name for the schema
    name: 'StoryStateModel',
    // Optional: a description of the schema
    description: 'A schema for a story state with location, plans, and memories.',
    // Optional:  the schema will be used in strict mode, meaning that only the fields defined in the schema will be allowed
    strict: true,
    // Required: a definition of the schema
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## Registrando macros personalizados

Puedes registrar macros personalizados que pueden ser usados en cualquier lugar donde se soporten sustituciones de macros, por ejemplo en los campos de la tarjeta de caracteres, comandos STscript, plantillas de prompt, etc.

Para registrar un macro, usa la función `registerMacro()` del objeto `SillyTavern.getContext()`. La función acepta un nombre de macro que debe ser una cadena única, y una cadena o una función que retorna una cadena. La función será llamada con una cadena de `nonce` única que será diferente entre cada llamada a `substituteParams`.

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

Cuando un macro personalizado ya no es necesario, elimínalo usando la función `unregisterMacro()`:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**Detalles importantes y limitaciones conocidas respecto a macros personalizados:**

1. Actualmente solo se soportan macros simples de sustitución de cadenas. Estamos trabajando en agregar soporte para macros más complejos en el futuro.
2. Los macros que usan funciones para proporcionar un valor *deben* ser síncronos. Retornar una `Promise` no funcionará.
3. No necesitas envolver el nombre del macro entre dobles llaves (`{{ }}`) cuando lo registres. SillyTavern lo hará por ti.
4. Como los macros son sustituciones simples de expresiones regulares, registrar muchos macros causará problemas de rendimiento, así que úsalos con moderación.

## Hacer solicitud de Extras

!!!warning
La API de Extras está deprecada. No se recomienda usarla en nuevas extensiones.
!!!

La función `doExtrasFetch()` te permite hacer solicitudes a tu servidor de API de Extras de SillyTavern.

Por ejemplo, para llamar al endpoint `/api/summarize`:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // Request body
    })
});
```

`getApiUrl()` devuelve la URL base del servidor de Extras.

La función `doExtrasFetch()`:

* Agrega los headers `Authorization` y `Bypass-Tunnel-Reminder`
* Maneja la obtención del resultado
* Devuelve el resultado (el objeto de respuesta)

Esto hace que sea fácil llamar a tu API de Extras desde tu extensión.

Puedes especificar:

* El método de solicitud: GET, POST, etc.
* Headers adicionales
* El cuerpo para solicitudes POST
* Cualquier otra opción de fetch
