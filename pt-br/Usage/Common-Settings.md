---
order: 160
icon: sliders
route: /usage/common-settings/
---

# Configurações Comuns

Estas configurações controlam o processo de amostragem ao gerar texto usando um modelo de linguagem. O significado dessas configurações é universal para todos os backends suportados.

## Configurações de Contexto

### Response (tokens)

O número máximo de tokens que a API gerará para responder.

- Quanto maior o comprimento da resposta, mais tempo levará para gerar a resposta.
- Se suportado pela API, você pode habilitar `Streaming` para exibir a resposta pouco a pouco à medida que está sendo gerada.
- Quando `Streaming` está desligado, as respostas serão exibidas de uma vez quando estiverem completas.

### Context (tokens)

O número máximo de tokens que o SillyTavern enviará para a API como prompt, menos o comprimento da resposta.

- O contexto compreende informações do personagem, prompts do sistema, histórico de chat, etc.
- Uma linha pontilhada entre as mensagens denota o alcance do contexto para o chat. Mensagens acima dessa linha não são enviadas para a IA.
- Para ver uma composição do contexto após gerar a mensagem, clique na opção de mensagem `Prompt Itemization` (expanda o menu `...` e clique no ícone de quadrado com linhas).

## Parâmetros de Amostragem

### Temperature

Temperature controla a aleatoriedade na seleção de tokens:

- Temperature baixa (<1.0) leva a texto mais previsível, favorecendo tokens de probabilidade mais alta
- Temperature alta (>1.0) aumenta a criatividade e diversidade na saída dando aos tokens de probabilidade mais baixa uma chance melhor.

Defina como 1 para as probabilidades originais.

### Repetition Penalty

Tenta conter a repetição penalizando tokens com base em quão frequentemente eles ocorrem no contexto.

Defina o valor como 1 para desabilitar seu efeito.

#### Repetition Penalty Range

Quantos tokens a partir do último token gerado serão considerados para a penalidade de repetição. Isso pode quebrar respostas se definido muito alto, pois palavras comuns como "the, a, and," etc. serão penalizadas mais.

Defina o valor como 0 para desabilitar seu efeito.

#### Repetition Penalty Slope

Se tanto este quanto `Repetition Penalty Range` estiverem acima de 0, a penalidade de repetição terá um efeito maior no final do prompt. Quanto maior o valor, mais forte o efeito.

Defina o valor como 0 para desabilitar seu efeito.

### Top K

Top K define uma quantidade máxima de tokens principais que podem ser escolhidos. Por exemplo, se Top K for 20, isso significa que apenas os 20 tokens de classificação mais alta serão mantidos (independentemente de suas probabilidades serem diversas ou limitadas).

Defina como 0 (ou -1, dependendo do seu backend) para desabilitar.

### Top P

Top P (também conhecido como nucleus sampling) soma todos os tokens principais necessários para alcançar a porcentagem alvo. Se os 2 tokens principais forem ambos 25%, e Top P for 0.50, apenas os 2 tokens principais são considerados.

Defina o valor como 1 para desabilitar seu efeito.

### Typical P

Typical P Sampling prioriza tokens com base em seu desvio da entropia média do conjunto. Mantém tokens cuja probabilidade cumulativa está próxima de um limite predefinido (por exemplo, 0.5), enfatizando aqueles com conteúdo de informação média.

Defina o valor como 1 para desabilitar seu efeito.

### Min P

Limita o pool de tokens cortando tokens de baixa probabilidade relativos ao token principal. Produz respostas mais coerentes, mas também pode piorar a repetição se definido muito alto.

- Funciona melhor em valores baixos como `0.1-0.01`, mas pode ser definido mais alto com uma `Temperature` alta. Por exemplo: `Temperature: 5, Min P: 0.5`

Defina o valor como 0 para desabilitar seu efeito.

### Top A

Top A define um limite para seleção de tokens com base no quadrado da maior probabilidade de token. Por exemplo, se o valor Top-A for 0.2 e a probabilidade do token principal for 50%, tokens com probabilidades abaixo de 5% (0.2 * 0.5^2) são excluídos.

Defina o valor como 0 para desabilitar seu efeito.

### Tail Free Sampling

Tail-Free Sampling (TFS) procura uma cauda de tokens de baixa probabilidade na distribuição, analisando a taxa de mudança nas probabilidades de tokens usando derivadas. Retém tokens até um limite (por exemplo, 0.3) com base na segunda derivada normalizada. Quanto mais próximo de 0, mais tokens descartados.

Defina o valor como 1 para desabilitar seu efeito.

### Smoothing Factor

Aumenta a probabilidade de tokens de alta probabilidade enquanto diminui a probabilidade de tokens de baixa probabilidade usando uma transformação quadrática. Visa produzir respostas mais criativas independentemente de `Temperature`.

- Funciona melhor sem amostradores de truncamento como `Top K`, `Top P`, `Min P`, etc.

Defina o valor como 0 para desabilitar seu efeito.

### Dynamic Temperature

Escala temperature dinamicamente com base na probabilidade do token principal. Visa produzir saídas mais criativas sem sacrificar a coerência.

- Aceita um intervalo de temperature de mínimo a máximo. Por exemplo: `Minimum Temp: 0.75` e `Minimum Temp: 1.25`
- `Exponent` aplica uma curva exponencial com base no token principal.

Desmarque para desabilitar seu efeito.

### Epsilon Cutoff

Epsilon cutoff define um piso de probabilidade abaixo do qual tokens são excluídos de serem amostrados. Em unidades de 1e-4; um valor razoável é 3.

Defina como 0 para desabilitar.

### Eta Cutoff

Eta cutoff é o parâmetro principal da técnica especial de Eta Sampling. Em unidades de 1e-4; um valor razoável é 3. Veja o artigo [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191) para detalhes.

Defina como 0 para desabilitar.

### DRY Repetition Penalty

DRY penaliza tokens que estenderiam o final da entrada em uma sequência que ocorreu anteriormente na entrada. Se você quiser permitir a repetição de certas sequências literalmente (por exemplo, nomes), você pode adicioná-las à lista de quebras de sequência. Veja o Pull Request [aqui](https://github.com/oobabooga/text-generation-webui/pull/5677).

Defina multiplicador como 0 para desabilitar.

### Exclude Top Choices (XTC)

O algoritmo de amostragem XTC remove os tokens mais prováveis da consideração em vez de podar os tokens menos prováveis. Remove todos exceto o token menos provável que atende a um determinado limite, com uma determinada probabilidade. Isso garante que pelo menos uma escolha "viável" permaneça, retendo coerência. Veja o Pull Request [aqui](https://github.com/oobabooga/text-generation-webui/pull/6335).

Defina probabilidade como 0 para desabilitar.

### Mirostat

Mirostat combina a perplexidade da saída com a da entrada, evitando assim a armadilha de repetição (onde, à medida que a inferência autoregressiva produz texto, a perplexidade da saída tende a zero) e a armadilha de confusão (onde a perplexidade diverge). Para detalhes, veja o artigo [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966).

Mode escolhe a versão do Mirostat.

- 0 = desabilitar,
- 1 = Mirostat 1.0 (apenas llama.cpp),
- 2 = Mirostat 2.0.

### Beam Search

Um algoritmo ganancioso de força bruta usado na amostragem de LLM para encontrar a sequência mais provável de palavras ou tokens. Expande múltiplas sequências candidatas de uma vez, mantendo um número fixo (beam width) de sequências principais em cada etapa.

### Top nsigma

Um método de amostragem que filtra logits com base em suas propriedades estatísticas. Mantém tokens dentro de n desvios padrão do valor máximo de logit, fornecendo uma alternativa mais simples à amostragem top-p/top-k, mantendo a estabilidade de amostragem em diferentes temperatures.
