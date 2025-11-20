---
route: /extensions/chat-vectorization/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        summarization,
        chats,
        messages
    ]
---

# Chat Vectorization

!!!warning Aviso
O uso desta extensão não garante uma melhor experiência de chat ou memória aprimorada de qualquer tipo. Use apenas se você entender todas as implicações da utilização de banco de dados vetorial.
!!!

A vectorização de chat busca mensagens em seu histórico de chat atual que parecem relevantes para suas mensagens mais recentes.
Ela reorganiza temporariamente as mensagens mais relevantes para o início ou fim do histórico do chat.
Isso acontece quando a resposta do modelo à sua última mensagem é gerada.

As mensagens no início e fim do histórico do chat tendem a ter o maior impacto na resposta do modelo.
Portanto, reorganizar mensagens relevantes para esses locais pode ajudar o modelo a se concentrar em informações relevantes em sua resposta.

Em particular, a vectorização de chat pode encontrar mensagens relevantes que estão muito atrás no histórico de mensagens para caber
no contexto da solicitação. Reorganizar essas mensagens no contexto fornece ao modelo informações que ele não teria de outra forma.

A vectorização de chat é um tipo de geração aumentada por recuperação (RAG). A geração aumentada por recuperação aumenta a
qualidade das respostas geradas por um modelo, fornecendo informações relevantes adicionais no prompt.

* Recuperação: as mensagens mais recentes são usadas para recuperar mensagens passadas relevantes
* Aumentada: o contexto do modelo é aumentado inserindo mensagens passadas de forma útil
* Geração: o modelo é instruído a usar as mensagens passadas ao gerar a resposta

!!!info Alguns termos:
Um *vetor* é um conjunto de números que pode representar os temas, conteúdo, estilo ou outras características de um pedaço de texto.

*Vectorização* é calcular o vetor que representa um pedaço de texto. Isso é feito por um modelo de vectorização.
Assim como modelos de geração de texto fazem texto a partir de texto, modelos de vectorização fazem vetores a partir de texto.

*Busca vetorial* encontra resultados relevantes comparando vetores em vez de, digamos, palavras-chave. Se calcularmos o vetor
para uma consulta de busca, podemos compará-lo com os vetores armazenados para uma coleção de pedaços de texto. Isso encontra os textos
em nossa coleção que são mais similares ao texto na consulta de busca. No caso da vectorização de chat, a
"consulta de busca" são as 2 mensagens mais recentes, e os "textos em nossa coleção" são todas as outras mensagens no chat.
!!!

## Configuração

!!!warning Compatibilidade com Cache de Prompt
Como qualquer fonte de prompt dinâmico (World Info, Summarization, etc.), Chat Vectorization reestrutura o prefixo do prompt entre as chamadas LLM, o que pode levar a falhas frequentes de cache. Quando usado com caching, a vectorização geralmente é contraproducente, pois os prompts modificados raramente acertam o cache – efetivamente tornando o caching inútil. Você tem que escolher um ou outro, mas não ambos.
!!!

Para habilitar a vectorização de chat, selecione "Extensions" > "Vector Storage" > "Enabled for chat messages".

Configure uma fonte de vectorização e modelo de vectorização. A vectorização de chat usa a mesma fonte de vetor que Data Bank,
então você pode já ter configurado isso. As configurações para Vectorization Source e Vectorization Model estão documentadas em [Data Bank](/Usage/Characters/data-bank.md).

A vectorização de chat usa o mesmo armazenamento de vetor que Data Bank, mas isso não precisa ser configurado.
Também há informações sobre Vector Storage em [Data Bank](/Usage/Characters/data-bank.md).

A vectorização de chat não usa Data Bank para armazenar as mensagens de chat. As mensagens são armazenadas no chat.

## Preparando mensagens de chat para busca (armazenamento vetorial)

Para que as mensagens de chat possam ser pesquisadas, um vetor é calculado para cada mensagem e armazenado.

A vectorização ocorre em segundo plano, sempre que você envia ou recebe uma mensagem.

Cada mensagem é armazenada individualmente, para que possa ser encontrada e reorganizada individualmente durante a geração.

Mensagens grandes são divididas em "chunks" para que o modelo possa receber a parte mais relevante de uma mensagem longa. O tamanho do chunk é de 400 caracteres.
Você pode alterar isso com "Chunk size (chars)".

As mensagens são divididas em chunks encontrando um limite de chunk, como uma quebra de parágrafo, quebra de linha ou espaço entre palavras. Isso é para que
todos os chunks façam sentido, na medida do possível. Se suas mensagens de chat tiverem alguma outra maneira de marcar pontos de divisão naturais, como `----`,
você pode adicionar isso a "Chunk boundary". A configuração para "Chunk boundary" é compartilhada com Data Bank.

### Controles de armazenamento vetorial

Para calcular vetores para todas as mensagens no chat atual, sem esperar que sejam processadas em segundo plano, escolha "Vectorize All" nas configurações.

Para ver quantas mensagens no chat atual foram vectorizadas, escolha "View Stats". Isso exibe o número total de vetores armazenados.
Também indica as mensagens de chat específicas que foram vectorizadas, marcando-as com uma bola verde.

Para remover todos os vetores de mensagens no chat atual, escolha "Purge Vectors".

!!!
Os controles para "Vectorize All" e "Purge Vectors" **dentro de Chat vectorization** afetam apenas os vetores armazenados para o chat atual.
No entanto, há botões idênticos em File vectorization que afetam os vetores para arquivos em Data Bank. Certifique-se de que você está purgando os vetores que pretende purgar.
!!!

## Encontrando mensagens relevantes para reorganizar (recuperação vetorial)

Para encontrar as mensagens mais relevantes no histórico do chat, as mensagens mais recentes são convertidas (vectorizadas) em um vetor de consulta. Por padrão, as 2 mensagens mais recentes são usadas. Para alterar isso, mude o valor de "Query messages". Este valor também é usado ao encontrar conteúdo relevante do Data Bank.

Mensagens passadas devem ter uma pontuação de relevância de pelo menos 25% para serem incluídas. Você pode alterar isso com "Score threshold". A configuração para score threshold é compartilhada com Data Bank.

As 3 mensagens mais relevantes do histórico do chat são reorganizadas. Você pode alterar isso com "Insert#".

Para evitar perturbar os eventos mais recentes no chat, as 5 mensagens mais recentes não são reorganizadas. Para alterar isso, mude o valor de "Retain#".

## Reorganizando mensagens (geração aumentada)

As mensagens são reorganizadas para um de 3 lugares:

* O topo do chat, após o Main Prompt / Story String (o padrão)
* O topo do chat e *antes* do Main Prompt / Story String
* O fim do chat, antes das últimas 2 mensagens ("In-chat @ Depth 2"). Como você acabou de enviar uma mensagem, esta posição geralmente está logo antes da resposta anterior do modelo.

Você pode alterar isso com "Injection Position" e "Depth".

As mensagens são incluídas em ordem de relevância, com mensagens mais relevantes mostradas após mensagens menos relevantes.

O nome da pessoa ou personagem que enviou cada mensagem é incluído.

As mensagens são mostradas ao modelo como "eventos passados". Isso ajuda o modelo a entender que as mensagens contêm informações de um ponto diferente no histórico do chat do que o ponto em que são inseridas. Você pode alterar isso com "Injection Template".

Você pode ver o prompt final para o modelo usando o popup Prompt Itemization, os logs do terminal ou os logs do console do navegador. Os logs do console do navegador são úteis para entender o que todas as etapas em Chat vectorization estão fazendo.

## Summarização vetorial

!!!warning Aviso
O recurso de summarização vetorial é experimental.

**A summarização vetorial não cria resumos do seu chat. Não transforma as mensagens recuperadas em resumos. Não torna seu histórico de chat mais curto. Não é "como [Summarize](/extensions/Summarize.md) mas melhor".**
!!!

A summarização vetorial destina-se a tornar a busca vetorial de mensagens de chat mais eficaz. Ela faz isso introduzindo uma etapa de summarização antes da vectorização. A etapa de summarização extrai as partes mais importantes da mensagem, para que o vetor resultante seja um melhor indicador do que a mensagem se relaciona.

A summarização vetorial pode tornar a busca vetorial menos eficaz.

Para resumir as mensagens no histórico do chat e gerar um vetor para cada mensagem resumida, escolha "Summarize chat messages for vector generation".

A mensagem resumida não substitui a mensagem original no chat. Se uma busca vetorial corresponder ao vetor de uma mensagem resumida, a mensagem original é recuperada do histórico do chat e reorganizada no contexto. As versões resumidas das mensagens são mantidas no Vector Storage, o que pode ser de interesse para debugging.

Para resumir o conteúdo das mensagens usadas para pesquisar o histórico do chat (as últimas 2 mensagens por padrão), escolha "Summarize chat messages when sending".

Cada vez que uma mensagem é resumida para vectorização, uma solicitação separada é feita ao modelo de summarização. Você pode escolher qual fonte de summarização é usada com "Summarize with". Escolher "Main API" gerará os resumos usando o mesmo modelo e configurações de conexão que você usa para gerar completions de chat ou texto.

A solicitação consiste no conteúdo bruto da mensagem e uma instrução sobre como o modelo deve produzir o resumo. Você pode alterar a instrução com "Summary Prompt".
