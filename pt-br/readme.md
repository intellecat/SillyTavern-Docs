---
route: /
---

# O que é SillyTavern?

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern (ou ST, abreviado) é uma interface de usuário instalada localmente que permite interagir com LLMs de geração de texto, motores de geração de imagens e modelos de voz TTS. Nosso objetivo é capacitar os usuários com o máximo de utilidade e controle sobre seus prompts de LLM possível, abraçando a curva de aprendizado íngreme como parte da diversão.

SillyTavern é um projeto de paixão trazido a você por uma comunidade dedicada de entusiastas de LLM e sempre será gratuito e de código aberto. Começando em fevereiro de 2023 como um fork do TavernAI 1.2.8, SillyTavern agora conta com mais de 200 contribuidores e 2 anos de desenvolvimento independente, e continua a servir como um software líder para entusiastas experientes de IA.

## Capturas de Tela

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## Requisitos de Instalação

Os requisitos de hardware são mínimos: funcionará em qualquer coisa que possa executar NodeJS 18 ou superior. Se você pretende fazer inferência de LLM em sua máquina local, recomendamos uma placa gráfica NVIDIA série 3000 com pelo menos 6GB de VRAM.

Siga o guia de instalação para sua plataforma:

* [Windows](/Installation/Windows.md)
* [Linux e Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Branches

SillyTavern está sendo desenvolvido usando um sistema de duas branches para garantir uma experiência tranquila para todos os usuários.

* `release` -🌟 **Recomendado para a maioria dos usuários.** Esta é a branch mais estável e recomendada, atualizada apenas quando lançamentos importantes são enviados. É adequada para a maioria dos usuários. Normalmente atualizada uma vez por mês.
* `staging` - ⚠️ **Não recomendado para uso casual.** Esta branch possui os recursos mais recentes, mas tenha cuidado, pois pode quebrar a qualquer momento. Apenas para usuários avançados e entusiastas. Atualizada várias vezes ao dia.

## O que eu preciso além do SillyTavern?

Como SillyTavern é apenas uma interface, você precisará de acesso a um backend de LLM para fornecer inferência. Você pode usar AI Horde para conversas instantâneas prontas para uso. Além disso, suportamos muitos outros backends de LLM locais e baseados em nuvem: API compatível com OpenAI, KoboldAI, Tabby e muitos outros. Você pode ler mais sobre nossas APIs suportadas na seção [Conexões de API](/Usage/API_Connections/index.md).

## Character Cards

SillyTavern é construído em torno do conceito de "character cards". Um character card é uma coleção de prompts que definem o comportamento do LLM e é necessário para ter conversas persistentes no SillyTavern. Eles funcionam de forma similar aos GPTs do ChatGPT ou aos bots do Poe. O conteúdo de um character card pode ser qualquer coisa: um cenário abstrato, um assistente personalizado para uma tarefa específica, uma personalidade famosa ou um personagem fictício.

Para ter uma conversa rápida sem selecionar um character card ou apenas para testar a conexão do LLM, simplesmente digite seu prompt na barra de entrada na [Tela de Boas-vindas](/Usage/welcome-assistants.md) após abrir o SillyTavern. Isso criará um character card "Assistente" vazio que você pode personalizar posteriormente.

Para ter uma ideia geral de como definir character cards, veja o personagem padrão (Seraphina) ou baixe cards selecionados feitos pela comunidade no menu "Download Extensions & Assets".

Você também pode criar seus próprios character cards do zero. Consulte o guia de [Design de Personagens](/Usage/Characters/characterdesign.md) para mais informações.

## Recursos Principais

* [Configurações avançadas de geração de texto](/Usage/Prompts/advancedformatting.md) com muitos presets feitos pela comunidade
* [Suporte a World Info](Usage/worldinfo.md): crie um lore rico ou economize tokens no seu character card
* [Chats em grupo](/Usage/Characters/groupchats.md): salas multi-bot para personagens conversarem com você e/ou entre si
* [Opções ricas de personalização da interface](/Usage/User_Settings/uicustomization.md): cores de tema, imagens de fundo, CSS personalizado e muito mais
* [Personas de usuário](/Usage/personas.md): deixe a IA saber um pouco sobre você para maior imersão
* [Suporte integrado a RAG](/Usage/Characters/data-bank.md): adicione documentos aos seus chats para a IA referenciar
* Extenso subsistema de [comandos de chat](/Usage/Chatting/slashcommands.md) e [motor de scripting](/For_Contributors/st-script.md) próprio

## Extensions

SillyTavern possui suporte a extensibilidade.

* [Expressões emocionais de personagens (sprites)](/extensions/Expression-Images.md)
* [Resumo automático do histórico de chat](/extensions/Summarize.md)
* Interface automática e [tradução de chat](extensions/Translation.md)
* [Geração de imagens Stable Diffusion/FLUX/DALL-E](/extensions/Stable-Diffusion.md)
* [Text-to-speech para mensagens de resposta da IA (via ElevenLabs, Silero ou TTS do Sistema do SO)](/extensions/TTS.md)
* [Capacidades de busca na Web para adicionar contexto adicional do mundo real aos seus prompts](/extensions/WebSearch.md)
* Muitas outras estão disponíveis para download no menu "Download Extensions & Assets".

## Como posso entrar em contato direto com os desenvolvedores?

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [Poste uma issue no GitHub](https://github.com/SillyTavern/SillyTavern/issues)

## Gosto do seu projeto! Como posso contribuir?

* Aceitamos pull requests! Siga as [Diretrizes de Contribuição](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md) para começar.
* Também aceitamos relatórios de bugs úteis e informados que usem os templates fornecidos em nosso GitHub.
* Não aceitamos doações monetárias para o projeto em si.

## Doações Pessoais

Seu apoio aos contribuidores individuais é apreciado, mas não influenciará a direção geral do desenvolvimento do SillyTavern.

* RossAscends tem um [Patreon](https://www.patreon.com/RossAscends) & [Kofi](https://ko-fi.com/rossascends) pessoal

## Licença

SillyTavern é um projeto gratuito e de código aberto lançado sob a [Licença AGPL-3.0](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE).
