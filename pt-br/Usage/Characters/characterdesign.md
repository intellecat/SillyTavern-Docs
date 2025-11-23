---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# Design de Personagem

!!!tip
Character Name é o único campo obrigatório. Você pode deixar o resto vazio e ainda usar o personagem em chats.
!!!

## Character Description

Usado para adicionar a descrição do personagem e outras informações relevantes para a IA. Esta informação está sempre incluída no prompt, então todos os fatos importantes devem ser incluídos aqui.

Por exemplo, você pode adicionar informações sobre o mundo em que a ação ocorre, descrever a aparência do personagem, personalidade e background.

Pode ser de qualquer tamanho (seja 200 ou 2000 tokens) e formatado em qualquer estilo (texto livre, estilo de conversa pseudo-código, etc.).

### Métodos e formato

Métodos de formatação de personagens são um tópico complicado além do escopo desta página de documentação.

Guias recomendados que foram testados com ou dependem dos recursos do SillyTavern:

* Guia PLists + Ali:Chat do Trappu: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* Guia Ali:Chat do AliCat: <https://rentry.co/alichat>
* Guia minimalista do kingbri: <https://rentry.co/kingbri-chara-guide>

## Character tokens

**TL;DR: Se você está trabalhando com um modelo de IA com limite de contexto de 2048 tokens, uma definição de personagem de 1000 tokens corta a 'memória' da IA pela metade.**

Para colocar isso em perspectiva, uma resposta decente de uma boa IA pode facilmente ter cerca de 200-300 tokens. Neste caso, a IA só seria capaz de 'lembrar' cerca de 3 trocas de histórico de chat.

### Por que o contador de tokens do meu personagem ficou vermelho?

Quando vemos que seu personagem tem mais da metade do comprimento de contexto definido pelo modelo de tokens em suas definições, destacamos isso para você porque isso pode diminuir as capacidades da IA de fornecer uma conversa agradável.

### O que acontece se meu Personagem tiver muitos tokens?

Não se preocupe - isso não vai quebrar nada. Na pior das hipóteses, se os tokens permanentes do Personagem forem muito grandes, isso simplesmente significa que haverá menos espaço no contexto para outras coisas (veja abaixo).

O único efeito colateral negativo que isso pode ter é que a IA terá menos 'memória', pois terá menos histórico de chat disponível para processar.

Isso ocorre porque todo modelo de IA tem um limite para a quantidade de contexto que pode processar de uma vez.

## 'Context'?

Esta é a informação que é enviada à IA cada vez que você pede para ela gerar uma resposta. O SillyTavern calcula automaticamente a melhor maneira de alocar os tokens de contexto disponíveis antes de enviar a informação ao modelo de IA.

Leia mais sobre como o contexto é construído na seção [Prompts](/Usage/Prompts/index.md).

### O que são os 'Permanent Tokens' de um Personagem?

Estes serão sempre enviados à IA com cada solicitação de geração:

* Character Name
* Character Description Box
* Character Personality Box
* Scenario Box

### Quais partes das Definições de um Personagem NÃO são permanentes?

* A caixa first message - apenas enviada uma vez no início do chat.
* Caixa example messages - apenas mantida até que o histórico de chat preencha o contexto (opcionalmente, estas podem ser forçadas a serem mantidas no contexto)

### Limites de Tokens de Contexto de Modelos de IA Populares

* LLaMA 3 e seus ajustes finos - 8192
* OpenAI GPT-4 - até 128k
* Google Gemini - até 2M
* Claude da Anthropic - 200k (Claude 3)
* NovelAI - 8192 (Erato e Kayra, tier Opus; Clio, todos os tiers), 6144 (Kayra, tier Scroll), ou 3072 (Kayra, tier Tablet)

## First message

A First Message é um elemento importante que define como e em que estilo o personagem se comunicará. O modelo tem maior probabilidade de captar o estilo e restrições de comprimento da primeira mensagem do que qualquer outra coisa, então é importante escrevê-la de uma maneira que você deseja que as respostas sejam (curta e concisa, longa e detalhada, etc.).

Suporta formatação Markdown e HTML.

Por exemplo:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Alternate Greetings

Mensagens adicionadas aqui são exibidas como 'swipes' adicionais para a primeira mensagem do personagem ao iniciar um novo chat. Se o personagem faz parte de um chat em grupo, o sistema seleciona aleatoriamente uma dessas saudações para iniciar a conversa.

## Favorite Character

Clique no botão **<i class="fa-solid fa-star"></i> Add to Favorites** para marcar o personagem como favorito para filtrá-los rapidamente na barra do menu lateral selecionando a opção de ordenação "Favorites". Personagens favoritos têm um destaque dourado na lista. Isso também fará com que o retrato do personagem apareça na área de hotswaps (se habilitado nas User Settings).

## Advanced Definitions

!!!info
Os campos a seguir estão ocultos por padrão. Para acessá-los e editá-los, você precisa clicar no botão **<i class="fa-solid fa-book"></i> Advanced Definitions** na barra de menu da página de definição de personagem.
!!!

### Prompt Overrides

* **Main Prompt**: Se a configuração de usuário "Prefer Char. Prompt" estiver habilitada, qualquer texto que você colocar aqui substituirá o [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) para o personagem.
* **Post-History Instructions**: Se a configuração de usuário "Prefer Char. Instructions" estiver habilitada, qualquer texto que você colocar aqui será usado como as [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) para o personagem.

!!!tip
Insira `{{original}}` em qualquer caixa para incluir o respectivo prompt padrão das configurações do sistema em um local designado.
!!!

### Creator's Metadata

!!!info
Não usado para construção de prompt, mas fornece metadados adicionais sobre o personagem.
!!!

* **Created by**: O nome do criador do personagem. Pode ser exibido na lista de personagens se a configuração de usuário "Char List Subheader" for definida de acordo.
* **Character Version**: A versão do personagem. Pode ser exibida na lista de personagens se a configuração de usuário "Char List Subheader" for definida de acordo.
* **Creator's Notes**: Quaisquer notas adicionais sobre o personagem que o criador deseja compartilhar. As primeiras linhas são exibidas na lista de personagens, e o texto completo é exibido na seção "Creator's Notes" na página do personagem. Suporta formatação Markdown/HTML.
* **Tags to Embed**: Uma lista separada por vírgulas de tags que serão incorporadas na descrição do personagem. Essas tags não são importadas por padrão ao importar o personagem, mas você pode mesclá-las com suas tags existentes selecionando "Import Tags" no menu "More..." na página do personagem.

### Personality summary

Um resumo breve da personalidade do personagem.

### Scenario

As circunstâncias e contexto do diálogo.

### Character's Note

Um texto a ser usado como uma injeção de prompt in-chat para o personagem em uma profundidade de mensagem específica. Geralmente é usado para reforçar certos traços de personagem, pois sempre permanece em uma profundidade estática no histórico do chat, independentemente de sua progressão.

* **@ Depth**: O número de mensagens no histórico do chat após o qual esta nota será injetada (em ordem da mais recente para a mais antiga). Se definido como 0, será injetado após a última mensagem.
* **Role**: A função da mensagem. Pode ser "User", "System" ou "Assistant".

### Talkativeness

Determina a probabilidade da resposta do personagem ser acionada em chats em grupo ao usar uma ordem de ativação [Natural](/Usage/Characters/groupchats.md#natural-order). Varia de 0% a 100%, sendo 50% o valor padrão.

### Examples of dialogue

Descreve como o personagem fala. Antes de cada exemplo, você precisa adicionar a tag `<START>`. Os blocos de exemplo de diálogo são inseridos apenas se houver espaço livre no contexto para eles e são empurrados para fora do contexto bloco por bloco. `<START>` não estará presente no prompt, pois é apenas um marcador; ele será substituído pelo "Example Separator" de Advanced Formatting para APIs de Text Completion e o conteúdo do prompt utilitário "New Example Chat" para APIs de Chat Completion.

* Use o prefixo `{{char}}:` para denotar uma mensagem de personagem.
* Use o prefixo `{{user}}:` para denotar uma mensagem de usuário.

Exemplo:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
