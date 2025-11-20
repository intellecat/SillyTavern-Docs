---
order: 190
icon: rocket
route: /usage/quick-start/
---

# Início Rápido

!!!light
Estou perdido. Apenas me mostre a maneira mais fácil e rápida de começar a usar o SillyTavern. -- *Anônimo*
!!!

Você pode começar a usar o SillyTavern em apenas alguns minutos. Aqui estão duas maneiras fáceis de começar:

* Você pode [usar o AI Horde](#inicio-rapido-com-ai-horde) gratuitamente. AI Horde é um serviço de IA orientado pela comunidade que fornece acesso a uma variedade de modelos de IA.

* Se você tem uma conta OpenAI ou quer registrar uma, pode [usar o OpenAI](#inicio-rapido-com-openai).

## Início rápido com AI Horde

1. Siga o [Guia de Instalação](/Installation/index.md) para instalar e iniciar o SillyTavern.

2. Na tela de integração do SillyTavern, insira um nome para sua persona. Este nome será usado no chat.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. Clique no botão API Connections na barra superior.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. Insira uma chave de API para o AI Horde. Você pode usar `0000000000` por enquanto, ou obter uma chave gratuita em [AI Horde](https://aihorde.net/).

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. Selecione alguns modelos de IA para usar. Apenas escolha alguns do topo. Você sempre pode alterá-los depois.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. Feche a janela de API Connections. Digite uma mensagem na caixa de chat na parte inferior e pressione Enter.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. Sua IA responderá em alguns instantes. Você pode continuar [conversando](/Usage/Chatting/index.md) com ela. Sucesso!

   ![This is an optional caption](/static/quick-start/6_success.png)

## Início rápido com OpenAI

### Instalar o SillyTavern

Siga o [Guia de Instalação](/Installation/index.md) para instalar e iniciar o SillyTavern.

### Obter acesso ao OpenAI

1. Cadastre-se no OpenAI.
2. Vá para <https://platform.openai.com>
3. Clique no ícone da sua conta no canto superior direito, depois em View API Keys.
4. Clique em "Create new secret key". Copie-a imediatamente em algum lugar. **NÃO COMPARTILHE ESTA CHAVE. QUEM A TIVER PODE USAR SUA CONTA PARA USAR O GPT ÀS SUAS CUSTAS.**

### Configurar o SillyTavern para usar sua API

1. Na barra superior do SillyTavern, clique em API Connections.
2. Em API, selecione Chat Completion (OpenAI).
3. Em Chat Completion Source, selecione OpenAI.
4. Cole a chave de API que você salvou na etapa anterior.
5. Clique no botão Connect. Confirme que diz Valid.
6. Por padrão, o SillyTavern usará GPT-4 Turbo. Você pode escolher um modelo diferente, mas informe-se sobre os preços.

### Testar sua configuração

1. Na barra superior do SillyTavern, clique em Character Management no canto direito.
2. Selecione um personagem existente como Seraphina.
3. Na caixa de texto na parte inferior, escreva algo para Seraphina e pressione Enter ou clique no botão Send.

Se você fez tudo certo, após alguns segundos, Seraphina deve responder.
