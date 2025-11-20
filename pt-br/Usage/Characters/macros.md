---
order: 90
route: /usage/core-concepts/macros/
---

# Macros (replacement tags)

!!! Nota
Esta lista pode estar incompleta ou desatualizada. Use o comando slash `/help macros` em qualquer chat do SillyTavern para obter a lista de macros que funcionam na sua instância.
!!!

Macros podem ser usados na descrição do personagem, author's notes, world info e muitos outros lugares e substituídos pelos valores correspondentes ao gerar uma resposta. Eles podem ser usados para inserir conteúdo dinâmico no prompt, como o nome do usuário, descrição do personagem ou a hora atual. Macros são delimitados por chaves duplas, por exemplo `{{user}}` e geralmente não diferenciam maiúsculas de minúsculas. **Tenha em mente que o aninhamento de macros atualmente não é suportado.**

Nota: algumas extensões também podem adicionar macros especiais específicos de contexto que funcionam apenas em certas áreas (ou seja, placeholders especiais para prompts de extensão). Estes não serão documentados aqui, a menos que o macro não esteja vinculado a uma funcionalidade específica.

## General Macros

| Macro | Descrição |
|-------|-------------|
| `{{pipe}}` | Apenas para batching de comandos slash. Substituído pelo resultado retornado do comando anterior. |
| `{{newline}}` | Insere uma quebra de linha. |
| `{{trim}}` | Remove quebras de linha ao redor deste macro. |
| `{{noop}}` | Sem operação, apenas uma string vazia. |
| `{{user}}` ou `<USER>` | Nome do usuário. |
| `{{charPrompt}}` | Substituição de Main Prompt do personagem. |
| `{{charJailbreak}}` | Substituição de Post-History Instructions Prompt do personagem. |
| `{{group}}` ou `{{charIfNotGroup}}` | Lista separada por vírgulas de nomes de membros do grupo ou nome do personagem em chats solo. |
| `{{groupNotMuted}}` | Igual a `{{group}}` mas exclui membros silenciados. |
| `{{notChar}}` | Lista separada por vírgulas de todos os participantes do chat, exceto o locutor atual (`{{char}}`). Em chats em grupo isso ainda inclui personagens silenciados, e quando nenhuma mensagem está sendo gerada lista todos os personagens no roster. |
| `{{char}}` ou `<BOT>` | Nome do personagem. |
| `{{description}}` | Descrição do personagem. |
| `{{scenario}}` | Cenário do personagem ou substituição de cenário do chat (se definido). |
| `{{personality}}` | Personalidade do personagem. |
| `{{persona}}` | Descrição da persona do usuário. |
| `{{mesExamples}}` | Exemplos de diálogo do personagem (formatados como instruct). |
| `{{mesExamplesRaw}}`  | Exemplos de diálogo do personagem (inalterados e não divididos). |
| `{{charVersion}}` | O número da versão do personagem. |
| `{{charDepthPrompt}}` | O prompt at-depth do personagem. |
| `{{model}}` | Nome do modelo de geração de texto para a API atualmente selecionada. **Pode ser impreciso!** |
| `{{lastMessageId}}` | ID da última mensagem do chat. |
| `{{lastMessage}}` | Texto da última mensagem do chat. |
| `{{firstIncludedMessageId}}` | O ID da primeira mensagem incluída no contexto. Requer que a geração seja executada pelo menos uma vez na sessão atual. |
| `{{lastCharMessage}}` | Última mensagem de chat enviada pelo personagem. |
| `{{lastUserMessage}}` | Última mensagem de chat enviada pelo usuário. |
| `{{currentSwipeId}}` | ID baseado em 1 do swipe da última mensagem atualmente exibido. |
| `{{lastSwipeId}}` | Número de swipes na última mensagem do chat. |
| `{{lastGenerationType}}` | Tipo da última solicitação de geração enfileirada. Valores: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue". |
| `{{original}}` | Pode ser usado em campos Prompt Overrides para incluir o prompt padrão das configurações do sistema. Aplicado apenas a APIs de Chat Completion e modo Instruct. |
| `{{time}}` | Hora atual do sistema. |
| `{{time_UTC±X}}` | Hora atual no deslocamento UTC especificado (fuso horário), por exemplo, para UTC+02:00 use `{{time_UTC+2}}`. |
| `{{timeDiff::(time1)::(time2)}}` | A diferença de tempo entre time1 e time2. Aceita macros de hora e data. |
| `{{date}}` | Data atual do sistema. |
| `{{input}}` | Conteúdo da barra de entrada do usuário. |
| `{{weekday}}` | O dia da semana atual. |
| `{{isotime}}` | A hora ISO atual (relógio de 24 horas). |
| `{{isodate}}` | A data ISO atual (AAAA-MM-DD). |
| `{{datetimeformat ...}}` | Data/hora atual no formato especificado (por exemplo, `{{datetimeformat DD.MM.YYYY HH:mm}}`). |
| `{{idle_duration}}` | Insere uma string humanizada do intervalo de tempo desde que a última mensagem do usuário foi enviada (exemplos: 4 hours, 1 day). |
| `{{random:(args)}}` | Retorna um item aleatório da lista (por exemplo, `{{random:1,2,3,4}}` retornará 1 dos 4 números aleatoriamente). |
| `{{random::arg1::arg2}}` | Sintaxe alternativa para random que suporta vírgulas em seus argumentos. |
| `{{pick::(args)}}` | Alternativa ao random, mas o argumento selecionado é estável em avaliações subsequentes no chat atual se a string de origem permanecer inalterada. |
| `{{roll:(formula)}}` | Gera um valor aleatório usando sintaxe de dados D&D: XdY+Z (por exemplo, `{{roll:d6}}` gera um valor 1-6). |
| `{{bias "text here"}}` | Define um viés comportamental para a IA até a próxima entrada do usuário. Aspas ao redor do texto são necessárias. |
| `{{// (note)}}` | Permite deixar uma nota que será substituída por conteúdo em branco. Não visível para a IA. |
| `{{banned "text here"}}` | Adiciona dinamicamente texto entre aspas a sequências de palavras banidas para o backend Text Generation WebUI. Não faz nada para outros backends. Aspas necessárias. |
| `{{reverse:(content)}}` | Inverte o conteúdo do macro. |
| `{{outlet::(name)}}` | Substituído pelo conteúdo do [World Info outlet](/Usage/worldinfo.md#outlet-name) nomeado, conterá entradas ativadas separadas por quebras de linha. |

## Instruct Mode and Context Template Macros

(habilitado nas configurações Advanced Formatting)

| Macro | Descrição |
|-------|-------------|
| `{{exampleSeparator}}` | Separador de diálogos de exemplo do context template. |
| `{{chatStart}}` | Linha de início de chat do context template. |
| `{{instructSystemPrompt}}` | Prompt de sistema Instruct. |
| `{{instructSystemPromptPrefix}}` | Sequência de prefixo do system prompt. |
| `{{instructSystemPromptSuffix}}` | Sequência de sufixo do system prompt. |
| `{{instructUserPrefix}}` | Sequência de prefixo de mensagem do usuário. |
| `{{instructAssistantPrefix}}` | Sequência de prefixo de mensagem do assistente. |
| `{{instructSystemPrefix}}` | Sequência de prefixo de mensagem do sistema. |
| `{{instructUserSuffix}}` | Sequência de sufixo de mensagem do usuário. |
| `{{instructAssistantSuffix}}` | Sequência de sufixo de mensagem do assistente. |
| `{{instructSystemSuffix}}` | Sequência de sufixo de mensagem do sistema. |
| `{{instructFirstAssistantPrefix}}` | Sequência de primeira saída do assistente. |
| `{{instructLastAssistantPrefix}}` | Sequência de última saída do assistente. |
| `{{instructFirstUserPrefix}}` | Sequência de primeira entrada do usuário Instruct. |
| `{{instructLastUserPrefix}}` | Sequência de última entrada do usuário Instruct. |
| `{{instructSystemInstructionPrefix}}` | Sequência de prefixo de instrução do sistema. |
| `{{instructUserFiller}}` | Texto de mensagem de preenchimento do usuário. |
| `{{instructStop}}` | Sequência de parada Instruct. |
| `{{maxPrompt}}` | Tamanho máximo do prompt em tokens (comprimento de contexto reduzido pelo comprimento de resposta). |
| `{{systemPrompt}}` | Conteúdo do system prompt, incluindo substituição de prompt do personagem se permitido e disponível. |
| `{{defaultSystemPrompt}}` | Conteúdo do system prompt (excluindo substituição de prompt do personagem). |

## Chat variables Macros

- Variáveis locais = exclusivas do chat atual
- Variáveis globais = funciona em qualquer chat para qualquer personagem

| Macro | Descrição |
|-------|-------------|
| `{{getvar::name}}` | Substituído pelo valor da variável local "name". |
| `{{setvar::name::value}}` | Substituído por string vazia, define a variável local "name" como "value". Permite valores vazios. |
| `{{addvar::name::increment}}` | Substituído por string vazia, adiciona um valor numérico de "increment" à variável local "name". |
| `{{incvar::name}}` | Substituído pelo resultado de incrementar o valor da variável "name" em 1. |
| `{{decvar::name}}` | Substituído pelo resultado de decrementar o valor da variável "name" em 1. |
| `{{getglobalvar::name}}` | Substituído pelo valor da variável global "name". |
| `{{setglobalvar::name::value}}` | Substituído por string vazia, define a variável global "name" como "value". Permite valores vazios. |
| `{{addglobalvar::name::value}}` | Substituído por string vazia, adiciona um valor numérico de "increment" à variável global "name". |
| `{{incglobalvar::name}}` | Substituído pelo resultado de incrementar o valor da variável global "name" em 1. |
| `{{decglobalvar::name}}` | Substituído pelo resultado de decrementar o valor da variável global "name" em 1. |
| `{{var::name}}` | Substituído pelo valor da variável com escopo "name" (apenas STscript). |
| `{{var::name::index}}` | Substituído pelo valor no índice da variável com escopo "name" (para arrays/objetos em STscript). |

## Extension-specific Macros

Adicionados por extensões e funcionam apenas sob certas condições.

| Macro | Descrição |
|-------|-------------|
| `{{summary}}` | Substituído pelo resumo da sessão de chat atual (se disponível). |
| `{{authorsNote}}` | Substituído pelo conteúdo do Author's Note. |
| `{{charAuthorsNote}}` | Substituído pelo conteúdo do Author's Note do Personagem. |
| `{{defaultAuthorsNote}}` | Substituído pelo conteúdo do Author's Note padrão. |
| `{{charPrefix}}` | Substituído por um prefixo de prompt positivo de Image Generation específico do personagem (se disponível). |
| `{{charNegativePrefix}}` | Substituído por um prefixo de prompt negativo de Image Generation específico do personagem (se disponível). |
