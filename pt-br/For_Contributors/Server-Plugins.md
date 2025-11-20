---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Plugins de Servidor

Esses plugins permitem adicionar funcionalidades que são impossíveis de alcançar usando apenas extensões de UI, como criar novos endpoints de API ou usar pacotes Node.JS que não estão disponíveis em um ambiente de navegador.

Os plugins estão contidos no diretório `plugins` do SillyTavern e são carregados na inicialização do servidor, mas *apenas* se `enableServerPlugins` estiver definido como `true` no arquivo `config.yaml`.

!!!warning Aviso
 **Plugins de Servidor não são isolados (sandboxed). Isso significa que eles podem potencialmente obter acesso a todo o seu sistema de arquivos, ou introduzir uma ampla gama de vulnerabilidades de segurança de uma forma que extensões de UI normais não podem. Instale apenas plugins de servidor de desenvolvedores em quem você confia!**
!!!

Para uma lista de todos os plugins de servidor oficiais, veja a lista da organização no GitHub: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## Tipos de plugins

### Arquivos

Um arquivo JavaScript executável com extensão ".js" (para módulos CommonJS) ou ".mjs" (para módulos ES) contendo um módulo que exporta uma função `init`. Esta função aceita um roteador Express (criado especificamente para seu plugin) como argumento e retorna uma Promise.

O módulo também deve exportar um objeto `info` contendo informações sobre o plugin (strings `id`, `name` e `description`). Isso fornecerá informações sobre o plugin para o carregador.

Você pode registrar rotas via o roteador que serão registradas sob o caminho `/api/plugins/{id}/{route}`. Por exemplo, `router.get('/foo')` para o plugin `example` produzirá uma rota como esta: `/api/plugins/example/foo`.

Um plugin também pode *opcionalmente* exportar uma função `exit` que realiza limpeza ao desligar o servidor. Ela não deve ter argumentos e deve retornar uma Promise.

Contrato TypeScript para exportações de plugin:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

Veja abaixo um exemplo de plugin "Hello world!":

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### Diretórios

Você pode carregar um plugin de um subdiretório no diretório `plugins` de uma das seguintes maneiras (em ordem de precedência):

1. Um arquivo `package.json` que contém um caminho para um arquivo executável no campo "main".
2. Um arquivo `index.js` para módulos CommonJS.
3. Um arquivo `index.mjs` para módulos ES.

Um arquivo resultante deve exportar uma função `init` e um objeto `info` com os mesmos requisitos que para arquivos individuais.

Exemplo de um plugin de diretório (com um arquivo `index.js`): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### Bundling

É preferível usar um bundler (como Webpack ou Browserify) que empacotará todos os requisitos em um arquivo. Certifique-se de definir "Node" como alvo de build.

Repositório modelo para plugins usando Webpack e TypeScript: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
