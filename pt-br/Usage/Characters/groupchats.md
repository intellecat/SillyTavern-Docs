---
order: 70
route: /usage/core-concepts/groupchats/
---

# Group Chats

## Estratégias de ordem de resposta

Decide como os personagens em chats em grupo são selecionados para suas respostas.

### Manual

Você pode selecionar o personagem para responder manualmente no menu ou com o comando `/trigger`. O membro do grupo selecionado será o único a responder. Mensagens do usuário não acionarão respostas automaticamente. Acionar uma geração com uma entrada de usuário vazia acionará um membro do grupo não silenciado aleatório para responder.

### Natural Order

Tenta simular o fluxo de uma conversa humana real. O algoritmo é o seguinte:

1. Menções dos nomes dos membros do grupo são extraídas da última mensagem no chat.

    Apenas palavras inteiras são reconhecidas como menções! Se o nome do seu personagem é "Misaka Mikoto", eles responderão apenas à ativação em "Misaka" ou "Mikoto", mas nunca a "Misa", "Railgun", etc.

    A menos que a configuração "Allow Self Responses" esteja habilitada, os personagens não responderão a menções de seu nome em sua própria mensagem!

2. Os personagens são ativados pelo fator "Talkativeness".

    Talkativeness define com que frequência o personagem fala se não foi mencionado. Ajuste este valor na tela "Advanced Definitions" no editor de personagem. Os valores do controle deslizante estão em uma escala linear de **0% / Shy** (o personagem nunca fala a menos que seja mencionado) a **100% / Chatty** (o personagem sempre responde). O valor padrão para novos personagens é 50% de chance.

3. Um personagem aleatório é selecionado.

    Se nenhum personagem foi ativado nas etapas anteriores, um locutor é selecionado aleatoriamente, ignorando todas as outras condições.

### List Order

Os personagens são selecionados com base na ordem em que são apresentados na lista de membros do grupo. Nenhuma outra regra se aplica.

### Pooled Order

Ativa um personagem aleatório que ainda não falou desde a última mensagem do usuário. Se todos os personagens falaram, seleciona um aleatoriamente até a próxima mensagem do usuário.

## Modo de tratamento de geração de grupo

Esta configuração decide como lidar com as informações do personagem dos membros do chat em grupo. Não importa a escolha, o histórico de chat em grupo é sempre compartilhado entre todos os membros.

### Swap character cards

Modo padrão. Toda vez que a mensagem é gerada, apenas as informações do card do personagem do locutor ativo são incluídas no contexto.

### Join character cards

As informações de todos os membros do grupo são combinadas em um prompt conjunto em sua ordem de lista. Isso pode ajudar em casos em que alterar grandes partes do contexto é indesejável, por exemplo, com cache de prompt llama.cpp.

Este modo tem dois submodos (você deve escolher um):

* Include muted - personagens silenciados sempre serão incluídos no prompt conjunto.
* Exclude muted - personagens silenciados não serão incluídos se não forem o locutor atual.

Os seguintes campos estão sendo combinados:

1. Description
2. Scenario, se não substituído para o chat
3. Personality
4. Message examples
5. Character notes / Depth prompts

**Importante!** Esteja ciente de que devido à forma como o card de personagem típico é estruturado, o uso deste modo pode levar a comportamento inesperado, incluindo, mas não limitado a: personagens ficando confusos sobre si mesmos, tendo personalidades mescladas, traços incertos, etc.

### Join Prefix and Suffix

Quando 'Join character cards' está selecionado, todos os respectivos campos dos personagens estão sendo unidos. Isso significa que no prompt resultante todas as descrições de personagem serão unidas em um grande bloco de texto. Se você quiser que esses campos sejam separados, pode definir um prefixo e/ou sufixo.

Essas opções suportam macros normais e também substituirão \{\{char\}\} com o nome do personagem relevante e \<FIELDNAME\> com o nome da parte (por exemplo: description, personality, scenario, etc.)

## Outras opções do menu de Group Chat

### Mute Character

O ícone de balão de fala riscado ao lado do avatar do personagem no menu de chat em grupo pode desabilitar ou habilitar respostas de um personagem específico no chat.

### Force Talk

O ícone de balão de fala ao lado do avatar do personagem no menu de chat em grupo acionará uma resposta apenas de um personagem específico, ignorando a estratégia de ordem de resposta. Funcionará mesmo se o membro do grupo estiver silenciado.

### Auto-mode

Enquanto o auto-mode está habilitado, o chat em grupo seguirá a ordem de resposta e acionará a geração de mensagem sem interação do usuário. O próximo turno de auto-mode é acionado após um atraso de 5 segundos quando o último personagem selecionado envia sua mensagem. Quando o usuário começa a digitar na área de texto de envio de mensagem, o auto-mode será desabilitado, mas as gerações já enfileiradas não são interrompidas automaticamente.

### Allow Self Responses

Permitirá respostas consecutivas do personagem que enviou a última mensagem de cada turno se ele for acionado devido a ser auto-mencionado quando a Natural Order estiver selecionada. Não tem efeito na ordem List.

### Group Chat Scenario Override

Todos os membros do grupo usarão o texto de cenário inserido em vez do especificado em seus cards de personagem. Chats ramificados herdam a substituição de cenário de seu pai e podem ser alterados individualmente depois disso.

### Peek Character Definitions

Clicar no ícone de card de personagem ao lado do avatar no menu de chat em grupo navegará rapidamente para a tela usual de definições de personagem. Quaisquer alterações feitas aqui serão salvas no próprio card.

Para voltar ao chat em grupo, clique no link do título Group Name.

### Member Management

Qualquer um dos seus personagens existentes pode ser adicionado, removido, silenciado ou reordenado dentro do chat em grupo. Por padrão, um novo membro é adicionado ao topo da lista de membros do grupo e então pode ser reordenado usando os ícones de seta.

### Group Chat pop-out

O pop-out do menu de chat em grupo pode ser ativado clicando no ícone ao lado do campo "Current Members". Isso cria um pop-out do menu de chat em grupo. Ao habilitar MovingUI nas configurações do usuário, este menu pode ser redimensionado e arrastado para qualquer posição dentro da interface e funciona exatamente como o menu de chat em grupo regular.
