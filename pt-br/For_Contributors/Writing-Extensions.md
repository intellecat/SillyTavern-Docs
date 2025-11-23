---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# Extensões de UI

As extensões de UI expandem a funcionalidade do SillyTavern conectando-se a seus eventos e API. Elas são executadas em um contexto de navegador e têm acesso praticamente irrestrito ao DOM, APIs JavaScript e ao contexto do SillyTavern. As extensões podem modificar a UI, chamar APIs internas e interagir com dados de chat. Este guia explica como criar suas próprias extensões (conhecimento de JavaScript é necessário).

!!!tip Só quer instalar extensões?
Vá aqui: [Extensions](../extensions/index.md).
!!!

Para estender a funcionalidade do servidor Node.js, consulte a página [Server Plugins](./Server-Plugins.md).

**Não sabe escrever JavaScript?**

* Considere [STscript](./st-script.md) como uma alternativa mais simples para escrever uma extensão completa.
* Faça o [Curso MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) e volte quando terminar.

## Submissões de extensões

Quer contribuir suas extensões para o [repositório oficial de conteúdo](https://github.com/SillyTavern/SillyTavern-Content)? Entre em contato conosco!

Para garantir que todas as extensões sejam seguras e fáceis de usar, temos alguns requisitos:

1. Sua extensão deve ser de código aberto e ter uma licença livre (veja [Choose a License](https://choosealicense.com/licenses/)). Se não tiver certeza, AGPLv3 é uma boa escolha.
2. As extensões devem ser compatíveis com a versão de lançamento mais recente do SillyTavern. Por favor, esteja pronto para atualizar sua extensão se algo no núcleo mudar.
3. As extensões devem ser bem documentadas. Isso inclui um arquivo README com instruções de instalação, exemplos de uso e uma lista de recursos.
4. Extensões que têm um requisito de plugin de servidor para funcionar não serão aceitas.

## Exemplos

Veja exemplos ao vivo de extensões simples do SillyTavern:

* <https://github.com/city-unit/st-extension-example> - modelo básico de extensão. Mostra criação de manifesto, importações de scripts locais, adição de painel de configurações de UI e uso de configurações persistentes da extensão.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - uma lista de todas as extensões oficiais do SillyTavern no GitHub.

## Bundling

As extensões também podem utilizar bundling para se isolar do resto dos módulos e usar quaisquer dependências do NPM, incluindo frameworks de UI como Vue, React, etc.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - repositório modelo de uma extensão usando TypeScript e Webpack (sem React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - repositório modelo de uma extensão básica usando React e Webpack.

Para usar importações relativas do bundle, você pode precisar criar um wrapper de importação. Aqui está um exemplo para Webpack:

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

Toda extensão deve ter uma pasta em `data/<user-handle>/extensions` e um arquivo `manifest.json`, que contém metadados sobre a extensão e um caminho para um arquivo de script JS que é o ponto de entrada da extensão.

As extensões baixáveis são montadas na pasta `/scripts/extensions/third-party` quando servidas via HTTP, portanto, importações relativas devem ser usadas com base nisso. Para facilitar o desenvolvimento local, considere colocar o repositório de sua extensão na pasta `/scripts/extensions/third-party` (opção "Install for all users").

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

### Campos do Manifest

* `display_name` é obrigatório. É exibido no menu "Manage Extensions".
* `loading_order` é opcional. Um número maior carrega depois.
* `js` é a referência do arquivo JS principal e é obrigatório.
* `css` é uma referência opcional de arquivo de estilo.
* `author` é obrigatório. Deve conter o nome ou informação de contato do(s) autor(es).
* `auto_update` é definido como `true` se a extensão deve atualizar automaticamente quando a versão do pacote ST mudar.
* `i18n` é um objeto opcional que especifica os locales suportados e seus arquivos JSON correspondentes (veja abaixo).
* `dependencies` é um array opcional de strings especificando outras **extensões** das quais esta extensão depende.
* `generate_interceptor` é uma string opcional que especifica o nome de uma função global chamada em solicitações de geração de texto.
* `minimum_client_version` é uma string opcional que especifica a versão mínima do SillyTavern necessária para esta extensão funcionar.

### Dependências

As extensões também podem depender de outras extensões do SillyTavern. A extensão não será carregada se alguma dessas dependências estiver faltando ou desabilitada.

As dependências são especificadas pelo **nome da pasta** como aparece no diretório `public/extensions`.

Exemplos:

* Extensões integradas: `"vectors"`, `"caption"`
* Extensões de terceiros: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### Campos obsoletos

* `requires` é um array opcional de strings especificando **módulos Extras** necessários. A extensão não será carregada se a API Extras conectada não fornecer todos os módulos listados.
* `optional` é um array opcional de strings especificando **módulos Extras** opcionais. A extensão ainda será carregada se estes estiverem faltando, e a extensão deve lidar com sua ausência de forma elegante.

Para verificar quais módulos são atualmente fornecidos pela API Extras conectada, importe o array `modules` de `scripts/extensions.js`.

## Scripting

### Usando getContext

A função `getContext()` no objeto global `SillyTavern` dá acesso ao contexto do SillyTavern, que é uma coleção de todos os objetos de estado do aplicativo principal, funções úteis e utilitários.

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTÁVEL
context.characters; // Lista de personagens
context.characterId; // Índice do personagem atual
context.groups; // Lista de grupos
context.groupId; // ID do grupo atual
// E muitos mais...
```

Você pode encontrar a lista completa de propriedades e funções disponíveis no [código-fonte do SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js).

!!!
Se estiver faltando alguma das funções/propriedades em `getContext`, entre em contato com os desenvolvedores ou nos envie um pull request!
!!!

### Bibliotecas compartilhadas

A maioria das bibliotecas npm usadas internamente pelo frontend do SillyTavern são compartilhadas na propriedade `libs` do objeto global `SillyTavern`.

* `lodash` - Biblioteca de utilitários. [Docs](https://lodash.com/).
* `localforage` - Biblioteca de armazenamento do navegador. [Docs](https://localforage.github.io/localForage/).
* `Fuse` - Biblioteca de busca fuzzy. [Docs](https://www.fusejs.io/).
* `DOMPurify` - Biblioteca de sanitização HTML. [Docs](https://github.com/cure53/DOMPurify).
* `Handlebars` - Biblioteca de templates. [Docs](https://handlebarsjs.com/).
* `moment` - Biblioteca de manipulação de data/hora. [Docs](http://momentjs.com/).
* `showdown` - Biblioteca de conversão Markdown. [Docs](https://showdownjs.com/).

Você pode encontrar a lista completa de bibliotecas exportadas no [código-fonte do SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js).

**Exemplo:** Usando a biblioteca DOMPurify.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### Aviso sobre TypeScript

Se você deseja acesso ao autocompletar para todos os métodos no objeto global `SillyTavern` (e provavelmente deseja), incluindo `getContext()` e `libs`, você deve adicionar uma declaração de módulo TypeScript `.d.ts`. Esta declaração deve importar tipos globais do código-fonte do SillyTavern, dependendo da localização de sua extensão. Abaixo está um exemplo que funciona para ambos os tipos de instalação: "all users" e "current user".

**global.d.ts** - coloque este arquivo na raiz do diretório de sua extensão (ao lado de `manifest.json`):

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

### Importando de outros arquivos

!!!warning
Usar importações do código do SillyTavern não é confiável e pode quebrar a qualquer momento se a estrutura interna dos módulos do ST mudar. `getContext` fornece uma API mais estável.
!!!

A menos que você esteja construindo uma extensão em bundle, você pode importar variáveis e funções de outros arquivos JS.

Por exemplo, este trecho de código gerará uma resposta da API atualmente selecionada em segundo plano:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## Gerenciamento de estado

### Configurações persistentes

Quando uma extensão precisa persistir seu estado, ela pode usar o objeto `extensionSettings` da função `getContext()` para armazenar e recuperar dados. Uma extensão pode armazenar quaisquer dados serializáveis em JSON no objeto de configurações e deve usar uma chave única para evitar conflitos com outras extensões.

Para persistir as configurações, use a função `saveSettingsDebounced()`, que salvará as configurações no servidor.

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

### Metadados do chat

Para vincular alguns dados a um chat específico, você pode usar o objeto `chatMetadata` da função `getContext()`. Este objeto permite armazenar dados arbitrários associados a um chat, o que pode ser útil para armazenar estado específico da extensão.

Para persistir os metadados, use a função `saveMetadata()`, que salvará os metadados no servidor.

!!!warning
Não salve a referência a `chatMetadata` em uma variável de longa duração, pois a referência mudará quando o chat for alterado. Sempre use `SillyTavern.getContext().chatMetadata` para acessar os metadados do chat atual.
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
O evento `CHAT_CHANGED` é emitido quando o chat é trocado, então você pode ouvir este evento para atualizar o estado de sua extensão de acordo. Veja mais na seção [Listening to events](#listening-to-events).
!!!

### Cartões de personagem

O SillyTavern oferece suporte completo à [Especificação Character Cards V2](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md), que permite armazenar dados arbitrários nos dados JSON do cartão de personagem.

Isso é útil para extensões que precisam armazenar dados adicionais associados ao personagem e torná-los compartilháveis ao exportar o cartão de personagem.

Para escrever dados no campo [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) dos dados do cartão de personagem, use a função `writeExtensionField` da função `getContext()`. Esta função recebe um ID de personagem, uma chave de string e um valor para escrever. O valor deve ser serializável em JSON.

!!!warning Estranheza à Frente
Apesar de ser chamado de `characterId`, não é um identificador único "real", mas sim um índice do personagem no array `characters`.

O índice do personagem atual é fornecido pela propriedade `characterId` no contexto. Se você quiser escrever dados para o personagem atualmente selecionado, use `SillyTavern.getContext().characterId`. Se você precisar armazenar dados para outro personagem, encontre o índice procurando o personagem no array `characters`.

**Cuidado: `characterId` é `undefined` em chats de grupo ou quando nenhum personagem está selecionado!**
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

### Presets de configurações

Dados JSON arbitrários podem ser armazenados nos presets de configurações dos principais tipos de API. Serão exportados e importados junto com o JSON do preset, então você pode usá-los para armazenar configurações específicas da extensão para o preset. Os seguintes tipos de API suportam extensões de dados em presets de configurações:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

Para ler ou escrever os dados, você primeiro precisa obter a instância do PresetManager do contexto:

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
Os eventos `PRESET_CHANGED` e `MAIN_API_CHANGED` são emitidos quando o preset é alterado ou a API principal é trocada, então você pode ouvir esses eventos para atualizar o estado de sua extensão de acordo. Veja mais na seção [Listening to events](#listening-to-events).
!!!

## Internacionalização

!!!
Para informações gerais sobre fornecimento de traduções, veja a página [Internationalization](/For_Contributors/i18n.md).
!!!

As extensões podem fornecer strings localizadas adicionais para uso com as funções `t`, `translate` e o atributo `data-i18n` em templates HTML.

Veja a lista de locales suportados aqui (chave `lang`): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### Chamada direta `addLocaleData`

Passe um código de locale e um objeto com as traduções para a função `addLocaleData`. Substituições de chaves existentes *NÃO* são permitidas. Se o código de locale passado não for um locale atualmente escolhido, os dados serão silenciosamente ignorados.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### Via manifesto da extensão

Adicione um objeto i18n com uma lista de locales suportados e seus caminhos de arquivo JSON correspondentes (relativos ao diretório de sua extensão) ao manifesto.

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

## Registrando comandos slash (nova forma)

Embora `registerSlashCommand` ainda exista para compatibilidade retroativa, novos comandos slash agora devem ser registrados através de `SlashCommandParser.addCommandObject()` para fornecer detalhes estendidos sobre o comando e seus parâmetros ao parser (e, por sua vez, ao autocomplete e à ajuda do comando).

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

Todos os comandos registrados podem ser usados em [STscript](/For_Contributors/st-script.md) de qualquer maneira possível.

## Eventos

### Ouvindo eventos

Use `eventSource.on(eventType, eventHandler)` para ouvir eventos:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

Os principais tipos de eventos são:

* `APP_READY`: o aplicativo está totalmente carregado e pronto para uso. Será disparado automaticamente toda vez que um novo listener for anexado após o aplicativo estar pronto.
* `MESSAGE_RECEIVED`: a mensagem do LLM é gerada e registrada no objeto `chat`, mas ainda não renderizada na UI.
* `MESSAGE_SENT`: a mensagem é enviada pelo usuário e registrada no objeto `chat`, mas ainda não renderizada na UI.
* `USER_MESSAGE_RENDERED`: a mensagem enviada por um usuário é renderizada na UI.
* `CHARACTER_MESSAGE_RENDERED`: a mensagem LLM gerada é renderizada na UI.
* `CHAT_CHANGED`: o chat foi trocado (por exemplo, mudou para outro personagem, ou outro chat foi carregado).
* `GENERATION_AFTER_COMMANDS`: a geração está prestes a começar após processar comandos slash.
* `GENERATION_STOPPED`: a geração foi interrompida pelo usuário.
* `GENERATION_ENDED`: a geração foi concluída ou teve erro.
* `SETTINGS_UPDATED`: as configurações do aplicativo foram atualizadas.

O restante pode ser encontrado [no código-fonte](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

!!!info Dados do evento
A forma como cada evento passa seus dados ao listener não é uniforme. Alguns eventos não emitem dados; alguns passam um objeto ou um valor primitivo. Por favor, consulte o código-fonte onde o evento é emitido para ver quais dados ele passa, ou verifique com o debugger.
!!!

### Emitindo eventos

Você pode produzir quaisquer eventos do aplicativo a partir de extensões, incluindo eventos personalizados, chamando `eventSource.emit(eventType, ...eventData)`:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Interceptadores de Prompt

Os Interceptadores de Prompt fornecem uma maneira para as extensões realizarem qualquer atividade, como modificar os dados do chat, adicionar injeções ou abortar a geração antes que uma solicitação de geração de texto seja feita.

Os interceptadores de diferentes extensões são executados sequencialmente. A ordem é determinada pelo campo `loading_order` em seus respectivos arquivos `manifest.json`. Extensões com valores de `loading_order` mais baixos são executadas mais cedo. Se `loading_order` não for especificado, `display_name` é usado como fallback. Se nenhum for especificado, a ordem é indefinida.

### Registrando um Interceptador

Para definir um interceptador de prompt, adicione um campo `generate_interceptor` ao arquivo `manifest.json` de sua extensão. O valor deve ser o nome de uma função global que será chamada pelo SillyTavern.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Função Interceptadora

A função `generate_interceptor` é uma função global que será chamada em solicitações de geração que não são dry runs. Ela deve ser definida no escopo global (por exemplo, `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) e pode retornar uma `Promise` se precisar realizar operações assíncronas.

A função interceptadora recebe os seguintes argumentos:

* `chat`: Um array de objetos de mensagem representando o histórico de chat que será usado para construção de prompt. Você pode modificar este array diretamente (por exemplo, adicionar, remover ou alterar mensagens). Observe que as mensagens são mutáveis, então quaisquer alterações que você fizer no array serão refletidas no histórico de chat real. Se você quiser que as alterações sejam efêmeras, use `structuredClone` para criar uma cópia profunda do objeto de mensagem.
* `contextSize`: Um número indicando o tamanho atual do contexto (em tokens) calculado para a próxima geração.
* `abort`: Uma função que, quando chamada, sinalizará para evitar que a geração de texto prossiga. Aceita um parâmetro booleano que impede a execução de interceptadores subsequentes se `true`.
* `type`: Uma string indicando o tipo ou gatilho da geração (por exemplo, `'quiet'`, `'regenerate'`, `'impersonate'`, `'swipe'`, etc.). Isso ajuda o interceptador a aplicar lógica condicionalmente com base em como a geração foi iniciada.

**Exemplo de Implementação:**

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

## Gerando texto

O SillyTavern fornece várias funções para gerar texto em diferentes contextos usando a API LLM atualmente escolhida. Essas funções permitem gerar texto no contexto de um chat, geração bruta sem qualquer contexto, ou com saídas estruturadas.

### Dentro de um contexto de chat

A função `generateQuietPrompt()` é usada para gerar texto no contexto de um chat com um prompt "quiet" adicionado (instrução pós-histórico) em segundo plano (a saída não é renderizada na UI). Isso é útil para gerar texto sem interromper a experiência do usuário, mantendo os dados de chat e personagem relevantes intactos, como gerar um resumo ou um prompt de imagem.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### Geração bruta

A função `generateRaw()` é usada para gerar texto sem qualquer contexto de chat. É útil quando você quer controlar totalmente o processo de construção de prompt.

Ela aceita um `prompt` como uma string de Text Completion ou um array de objetos Chat Completion, construindo a solicitação com um formato apropriado dependendo do tipo de API selecionado, por exemplo, convertendo entre modos chat/text, aplicando formatação de instrução, etc. Você também pode passar um `systemPrompt` adicional e um `prefill` para a função para ainda mais controle sobre o processo de geração.

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

### Saídas Estruturadas

!!!info
Atualmente suportado apenas pela API Chat Completion. A disponibilidade varia com base na fonte e modelo selecionados. Se o modelo selecionado não suportar saídas estruturadas, a geração falhará ou retornará um objeto vazio (`'{}'`). Verifique a documentação da API específica que você está usando para ver se saídas estruturadas são suportadas.
!!!

Você pode usar o recurso de saídas estruturadas para garantir que o modelo produza um objeto JSON válido que adere a um [JSON Schema](https://json-schema.org/learn) fornecido. Isso é útil para extensões que requerem dados estruturados, como rastreamento de estado, classificação de dados, etc.

Para usar saídas estruturadas, você deve passar um objeto de esquema JSON para `generateRaw()` ou `generateQuietPrompt()`. O modelo então gerará uma resposta que corresponde ao esquema, e será retornada como um objeto JSON em string.

!!!warning
As saídas não são validadas contra o esquema, você deve lidar com a análise e validação da saída gerada você mesmo. Se o modelo falhar em gerar um objeto JSON válido, a função retornará um objeto vazio (`'{}'`).

[Zod](https://zod.dev/json-schema) é uma biblioteca popular para gerar e validar esquemas JSON. Seu uso não será coberto aqui.
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

Você pode registrar macros personalizados que podem ser usados em qualquer lugar onde substituições de macro são suportadas, por exemplo, nos campos de cartão de personagem, comandos STscript, templates de prompt, etc.

Para registrar um macro, use a função `registerMacro()` do objeto `SillyTavern.getContext()`. A função aceita um nome de macro que deve ser uma string única, e uma string ou uma função que retorna uma string. A função será chamada com uma string `nonce` única que será diferente entre cada chamada `substituteParams`.

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

Quando um macro personalizado não for mais necessário, remova-o usando a função `unregisterMacro()`:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**Detalhes importantes e limitações conhecidas sobre macros personalizados:**

1. Atualmente, apenas macros de substituição de string simples são suportados. Estamos trabalhando para adicionar suporte a macros mais complexos no futuro.
2. Macros que usam funções para fornecer um valor *devem* ser síncronos. Retornar uma `Promise` não funcionará.
3. Você não precisa envolver o nome do macro em chaves duplas (`{{ }}`) ao registrá-lo. O SillyTavern fará isso por você.
4. Como os macros são substituições de expressão regular simples, registrar muitos macros causará problemas de desempenho, então use-os com moderação.

## Fazer solicitação Extras

!!!warning
A API Extras está obsoleta. Não é recomendado usá-la em novas extensões.
!!!

A função `doExtrasFetch()` permite fazer solicitações ao seu servidor de API SillyTavern Extras.

Por exemplo, para chamar o endpoint `/api/summarize`:

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

`getApiUrl()` retorna a URL base do servidor Extras.

A função `doExtrasFetch()`:

* Adiciona os cabeçalhos `Authorization` e `Bypass-Tunnel-Reminder`
* Lida com a busca do resultado
* Retorna o resultado (o objeto de resposta)

Isso facilita chamar sua API Extras de sua extensão.

Você pode especificar:

* O método de solicitação: GET, POST, etc.
* Cabeçalhos adicionais
* O corpo para solicitações POST
* Quaisquer outras opções de fetch
