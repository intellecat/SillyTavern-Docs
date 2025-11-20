---
order: 150
icon: repo-forked
expanded: false
route: /usage/api-connections/
---

# Conexões de API

O SillyTavern pode se conectar a uma ampla gama de APIs de LLM.
Abaixo está uma descrição de seus respectivos pontos fortes, fracos e casos de uso.

## ELI5: Chat Completions vs Text Completions

Quando você navega pela primeira vez para a página "API Connections" no ST, você notará uma opção suspensa para selecionar entre opções usando nomenclaturas como "Chat Completion" e "Text Completion". É útil entender o que isso significa.

O que não é: É fácil pensar em "Text Completion" como modelos locais e "Chat Completion" como LLMs baseados em nuvem, mas não é o caso. Nem por exemplo "Novel AI" ou "Kobold" são realmente um tipo separado de modelo, mesmo que sejam opções separadas no menu suspenso de API no ST. Você pode forçar modelos em diferentes estruturas de API com o backend apropriado, mas esse não é o ponto desta seção.

Quando você envia uma mensagem usando o ST, seu chat, descrição do personagem e outros prompts como lorebooks ou author's notes são construídos em um único "prompt" para ser enviado ao modelo. O "tipo" de API para o modelo que você está usando decide exatamente como esse prompt será construído (algo que o ST cuida para você automaticamente em segundo plano - você pode abrir seu terminal ST e ver exatamente como é o prompt sendo enviado para a IA).

### Chat Completions

Um modelo de Chat Completion, como seu nome sugere, estruturará seu prompt em uma série de mensagens entre o User (você) e o Assistant (a IA) ou System (neutro). Modelos que são treinados para Chat Completion ajudam a criar a sensação de um "Chat", com a IA "respondendo" à última mensagem. Quando você está usando o site do ChatGPT, você está lidando com uma API de Chat Completions em segundo plano.

### Text Completions (também conhecido como apenas "Completions")

Um Text Completion, por outro lado, e novamente como seu nome sugere, converterá seu prompt em uma longa string, e o modelo simplesmente tentará continuar isso (tipo, literalmente imagine todo o seu texto, suas centenas de mensagens, toda a sua formatação, quebras de linha, etc. comprimidas em uma sentença muito longa).

Se suas mensagens no ST por acaso estiverem formatadas como uma série de mensagens entre YourPersona: e Character:, o modelo Text Completion tentará continuar esse padrão e o ST irá renderizá-lo como uma nova mensagem de chat para você, mas na verdade o modelo está apenas tentando continuar o texto. Se você oferecesse uma entrada de "The Sun rises in the", um modelo text completion provavelmente terminaria essa mensagem para você com "East".

A maioria dos modelos Text Completion tem um "Instruct Template" recomendado (geralmente mencionado na documentação do modelo ou página de download) que os ajuda a "responder" a mensagens e instruções, assim como um modelo Chat Completion. O ST geralmente tem a maioria (se não todos) os Instruct Templates disponíveis para você escolher na página "Advanced Formatting".

## APIs Locais

- Essas APIs de LLM podem ser executadas no seu PC.
- Elas são gratuitas para usar e não têm filtro de conteúdo.
- O processo de instalação pode ser complexo (**A equipe de desenvolvimento do SillyTavern não fornece suporte para isso**).
- Requer download separado de modelos LLM do [HuggingFace](https://huggingface.co/models?other=LLM) que podem ter de 5 a 50GB cada.
- A maioria dos modelos não é tão poderosa quanto as APIs de LLM em nuvem.

### KoboldCpp

- API fácil de usar com offloading de CPU (útil para usuários com pouca VRAM) e streaming
- Executa a partir de um único arquivo binário no Windows, Mac e Linux
- Suporta modelos GGUF
- Mais lento que loaders somente GPU como AutoGPTQ e Exllama/v2
- [GitHub](https://github.com/LostRuins/koboldcpp), [Instruções de Configuração](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- A fonte original da qual KoboldCpp e Ollama foram derivados
- Fornece binários pré-compilados e uma opção para compilar a partir do código fonte
- Suporta modelos GGUF
- Interface CLI leve para llama-server
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- O mais fácil de configurar e usar de todas as APIs baseadas em llama.cpp
- Um [catálogo](https://ollama.com/library) interessante de modelos disponíveis para download com um clique
- Suporta modelos GGUF encapsulados no próprio formato do Ollama
- [GitHub](https://github.com/ollama/ollama), [Website](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- UI Gradio tudo-em-um com streaming
- Suporte mais amplo para modelos quantizados (AWQ, Exl2, GGML, GGUF, GPTQ) e FP16
- Instaladores de um clique disponíveis
- Atualizações regulares, que às vezes podem quebrar a compatibilidade com o SillyTavern
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**Maneira Correta de Conectar o SillyTavern à nova API OpenAI do Ooba:**

1. Certifique-se de estar na última atualização do TextGen do Oobabooga (a partir de 14 de novembro de 2023).
2. Edite o arquivo CMD_FLAGS.txt e adicione a flag `--api` lá. Em seguida, reinicie o servidor do Ooba.
3. Conecte o ST a `http://localhost:5000/` (por padrão) sem marcar a caixa 'Legacy API'. Você pode remover o sufixo `/v1` da URL que o console do Ooba fornece para você.

*Você pode alterar a porta de hospedagem da API com a flag `--api-port 5001`, onde 5001 é sua porta personalizada.*

### TabbyAPI

- API leve baseada em [Exllamav2](https://github.com/turboderp/exllamav2) com streaming
- Suporta modelos Exl2, GPTQ e FP16
- [Extensão oficial](https://github.com/theroyallab/ST-tabbyAPI-loader) permite carregar/descarregar modelos diretamente do SillyTavern
- Não recomendado para usuários com pouca VRAM (sem offloading de CPU)
- [GitHub](https://github.com/theroyallab/tabbyAPI), [Instruções de Configuração](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic (obsoleto, abandonado)

- Executa no seu PC, 100% privado, ampla gama de modelos disponíveis
- Dá o controle mais direto das configurações de geração da IA
- Requer grandes quantidades de VRAM na sua GPU (6-24GB, dependendo do modelo LLM)
- Modelos limitados a contexto de 2k
- Sem streaming
- Versões populares do KoboldAI:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## APIs de LLM em Nuvem

- Essas APIs de LLM são executadas como serviços em nuvem e não requerem recursos no seu PC
- Elas são mais fortes/inteligentes que a maioria dos LLMs locais
- No entanto, todas têm filtragem de conteúdo em graus variados, e a maioria requer pagamento

### AI Horde

- O SillyTavern pode acessar esta API pronta para uso sem configurações adicionais necessárias
- Usa a GPU de voluntários individuais (Horde Workers) para processar respostas para suas entradas de chat
- À mercê do Worker em termos de tempos de espera de geração, configurações de IA e modelos disponíveis
- [Website](https://aihorde.net/), [Instruções de Configuração](./horde.md)

### OpenAI (ChatGPT)

- Fácil de configurar e adquirir uma chave de API
- Requer pré-pagamento de créditos e cobra por prompt
- Muito lógico. O estilo criativo pode ser repetitivo e previsível
- A maioria dos modelos mais recentes (gpt-4-turbo, gpt-4o) suporta multimodalidade
- [Website](https://platform.openai.com/), [Instruções de Configuração](/Usage/API_Connections/openai.md#openai)

### Claude (by Anthropic)

- Recomendado para usuários que desejam que seus chats de IA tenham um estilo de escrita criativo e único
- Requer pré-pagamento de créditos e cobra por prompt
- Os modelos mais recentes (Claude 3) suportam multimodalidade
- Requer um estilo específico de prompting e utilização de [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) para direcionamento de resposta
- [Website](https://console.anthropic.com/), [Instruções de Configuração](/Usage/API_Connections/openai.md#claude)

### Google AI Studio and Vertex AI

- Tem uma camada gratuita com limites de taxa (Gemini Flash), pode exigir informações de faturamento
- [AI Studio](https://aistudio.google.com/) geralmente tem os modelos e recursos mais recentes
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio) é mais complicado de configurar, mas mais estável
- [Instruções de Configuração](/Usage/API_Connections/google.md)

### Mistral (by Mistral AI)

- Modelos eficientes de vários tamanhos e casos de uso. Você pode criar uma conta e chave de API na [plataforma deles](https://console.mistral.ai/api-keys/).
- De 32k a 128k de tamanhos de contexto para uso geral, e 32k a 256k de tamanhos de contexto para codificação.
- Camada Gratuita com limites de taxa.
- Moderação razoável, com os princípios principais da Mistral sendo neutros e capacitar os usuários, mais informações [aqui](https://mistral.ai/terms/).
- [Website](https://console.mistral.ai/), [Instruções de Configuração](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- Fornece uma API unificada para acessar todos os principais LLMs do mercado
- Sistema de crédito pago por token, bem como modelos gratuitos com solicitações diárias limitadas
- Sem moderação imposta, a menos que exigido pelo fornecedor do LLM
- [Website](https://openrouter.ai), [Instruções de Configuração](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- Fornece acesso às versões mais recentes dos modelos DeepSeek V3 (`deepseek-chat`) e DeepSeek R1 (`deepseek-reasoner`) muito populares
- Requer pagamento de créditos (mínimo de $2), mas os modelos são bastante baratos para sua qualidade
- Sem moderação na API, mas os modelos podem recusar certos prompts
- [Website](https://platform.deepseek.com/), [Instruções de Configuração](/Usage/API_Connections/openai.md#deepseek)

### AI21

- Fornece acesso aos modelos abertos da família Jamba
- Tem um teste gratuito ($10 por três meses), então requer pagar mensalmente por token
- [Website](https://ai21.com/), [Instruções de Configuração](/Usage/API_Connections/openai.md#ai21)

### Cohere

- Fornece acesso aos modelos mais recentes da Cohere (command-r, command-a, c4ai-aya, etc.)
- Tem uma camada gratuita (Trial Keys) com limites de taxa suficientes para uso casual
- [Website](https://cohere.com/), [Instruções de Configuração](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- Fornece acesso a modelos únicos Perplexity Sonar habilitados para online via sua API
- Requer ter faturamento configurado e créditos comprados
- [Website](https://perplexity.ai/), [Instruções de Configuração](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- Serviço que hospeda modelos sem restrições de várias famílias
- Usa 'créditos' para pagar por tokens em vários modelos
- Não registra prompts por padrão, mas você pode habilitar para obter descontos de crédito em tokens.
- Usa uma API similar ao `Oobabooga TextGeneration WebUI`, veja [documentação do Mancer](https://mancer.tech/docs/clients/#sampling-parameters) para detalhes.
- [Website](https://mancer.tech/), [Instruções de Configuração](/Usage/API_Connections/mancer.md)

### DreamGen

- Modelos sem censura ajustados para escrita criativa dirigível
- Créditos mensais gratuitos, bem como assinatura paga
- Modelos variando de 7B a 70B
- [Instruções de Configuração](DreamGen.md)

### Pollinations

- Não requer configuração, pode ser usado imediatamente
- Fornece acesso a uma ampla gama de modelos gratuitamente
- As saídas podem ocasionalmente incluir anúncios com links para serviços de terceiros

### NovelAI

- Sem filtro de conteúdo, o modelo mais recente é baseado em Llama 3
- Assinatura paga necessária, o nível determina o comprimento máximo de contexto
- [Website](https://novelai.net/), [Instruções de Configuração](/Usage/API_Connections/novelai.md)

### Electron Hub

- Uma chave de API desbloqueia modelos de vários fornecedores (OpenAI, Anthropic, DeepSeek, etc.) para geração de texto e imagem
- $0,25 de créditos gratuitos todos os dias, planos pagos disponíveis
- [Website](https://www.electronhub.ai/), [Instruções de Configuração](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- API unificada para mais de 300 modelos incluindo Claude, GPT-4o, Gemini, LLaMA 3, Mistral e outros
- Tem uma camada gratuita com limites de taxa, planos de assinatura e opções de pagamento conforme o uso
- [Website](https://aimlapi.com), [Docs](https://docs.aimlapi.com), [Models](https://aimlapi.com/models)
