---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Function Calling

Function Calling permite adicionar funcionalidades dinâmicas às suas extensões permitindo que o LLM use dados estruturados que você pode então usar para acionar uma funcionalidade específica da extensão.

## Casos de uso de exemplo

1. Consultar APIs externas para informações adicionais (notícias, clima, pesquisa web, etc.).
2. Realizar cálculos ou conversões com base na entrada do usuário.
3. Armazenar e recuperar memórias ou fatos importantes, incluindo RAG e consultas de banco de dados.
4. Introduzir aleatoriedade verdadeira na conversa (rolagens de dados, cara ou coroa, etc.).

## Extensões oficialmente suportadas usando function calling

1. [Image Generation](/extensions/Stable-Diffusion.md) (integrada) - gerar imagens baseadas em prompts do usuário.
2. [Web Search](/extensions/WebSearch.md) - acionar uma pesquisa web para uma consulta.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - buscar as últimas notícias de feeds RSS.
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - buscar informações meteorológicas do AccuWeather.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - rolar dados para jogos de D&D.

## Pré-requisitos e limitações

1. Este recurso está disponível apenas para certas fontes de Chat Completion: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI, DeepSeek, AI/ML API e fontes Custom API.
2. APIs Text Completion não suportam chamadas de função, mas alguns backends hospedados localmente como Ollama e TabbyAPI podem executar no modo Custom OpenAI-compatible sob Chat Completion.
3. O suporte para function calling deve ser explicitamente permitido pelo usuário primeiro. Isso é feito habilitando a opção "Enable function calling" no painel AI Response Configuration.
4. Não há garantia de que um LLM realizará quaisquer chamadas de função. A maioria deles requer uma "ativação" explícita através do prompt (por exemplo, o usuário pedindo para "Roll a dice", "Get the weather", etc.).
5. Nem todos os prompts podem acionar uma chamada de ferramenta. Continuações, impersonificação, prompts em segundo plano ('quiet') não têm permissão para acionar uma chamada de ferramenta. Eles ainda podem usar chamadas de ferramentas bem-sucedidas anteriores em suas respostas.

## Como criar uma ferramenta de função

### Verificar se o recurso é suportado

Para determinar se o recurso de chamada de ferramenta de função é suportado, você pode chamar `isToolCallingSupported` do objeto `SillyTavern.getContext()`. Isso verificará se a API atual suporta chamada de ferramenta de função e se está habilitado nas configurações. Aqui está um exemplo de como verificar se o recurso é suportado:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### Registrar uma função

Para registrar uma ferramenta de função, você precisa chamar a função `registerFunctionTool` do objeto `SillyTavern.getContext()` e passar os parâmetros necessários. Aqui está um exemplo de como registrar uma ferramenta de função:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### Cancelar registro de uma função

Para desativar uma ferramenta de função, você precisa chamar a função `unregisterFunctionTool` do objeto `SillyTavern.getContext()` e passar o nome da ferramenta de função para desabilitar. Aqui está um exemplo de como cancelar o registro de uma ferramenta de função:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## Dicas e truques

1. Chamadas de ferramentas bem-sucedidas são salvas como parte do histórico visível e serão exibidas na UI do chat, então você pode inspecionar os parâmetros e resultados reais. Se isso não for desejável, defina a flag `stealth: true` ao registrar uma ferramenta de função.
2. Se você não quiser ver a chamada de ferramenta no histórico do chat. Se você quiser estilizar ou ocultá-las com CSS customizado, aponte para a classe `toolCall` nos elementos `.mes`, ou seja, `.mes.toolCall { display: none; }` ou `.mes.toolCall { color: #999; }`.
