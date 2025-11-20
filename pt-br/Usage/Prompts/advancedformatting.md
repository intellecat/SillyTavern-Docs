---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# Advanced Formatting

As configurações fornecidas nesta seção permitem mais controle sobre a estratégia de [construção de prompt](index.md), principalmente para APIs de Text Completion.

A maioria das configurações neste painel não se aplica a APIs de Chat Completions, pois são governadas pelo sistema de prompt manager.

+++ APIs de Text Completion
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ APIs de Chat Completion
* System Prompt: não aplicável, use [Prompt Manager](prompt-manager.md)
* Context Template: não aplicável, use [Prompt Manager](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## Redefinindo Templates

Você pode restaurar os templates padrão ao seu estado original. Isso pode ser feito através da interface do usuário ou excluindo manualmente os arquivos de dados relevantes.

### Redefinição pela Interface

1. Abra o menu **<i class="fa-solid fa-font"></i> Advanced Formatting**.
2. Escolha o template que deseja redefinir.
3. Clique no botão **<i class="fa-solid fa-recycle"></i> Restore current template**.
4. Confirme a ação quando solicitado.

### Redefinição Manual

!!!
Certifique-se de que a configuração `skipContentCheck` esteja definida como `false` em [config.yaml](/Administration/config-yaml.md#data-configuration), caso contrário a verificação de conteúdo não será acionada.
!!!

1. Navegue até seu diretório de dados do usuário (veja [Data paths](/Installation/index.md#data-paths) para detalhes).
2. Exclua o arquivo `content.log` da raiz do seu diretório de dados do usuário. Este arquivo rastreia os arquivos padrão copiados para seu usuário.
3. Exclua os arquivos JSON de template dos subdiretórios relevantes (`context`, `instruct`, `sysprompt`, etc.).
4. Reinicie o servidor SillyTavern. A aplicação repovoará o conteúdo padrão, restaurando quaisquer templates padrão excluídos.

## Templates definidos pelo Backend

!!! Aplica-se a: APIs de Text Completion
Não aplicável a APIs de Chat Completion, pois usam um construtor de prompt diferente.
!!!

Algumas fontes de Text Completion fornecem uma capacidade de escolher automaticamente templates recomendados pelo autor do modelo. Isso funciona comparando um hash do chat template definido no arquivo `tokenizer_config.json` do modelo com um dos templates padrão do SillyTavern.

1. A opção **<i class="fa-solid fa-bolt"></i> Derive templates** deve estar habilitada no menu **<i class="fa-solid fa-font"></i> Advanced Formatting**. Isso pode ser aplicado a Context, Instruct, ou ambos.
2. Um backend suportado deve ser escolhido como fonte de Text Completion. Atualmente, apenas llama.cpp e KoboldCpp suportam derivação de templates.
3. O modelo deve relatar corretamente seus metadados quando a conexão com a API é estabelecida. Se isso não funcionou, tente atualizar o backend para a versão mais recente.
4. O hash do chat template relatado deve corresponder ao de um dos [templates conhecidos do SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js). Isso cobre apenas templates padrão, como Llama 3, Gemma 2, Mistral V7, etc.
5. Se o hash corresponder, o template será automaticamente selecionado se existir na lista de templates (ou seja, não renomeado ou excluído).

## System Prompt

!!! Aplica-se a: APIs de Text Completion
Para configurações equivalentes em APIs de Chat Completion, use [Prompt Manager](prompt-manager.md). O **Main Prompt** é o equivalente do System Prompt em APIs de Chat Completion.
!!!

O System Prompt define as instruções gerais para o modelo seguir. Ele define o tom e o contexto para a conversa. Por exemplo, ele diz ao modelo para agir como um assistente de IA, um parceiro de escrita ou um personagem fictício.

O System Prompt é uma parte da [Story String](context-template.md#story-string) e geralmente a primeira parte do prompt que o modelo recebe.

Veja o [guia de prompting](index.md#main-prompt-system-prompt) para saber mais sobre o System Prompt.

## Context Template

!!! Aplica-se a: APIs de Text Completion
Para configurações equivalentes em APIs de Chat Completion, use [Prompt Manager](prompt-manager.md).
!!!

Geralmente, modelos de IA exigem que você forneça os dados do personagem de alguma maneira específica. O SillyTavern inclui uma lista de regras de conversão pré-fabricadas para diferentes modelos, mas você pode personalizá-las como quiser.

As opções para esta seção são explicadas em [Context Template](context-template.md).

## Tokenizer

Um tokenizer é uma ferramenta que divide um texto em unidades menores chamadas tokens. Esses tokens podem ser palavras individuais ou até partes de palavras, como prefixos, sufixos ou pontuação. Uma regra geral é que um token geralmente corresponde a 3~4 caracteres de texto.

As opções para esta seção são explicadas em [Tokenizer](tokenizer.md).

## Custom Stopping Strings

Aceita um array serializado em JSON de strings de parada. Exemplo: `["\n", "\nUser:", "\nChar:"]`. Se não tiver certeza sobre a formatação, use um [validador JSON online](https://jsonlint.com/). Se a saída do modelo **terminar** com qualquer uma das strings de parada, elas serão removidas da saída.

APIs suportadas:

1. KoboldAI Classic (versões 1.2.2 e superiores) ou KoboldCpp
2. AI Horde
3. APIs de Text Completion: Text Generation WebUI (ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama, etc.
4. NovelAI
5. OpenAI (máximo 4 strings) e APIs compatíveis
6. OpenRouter (tanto Text quanto Chat Completion)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! Nota
Por padrão, o prefixo Start Reply With não será mostrado na mensagem resultante. Habilite "Show reply prefix in chat" para exibi-lo.
!!!

### APIs de Text Completion

Pré-preenche a última linha do prompt, forçando o modelo a continuar a partir desse ponto. Isso é útil para impor conteúdo, como guiar para o [Model Reasoning](/Usage/Prompts/reasoning.md) com o prefixo definido:

```txt
<think>
Sure!
```

### APIs de Chat Completion

Adiciona uma mensagem de função assistant ao final do prompt. Para alguns modelos de backend, isso é equivalente a pré-preencher a resposta do modelo, mas alguns podem não suportar isso de forma alguma e falharão com um erro de validação. Se não tiver certeza, deixe este campo vazio.
