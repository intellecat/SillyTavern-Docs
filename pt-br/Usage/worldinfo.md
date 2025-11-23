---
order: 130
icon: globe
route: /usage/core-concepts/worldinfo/
templating: false
---

# World Info

**World Info (também conhecido como Lorebooks ou Memory Books) é uma ferramenta poderosa disponível no ST para inserir prompts dinamicamente em seu chat para ajudar a orientar as respostas da IA.**

Comumente, World Info (WI para abreviar) é usado para aprimorar a compreensão da IA sobre os detalhes do seu mundo ficcional, no entanto, você pode usar uma entrada de World Info para inserir QUALQUER COISA que gostaria de inserir no prompt.

Funciona como um dicionário dinâmico que insere apenas informações relevantes de entradas de World Info quando palavras-chave associadas às entradas estão presentes no texto da mensagem.

O motor do SillyTavern ativa e integra perfeitamente o lore apropriado no prompt, fornecendo informações de contexto para a IA.

*É importante notar que, embora o World Info ajude a orientar a IA para o conteúdo desejado, ele não garante seu aparecimento nas mensagens de saída geradas. Isso depende de quão bom seu modelo é em fazer uso de informações adicionais!*

## Dicas Profissionais

* O motor de World Info é uma ferramenta de gerenciamento de prompts muito poderosa. Não se fixe apenas em adicionar lore de personagens, sinta-se livre para experimentar.
* Palavras-chave de ativação, títulos e outras informações que não estão no campo **Conteúdo** não são inseridas no contexto, então cada entrada de World Info deve ter uma descrição abrangente e independente.
* Para criar lore de mundo rico e detalhado, as entradas podem ser interligadas e fazer referência umas às outras usando ativação recursiva. Veja mais sobre [Recursão](#recursive-scanning) abaixo.
* O SillyTavern oferece orçamento de contexto flexível para informações de contexto inseridas. Para conservar tokens de prompt, é aconselhável manter o conteúdo das entradas conciso.

## Leitura Adicional

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): Guia exaustivo e detalhado sobre World Info e Lorebooks. Por kingbri, Alicat, Trappu.

## Lore de Personagem

Opcionalmente, arquivos de World Info podem ser atribuídos a um personagem para servir como fontes de lore dedicadas em todos os chats com esse personagem (incluindo grupos).

Um World Info primário pode ser vinculado ao personagem. Para fazer isso, navegue até o painel de Gerenciamento de Personagens e clique no botão de globo, depois escolha World Info de uma lista suspensa e clique em "Ok". Ao exportar o personagem, este arquivo também será incorporado nos dados do cartão do personagem.

Para desvincular, alterar ou atribuir arquivos adicionais de World Info como lore de personagem, shift-clique no botão de globo ou clique em "More..." e depois em "Link World Info". Observe que apenas o arquivo primário de World Info é exportado com o personagem.

### Estratégia de Inserção de Lore de Personagem

Ao gerar uma resposta da IA, as entradas do World Info do personagem serão combinadas com as entradas de um seletor global de World Info usando uma das seguintes estratégias:

#### Sorted Evenly (padrão)

Todas as entradas serão ordenadas de acordo com sua Ordem de Inserção como se fossem parte de um grande arquivo, ignorando a origem.

#### Character Lore First

Entradas do World Info do Personagem seriam incluídas primeiro por sua Ordem de Inserção, depois entradas do World Info Global.

#### Global Lore First

Entradas do World Info Global seriam incluídas primeiro por sua Ordem de Inserção, depois entradas do World Info do Personagem.

### Entrada de World Info

#### Key

Uma lista de palavras-chave que acionam a ativação de uma entrada de World Info. As chaves não diferenciam maiúsculas de minúsculas por padrão (isso é [configurável](#case-sensitive-keys)).

##### Expressão Regular (Regex) como Chaves

As chaves permitem uma abordagem mais flexível para correspondência, suportando regex. Isso torna possível corresponder a conteúdo mais dinâmico com palavras ou caracteres opcionais, espaçamento e todas as outras utilidades que regex fornece.  
Se uma chave definida for um regex válido (estilo regex Javascript, com `/` como delimitadores. Todas as flags são permitidas), será tratada como tal ao verificar se uma entrada deve ser acionada. Múltiplos regexes podem ser inseridos como chaves separadas e funcionarão em conjunto. Dentro de um regex, vírgulas são possíveis. Chaves de texto simples não suportam vírgulas, pois são tratadas como separadores de chave.  

Um exemplo de caso de uso para correspondência avançada de regex:  
Uma entrada/instrução que deve ser inserida quando o personagem está realizando uma ação relacionada ao clima

```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

Para mais informações sobre sintaxe e possibilidades de Regex: [Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

###### Correspondência Avançada de Regex Por Mensagem

ST prefixa cada mensagem de chat no buffer de varredura WI com `character name:` e após v1.12.6, concatena e os prefixa usando o caractere valor 1 (`\x01`).  
Isso significa que você pode corresponder entrada ou saída específica de um determinado personagem usando um regex vinculado a esse caractere de separação.

Por exemplo, para corresponder apenas o usuário dizendo "hello", você poderia usar o seguinte regex:

```js
/\x01{{user}}:[^\x01]*?hello/
```

##### Entrada de Chave

Existem dois modos para inserir palavras-chave, cada um com uma IU ligeiramente diferente. No ⌨️ *modo de texto simples* (padrão), as chaves podem ser inseridas como uma lista separada por vírgulas em um único campo de texto. Regexes também podem ser incluídos, mas não têm destaque especial. No ✨ *modo sofisticado*, as chaves aparecem como elementos separados e regexes serão destacados como tal. O controle suporta edição e exclusão de chaves. O modo pode ser alternado através do botão inline dentro do controle de entrada.

#### Optional Filter

Lista separada por vírgulas de palavras-chave adicionais em conjunto com a chave primária.
Se nenhum argumento for fornecido, esta flag é ignorada.
Suporta lógica para AND ANY, NOT ANY ou NOT ALL

1. AND ANY = Ativa a entrada apenas se a chave primária e Qualquer uma das chaves de filtro opcional estiverem no contexto escaneado.
2. AND ALL = Ativa a entrada apenas se a chave primária e TODAS as chaves de filtro opcional estiverem presentes.
3. NOT ANY = Ativa a entrada apenas se a chave primária e Nenhuma das chaves de filtro opcional estiverem no contexto escaneado.
4. NOT ALL = Impede a ativação da entrada apesar do acionamento da chave primária, se todos os filtros opcionais estiverem no contexto escaneado.

Essas chaves também suportam [regex](#regular-expression-regex-as-keys).

#### Entry Content

O texto que é inserido no prompt após a ativação da entrada.

#### Insertion Order

Valor numérico. Define uma prioridade da entrada se várias foram ativadas de uma vez. Entradas com números de ordem maiores serão inseridas mais próximas do final do contexto, pois terão mais impacto na saída. Por exemplo, uma entrada com número de Ordem 100 aparecerá no contexto antes de uma entrada com número de Ordem 250.

#### Insertion Position

* **Before Char Defs:** A entrada de World Info é inserida antes da descrição e cenário do personagem. Tem um impacto moderado na conversa.
* **After Char Defs:** A entrada de World Info é inserida após a descrição e cenário do personagem. Tem um impacto maior na conversa.
* **Before Example Messages:** A entrada de World Info é analisada como um bloco de diálogo de exemplo e inserida antes dos exemplos fornecidos pelo cartão do personagem.
* **After Example Messages:** A entrada de World Info é analisada como um bloco de diálogo de exemplo e inserida após os exemplos fornecidos pelo cartão do personagem.
* **Top of AN:** A entrada de World Info é inserida no topo do conteúdo de Author's Note. Tem um impacto variável dependendo da posição de Author's Note.
* **Bottom of AN:** A entrada de World Info é inserida na parte inferior do conteúdo de Author's Note. Tem um impacto variável dependendo da posição de Author's Note.
* **@ D:** A entrada de World Info é inserida em uma profundidade específica no chat (Depth 0 sendo a parte inferior do prompt).
  * ⚙️ - como uma mensagem de função de sistema
  * 👤 - como uma mensagem de função de usuário
  * 🤖 - como uma mensagem de função de assistente
* **Outlet:** A entrada de World Info não é injetada automaticamente. Em vez disso, seu conteúdo é armazenado sob um outlet nomeado para que você possa decidir exatamente onde ele aparece no prompt chamando-o com a [macro `{{outlet::Name}}`](#outlet-name).

Entradas de Example Message serão formatadas de acordo com as configurações de construção de prompt: Instruct Mode ou gerenciador de prompt de Chat Completion. Elas também seguem as regras de Comportamento de Example Messages: sendo gradualmente empurradas para fora em contexto cheio, sempre mantidas ou desabilitadas completamente.

Se sua Author's Note estiver desabilitada (Insertion Frequency = 0), entradas de World Info em posições A/N serão ignoradas!

#### Outlet Name

Quando a posição de inserção **Outlet** é selecionada, um campo adicional **Outlet Name** fica disponível para a entrada. O nome que você fornece aqui agrupa entradas juntas e define o token que você usará para puxá-las para o prompt manualmente.

Use a macro `{{outlet::YourName}}` no [Gerenciador de Prompts](./Prompts/prompt-manager.md) ou campos de prompt de [Formatação Avançada](./Prompts/advancedformatting.md). Quando o prompt é construído, a macro é substituída pelo conteúdo combinado de cada entrada de World Info que compartilha o mesmo nome de outlet, separado por novas linhas, ordenado por seu valor de [Ordem de Inserção](#insertion-order).

Se uma entrada de outlet estiver sem nome, ela será ignorada durante a geração, então certifique-se de preencher o campo. Nomes de outlet suportam autocompletar baseado nos nomes que você já usou para facilitar a reutilização de rótulos consistentes.

##### Limitações e ressalvas

* Colocar macros de outlet dentro de entradas de World Info não é suportado e não funcionará. Isso entra em conflito com a ordem de avaliação de World Info e pode levar a loops infinitos.
* Aninhar outlets não é suportado. Você não pode colocar uma macro de outlet dentro do conteúdo de outro outlet. Assim como acima, isso pode levar a loops infinitos.
* Campos de cartão de personagem (Description, Personality, Scenario, etc.) não podem expandir outlets. Esses campos são analisados cedo para que possam atuar como [fontes de correspondência adicionais](#additional-matching-sources) para acionadores de World Info, o que significa que outlets não estão disponíveis quando seu texto é processado. Use outro campo com suporte a macros se precisar colocar conteúdo de outlet no corpo do prompt.
* O editor de Author's Note também não pode resolver outlets. Para colocar conteúdo de outlet ao redor de Author's Note, atribua as entradas às posições de inserção **Top of AN** ou **Bottom of AN** em vez de confiar na macro.
* Nomes de outlet diferenciam maiúsculas de minúsculas. A macro `{{outlet::}}` deve usar exatamente a mesma capitalização que o **Outlet Name** da entrada, caso contrário nenhum conteúdo é retornado.
* Espaços iniciais ou finais em um nome de outlet são ignorados quando você chama a macro, então nomes salvos com espaço extra não corresponderão. Evite preencher nomes para que possam ser resolvidos corretamente.
* Macros de outlet que não têm conteúdo atribuído a elas serão substituídas por uma string vazia.

#### Entry Title / Memo

Um campo de texto para sua conveniência para rotular suas entradas, que não é utilizado pela IA ou por qualquer lógica de acionamento.

Se vazio, pode ser preenchido usando a primeira chave das entradas clicando no botão "Fill empty memos".

#### Strategy

1. 🔵 (Círculo Azul) = A entrada estaria sempre presente no prompt.
2. 🟢 (Círculo Verde) = A entrada será acionada apenas na presença da palavra-chave.
3. 🔗 (Link de Corrente) = A entrada pode ser inserida por similaridade de embedding.

Cada Entrada também tem um botão de alternância que permite habilitar ou desabilitar a entrada.

#### Probability (Trigger %)

Este valor atua como um filtro adicional que adiciona uma chance de a entrada NÃO ser inserida quando é ativada por qualquer meio (constante, chave primária, recursão).

1. Probability = 100 significa que a entrada será inserida em cada ativação.
2. Probability = 50 significa que a entrada será inserida com uma chance de 1:1.
3. Probability = 0 significa que a entrada NÃO será inserida (essencialmente desabilitando-a).

Use isso para criar eventos aleatórios em seus chats. Por exemplo, cada mensagem poderia ter 1% de chance de despertar um Deus Ancião se seu nome for mencionado na mensagem.

#### Inclusion Group

Grupos de inclusão controlam como as entradas são selecionadas quando várias entradas com o mesmo rótulo de grupo são acionadas simultaneamente. Se várias entradas tendo o mesmo rótulo de grupo foram ativadas, apenas uma será inserida no prompt.

Por padrão, a entrada escolhida é selecionada aleatoriamente com base em seu Group Weight (padrão é 100 pontos) — quanto maior o número, maior a probabilidade de seleção. Isso permite uma seleção aleatória entre as entradas acionadas, adicionando um elemento de surpresa e variedade às interações.

Uma única entrada pode fazer parte de múltiplos grupos de inclusão se forem definidos como uma lista separada por vírgulas. A mesma lógica explicada acima se aplicará. Se essa entrada for acionada, ela *desabilitará* todas as outras entradas que fazem parte de qualquer um de seus grupos. Portanto, se qualquer um dos grupos for ativado, esta entrada não será ativada.

#### Prioritize Inclusion

Para fornecer mais controle sobre quais entradas são ativadas via [Inclusion Group](/Usage/worldinfo.md#inclusion-group), você pode usar a configuração 'Prioritize Inclusion'. Esta opção permite especificar deterministicamente qual entrada escolher em vez de rolar aleatoriamente chances de Group Weight.

Se várias entradas tendo o mesmo rótulo de grupo e esta configuração ativada foram ativadas, aquela com o valor de 'Order' mais alto será selecionada. Isso é útil para criar sequências de fallback via grupos de inclusão. Por exemplo, para priorizar entradas de baixa profundidade com mais ênfase, ou para escolher uma instrução específica sobre configurar a cena sobre outra se ambas forem válidas.

#### Use Group Scoring

Quando esta configuração está habilitada globalmente ou por entrada, o número de chaves de entrada ativadas determina a seleção do vencedor do grupo. Apenas o subconjunto de um grupo com o maior número de correspondências de chave será deixado para ser ativado por Group Weight ou Inclusion Priority - o resto será desativado e removido do grupo.

Use isso para dar mais especificidade para entradas individuais em grupos grandes. Por exemplo, elas podem ter uma chave comum e uma chave específica. Uma entrada aleatória será inserida quando uma chave específica não for fornecida, e vice-versa.

A lógica de cálculo de pontuação para chaves primárias é 1 correspondência = 1 ponto.

Para chaves secundárias, a interação depende da Lógica Seletiva escolhida:

1. AND ANY: 1 correspondência secundária = 1 ponto.
2. AND ALL: 1 ponto para cada chave secundária se todas corresponderem.
3. NOT ANY e NOT ALL: sem alteração.

Exemplo:

* Entry 1. Keys: song, sing, Black Cat. Group: songs
* Entry 2. Keys: song, sing, Ghosts. Group: songs

A entrada `sing me a song` pode ativar qualquer entrada (ambas ativaram 2 chaves), mas `sing me a song about Ghosts` ativará apenas Entry 2 (ativou 3 chaves).

#### Automation ID

Permite integrar entradas de World Info com [STscripts](/For_Contributors/st-script.md) da extensão Quick Replies. Se tanto o comando de resposta rápida quanto a entrada WI tiverem o mesmo Automation ID, o comando será executado automaticamente quando a entrada com um ID correspondente for ativada.

As automações são executadas na ordem em que são acionadas, aderindo à sua estratégia de ordenação designada, combinando a [Estratégia de Inserção de Lore de Personagem](#character-lore-insertion-strategy) com a ordenação de 'Priority'. O que leva a entradas de [Círculo Azul](#strategy) processadas primeiro, seguidas por outras em sua 'Order' especificada. Entradas acionadas recursivamente serão processadas depois na mesma ordem.

O comando de script será executado apenas uma vez se várias entradas com o mesmo Automation ID forem ativadas.

#### Character Filter

Uma lista de nomes de personagens para os quais esta entrada pode ser ativada. Se esta lista não estiver vazia, a entrada só será ativada para personagens cujos nomes estão na lista. Quando uma tag é selecionada, a entrada só será ativada para personagens que tenham essa tag específica.

O modo "Exclude" inverte o filtro, significando que a entrada será ativada para todos os personagens exceto aqueles adicionados à lista ou que tenham a(s) tag(s) selecionada(s).

#### Triggers

Os tipos de geração para os quais esta entrada de World Info pode ser ativada. Se nada for selecionado, a entrada pode ser ativada para todos os tipos de geração. Se um ou mais forem selecionados, a entrada só será ativada para esses tipos específicos de geração:

* **Normal:** Solicitação de geração de mensagem regular.
* **Continue:** Quando o botão Continue é pressionado.
* **Impersonate:** Quando o botão Impersonate é pressionado.
* **Swipe:** Quando a geração é acionada deslizando.
* **Regenerate:** Quando o botão Regenerate é pressionado em chats solo.
* **Quiet:** Solicitações de geração em segundo plano, geralmente acionadas por [extensões](/extensions/index.md) ou comandos [STscript](/For_Contributors/st-script.md).

!!!
O trigger "Regenerate" não está disponível em chats em grupo, pois usa lógica de regeneração diferente: todas as mensagens da última resposta são excluídas e as mensagens são enfileiradas usando o tipo de geração "Normal" de acordo com a [estratégia de resposta de Grupo](/Usage/Characters/groupchats.md#reply-order-strategies) escolhida.
!!!

#### Additional matching sources

Por padrão, as Entradas de World Info são correspondidas apenas contra o conteúdo da conversa atual. Essas opções permitem que você corresponda a entrada contra diferentes informações de personagem que não aparecem no chat, ou mesmo informações de persona. Isso é útil quando você quer ter uma ampla gama de entradas que devem ser usadas entre vários personagens, mas não quer ter que gerenciar grandes listas de tags, ou não quer ter que atualizar listas de filtros de personagem toda vez que criar um novo. Isso também permite que você corresponda entradas com base na persona que você tem ativa.

* **Character Description**: Corresponde contra a descrição do personagem.
* **Character Personality**: Corresponde contra o resumo de personalidade do personagem, encontrado em Advanced Definitions.
* **Scenario**: Corresponde contra o cenário especificado do personagem, encontrado em Advanced Definitions.
* **Persona Description**: Corresponde contra a descrição da persona atualmente selecionada.
* **Character's Note**: Corresponde contra a nota do personagem, que pode ser encontrada em Advanced Definitions.
* **Creator's Notes**: Corresponde contra as notas do criador do personagem, que podem ser encontradas em Advanced Definitions. As notas do criador geralmente não são incluídas no prompt.

## Vector Storage Matching

A extensão Vector Storage fornece uma alternativa à correspondência de palavras-chave usando a similaridade entre as mensagens recentes do chat e o conteúdo da entrada de World Info.

Para habilitar e usar isso, os seguintes pré-requisitos precisam ser atendidos:

1. A extensão Vector Storage está habilitada e está configurada para usar uma das fontes de embedding disponíveis.
2. A caixa de seleção "Enable for World Info" está marcada nas configurações da extensão Vector Storage.
3. Ou as entradas de World Info que são permitidas para correspondência sem chave têm o status "Vectorized" (🔗) ou a opção "Enabled for all entries" está marcada nas configurações de Vector Storage.

A escolha do modelo de vetorização na extensão e o significado teórico por trás do termo "embeddings" não serão cobertos aqui. Confira o guia [Data Bank](/Usage/Characters/data-bank.md#vector-storage) se precisar de mais informações sobre este tópico.

A correspondência de Vector Storage segue este conjunto de regras:

* O número máximo de entradas que podem ser correspondidas com o Vector Storage pode ser ajustado com a configuração "Max Entries". Este número apenas define o limite e não influencia o orçamento de tokens definido nas configurações de ativação para World Info. Todas as regras de orçamento ainda se aplicam.
* Este recurso apenas substitui a verificação de palavras-chave. Todas as verificações adicionais devem ser atendidas para que a entrada seja inserida: trigger%, filtros de personagem, grupos de inclusão, etc.
* A configuração "Scan Depth" de Activation Settings ou substituições de entrada não é usada. O valor "Query messages" de Vector Storage é utilizado em vez disso para obter o texto para corresponder. Isso permite uma configuração como "Scan Depth" definido como 0, então nenhuma correspondência regular de palavra-chave será feita, mas as entradas ainda podem ser ativadas por vetores.
* Um status "Vectorized" é apenas um marcador adicional. A entrada ainda se comportaria como um registro normal, habilitado, não constante que será ativado por palavras-chave se elas forem definidas. Remova as palavras-chave se quiser que sejam ativadas apenas por vetores.

!!!info Nota
Como a qualidade de recuperação depende inteiramente das saídas do modelo de embedding, é impossível prever exatamente quais entradas serão inseridas. Se você quer resultados determinísticos e previsíveis, fique com a correspondência de palavras-chave.
!!!

## Timed Effects

Geralmente, a avaliação de World Info é sem estado, significando que o resultado da avaliação é o mesmo, dependendo apenas do contexto atual do chat. No entanto, com a introdução de Timed Effects, você pode criar entradas que têm um atraso de ativação, permanecem ativas após serem acionadas ou não podem ser acionadas após a ativação.

### Regras de Timed Effects

1. Os períodos de tempo para os efeitos são medidos em mensagens (não pares de mensagens/trocas), com 0 significando que não há efeito.
2. Os efeitos se aplicam apenas no chat onde a entrada foi ativada. Ramificações herdam o estado do chat pai.
3. Efeitos cronometrados ativos são removidos se o chat não avançar, por exemplo, se a última mensagem foi deslizada ou excluída.
4. Fazer qualquer alteração na entrada que está atualmente com efeito cronometrado fará com que o efeito seja forçadamente removido.
5. O acionamento consequente de palavras-chave não atualiza a duração do efeito se já estiver ativo.

### Tipos de Timed Effects

1. Sticky - a entrada permanece ativa por N mensagens após ser ativada. Entradas sticky ignoram verificações de probabilidade em varreduras consequentes até expirarem.
2. Cooldown - a entrada não pode ser ativada por N mensagens após ser ativada. Pode ser usado junto com sticky: a entrada entra em cooldown quando a duração sticky termina.
3. Delay - a entrada não pode ser ativada a menos que haja pelo menos N mensagens no chat no momento da avaliação.
    * Delay = 0 -> A entrada pode ser ativada a qualquer momento.
    * Delay = 1 -> A entrada não pode ser ativada se o chat estiver vazio (sem saudação).
    * Delay = 2 -> A entrada não pode ser ativada se houver zero ou apenas uma mensagem no chat, etc.

### Exemplo de Timed Effects

Configuração de entrada: sticky = 3, cooldown = 2, delay = 2.

```txt
Message 0: delay
Message 1: entry activated
Message 2: sticky
Message 3: sticky
Message 4: sticky
Message 5: cooldown
Message 6: cooldown
Message 7: entry can be activated again
```

## Activation Settings

Menu recolhível no topo da tela de World Info.

### Scan Depth

> Pode ser substituído em nível de entrada.

Define quantas mensagens no histórico do chat devem ser escaneadas para chaves de World Info.

* Se definido como 0, então apenas entradas recursadas e Author's Note são avaliadas.
* Se definido como 1, então SillyTavern apenas escaneia a última mensagem.
* 2 = duas últimas mensagens, etc.

### Include Names

Define se os nomes dos participantes do chat devem ser incluídos no buffer de texto escaneado como prefixos de mensagem. Isso permite ativar entradas que usam nomes como palavras-chave sem mencionar diretamente os nomes nas mensagens.

Veja um exemplo do texto a ser escaneado abaixo, assumindo que os participantes do chat são nomeados Alice e Bob.

Habilitado (padrão):

```txt
Alice: Hello! Good to see you.
Bob: How is the weather today?
```

Desabilitado:

```txt
Hello! Good to see you.
How is the weather today?
```

### Context % / Budget

**Define quantos tokens podem ser usados por entradas de World Info de uma vez.**
Você pode definir um limite relativo às configurações de contexto máximo da sua API (Context %) ou um limite objetivo de token (Budget)

Se o orçamento for esgotado, então nenhuma entrada adicional é ativada mesmo se as chaves estiverem presentes no prompt.

Entradas constantes serão inseridas primeiro. Depois entradas com números de ordem maiores.

Entradas inseridas ao mencionar diretamente suas chaves têm prioridade maior do que aquelas que foram mencionadas nos conteúdos de outras entradas.

### Min Activations

**Esta configuração é mutuamente exclusiva com Max Recursion Steps.**

Minimum Activations: Se definido como um valor diferente de zero, isso desconsiderará a limitação de "scan-depth", buscando todo o log de chat para trás a partir da última mensagem por palavras-chave até que tantas entradas quanto especificado em min activations tenham sido acionadas. Isso ainda será limitado pela configuração Max Depth ou seu limite geral de Budget.

*Varreduras de scan adicionais acionadas por Min Activations não verificarão entradas adicionadas por recursão em etapas anteriores. Apenas mensagens de chat e prompts de extensão podem acionar essas ativações adicionais. No entanto, as entradas ativadas por Min Activations podem acionar outras entradas como de costume.*

### Max Depth

Profundidade Máxima para escanear ao usar a configuração Min Activations.

### Recursive scanning

A varredura recursiva permite que entradas ativem outras entradas ou sejam ativadas por outras, habilitando interações complexas e dependências entre diferentes entradas de World Info. Este recurso pode aprimorar significativamente a natureza dinâmica de seus cenários criativos.  
Se a varredura recursiva está habilitada pode ser controlado com a configuração global **Recursive Scan**.  
Existem três opções disponíveis para controlar a recursão para cada entrada:

8 **Non-recursable**: Quando esta caixa de seleção está marcada, a entrada não será ativada por outras entradas. Isso é útil para informações estáticas que não devem mudar ou ser influenciadas por outras entradas de world info.
  
* **Prevent further recursion**: Selecionar esta opção garante que, uma vez que esta entrada seja ativada, ela não acionará nenhuma outra entrada. Isso é útil para evitar cadeias não intencionais de ativações.

* **Delay until recursion**: Esta entrada só será ativada durante verificações recursivas, significando que não será acionada na passagem inicial, mas pode ser ativada por outras entradas que têm recursão habilitada. Agora, com o **Recursion Level** adicionado para esses atrasos, as entradas são agrupadas por níveis. Inicialmente, apenas o primeiro nível (menor número) corresponderá. Uma vez que nenhuma correspondência seja encontrada, o próximo nível se torna elegível para correspondência, repetindo o processo até que todos os níveis sejam verificados. Isso permite mais controle sobre como e quando camadas mais profundas de informação são reveladas durante a recursão, especialmente em combinação com critérios como NOT ANY ou NOT ALL de combinação de chaves.

**Entradas podem ativar outras entradas mencionando suas palavras-chave no texto de conteúdo.**

Por exemplo, se seu World Info contém duas entradas:

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friends with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

**Ambas** serão puxadas para o contexto se o texto da mensagem mencionar **apenas Bessie**.

### Max Recursion Steps

**Esta configuração é mutuamente exclusiva com Min Activations.**

Quando definido como zero, o aninhamento de recursão é limitado apenas pelo seu orçamento de prompt. Quando definido como um valor diferente de zero, limita o número total de varreduras de scan ao máximo desejado de "nível de aninhamento".

Valores de exemplo:

* 1 efetivamente desabilita a recursão pois a verificação para após o primeiro passo.
* 2 pode ativar entradas recursivas apenas uma vez.
* 3 pode acionar recursão duas vezes...

### Case-sensitive keys

> Pode ser substituído em nível de entrada.

**Para serem puxadas para o contexto, as chaves de entrada precisam corresponder ao caso como estão definidas na entrada de World Info.**

Isso é útil quando suas chaves são palavras comuns ou partes de palavras comuns.

Por exemplo, quando esta configuração está ativa, as chaves 'rose' e 'Rose' serão tratadas de forma diferente, dependendo das entradas.

### Match whole words

> Pode ser substituído em nível de entrada.

Entradas com chaves contendo apenas uma palavra serão correspondidas apenas se a palavra inteira estiver presente no texto de pesquisa. Habilitado por padrão.

Por exemplo, se a configuração estiver habilitada e a chave de entrada for "king", então texto como "long live the king" seria correspondido, mas "it's not to my liking" não seria.

**Importante:** esta configuração pode ter um efeito prejudicial quando usada com idiomas que não usam espaço em branco para separar palavras (por exemplo, japonês ou chinês). Se você escreve entradas nesses idiomas, é aconselhável mantê-la desligada.

### Alert on overflow

Mostra um alerta se o World Info ativado exceder o orçamento de tokens alocado.
