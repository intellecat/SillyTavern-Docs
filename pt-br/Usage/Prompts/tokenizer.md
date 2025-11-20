---
order: 70
route: /usage/prompts/tokenizer/
---

# Tokenizer

Um tokenizer é uma ferramenta que divide um texto em unidades menores chamadas tokens. Esses tokens podem ser palavras individuais ou até partes de palavras, como prefixos, sufixos ou pontuação. Uma regra geral é que um token geralmente corresponde a 3~4 caracteres de texto.

O SillyTavern fornece uma opção "Best match" que tenta corresponder o tokenizer usando as seguintes regras, dependendo do provedor de API usado.

APIs de Text Completion **(substituíveis)**:

1. NovelAI Clio: tokenizer NerdStash.
2. NovelAI Kayra: tokenizer NerdStash v2.
3. Text Completion: tokenizer da API (se suportado) ou tokenizer Llama.
4. KoboldAI Classic / AI Horde: tokenizer Llama.
5. KoboldCpp: tokenizer da API do modelo.

Se você obtiver resultados imprecisos ou desejar experimentar, pode definir um _tokenizer de substituição_ para o SillyTavern usar ao formar uma solicitação para o backend de IA:

1. None. Cada token é estimado em ~3,3 caracteres, arredondado para o inteiro mais próximo. **Experimente isto se seus prompts forem cortados em comprimentos de contexto altos.** Esta abordagem é usada pelo KoboldAI Lite.
2. Llama tokenizer. Usado pela família de modelos Llama 1/2: Vicuna, Hermes, Airoboros, etc. **Escolha se você usar um modelo Llama 1/2.**
3. Llama 3 tokenizer. Usado pelos modelos Llama 3/3.1. **Escolha se você usar um modelo Llama 3/3.1.**
4. NerdStash tokenizer. Usado pelo modelo Clio da NovelAI. **Escolha se você usar o modelo Clio.**
5. NerdStash v2 tokenizer. Usado pelo modelo Kayra da NovelAI. **Escolha se você usar o modelo Kayra.**
6. Mistral V1 tokenizer. Usado pela família de modelos Mistral mais antigos e seus ajustes finos. **Escolha se você usar um modelo Mistral mais antigo.**
7. Mistral Nemo tokenizer. Usado pela família de modelos Mistral Nemo e seus ajustes finos. **Escolha se você usar um modelo Mistral Nemo/Pixtral.**
8. Yi tokenizer. Usado pelos modelos Yi. **Escolha se você usar um modelo Yi.**
9. Gemma tokenizer. Usado pelos modelos Gemini/Gemma. **Escolha se você usar um modelo Gemma.**
10. DeepSeek tokenizer. Usado pelos modelos DeepSeek (como R1). **Escolha se você usar um modelo DeepSeek.**
11. API tokenizer. Consulta a API de geração para obter a contagem de tokens diretamente do modelo. Backends conhecidos que suportam: Text Generation WebUI (ooba), koboldcpp, TabbyAPI, Aphrodite API. **Escolha se você usar um backend suportado.**

APIs de Chat Completion **(não substituíveis)**:

1. OpenAI: tokenizer dependente do modelo via [tiktoken](https://github.com/openai/tiktoken).
2. Claude: tokenizer dependente do modelo via [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: tokenizers Llama, Mistral, Gemma, Yi para seus respectivos modelos.
4. Google AI Studio: tokenizer Gemma.
5. AI21 API: tokenizer Jamba (requer um download único).
6. Cohere API: tokenizer Command-R ou Command-A (requer um download único).
7. MistralAI API: tokenizer Mistral V1 ou V3 (requer um download único).
8. DeepSeek API: tokenizer DeepSeek (requer um download único).
9. Tokenizer de fallback: tokenizer GPT-3.5 turbo.

#### Tokenizers Adicionais

Esses tokenizers não estão incluídos na instalação padrão devido ao seu tamanho. Um download único é necessário quando são usados pela primeira vez.

1. Tokenizer Qwen2.
2. Tokenizers Command-R / Command-A. Usados pela fonte Cohere em Chat Completion.
3. Tokenizer Mistral V3 (Nemo). Usado pela fonte MistralAI em Chat Completion (modelos Nemo e Pixtral).
4. Tokenizer DeepSeek (deepseek-chat). Usado pela fonte DeepSeek em Chat Completion.

Se você não quiser usar downloads da internet, a opção de exclusão existe em config.yaml: `enableDownloadableTokenizers`. Defina como `false` para desabilitar downloads.

Você também pode baixar tokenizers manualmente do repositório [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers). Baixe os arquivos JSON e coloque-os no subdiretório `_cache` da raiz de seus dados, o caminho é `./data/_cache` por padrão. Crie o diretório `_cache` se ele não existir. Depois disso, reinicie o servidor SillyTavern para reinicializar os tokenizers.

Se o modelo de tokenizer necessário não estiver em cache e os downloads estiverem desabilitados, um tokenizer de fallback (Llama 3) será usado para contagem.

### Token Padding

!!! Aplica-se a: APIs de Text Completion
O SillyTavern sempre usará o tokenizer correspondente para modelos de Chat Completion, portanto não há necessidade de token padding.
!!!

A menos que o SillyTavern use um tokenizer fornecido pela API de backend remota que executa o modelo, todas as contagens de tokens assumidas durante a geração de prompt são estimadas com base no tipo de [tokenizer](#tokenizer) selecionado.

Como os resultados da tokenização podem ser imprecisos em tamanhos de contexto próximos ao máximo definido pelo modelo, algumas partes do prompt podem ser cortadas ou descartadas, o que pode afetar negativamente a coerência das definições de personagem.

Para evitar isso, o SillyTavern aloca uma porção do tamanho do contexto como padding para evitar adicionar mais itens de chat do que o modelo pode acomodar. Se você descobrir que alguma parte do prompt está sendo cortada mesmo com o tokenizer mais correspondente selecionado, ajuste o padding para que a descrição não seja truncada.

Você pode inserir valores negativos para padding reverso, o que permite alocar mais do que a quantidade máxima definida de tokens.
