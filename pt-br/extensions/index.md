---
label: Extensions
icon: plug
expanded: true
order: 35
route: /extensions/
---

# Extensions

O SillyTavern vem com muitas extensões que podem ser ativadas ou desativadas no painel de Extensions. As extensões podem adicionar novos recursos, alterar o comportamento de recursos existentes ou fornecer conteúdo adicional para sua IA usar. Mais extensões podem ser instaladas no menu "Download Extensions & Assets" no painel de Extensions.

## Painel de Extensions

Para abrir ou fechar o painel de Extensions, escolha **<i class="fa-solid fa-cubes fa-fw"></i> Extensions** na barra superior.

- **<i class="fa-solid fa-cubes"></i> Manage extensions**: Ativar, desativar e atualizar extensões
- **Download Extensions & Assets**: Instalar [mais extensões](#installable-extensions), personagens, sons e fundos do repositório do SillyTavern
- **Notify on extension updates**: Marque para ser notificado quando houver atualizações disponíveis para extensões instaladas
- **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**: Importar uma [extensão de terceiros](#third-party-extensions) de uma URL de repositório Git

## Extensões integradas

Essas extensões são integradas ao SillyTavern e não precisam ser instaladas. Elas podem ser ativadas ou desativadas no painel de Extensions.

:::callout
**[Chat Translation](Translation.md)**

Traduzir mensagens de chat para um idioma diferente
:::

:::callout
**[Image Captioning](captioning.md)**

Gera texto a partir de imagens para que sua IA possa "ver" e responder ao conteúdo visual em suas conversas
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

Use APIs locais ou na nuvem de Stable Diffusion, FLUX ou DALL-E para gerar imagens
:::

:::callout
**[Expression Images](Expression-Images.md)**

Imagens (também conhecidas como 'sprites') do seu personagem de IA, mostradas ao lado ou atrás da janela de chat
:::

:::callout
**[Summarize](Summarize.md)**

Resumo automático do histórico de chat
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

Encontra mensagens relevantes do histórico de chat e as adiciona ao contexto
:::

:::callout
**[Text To Speech](TTS.md)**

Narração de voz para suas mensagens de chat via ElevenLabs, Silero, seu TTS do sistema, **[AllTalk](AllTalk.md)**, **[XTTS](XTTS.md)** e mais
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

Responda a mensagens de chat com um único clique, execute comandos e STscripts, e muito mais
:::

:::callout
**Token Counter**

Converte texto em tokens e conta o número de tokens
:::

---

## Extensões instaláveis

!!!tip
Você **deve** ter o git instalado para baixar extensões. Siga as instruções na [página de instalação do Git](https://git-scm.com/downloads) se você não o tiver instalado.
!!!

Você pode navegar por uma lista de todas as extensões disponíveis diretamente do aplicativo indo para o menu **<i class="fa-solid fa-cubes"></i> Extensions** => **Download Extensions & Assets** e clicando no botão **<i class="fa-solid fa-plug-circle-exclamation"></i> Load Asset List**. Para instalar uma extensão, clique no botão **<i class="fa-solid fa-download"></i> Download**. Para ler mais sobre uma extensão, clique no botão **<i class="fa-solid fa-arrow-up-right-from-square"></i> Link** ao lado do nome dela para abrir sua página do GitHub.

!!!info Extensions não são Extras
O projeto Extras foi descontinuado em abril de 2024. Você não precisa instalar o Extras para usar extensões.
!!!

:::callout
**[Blip](Blip.md)**

Anima o texto das mensagens de personagens com velocidade variável e reproduz som junto com a animação.
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

Adiciona música de fundo imersiva e sons ambientes aos seus chats.
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

Jogue jogos de consoles retro diretamente nos chats do SillyTavern.
:::

:::callout
**[Live2d](Live2d.md)**

Adiciona suporte para modelos live2d. Expressões, animações e interações personalizáveis.
:::

:::callout
**[Objective](Objective.md)**

Defina um Objetivo para a IA buscar durante o chat.
:::

:::callout
**[RVC](RVC.md)**

Adiciona recursos de Clonagem de Voz em Tempo Real ao módulo Text-to-Speech.
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

Converta sua fala em texto usando o navegador ou extras.
:::

:::callout
**[VRM](VRM.md)**

Adiciona suporte para modelos VRM. Expressões, animações e interações personalizáveis.
:::

:::callout
**[Web Search](WebSearch.md)**

Adiciona resultados de pesquisa na web aos prompts de LLM.
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

Fornece informações meteorológicas usando a API AccuWeather como um comando slash ou uma ferramenta de função.
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

Adiciona uma barra superior à janela de chat com atalhos para ações rápidas.
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

Jogue o jogo de xadrez com o LLM.
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

Permite executar código JavaScript e STscript de blocos de código no chat.
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

Um conjunto de 7 dados clássicos de D&D para todas as suas necessidades de rolagem de dados.
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

Adiciona a capacidade de agrupar personagens por grupos de similaridade para encontrar facilmente duplicatas.
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

Adiciona um botão para inserir rapidamente emojis em uma mensagem de chat.
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

Permite definir saudações alternativas que são específicas para chats em grupo.
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

Adiciona um botão para inserir rapidamente um template de comando /sendas para o membro do grupo selecionado.
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

Mostre sugestões personalizadas com base em seus chats recentes usando o motor HypeBot da NovelAI. Requer uma assinatura ativa da NovelAI.
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

Adiciona "prompting ocioso" depois que o usuário está ocioso por algum tempo para continuar organicamente a conversa.
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

Visualize metadados de imagens ampliadas anexadas a um chat.
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

Renderiza fórmulas LaTeX e AsciiMath em mensagens de chat.
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

Adiciona renderização de diagramas e fluxogramas Mermaid aos chats do SillyTavern.
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

Adiciona um lugar para armazenar suas notas. Suporta formatação de rich text.
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

Adiciona a capacidade de randomizar controles deslizantes de configurações da API a cada geração.
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

Aprimora a experiência atual de visual novel com mais recursos (Focus Mode, Letterbox Mode e muito mais)!
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

Adiciona uma opção para inspecionar e editar prompts de saída antes de enviá-los ao servidor.
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

Permite receber notificações push para mensagens de chat recebidas.
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

Adiciona um menu suspenso para selecionar personas de usuário na barra de chat.
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

Obtém as últimas notícias de feeds RSS como um comando slash ou uma ferramenta de função.
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

Fornece a imagem da tela para modelos multimodais quando você envia uma mensagem.
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

Adiciona um reprodutor de áudio de silêncio ao menu de extensões. Pode ajudar se a aba do navegador está sendo encerrada em segundo plano.
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

Adiciona uma navegação de linha do tempo ao histórico de chat.
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

Maneira fácil de visualizar e modificar variáveis.
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

Fornece uma interface para extensões usarem modelos de linguagem diretamente no navegador.
:::

## Extensões de terceiros

!!!danger
Usar extensões de terceiros pode ter efeitos colaterais não intencionais e pode representar riscos de segurança.
Sempre certifique-se de confiar na fonte antes de importar uma extensão via **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**.
Não somos responsáveis por qualquer dano causado por extensões de terceiros.
!!!

Para instalar uma extensão de terceiros, vá para o menu **<i class="fa-solid fa-cubes"></i> Extensions** => **<i class="fa-solid fa-cloud-arrow-down"></i> Install Extension** e cole a URL do repositório da extensão. Opcionalmente, especifique o branch e (em cenários de [multi-usuário](../Administration/multi-user.md)) o destino da instalação: todos os usuários ou apenas o usuário atual. A extensão será baixada e carregada automaticamente.
