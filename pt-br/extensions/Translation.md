---
route: /extensions/translation/
templating: false
---

# Chat Translation

## Visão geral

A Extensão Chat Translation permite a tradução em tempo real de mensagens de chat entre
diferentes idiomas usando vários provedores de tradução. Suporta modos de tradução manual e
automática.

![Mensagem de personagem traduzida do inglês para chinês usando o botão de ação de mensagem 'Translate Message/翻譯訊息'](../static/extensions/translation/sensei.png)

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

Todas as maneiras de traduzir mensagens de chat:

Botão **<i class="fa-solid fa-language"></i> Translate Chat** no menu **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- Traduz todo o histórico de chat de uma vez

Botão **<i class="fa-solid fa-keyboard"></i> Translate Input** no menu **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- Traduz apenas o texto de entrada atual
- Útil antes de enviar uma mensagem

Ícone **<i class="fa-solid fa-language"></i> Translate Message** nas **<i class="fa-solid fa-ellipsis"></i> Message
Actions**
barra de ferramentas de qualquer mensagem

- Clique para traduzir apenas essa mensagem
- Clique novamente para reverter para o texto original

Configuração de **Auto-mode** na gaveta **Chat Translation** do painel **<i class="fa-solid fa-cubes"></i>
Extensions**

- Traduz automaticamente entradas do usuário, respostas da IA, ou ambos

Comando slash **/translate**

- Use `/translate [target=language_code] text` para traduzir texto

## Configuração

As opções de configuração estão disponíveis na gaveta **Chat Translation** do painel **<i class="fa-solid fa-cubes"></i>
Extensions**.

#### Provider

- Escolha seu [serviço de tradução](#translation-providers) preferido
- Clique no ícone **<i class="fa-solid fa-key"></i> API Key**, se aparecer, para inserir uma chave de API
- Clique no ícone **<i class="fa-solid fa-link"></i> Custom URL**, se aparecer, para inserir uma URL de API personalizada

#### Target Language

Selecione o idioma em que você deseja escrever suas mensagens ou ler as respostas da IA.

#### Auto-mode

Configure o comportamento de tradução automática.

- **None**: Sem tradução automática
- **Translate responses**: Traduz automaticamente as respostas da IA para o idioma de destino
- **Translate inputs**: Traduz automaticamente as entradas do usuário para inglês
- **Translate both**: Traduz tanto entradas do usuário quanto respostas da IA

#### Clear Translations

O botão **<i class="fa-solid fa-trash-can"></i> Clear Translations** remove todas as traduções de mensagens no
chat atual. As mensagens originais são preservadas.

### Exemplo de Configuração: Conversando de Chinês para Inglês

Para configurar um fluxo de trabalho onde um usuário falante de chinês pode conversar em chinês com uma IA que opera em inglês:

1. Defina Auto-mode como "Translate both"
2. Defina Target Language como "Chinese (Simplified)" ou "Chinese (Traditional)"
3. Escolha um provedor de tradução com boa detecção automática de idioma (por exemplo, Google ou DeepL)

Esta configuração irá:

- Traduzir a entrada em chinês do usuário para inglês para a IA
- Traduzir as respostas em inglês da IA de volta para chinês para o usuário

Esta configuração depende de detecção automática de idioma para entrada. Para controle mais preciso, atualizações futuras podem incluir seleção explícita de idioma de origem.

## Provedores de tradução

**:icon-cloud:** Baseado em nuvem
**<i class="fa-solid fa-link"></i>** Local, URL personalizada
**<i class="fa-solid fa-key"></i>** Requer chave de API

| Provedor                                                            | Localização                                                                           | Recursos                                                                                                           |
|---------------------------------------------------------------------|---------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i>        | Alternativa auto-hospedada (AGPL-3.0) aos serviços de tradução proprietários, com camada Pro hospedada na nuvem   |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                          | Amplamente usado, suporta muitos idiomas, boa precisão                                                             |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                                      | Front-end alternativo para Google Translate, código aberto (AGPL-3.0), focado em privacidade                      |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                          | Traduções de alta qualidade, especialmente para idiomas europeus                                                   |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                                      | Proxy DeepL auto-hospedado, código aberto (MIT), gratuito mas proxy do DeepL Pro requer chave de API DeepL        |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                          | Serviço de tradução da Microsoft, integra-se com serviços Azure                                                    |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                                      | Front-end auto-hospedado para Google Translate e outros provedores, focado em privacidade, código aberto (AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                          | Bom para idiomas russos e do leste europeu                                                                         |

### Configuração específica do DeepL

- Níveis de formalidade disponíveis para alemão, francês, italiano, espanhol, holandês, japonês e russo
- Configure via `deepl.formality` em [config.yaml](/Administration/config-yaml.md#deepl-configuration)

## Comandos Slash

Use o comando `/translate` para traduções rápidas. Sintaxe: `/translate [target=language_code] text`. Se o idioma de destino não for fornecido, o valor das configurações da extensão será usado.

### Uso básico

Traduzir texto para o idioma de destino atual e mostrá-lo em um popup:

```
/translate Welcome to the Tavern | /echo
```

![Popup em chinês (simplificado), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

Traduzir texto para espanhol e adicioná-lo ao chat:

```
/translate target=es Hello world | /send
```

![Mensagem do usuário em espanhol, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### Teste, tradução em pipeline, localização

Solicitar ao usuário uma mensagem e um idioma, traduzir a mensagem para esse idioma, depois re-traduzir para o
idioma de destino configurado e mostrar ambas as traduções em um popup. Este exemplo usa os comandos `/input` e `/buttons` para coletar entrada do usuário:

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

Isso é útil para verificar a qualidade de uma tradução para um idioma que você não fala, antes de escrevê-la
em algum lugar importante.

![Popup, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Popup, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

Os controles de UI são mostrados no idioma atual, independentemente do idioma de destino configurado.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Diálogo de entrada, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Diálogo de botões, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Popup, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

A detecção de idioma de entrada é relativamente eficaz nos seguintes exemplos:

![Popup, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Popup, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Popup, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## Notas Técnicas

- Codificação UTF-8, caracteres especiais e emojis são suportados
- Lida com mensagens grandes dividindo em pedaços quando necessário
- Preserva formatação e imagens incorporadas em mensagens
- Armazena traduções em cache para evitar chamadas de API redundantes

### Idioma de entrada da IA

`internal_language` controla o idioma para o qual as mensagens do usuário são auto-traduzidas antes de serem enviadas à IA. Está fixado como 'en' nas configurações padrão e não pode ser alterado pela UI. Assim, o idioma de destino de tradução para mensagens *para a IA* é sempre inglês. Testes anteriores mostraram que o desempenho da IA era melhor ao receber mensagens em inglês, mas isso pode mudar à medida que mais LLMs estão sendo treinados em dados de idiomas mais variados. Suponho que se poderia alterar `internal_language` em `settings.json` e descobrir.

### Tratamento de variantes do chinês

A extensão suporta chinês simplificado e tradicional, mas nem todos os provedores de tradução suportam. A UI apresenta
estes como 'Chinese (Simplified)' e 'Chinese (Traditional)' respectivamente, com códigos de idioma 'zh-CN' e 'zh-TW'. Eles
são mapeados para os seguintes códigos de idioma para provedores de tradução:

* Libre Translate: 'zh-CN' para 'zh' e 'zh-TW' para 'zt'.
* DeepL e DeepLX: ambas as variantes para 'ZH'.
* Bing: 'zh-CN' para 'zh-Hans', 'zh-TW' como está.
* Outros provedores usam 'zh-CN' e 'zh-TW' como fornecidos.

### Limites de comprimento de texto

Alguns provedores têm limites de caracteres por solicitação:

- Yandex: 5000 caracteres
- DeepLX: 1500 caracteres
- Bing: 1000 caracteres
- Google: 5000 caracteres

Textos mais longos são automaticamente divididos em pedaços para tradução.
