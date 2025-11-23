---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# Reasoning

Em modelos de linguagem, reasoning (também conhecido como model thinking) refere-se a uma técnica de cadeia de pensamento (CoT) que espelha a resolução de problemas humanos através de análise passo a passo. O SillyTavern fornece vários recursos que tornam o uso de modelos de reasoning mais eficiente e consistente em backends suportados.

## Problemas Comuns

1. Ao usar modelos de reasoning, o processo de raciocínio interno do modelo consome parte do seu limite de tokens de resposta, mesmo que esse raciocínio não seja mostrado na saída final (por exemplo, o3-mini ou Gemini Thinking). Se você notar que suas respostas estão voltando incompletas ou vazias, você deve tentar ajustar a configuração Max Response Length encontrada no painel **<i class="fa-solid fa-sliders"></i> AI Response Configuration**. Para modelos de reasoning, é típico usar limites de tokens significativamente maiores - de 1024 a 4096 tokens - em comparação com modelos conversacionais padrão.

## Configuração

!!!
A maioria das configurações relacionadas a reasoning pode ser configurada na seção "Reasoning" do painel **<i class="fa-solid fa-font"></i> Advanced Formatting**.
!!!

Blocos de reasoning aparecem no chat como seções de mensagem recolhíveis. Eles podem ser adicionados manualmente, automaticamente pelo backend, ou através de análise de resposta (veja abaixo).

Por padrão, os blocos de reasoning são recolhidos para economizar espaço. Clique em um bloco para expandir e visualizar seu conteúdo. Você pode configurar os blocos para expandir automaticamente habilitando **Auto-Expand** nas configurações de reasoning.

Quando um bloco de reasoning está expandido, você pode copiar ou editar seu conteúdo usando os botões **<i class="fa-solid fa-copy"></i> Copy** e **<i class="fa-solid fa-pencil"></i> Edit**.

Alguns modelos suportam reasoning, mas não enviarão seus pensamentos de volta. É possível ainda mostrar o bloco de reasoning com o tempo de raciocínio para esses modelos alternando a configuração **Show Hidden**.

## Adicionando Reasoning

### Manualmente

Adicione um bloco de reasoning a qualquer mensagem através do menu **<i class="fa-solid fa-pencil"></i> Message Edit**. Clique em **<i class="fa-solid fa-lightbulb"></i>** durante a edição para adicionar uma seção de reasoning. Extensões de terceiros também podem adicionar reasoning escrevendo no campo `extra.reasoning` do objeto de mensagem antes de adicioná-lo ao chat.

### Com um Comando

Use o comando STscript `/reasoning-set` para adicionar reasoning a uma mensagem. O comando aceita `at` (ID da mensagem, padrão é a última mensagem) e texto de reasoning como argumentos.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### Por Backend

Se o backend e modelo de LLM escolhidos suportarem saída de reasoning, habilitar "Request model reasoning" no painel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** adicionará um bloco de reasoning contendo o processo de pensamento do modelo.

Fontes suportadas:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

"Request model reasoning" não determina se um modelo faz reasoning. Claude e Google (2.5 Flash) permitem que o modo de pensamento seja alternado; veja [Reasoning Effort](#reasoning-effort).

### Por Análise

Habilite "Auto-Parse" no painel **<i class="fa-solid fa-font"></i> Advanced Formatting** para analisar automaticamente o reasoning da saída do modelo.

A resposta deve conter uma seção de reasoning envolvida nas sequências de Prefix e Suffix configuradas. As sequências fornecidas por padrão correspondem ao formato de reasoning do DeepSeek R1.

Exemplo com prefixo `<think>` e sufixo `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Prompting com Reasoning

Por padrão, o conteúdo dos blocos de reasoning reconhecidos não é enviado de volta ao modelo. Para incluir reasoning nos prompts, habilite "Add to Prompts" no painel **<i class="fa-solid fa-font"></i> Advanced Formatting**. O conteúdo do reasoning será envolvido nas sequências de Prefix e Suffix configuradas e separado por um Separator do contexto principal. A configuração numérica Max Additions controla quantos blocos de reasoning podem ser incluídos, contando do final do prompt.

!!!
A maioria dos provedores de modelo não recomenda enviar CoT de volta ao modelo em conversas multi-turno.
!!!

### Continuando a partir do Reasoning

Um caso especial quando o reasoning pode ser enviado de volta ao modelo sem ter a opção "Add to Prompts" habilitada é quando a geração é continuada (por exemplo, pressionando "Continue" no menu **<i class="fa-solid fa-bars"></i> Options**), mas a mensagem sendo continuada contém apenas o reasoning sem conteúdo real. Isso dá ao modelo a oportunidade de terminar um reasoning incompleto e começar a gerar o conteúdo principal. O prompt será enviado da seguinte forma:

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

Scripts de expressão regular da [extensão Regex](/extensions/Regex.md) podem ser aplicados ao conteúdo dos blocos de reasoning. Marque "Reasoning" na seção "Affects" do editor de scripts para direcionar especificamente os blocos de reasoning.

Diferentes opções de efemeridade afetam os blocos de reasoning das seguintes maneiras:

1. Sem efemeridade: o conteúdo do reasoning é permanentemente alterado.
2. Run on edit: o script regex será reavaliado quando o bloco de reasoning for editado.
3. Alter chat display: o regex é aplicado ao texto de exibição do bloco de reasoning, não ao conteúdo subjacente.
4. Alter outgoing prompts: o regex é aplicado apenas aos blocos de reasoning antes de serem enviados ao modelo.

## Reasoning Effort

Reasoning Effort é uma configuração de Chat Completion no painel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** que influencia quantos tokens podem potencialmente ser usados no reasoning. O efeito de cada opção depende da fonte conectada. Para as fontes abaixo, Auto significa simplesmente que o parâmetro relevante não está incluído na solicitação.

| Option  | Claude (≤ 21333 if no streaming) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) |
| ------- | -------------------------------- | -------------------- | -------------------------------- | -------------------- | -------------------- |
| Models  | Opus 4, Sonnet 4/3.7             | o4-mini, o3\*, o1\*  | applicable models                | grok-3-mini          | sonar-deep-research  |
| Auto    | not specified, **no thinking**   | not specified        | not specified, effect depends    | not specified        | not specified        |
| Minimum | budgets 1024 tokens              | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Low     | 15% of max response, min 1024    | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Medium  | 25% of max response, min 1024    | "medium"             | "medium", or 50% of max response | "low"                | "medium"             |
| High    | 50% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |
| Maximum | 95% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |

- Para Claude, o orçamento é limitado a 21333 se o streaming estiver desabilitado. Se o orçamento calculado for menor que 1024, então a resposta máxima é alterada para 2048.
- Para OpenRouter, Perplexity e AI/ML API, apenas uma palavra-chave no estilo OpenAI é enviada.

Google AI Studio e Vertex AI são os seguintes:

| Model          | Auto (dynamic thinking) | Minimum            | Low                          | Medium     | High       | Maximum               |
| -------------- | ----------------------- | ------------------ | ---------------------------- | ---------- | ---------- | --------------------- |
| 2.5 Pro        | thinkingBudget = -1     | 128                | 15% of max response, min 128 | 25% of max | 50% of max | lower of max or 32768 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **no thinking** | 15% of max response          | 25% of max | 50% of max | lower of max or 24576 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **no thinking** | 15% of max response, min 512 | 25% of max | 50% of max | lower of max or 24576 |

- Para Gemini 2.5 Pro e 2.5 Flash/Lite, o orçamento é limitado a 32768 ou 24576 tokens respectivamente, independentemente da configuração de streaming.
