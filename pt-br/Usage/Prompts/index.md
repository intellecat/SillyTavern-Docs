---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# Prompts

Quando você envia uma mensagem para sua IA, o texto que você escreve é combinado com outro texto para formar uma única solicitação que é enviada para a IA. Este texto combinado é chamado de "prompt" ou às vezes a "solicitação" ou "contexto".

O prompt pode incluir uma variedade de diferentes tipos de texto, incluindo:

* [Instruções principais](#main-prompt-system-prompt) para a IA sobre como gerar uma resposta
* Definições dos [papéis que a IA deve assumir](/Usage/Characters/characterdesign.md)
* Definições [do papel que você está assumindo](/Usage/personas.md)
* [Informações sobre o "mundo"](/Usage/worldinfo.md) com o qual a IA está interagindo
* Documentos ou informações relevantes do [Data Bank](/Usage/Characters/data-bank.md)
* [Resumos](/extensions/Summarize.md) da conversa passada
* Resultados de [buscas na web](/extensions/WebSearch.md) ou outras [fontes de dados externas](/For_Contributors/Function-Calling.md)
* Mensagens anteriores na conversa
* **Sua mensagem para a IA**
* [Instruções finais](#post-history-instructions) para a IA sobre como gerar uma resposta

Isso pode ser muito para gerenciar! Para ajudá-lo a entender como estruturar e modificar a solicitação que é enviada para a IA, o SillyTavern identifica diferentes elementos que você pode querer incluir em seu prompt. Você pode então estruturar seu prompt para incluir as coisas que fazem sentido para a maneira como você quer interagir com a IA.

Muitos desses elementos são explicados nas seções onde você os alterará. Por exemplo, para descrever o papel que você gostaria que a IA assumisse, você poderia usar o campo [Description](/Usage/Characters/characterdesign.md#personality-summary) em [Character Design](/Usage/Characters/characterdesign.md).

## Visualizando o Prompt

Ler o prompt final que é enviado para a IA é muito útil para entender o que foi dito à IA e por que ela gerou a resposta que gerou. Você pode visualizar o prompt de várias maneiras:

* Usando o ícone Prompt Itemization na mensagem de resposta da IA
* Usando a extensão [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)
* Verificando os logs na janela do terminal em que você está executando o SillyTavern
* Verificando o console nas ferramentas do desenvolvedor do seu navegador

## Alterando como o Prompt é Construído

Apresentar todas as partes do seu prompt à IA da maneira certa é crucial para obter as melhores respostas. Você pode controlar como o prompt é construído.

+++ APIs de Text Completion

Use o painel [Advanced Formatting](advancedformatting.md) para personalizar a construção de prompt para APIs de Text Completion.

+++ APIs de Chat Completion

Use o [Prompt Manager](prompt-manager.md) para personalizar a construção de prompt para APIs de Chat Completion.

+++

## Main Prompt (System Prompt)

O Main Prompt (ou System Prompt) define as instruções gerais para o modelo seguir. Ele define o tom e o contexto para a conversa. Por exemplo, ele diz ao modelo para agir como um assistente de IA, um parceiro de escrita ou um personagem fictício.

+++ APIs de Text Completion

O [System Prompt](advancedformatting.md#system-prompt) é uma parte da [Story String](context-template.md#story-string) e geralmente a primeira parte do prompt que o modelo recebe.

+++ APIs de Chat Completion

O Main Prompt é um dos prompts padrão no [Prompt Manager](prompt-manager.md). Geralmente é a primeira mensagem no contexto que o modelo recebe, atribuída à ("enviada por") função system.

+++

O Main Prompt padrão é:

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

Os placeholders \{\{char\}\} e \{\{user\}\} são substituídos pelos nomes do personagem e persona que você definiu na conversa.

Você pode usar qualquer uma das tags [\{\{macro\}\}](/Usage/Characters/macros.md) suportadas no Main Prompt para incluir informações que podem variar entre conversas ou mudar à medida que a conversa progride.

### Ajustando o Main Prompt

O main prompt padrão ajuda o modelo a entender o que se espera dele com as informações de personagem e persona que seguem, como interpretar a conversa passada e que tipo de resposta gerar. É um prompt flexível de propósito geral que funciona bem para muitas situações, porque estabelece que a IA está escrevendo como um personagem em uma conversa com sua persona.

No entanto, você pode ajustar o main prompt para melhor atender às suas necessidades. Aqui estão algumas razões comuns para ajustar o main prompt:

* **Fornecer instruções adicionais**: por exemplo, você quer que a IA explique seu raciocínio, siga regras específicas ou evite certos tópicos
* **Esclarecer o papel da IA**: por exemplo, você quer que a IA aja como um narrador, um contador de histórias ou um guia
* **Mudar o contexto da conversa**: por exemplo, você quer que a IA responda como se fosse um assistente de IA, jogo de aventura de texto ou um parceiro de escrita

!!! Experimente e veja o que funciona melhor para você
Todos os exemplos neste guia funcionaram bem para outros usuários, mas o prompt que funciona para suas necessidades e o modelo que você está usando pode ser diferente. Experimente diferentes instruções e estilos de prompting para ver o que funciona melhor para você. Se não tiver certeza do que tentar, você sempre pode pedir ajuda no [Discord do SillyTavern](https://discord.gg/sillytavern).
!!!

Dar à IA instruções adicionais no Main Prompt pode ajudá-la a entender o que você quer da conversa.

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

A IA seguirá mais facilmente instruções sobre o que deve fazer do que sobre o que não deve fazer. Por exemplo, se você quiser que a IA evite escrever de uma certa maneira, é melhor dizer a ela como você quer que ela escreva. E enquanto *"Do not decide what \{\{user\}\} says or does"* é comumente incluído em prompts para evitar que a IA controle sua persona, alguns usuários acham que *"Write \{\{char\}\}'s responses in a way that respects \{\{user\}\}'s autonomy"* é mais eficaz.

Muitas vezes há um lugar melhor do que o Main Prompt para incluir informações sobre o usuário ou personagens, modificar o estilo de escrita e fala de um personagem ou dar outras instruções específicas. O Main Prompt é melhor usado para instruções gerais sobre a conversa como um todo, ou sobre um tipo de conversa que você deseja ter.

### Efeito do Histórico de Mensagens

Ao ajustar o main prompt para melhorar as respostas da IA, considere que a IA capta muito do histórico de mensagens. O histórico é sua memória de eventos passados, interações e relacionamentos de personagens, e seu guia de estilo para escolha de palavras e estilo de escrita.

Use isso a seu favor também fornecendo [mensagens de exemplo](/Usage/Characters/characterdesign.md#examples-of-dialogue) mostrando como você quer que a IA responda. Mostrar o que você quer é muitas vezes mais fácil do que tentar explicar!

Quando sua conversa já tem histórico, mudar o main prompt tem um efeito limitado nas respostas da IA. Em termos de eventos e relacionamentos, a IA assume que o main prompt ocorreu no passado distante, e o histórico de mensagens o atualiza. Em termos de estilo de escrita e escolha de palavras, a IA assume que todas as mensagens no histórico foram geradas de acordo com as regras no *atual* main prompt, e que deve continuar a gerar mensagens da mesma maneira. Algumas sugestões para lidar com isso são:

* inserir instruções atuais perto ou após o final do histórico de mensagens, por exemplo, usando uma [Author's Note](/Usage/Characters/Author's-Note.md)
* testar suas alterações no main prompt iniciando uma nova conversa
* editar o histórico de mensagens para remover ou corrigir exemplos de comportamento indesejado
* usar as [Post-History Instructions](#post-history-instructions) para fornecer instruções finais à IA

!!! Acerte da primeira vez!
Nunca deixe a IA "se safar" com algo que você não quer que ela faça. Se você não gostar da resposta da IA, não continue a conversa como se estivesse correta. Em vez disso, modifique os prompts, regenere a mensagem e continue a partir daí. Isso ajudará a IA a aprender o que você quer.
!!!

### Removendo o Contexto de "Fictional Chat"

Há situações em que "fictional chat" pode não ser o contexto certo para sua conversa.

Você pode remover o contexto "fictional" do Main Prompt:

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

Você pode não querer que a IA pense em si mesma como interpretando um papel de jeito nenhum. Em vez de remover a ideia de um personagem, você pode remover a ideia de uma IA:

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### IA como Narrador ou Contador de Histórias

E se você quiser que a IA aja como um narrador, descrevendo eventos de uma perspectiva onisciente, inventando seus próprios personagens e cenários?

Uma abordagem é criar um personagem nomeado para a IA usar como narrador. Este personagem pode ser chamado de "Narrator" ou "AI", sugerindo que a IA é um contador de histórias de propósito geral, ou pode ser nomeado após um cenário ou configuração específica, dando à IA a tarefa de narrar uma história nessa configuração. Os detalhes da configuração podem então ser definidos no [Character](/Usage/Characters/characterdesign.md) ou em [World Info](/Usage/worldinfo.md).

Você precisará ajustar o main prompt padrão para refletir o papel da IA. Para um narrador de propósito geral, você pode usar:

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

ou para uma configuração específica:

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

Ajuda esclarecer o papel do usuário na conversa. Suas mensagens são parte da história, ou são instruções para o narrador sobre o que seu personagem faz ou diz? Um exemplo que inclui o usuário na história:

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

Um exemplo que mantém o usuário fora da história:

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

Definir o papel do usuário não apenas ajuda a IA a entender como responder às suas mensagens, mas também até que ponto ela pode controlar sua persona. Isso evita situações em que a IA toma decisões por sua persona que você preferiria tomar você mesmo.

## Post-History Instructions

Post-History Instructions (PHI) são instruções adicionais enviadas à IA após o main prompt e a mensagem do usuário. Elas podem ser usadas para fornecer contexto ou instruções adicionais à IA com base no histórico de mensagens.

Como as Post-History Instructions são enviadas após a mensagem do usuário, elas são as instruções finais que a IA recebe antes de gerar uma resposta. A IA geralmente dá a elas uma prioridade maior do que o main prompt, e elas podem substituir as instruções do main prompt.

Para usar Post-History Instructions por personagem, adicione-as às [Post-History Instructions](/Usage/Characters/characterdesign.md) do personagem e habilite [Prefer Char. Instructions](/Usage/User_Settings/index.md). Para preservar o PHI definido globalmente enquanto usa instruções específicas do personagem, você pode usar a macro `{{original}}` no campo Post-History Instructions do personagem.

+++ APIs de Text Completion

Post-History Instructions são definidas no painel [Advanced Formatting](/Usage/Prompts/advancedformatting.md) na categoria System Prompt. As Post-History Instructions são adicionadas como uma injeção de função de usuário invisível que precede a última linha do prompt (geralmente contendo um "cabeçalho" de mensagem de resposta). Observe que a opção "Enable System Prompt" deve estar habilitada para que as Post-History Instructions sejam aplicadas (mesmo se o próprio System Prompt estiver vazio).

+++ APIs de Chat Completion

Post-History Instructions é um dos prompts padrão no [Prompt Manager](prompt-manager.md). Geralmente é a última mensagem no contexto que o modelo recebe, atribuída à ("enviada por") função system. Se sua API de Chat Completion não suportar a função system, geralmente será atribuída à função user.

+++

## Adicionando ao Prompt (World Info)

Você pode inserir informações adicionais em qualquer lugar do prompt usando o recurso [World Info](/Usage/worldinfo.md). Ao definir as condições para quando as informações devem ser inseridas, você pode guiar a IA a incluir detalhes específicos, mudar como ela responde ou adicionar novos elementos à conversa.

Alguns usos comuns do World Info incluem:

* um "lorebook" ou "enciclopédia" com informações sobre o mundo ou configuração
* uma maneira de gerenciar diferentes prompts de sistema para vários personagens e situações
* um lugar para armazenar memórias que a IA deve "lembrar" na conversa
* um sistema mais modular para criar, editar e compartilhar detalhes de personagens
* uma fonte de eventos aleatórios e surpresas para a IA reagir, ou para fazer você reagir!
