---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# Context Template

!!! Aplica-se a: APIs de Text Completion
Para configurações equivalentes em APIs de Chat Completion, use [Prompt Manager](prompt-manager.md).
!!!

Geralmente, modelos de IA exigem que você forneça os dados do personagem de alguma maneira específica. O SillyTavern inclui uma lista de regras de conversão pré-fabricadas para diferentes modelos, mas você pode personalizá-las como quiser.

Edite essas configurações no painel "[Advanced Formatting](advancedformatting.md)".

## Story String

Este campo é um template para o preâmbulo do prompt (conhecido internamente como story string). Esta é a principal maneira de adicionar as informações definidas em [Character Cards](/Usage/Characters/index.md) para modelos de text completion e instruct.

O template suporta sintaxe Handlebars, injeções de texto personalizadas ou formatação, e quaisquer outras [macros](/Usage/Characters/macros.md). Veja a referência da linguagem aqui: <https://handlebarsjs.com/guide/>

Fornecemos os seguintes parâmetros ao avaliador Handlebars (envolvidos em chaves duplas):

1. `{{anchorBefore}}`: Prompts definidos para usar a posição "Before Story String".
2. `{{anchorAfter}}`: Prompts definidos para usar a posição "After Story String".
3. `{{description}}`: A [Description](/Usage/Characters/characterdesign.md#character-description) do personagem.
4. `{{scenario}}`: O [Scenario](/Usage/Characters/characterdesign.md#scenario) do personagem.
5. `{{personality}}`: A [Personality](/Usage/Characters/characterdesign.md#personality-summary) do personagem.
6. `{{system}}`: O [system prompt](advancedformatting.md#system-prompt) OU a substituição do [main prompt](/Usage/Characters/characterdesign.md#prompt-overrides) do personagem (se existir e "Prefer Char. Prompt" estiver habilitado em User Settings).
7. `{{persona}}`: A [descrição da persona](/Usage/personas.md#persona-description) selecionada.
8. `{{char}}`: O nome do personagem.
9. `{{user}}`: O nome da persona selecionada.
10. `{{wiBefore}}` ou `{{loreBefore}}`: Entradas combinadas de [World Info](/Usage/worldinfo.md) ativadas com Position definido como "Before Char Defs".
11. `{{wiAfter}}` ou `{{loreAfter}}`: Entradas combinadas de [World Info](/Usage/worldinfo.md) ativadas com Position definido como "After Char Defs".
12. `{{mesExamples}}`: (Opcional) Os [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) do personagem, formatados em instruct com um separador.
13. `{{mesExamplesRaw}}`: Os [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) do personagem em formato bruto, sem qualquer formatação.

!!!tip **Importante**
Ao usar `{{mesExamples}}` na Story String, defina **"Example Messages Behavior"** no painel **<i class="fa-solid fa-user-cog"></i> User Settings** como **"Never include examples"** para evitar duplicar mensagens de exemplo no prompt.
!!!

Uma macro especial `{{trim}}` é suportada para remover quaisquer novas linhas que a cercam. Use-a se você quiser que uma parte do texto não seja separada da linha anterior por uma nova linha (_espaços **não são** removidos_).

**AVISO**: Se qualquer um dos parâmetros acima estiver faltando no template de story string, eles não serão enviados no prompt de forma alguma.

### Prompt Anchors

Os `{{anchorBefore}}` e `{{anchorAfter}}` são placeholders genéricos para prompts adicionados por várias extensões e recursos diversos em uma posição estática escolhida, por exemplo:

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [Injeções STscript](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Posição da Story String

Por padrão, a story string renderizada (com todos os placeholders substituídos) é colocada no início do prompt, seguida por mensagens de exemplo e o histórico de chat visível.

Alternativamente, você pode movê-la para uma posição dinâmica escolhendo a opção "In-chat @ Depth", que coloca a story string em uma profundidade específica no contexto do chat.

!!!warning **Atenção**
Se o template contiver elementos de prompt estáticos (prefixos ou sufixos específicos do modelo) para envolver a story string, usar a posição "In-Chat @ Depth" fará com que ela seja incorretamente envolvida duas vezes com sequências duplicadas, o que pode levar a resultados inesperados.

Neste caso, você pode corrigir o problema de uma das seguintes maneiras:

1. **Templates integrados**: Redefina os templates para seus padrões usando as etapas descritas em [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates).
2. **Templates personalizados**: Mova os elementos estáticos do template de story string para [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
!!!

### Envolvimento da Story String

!!!
A seção seguinte aplica-se apenas quando **Instruct Mode** está ON.
!!!

* Posição **Default**: A Story String renderizada será envolvida usando as sequências definidas em [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
* Posição **In-chat @ Depth**: A Story String renderizada será envolvida usando as sequências definidas em [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) para uma função escolhida (padrão: System).

## Example Separator

Usado como um cabeçalho de bloco e um separador entre os blocos de diálogo de exemplo. Qualquer instância de tags `<START>` nos diálogos de exemplo será substituída pelo conteúdo deste campo.

## Chat Start

Inserido como um separador após a story string renderizada e após os blocos de diálogos de exemplo, mas antes da primeira mensagem no contexto.

## Separators as Stop Strings

Adiciona "Example Separator" e "Chat Start" à lista de strings de parada.

Útil se o modelo tende a alucinar ou vazar blocos inteiros de diálogo de exemplo precedidos pelo separador.

## Names as Stop Strings

Adiciona nomes de Character e User Persona à lista de strings de parada.

Recomendado manter ligado para evitar personificação do modelo.

## Always add character's name to prompt

!!!info
Esta configuração não tem efeito quando Instruct Mode está ON. O comportamento do nome é definido pela opção selecionada [Include Names](/Usage/Prompts/instructmode.md#include-names).
!!!

Adiciona o nome do personagem ao prompt para forçar o modelo a completar a mensagem como o personagem:

```txt
** OTHER CONTEXT HERE **
Character:
```
