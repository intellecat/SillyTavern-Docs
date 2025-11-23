---
order: 20
route: /usage/api-connections/openai/
---

# Chat Completions

## Instruções específicas da fonte

!!!warning **Importante!**
A maioria das plataformas de API permite que você visualize a chave de API gerada apenas uma vez, no momento de sua criação. Se você perdê-la, precisará gerar uma nova chave. Certifique-se de mantê-la segura!
!!!

### OpenAI

Use a plataforma de desenvolvedor da OpenAI para acessar vários modelos OpenAI, incluindo gpt-4o, gpt-4.1, o3, etc.

**Como obter uma chave de API:**

1. Vá para [OpenAI](https://platform.openai.com/) e faça login.
2. Use a opção "[View API keys](https://platform.openai.com/account/api-keys)" para criar uma nova chave de API.

### Claude

Claude é uma família de modelos de IA desenvolvida pela Anthropic. Você pode acessar os modelos Claude através do console Anthropic.

**Como obter uma chave de API:**

1. Vá para [Anthropic Console](https://console.anthropic.com/) e faça login.
2. Use a seção "[Get API Key](https://console.anthropic.com/settings/keys)" para criar uma nova chave de API.

### Mistral AI

Mistral AI é uma equipe desenvolvendo modelos abertos e proprietários com altos padrões científicos e foco em abertura. Você pode executar seus modelos localmente ou através de seu serviço de API, La Plateforme.

**Como obter uma chave de API:**

1. O primeiro passo é criar uma conta na [La Plateforme](https://console.mistral.ai/).
2. Feito isso, você pode escolher um [plano](https://console.mistral.ai/billing/plans) e configurar suas informações de pagamento ou optar pelo Free Tier.
3. Em seguida, você pode criar sua [chave de API](https://console.mistral.ai/api-keys/). Pode ser necessário aguardar alguns minutos antes que a chave se torne válida!

### DeepSeek

A Plataforma DeepSeek fornece acesso aos modelos DeepSeek mais recentes através de uma API. Eles oferecem uma variedade de modelos, incluindo DeepSeek V3 e DeepSeek R1.

**Como obter uma chave de API:**

1. Inscreva-se na [Plataforma DeepSeek](https://platform.deepseek.com/).
2. Após se inscrever e recarregar sua conta, você pode criar uma chave de API na seção "[API keys](https://platform.deepseek.com/api_keys)".

### AI21

AI21 Labs oferece uma variedade de modelos de IA, incluindo sua série principal Jamba. Você pode acessar seus modelos através da API do AI21 Studio.

**Como obter uma chave de API:**

1. Vá para [AI21 Studio](https://studio.ai21.com/) e faça login.
2. Navegue até a seção "Settings => API Keys" para criar uma nova chave de API.

### Cohere

Cohere fornece um conjunto de modelos de IA para várias tarefas, incluindo geração de texto e embeddings. Você pode acessar seus modelos através da API Cohere.

**Como obter uma chave de API:**

1. Vá para [Cohere](https://cohere.com/) e faça login.
2. Navegue até a seção "[API Keys](https://dashboard.cohere.com/api-keys)" nas configurações da sua conta para criar uma nova chave de API.

### Perplexity

Perplexity AI oferece acesso aos modelos Sonar habilitados para online através de sua API para pesquisa e recuperação de informações em tempo real.

Guia oficial de Introdução: [Perplexity Quickstart](https://docs.perplexity.ai/getting-started/quickstart)

**Como obter uma chave de API:**

1. Vá para [Perplexity](https://perplexity.ai/) e faça login.
2. Vá para a seção "[API billing](https://www.perplexity.ai/account/api/billing)" para comprar créditos para uso da API.
3. Navegue até a seção "[API keys](https://www.perplexity.ai/account/api/keys)" nas configurações para criar uma nova chave de API.

### Fireworks AI

Fireworks AI é uma plataforma de alto desempenho que fornece acesso rápido e econômico a modelos de linguagem de código aberto de última geração. A plataforma oferece implantação serverless com APIs compatíveis com OpenAI e suporta janelas de contexto de até 256.000 tokens.

**Como obter uma chave de API:**

1. Vá para [Fireworks AI](https://fireworks.ai/) e crie uma conta ou faça login.
2. Navegue até a [página de Chaves de API](https://app.fireworks.ai/settings/users/api-keys) nas configurações da sua conta.
3. Clique em "Create API key" e forneça um nome descritivo (por exemplo, "SillyTavern").

## Electron Hub

Electron Hub é uma plataforma unificada compatível com OpenAI que fornece acesso a modelos de vários fornecedores através de uma única chave de API.

**Como obter uma chave de API:**

1. Crie uma conta em [Electron Hub](https://playground.electronhub.ai/console).
2. Gere uma chave de API na página **Console → API Keys**.

## Endpoint personalizado compatível com OpenAI

!!!warning
É importante notar que não fornecemos suporte para possíveis problemas que você possa ter!
Não garantimos compatibilidade com todos os possíveis endpoints de API!
!!!

!!!
Se você pretende usar este recurso para usar um endpoint local, como TabbyAPI, Oobabooga, Aphrodite, ou qualquer um semelhante, talvez queira verificar a [compatibilidade integrada para esses](/Usage/API_Connections/index.md) em vez disso. O recurso de endpoint personalizado é destinado principalmente para uso com outros serviços e programas que expõem um endpoint de Chat Completion de API compatível com OpenAI.

A maioria das APIs de Text Completion suporta opções de personalização muito maiores do que os padrões da OpenAI permitem. Essas opções de personalização maiores, como o sampler Min-P, podem valer a pena para os usuários do SillyTavern verificarem, o que pode melhorar muito a qualidade das gerações.
!!!

Você pode configurar um endpoint alternativo para o backend de Chat Completions. Este endpoint personalizado pode se conectar a qualquer servidor que suporte o esquema genérico de API OpenAI.

Exemplos de backends compatíveis incluem:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### Conectando

Para acessar este recurso:

1. Mude para o tipo de API 'Chat Completion'
2. Selecione 'Custom (OpenAI-compatible)' para 'Chat Completion Source'

Digite a URL do endpoint personalizado e uma chave de API, se necessário. Por exemplo, TabbyAPI requer uma chave de API para autenticação.

!!!tip
**Dica:** Se você tiver problemas de conexão, tente adicionar `/v1` ao final da URL do endpoint. NÃO adicione o sufixo `/chat/completions`.
!!!

### Selecionando um Modelo

Se a API personalizada implementar o endpoint `/v1/models` para fornecer uma lista de modelos disponíveis, você pode escolher de uma lista suspensa. Caso contrário, use o campo de texto para inserir manualmente um ID de modelo.

Marque 'Bypass API status check' para evitar que o SillyTavern o alerte sobre um endpoint de API não funcional. Ative esta opção se seu endpoint de API funcionar corretamente, mas o SillyTavern continuar a exibir avisos.

Clique em "Test Message" para verificar a conectividade enviando um prompt simples para o modelo.

## Pós-Processamento de Prompt

!!!warning
**Nota:** Tool Calling não é suportado quando a opção de Pós-Processamento com "no tools" é usada!
!!!

Alguns endpoints podem impor restrições específicas no formato de prompts recebidos, como exigir apenas uma mensagem do sistema ou funções estritamente alternadas.

O SillyTavern fornece conversores de prompt integrados para ajudar a atender a esses requisitos (do menos ao mais restritivo):

1. None - nenhum processamento explícito aplicado, a menos que estritamente exigido pela API
2. Merge consecutive messages from the same role
3. Semi-strict - mesclar funções e permitir apenas uma mensagem de sistema opcional
4. Strict - mesclar funções, permitir apenas uma mensagem de sistema opcional e exigir que uma mensagem do usuário seja a primeira
5. Single user message - mesclar todas as mensagens de todas as funções em uma única mensagem do usuário

Merge, semi-strict e strict também removem quaisquer chamadas de ferramentas do prompt, a menos que a variante "with tools" seja selecionada. Isso é útil para APIs que não suportam chamada de ferramentas e seus prompts existentes contêm chamadas de ferramentas.

Opções menos restritivas não têm efeito em endpoints mais restritivos implementados no SillyTavern além de "Custom OpenAI-compatible"; Custom pode dar erro em requisições inválidas.

No modo strict, se nenhuma mensagem do usuário existir antes da primeira mensagem do assistente, então `promptPlaceholder` de `config.yaml` será inserido, que por padrão é "\[Start a new chat]".
