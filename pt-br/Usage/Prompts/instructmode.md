---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct Mode

O Instruct Mode permite ajustar o prompting para modelos de seguimento de instruções treinados em vários formatos de prompt, como Alpaca, ChatML, Llama2, etc.

!!! Aplica-se a: APIs de Text Completion
Para configurações equivalentes em APIs de Chat Completion, use [Prompt Manager](prompt-manager.md).
!!!

## Suporte de API

### API de Text Completion

Totalmente suportado. Isso inclui:

* Todas as fontes sob Text Completion
* KoboldAI Classic
* AI Horde

#### Escolhendo uma formatação

Um template de instruct escolhido deve corresponder às expectativas de um modelo real que está sendo executado em um backend.

Isso geralmente é refletido em um cartão de modelo no HuggingFace, e alguns até fornecem arquivos JSON compatíveis com SillyTavern.

Exemplo: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### API de Chat Completion (OpenAI, Claude, etc)

Isso não é suportado **(e não é necessário)** para APIs de Chat Completion. Elas usam um construtor de prompt completamente diferente.

### NovelAI

Embora *tecnicamente* suportado para NovelAI, nenhum de seus modelos foi treinado para entender formatação de instruct. Os modelos NovelAI podem usar um módulo de instruct especial que é ativado *automaticamente* quando uma instrução envolvida em chaves é encontrada nas mensagens de chat, então usar Instruct Mode para todo o prompt levará a **qualidade degradada** das saídas.

Aqui está um exemplo que auto-ativa o módulo de instruct para NovelAI:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Configurações do Instruct Mode

### System Prompt

!!!warning Mudança recente
O System Prompt agora é uma entidade separada. Veja a página [Advanced Formatting](advancedformatting.md#system-prompt) para mais detalhes.
!!!

### Templates

Fornece templates prontos com sequências para alguns modelos de instruct bem conhecidos.

*Alterar um template redefine as configurações não salvas para o último estado salvo! Não se esqueça de salvar seu template se você fez alterações que não deseja perder.*

### Activation Regex

Se definido como uma expressão regular válida, quando conectado a um modelo e seu nome corresponder a este regex, selecionará automaticamente este template.

O modo instruct precisa estar habilitado anteriormente. Apenas a primeira correspondência regex entre templates será selecionada (avaliada em ordem alfabética).

### Wrap Sequences with Newline

Cada texto de sequência será envolvido com caracteres de nova linha quando inserido no prompt. Necessário para Alpaca e seus derivados.

Desabilite se você quiser ter controle total sobre os terminadores de linha.

### Replace Macro in Sequences

Se habilitado, substituições conhecidas de \{\{macro\}\} serão substituídas se definidas nas sequências de envolvimento de mensagem.

Além disso, uma macro especial \{\{name\}\} pode ser usada em prefixos de mensagem para referenciar o nome real anexado a uma mensagem (em vez de um \{\{char\}\} ou \{\{user\}\} atualmente ativo), o que pode ser útil ao usar chats em grupo ou comando /sendas. Se o nome não puder ser determinado, "System" é usado como placeholder de fallback.

### Include Names

Se habilitado, adiciona nomes de personagens e usuários aos logs de histórico de chat após a sequência de prefixo.

As seguintes opções estão disponíveis:

* **Never**: Não adicionar prefixos de nome antes do conteúdo da mensagem.
* **Groups and Past Personas**: Adicionar prefixos de nome apenas a mensagens de personagens de grupo e personas passadas.
* **Always**: Sempre adicionar prefixos de nome antes do conteúdo da mensagem.

### Sequences: Story String Wrapping

!!!warning Mudança recente
O envolvimento de System Prompt foi removido e substituído pelo envolvimento de Story String.
!!!

Define como a Story String será envolvida quando a Position estiver definida como "Default (top of context)"

#### Story String Prefix

Inserido antes de uma Story String.

#### Story String Suffix

Inserido após uma Story String.

### Sequences: Chat Messages Wrapping

Essas configurações definem como as mensagens pertencentes a diferentes funções serão envolvidas ao construir um prompt.

Todas as sequências de prefixo também serão automaticamente usadas como strings de parada.

#### User Message Prefix

Inserido antes de uma mensagem de User e como uma última linha de prompt ao se passar por.

#### User Message Suffix

Inserido após uma mensagem de User.

#### Assistant Message Prefix

Inserido antes de uma mensagem de Assistant e como uma última linha de prompt ao gerar uma resposta de IA.

#### Assistant Message Suffix

Inserido após uma mensagem de Assistant

#### System Message Prefix

Inserido antes de uma mensagem de System (adicionada por comandos de barra ou extensões).

#### System Message Suffix

Inserido após uma mensagem de System.

#### System same as User

Se marcado como verdadeiro, mensagens de System usarão sequências de mensagem de função User.

Caso contrário, mensagens de System usam suas próprias sequências (se não estiverem vazias) ou não farão nenhum envolvimento (se estiverem vazias).

### Misc. Sequences

Várias configurações avançadas para ajuste fino da construção de prompt

#### First Assistant Prefix

Inserido antes da primeira mensagem do Assistant.

!!!info
Apenas a primeira mensagem do **histórico de chat** conta, não a mensagem que realmente vai primeiro no prompt!
!!!

#### Last Assistant Prefix

Inserido antes da última mensagem do Assistant ou como uma última linha de prompt ao gerar uma resposta de IA.

!!!info
Não usado ao gerar texto em segundo plano (por exemplo, prompts de Stable Diffusion ou Summaries). System Instruction Prefix ou Regular Assistant Prefix serão usados em vez disso.
!!!

#### System Instruction Prefix

Inserido como uma última linha de prompt ao gerar texto neutro/sistema em segundo plano (por exemplo, prompts de Stable Diffusion ou Summaries).

#### User Filler Message

Será inserido no início do histórico de chat se ele não começar com uma mensagem de User.

**Caso de uso:** quando um formato de instruct *requer estritamente* que os prompts sejam primeiro do usuário e tenham mensagens com funções alternadas apenas, exemplos: Llama 2 Chat, Mistral Instruct.

#### Stop Sequence

Texto que denota o final da resposta. Também enviado como uma string de parada para a API do backend.

Se uma sequência de parada for gerada, tudo depois dela será removido da saída (incluindo a própria sequência).
