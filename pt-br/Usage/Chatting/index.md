---
icon: report
order: 170
expanded: false
route: /usage/chatting/
---

# Conversando

Quando você está [conectado a uma API](/Usage/API_Connections/index.md), envie mensagens para a IA digitando na barra de chat na parte inferior da tela. Em seguida, clique em <i class="fa-solid fa-paper-plane"></i> **Send** ou pressione Enter.
![Chat bar](/static/chatbox.png)

A IA responderá com uma mensagem que continua a conversa.

![Chat message](/static/chatmessage.png)

Agora você pode:

* **Enviar outra mensagem**
* **Fazer swipe na resposta**: Clique no botão <i class="fa-solid fa-chevron-right"></i> **Swipe** na mensagem para gerar uma resposta diferente.
* **Editar a mensagem**: Clique no botão <i class="fa-solid fa-pencil"></i> **Edit** em qualquer mensagem para [editar o conteúdo da mensagem](#edit-message-content).
* **Message actions**: Clique no botão <i class="fa-solid fa-ellipsis"></i> **Message actions** em uma mensagem para mais [opções de mensagem](#message-actions-panel) como [tradução](../../extensions/Translation.md), geração de imagens e ramificação de história.
* **Chat options**: Clique no botão <i class="fa-solid fa-bars"></i> **Options** ao lado da barra de chat para mais [opções de chat](#chat-options-panel) como author's notes e gerenciamento de arquivos de chat.

!!! Edit and swipe
Se você deseja ter dito algo diferente, pode editar sua mensagem e então fazer swipe na resposta da IA para obter uma nova.
!!!

!!! Atalhos de teclado
Você também pode usar a tecla de seta **Direita** para fazer swipe, e a tecla de seta **Para cima** para editar a última mensagem no chat. Para mais hotkeys, use o [slash command](/Usage/Chatting/slashcommands.md) `/help hotkeys` no chat ou confira a página [HotKeys](/Usage/Chatting/hotkeys.md).
!!!

## Message actions panel

Gerencie mensagens de chat individuais através do botão de reticências (•••) na mensagem.

Para exibir essas opções para todas as mensagens em seus chats, habilite a configuração [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) nas suas configurações de usuário.

### Funções Principais

* <i class="fa-solid fa-language"></i> **Translate**: Converter mensagem para idioma diferente
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: [Criar uma imagem](/extensions/Stable-Diffusion.md) a partir do conteúdo da mensagem
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: Conversão [texto para fala](/extensions/TTS.md)
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: Visualizar o prompt de geração e uso de tokens

### Visibilidade de Mensagem

* <i class="fa-solid fa-eye"></i> **Included**: A IA vê esta mensagem; clique para excluí-la
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: A IA não vê esta mensagem; clique para incluí-la

### Gerenciamento de Conteúdo

* <i class="fa-solid fa-paperclip"></i> **Embed**: [Anexar arquivos ou imagens](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: Criar checkpoint de história
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: Clique para abrir chat de checkpoint, Shift+Clique para atualizar
  checkpoint existente
* <i class="fa-solid fa-code-branch"></i> **Branch**: Iniciar caminho alternativo de história
* <i class="fa-solid fa-copy"></i> **Copy**: Copiar texto da mensagem
* <i class="fa-solid fa-pencil"></i> **Edit**: Editar conteúdo da mensagem

## Edit message content

Um painel compacto de ferramentas de manipulação de mensagens que aparece quando você <i class="fa-solid fa-pencil"></i> **Edita** uma mensagem de chat.

### Ações Principais

* <i class="fa-solid fa-check"></i> **Confirm**: Salvar alterações da mensagem
* <i class="fa-solid fa-xmark"></i> **Cancel**: Descartar alterações da mensagem

### Operações de Mensagem

* <i class="fa-solid fa-copy"></i> **Copy**: Duplicar conteúdo da mensagem
* <i class="fa-solid fa-trash-can"></i> **Delete**: Remover mensagem

### Posição da Mensagem

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: Mover mensagem para cima no chat
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: Mover mensagem para baixo no chat

Nota: Os controles de movimento podem estar desabilitados com base na posição da mensagem no histórico do chat.

## Chat options panel

Gerencie configurações e operações de chat através do botão <i class="fa-solid fa-bars"></i> **Options** na parte inferior esquerda da interface de chat.

### Controles de Exibição

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: Sair da sessão de chat atual
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: Mostrar/ocultar [painéis de interface](/Usage/index.md#control-panels)

### Configurações de Geração

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: Instruções de contexto personalizadas
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: Ajustar criatividade da resposta
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: Visualizar estatísticas de geração de tokens

### Navegação de Chat

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: Retornar à conversa principal
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: Criar checkpoint de história
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: Transformar em [chat em grupo](/Usage/Characters/groupchats.md)

### Gerenciamento de Chat

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: Iniciar conversa nova
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: [Operações de arquivo de chat](/Usage/Characters/chatfilemanagement.md) como importar, exportar e renomear

### Controles de Mensagem

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: Selecionar e remover múltiplas mensagens
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: Criar nova resposta
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: A IA escreve mensagem como usuário
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: Estender última mensagem

Nota: Algumas opções podem estar ocultas dependendo do contexto e estado do chat.

## Token Probabilities Panel

O painel Token Probabilities permite que você veja o processo de amostragem da IA para geração de texto. Ele mostra não apenas o que a IA escreveu, mas quais outras opções ela considerou em cada ponto do texto.

Para abri-lo, clique no botão <i class="fa-solid fa-pie-chart"></i> **Token Probabilities** no painel <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Chat Options**.

![Example message](/static/token-probs/fling-msg.png){ width=500}

![Token probabilities display for example message](/static/token-probs/fling-probs.png){ width=500}

Quando você clica em qualquer token (palavra, pontuação ou caractere de formatação) no texto gerado, o painel exibe tokens alternativos que a IA considerou naquela posição, junto com suas pontuações de probabilidade. Isso lhe dá uma visão do "processo de pensamento" da IA e mostra outras direções que a resposta poderia ter tomado. Olhar para essas alternativas pode ajudá-lo a entender se havia várias opções prováveis ou uma única escolha clara.

![Alternative tokens and probabilities](/static/token-probs/fling-probs-logprob.png){ width=500}

Se você ver um token que acha que a IA deveria ter escolhido diferentemente, escolha uma alternativa e a mensagem será regenerada a partir daquele ponto em diante, potencialmente dando-lhe uma resposta diferente.

### Rerolling

Se você mudar um token específico e regenerar a resposta, a parte da nova resposta antes do token alterado será a mesma que a resposta original. Esta parte é mostrada em cinza. Como não foi gerada, não há informações de probabilidade para esta parte.

Você pode querer ver outras respostas que poderiam ter sido geradas com base em seu token alternativo.

Você pode clicar na porção cinza para "reroll" a geração, dando-lhe uma nova variação do texto. Clicar em qualquer parte da porção cinza manterá toda a porção cinza e regenerará toda a porção branca/tingida.

Segurar Ctrl enquanto clica em um token na porção cinza manterá a porção cinza até o token clicado e regenerará o resto do texto. Sua escolha de token alternativo não pode ser mantida neste caso.

### Controles

**Token Display**:

* O texto gerado é dividido em tokens individuais
* Cada token é interativo, clique em um token para ver alternativas consideradas pela IA
* Os tokens são tingidos como uma ajuda visual, mas isso não indica probabilidade
* Caracteres especiais (espaços, quebras de linha) são marcados visivelmente

**Token Selection**:

* Clique em um token para visualizar alternativas
* Clique em uma alternativa para substituir o token e regenerar a resposta
* Passe o mouse sobre um token para ver sua pontuação de log-probabilidade bruta

**Window Controls**:

* <i class="fa-solid fa-grip"></i> Alça de arrastar para reposicionamento do painel (apenas MovingUI)
* <i class="fa-solid fa-window-maximize"></i> Maximizar/restaurar tamanho do painel
* <i class="fa-solid fa-circle-chevron-up"></i> Expandir/recolher conteúdo do painel
* <i class="fa-solid fa-circle-xmark"></i> Fechar painel

### Disponibilidade

Você deve selecionar **Request token probabilities** nas [User Settings](/Usage/User_Settings/index.md#chatmessage-handling) para habilitar este recurso.

As probabilidades de token estão disponíveis apenas para a mensagem mais recente e não são salvas no chat. Se as informações de probabilidade de token não estiverem mais disponíveis para uma mensagem, o painel exibirá uma mensagem indicando isso.

As probabilidades de token não estão disponíveis ao usar Smooth Streaming.

As probabilidades de token não estão disponíveis em todas as APIs. Se você estiver usando uma API que não suporta probabilidades de token, o painel abrirá mas não exibirá nenhuma informação.

#### Text Completion
* **LlamaCPP**: Disponível
* **Text Generation WebUI** (oobabooga): Disponível
* **TabbyAPI**: Disponível
* **NovelAI**: Disponível
* **KoboldCPP**: Disponível
* **Ollama**: Parece estar indisponível
* **OpenRouter Text**: Parece estar indisponível

#### Chat Completion
* **OpenAI** ou **Custom**: Disponível, mas rerolling não é suportado
* **Anthropic**: Parece estar indisponível
* **Google AI Studio**: Parece estar indisponível
* **OpenRouter Chat**: Parece estar indisponível
