---
order: 50
templating: false
route: /usage/prompts/prompt-manager/
---

# Prompt Manager

O Prompt Manager é um sistema que fornece mais controle sobre a estratégia de [construção de prompt](index.md) para APIs de Chat Completion.

!!! Aplica-se a: APIs de Chat Completion
Para configurações equivalentes em APIs de Text Completion, use [Advanced Formatting](advancedformatting.md).
!!!

!!!tip Nomeando Presets
Se um preset compartilhar um nome com um de seus cartões de personagem, ele será automaticamente selecionado ao iniciar um chat com esse personagem. Nomeie presets com algo único para evitar esse comportamento.
!!!

Acesse o Prompt Manager clicando no botão "AI Response Configuration" na barra de navegação. O Prompt Manager está localizado abaixo do painel de [configurações comuns](/Usage/Common-Settings.md).

## Quick Prompts Edit

Fornece espaço para editar rapidamente seções comuns de prompt, como **Main Prompt**, **Auxiliary Prompt** e **Post-History Instructions**. Mais informações sobre esses prompts podem ser encontradas na página de [construção de prompt](index.md).

## Utility Prompts

Esses prompts são enviados ao modelo de Chat Completion para ajudá-lo a entender as informações que estão sendo enviadas, ou para instruí-lo a agir de maneiras específicas durante certos tipos de interações.

### Format Templates

!!!tip
Se o template de formato não estiver definido, as informações serão enviadas como estão, sem nenhum envolvimento.
!!!

Estes são templates de string usados para envolver as informações extraídas de [World Info](/Usage/worldinfo.md) e [Character Cards](/Usage/Characters/characterdesign.md).

Um marcador especial é usado para indicar onde as informações devem ser inseridas:

- `{0}` para o template de formato de World Info.
- `{{scenario}}` para o template de formato de Scenario.
- `{{personality}}` para o template de formato de Personality.

### Group Nudge Prompt Template

Usado apenas em chats em grupo. Colocado no final do prompt para forçar uma resposta de um personagem específico.

Deixe isso vazio para desabilitar a funcionalidade de Group Nudge.

### New Chat, New Group Chat, New Example Chat

Estes são enviados antes do histórico de chat e antes de cada bloco de [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) para informar o modelo onde as informações de fundo terminam e o histórico de chat começa.

- **New Chat:** Usado para chats individuais.
- **New Group Chat:** Usado para chats em grupo.
- **New Example Chat:** Usado para blocos de diálogo de exemplo.

Deixe esses vazios para desabilitar esta funcionalidade.

### Continue Nudge

Enviado no final do prompt para instruir o modelo sobre o que fazer quando Continue for acionado, como quando o botão Continue é pressionado ou quando acionado por STScript.

!!! Continues de Chat Completion
Tenha em mente que modelos de Chat Completion lidam com Continues de forma diferente dos modelos de **Text Completion**, e podem nem sempre entregar resultados perfeitos independentemente do seu Continue Nudge.
!!!

### Replace Empty Message

Envia o conteúdo deste campo em vez de uma mensagem em branco quando a caixa de texto está vazia e **Send a message** é pressionado.

## Character Names Behavior

Fornece diferentes estratégias para instruir o modelo sobre como associar mensagens com personagens. Se um modelo de Chat Completion estiver tendo problemas para determinar quais mensagens pertencem a quais personagens, pode ser necessário selecionar uma estratégia diferente.

## Continue Postfix

Quando Continue é acionado, a mensagem 'continuada' retornada pelo modelo terá o Continue Postfix selecionado adicionado ao início. Por exemplo, pode adicionar um espaço antes do texto continuado.

## Additional Settings

### Wrap in Quotes

!!!warning
Opção obsoleta. Prefira [scripts Regex](/extensions/Regex.md) em vez disso.
!!!

Envolve toda a mensagem do usuário em aspas ocultas antes de enviar. Isso é útil para sessões onde os personagens não usam aspas para indicar fala. Se sua sessão usa aspas para indicar fala, deixe isso desmarcado.

### Continue Prefill

!!!warning
Pode não funcionar com todas as fontes de Chat Completion.
!!!

Envia o Continue Nudge como uma mensagem de função Assistant em vez de uma mensagem System. Se isso estiver habilitado, o prompt Continue Nudge não será usado.

### Squash system messages

!!!warning
Opção obsoleta. Prefira [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) em vez disso.
!!!

Combina mensagens System consecutivas em uma única mensagem combinada (excluindo Example Dialogue).

### Enable web search

!!!
Não deve ser confundido com a [extensão Web Search](/extensions/WebSearch.md).
!!!

Habilita recursos de busca na web fornecidos pelo backend de Chat Completion. O prompt geralmente é enriquecido com resultados de busca pelo provedor do modelo e pode incorrer em custos adicionais.

### Enable function calling

Veja [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
Não deve ser confundido com a [extensão Image Captioning](/extensions/captioning.md).
!!!

Se o modelo de Chat Completion tiver capacidades multimodais para processar imagens e vídeos enviados, isso alterna sua capacidade de fazê-lo. Para anexar mídia ao prompt, use a opção **Attach A File** no menu "Magic Wand".

### Request inline images

!!!
Não deve ser confundido com a [extensão Image Generation](/extensions/Stable-Diffusion.md).
!!!

Permite que o modelo retorne anexos de imagem.

### Use system prompt

!!!
Suportado apenas pelos backends Google Gemini e Anthropic Claude.

Apesar de terem configurações muito semelhantes para esses dois, elas são tecnicamente opções separadas, então podem ser configuradas separadamente.
!!!

Mescla todas as mensagens do sistema até a primeira mensagem com uma função não-sistema (User/Assistant) e as envia como um campo de instrução de sistema separado.

## Reasoning Settings

Se o modelo de Chat Completion usar reasoning, essas configurações afetam sua visibilidade e funcionalidade.

### Request model reasoning

Veja [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend).

### Reasoning Effort

Veja [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort).

## "Prompts"

O Prompt Manager forma a espinha dorsal do prompt enviado ao modelo de Chat Completion. Ele controla o que é enviado, bem como a *ordem* em que é enviado.

### O Dropdown 'Prompts'

Contém uma lista suspensa de todos os prompts (não padrão) que o preset de Chat Completion atual inclui. Para que um desses prompts seja adicionado à mensagem de saída, ele precisa ser selecionado na lista suspensa e então adicionado ao Prompt Manager pressionando o botão **Insert prompt**. Para criar um novo prompt para adicionar a esta lista suspensa, pressione o botão **New prompt**. Uma vez que o novo prompt seja escrito e salvo, ele é adicionado ao dropdown e pode então ser inserido.

### Prompts List

Esta é uma interface de arrastar e soltar que lista os prompts selecionados para potencialmente serem enviados ao modelo de Chat Completion. Prompts colocados mais próximos ao **topo** da interface são enviados mais cedo. O **fundo** da lista é a **última coisa** enviada ao modelo (tipicamente, isso seria suas **Post-History Instructions**).

!!! Prompts 'Fixados' = Prompts Padrão
Os prompts padrão não podem ser removidos da lista de prompts selecionados. Isso inclui Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History e Post-History Instructions. Se estes não forem desejados, eles podem ser **desativados**, mas não removidos ou excluídos completamente.
!!!

## Editing a Prompt

Clicar no **botão lápis** em um prompt levará você à **interface de Edição**. Aqui, você pode editar o prompt diretamente.

!!! Certifique-se de salvar suas alterações!
Para salvar permanentemente as alterações nesses prompts em seu preset de Chat Completion, você deve clicar no botão **Save** no canto inferior direito da **interface de Edição**, bem como salvar o próprio preset usando o botão **Save** localizado no topo da seção **AI Response Configuration**! Caso contrário, as alterações feitas serão perdidas quando o preset de Chat Completion for trocado por outro diferente.
!!!

### Name

O nome do prompt. Isso não é enviado ao modelo de Chat Completion; é para sua referência dentro do Prompt Manager apenas.

### Role

Qual função envia o prompt. Você pode escolher entre System, AI Assistant ou User.

### Triggers

Os tipos de geração para os quais este prompt é enviado. Se nada for selecionado, o prompt será enviado para todos os tipos de geração. Se um ou mais forem selecionados, o prompt será enviado apenas para esses tipos de geração específicos:

- **Normal:** Solicitação de geração de mensagem regular.
- **Continue:** Quando o botão Continue é pressionado.
- **Impersonate:** Quando o botão Impersonate é pressionado.
- **Swipe:** Quando a geração é acionada deslizando.
- **Regenerate:** Quando o botão Regenerate é pressionado em chats solo.
- **Quiet:** Solicitações de geração em segundo plano, geralmente acionadas por [extensões](/extensions/index.md) ou comandos [STscript](/For_Contributors/st-script.md).

!!!
O trigger "Regenerate" não está disponível em chats em grupo, pois usa lógica de regeneração diferente: todas as mensagens da última resposta são excluídas, e as mensagens são enfileiradas usando o tipo de geração "Normal" de acordo com a [estratégia de resposta de Grupo](/Usage/Characters/groupchats.md#reply-order-strategies) escolhida.
!!!

### Position

Quando Position está definido como **Relative**, este prompt é enviado onde está localizado na interface de arrastar e soltar com todos os outros prompts. Quando está definido como **In-Chat** e recebe uma **Depth**, ele é enviado **dentro do Chat History** como a Role selecionada, e **ignora** a ordem da interface de arrastar e soltar.

### Depth

Quando Position está definido como **In-Chat**, isso define quão profundo o prompt é enviado dentro do histórico de chat. Quanto maior o número, mais profundo ele é enviado. Por exemplo, uma Depth de 0 será enviada após a última mensagem de chat, uma Depth de 1 será enviada antes da última mensagem de chat, e uma Depth de 2 será enviada antes da penúltima mensagem de chat, e assim por diante.

### Order

!!!
Prompts que têm a mesma Role e Depth serão agrupados e ordenados por seu valor de Order.
A ordem é a seguinte (de cima para baixo): User, AI Assistant, System.
!!!

Quando Position está definido como **In-Chat**, isso define a ordem em que o prompt é enviado dentro do histórico de chat. Quanto menor o número, mais cedo ele é enviado.

## Building Your Prompt: Tips and Tricks

Visite a seção de [construção de prompt](index.md) da documentação do SillyTavern para mais informações sobre como escrever prompts eficazes. As informações podem ser amplamente aplicadas a presets de Chat Completion.
