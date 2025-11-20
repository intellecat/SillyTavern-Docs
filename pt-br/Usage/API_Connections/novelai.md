---
route: /usage/api-connections/novelai/
---

# NovelAI

NovelAI é um serviço de assinatura pago que permite acesso mensal ilimitado aos seus modelos internos de alta qualidade de geração de texto, geração de imagens e modelos de conversão de texto em fala. Registre uma conta aqui para começar: <https://novelai.net/>

Você receberá apenas *50 gerações* gratuitas para avaliar o modelo. Quando o erro **"Not eligible for this model"** aparecer, isso significa que você esgotou seu período de teste e precisa assinar um plano pago.

## Chave de API

Para obter sua chave de API NovelAI, siga estas etapas:

1. Selecione o ícone de engrenagem no topo da barra lateral esquerda.
![Left Sidebar](/static/novel-side.png)

2. Selecione "Account" em "User Settings".
![User Settings](/static/novel-user.png)

3. Selecione "Get Persistent API Token".
![Account](/static/novel-account.png)

4. Selecione o ícone de cópia para copiar seu token de API NovelAI para a área de transferência.
![Persistent API Token](/static/novel-token.png)

## Modelos

Se você tem Opus, então Erato é o modelo a ser usado. Se você não tem Opus, então Kayra é o melhor modelo disponível.

Clio tem um tamanho de contexto maior nos níveis Tablet/scroll, mas a força do Kayra geralmente compensa essa diferença.

## Configurações

Os arquivos com as configurações estão aqui (`SillyTavern/data/<user-handle>/NovelAI Settings`).
Você também pode adicionar manualmente seus próprios arquivos de configurações.

### Response Length

Quanto texto você deseja gerar por mensagem. Note que NovelAI tem um limite de 150 tokens por resposta.

### Context Size

Quantos tokens do chat são mantidos no contexto a qualquer momento. O tamanho máximo de contexto que você pode usar depende do modelo e do seu nível de assinatura:

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (exclusivo Opus), Kayra (Opus) e Clio (todos os níveis) - 8192 tokens

### Preamble

Texto que é inserido logo acima do chat para modificar o estilo de escrita. O formato recomendado é uma lista de tags curtas, como "[ Style: chat, detailed, sensory ]".

## Descrições de Presets
Isto é, de acordo com a Novel AI, para o que os presets padrão são bons.

### Erato

* Golden Arrow - Um bom all-rounder.
* Wilder - Maior variedade de escolha de palavras, mais diferenças entre rerolls, mais propenso a erros.
* Zany Scribe - Evita erros e repetição. Prioriza palavras mais complexas.
* Dragonfruit - Linguagem variada e complexa com pouca repetição. Erros e contradições mais frequentes.
* Shosetsu - Projetado para escrever em japonês. Funciona bem para inglês também.

### Kayra

* Asper - Para escrita criativa. Espere reviravoltas inesperadas.
* Carefree - Um bom All-rounder
* Fresh-Coffee - Mantém as coisas no caminho certo. Lida bem com instruct.
* Pro_Writer - Imita o ritmo e a sensação de ficção best-seller
* Stelenes - Mais propenso a escolher alternativas razoáveis. Variedade em tentativas.
* Tea_Time - Fica bom quando engrena.
* Writers-Daemon - Extremamente imaginativo, às vezes demais.

### Clio

* Edgewise - Lida bem com uma variedade de estilos de geração
* Fresh Coffee - Mantém as coisas no caminho certo.
* Long-Press - Destinado a prosa criativa.
* Talker Chat - Projetado para geração no estilo de chat.
* Vingt-Un - Um bom padrão geral com uma inclinação para prosa.

## Dicas e FAQs para usar NovelAI com SillyTavern

Há muitos problemas e perguntas comuns que surgem ao mudar para NovelAI de outro backend API do ST. A diferença se resume ao que os modelos são treinados. Provavelmente, você usou um modelo OpenAI ou Anthropic (ou um modelo local feito para se parecer com esses), que é construído em torno de seguir as instruções do usuário. Os modelos da NovelAI são construídos puramente em torno da conclusão de texto: em vez de pegar sua entrada como uma mensagem e formular uma resposta, os modelos da NAI tentam continuar o prompt recebido. Devido a essa diferença, muitas dicas e conhecimento comum que funcionam para outras APIs não funcionarão para NAI.

### Ajustando configurações para NovelAI

Em Advanced Formatting (o ícone A):
- Defina "Context Template" como "NovelAI"
- Defina "Tokenizer" como "Best match"
- Marque "Always add character's name to prompt"
- Marque "Collapse Consecutive Newlines"
- Desmarque a caixa "Enabled" em "Instruct Mode"

Em User Settings (a pessoa com uma engrenagem)
- Ative "Swipes" (Não específico para NAI, mas é tão útil que você deveria fazer isso)

### Construindo/Adaptando cards de personagem para NovelAI

Para otimizar seus cards de personagem para NovelAI, existem alguns métodos recomendados para escrever a descrição do seu personagem: prosa e atributos.

Prosa é tão simples que não parece que deveria funcionar: "Sylpheed is a young-looking but actually 900 year old nymph. She's short and petite, with long white hair that fades into a green gradient in her braided side ponytail, and emerald green eyes shaped like crosses.[...]" Não, realmente, é isso. Apenas escreva, em sentenças normais, como o personagem se parece, age, etc., e a IA vai captar.

Se você não confia nas suas habilidades de escrita ou quer uma maneira mais estruturada de fazer isso, você pode usar o método de atributos, que está presente nos dados de treinamento do NovelAI. Isso funciona como uma lista simples de traços de personagem de diferentes tipos. Aqui está uma lista de possíveis atributos que foram testados para serem eficazes com os modelos do NovelAI:

```
Name:
AKA:
Type: character
Setting:
Nationality:
Species:
Gender:
Age:
Height:
Weight:
Appearance:
Clothing:
Attire:
Personality:
Mind:
Mental:
Likes:
Dislikes:
Sexuality:
Speech:
Voice:
Abilities:
Skills:
Quote:
Affiliation:
Occupation:
Reputation:
Secret:
Family:
Allies:
Enemies:
Background:
Description:
Attributes:
```

"Type: character" está lá para dizer à IA que isso está descrevendo um personagem (em oposição a uma localização, objeto ou outro tipo de coisa). O resto dos atributos é opcional, e alguns são redundantes (por exemplo, Personality, Mind e Mental significam basicamente a mesma coisa), mas estes foram testados e funcionam bem com os modelos do NovelAI. Preencha aqueles que são relevantes para o seu personagem. Os atributos devem ser escritos em minúsculas e separados por vírgulas, sem necessidade de aspas ao redor das palavras. Por exemplo:

```
Skills: lockpicking, stealth, running away very fast
```

Estes métodos são recomendados porque estão presentes nos dados de treinamento do NovelAI, então eles funcionam especificamente bem com o modelo.

#### Cards de exemplo

Aqui estão alguns cards de exemplo, feitos para NovelAI, que mostram diferentes maneiras de criar cards especificamente para NovelAI. O primeiro card, Valka, usa o método de atributos para a descrição do personagem, enquanto Eris, o segundo card, usa descrições em prosa, junto com uma grande quantidade de diálogo de exemplo.

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### O que não fazer

A maioria dos formatos de card de personagem existentes não se adequa bem ao NovelAI. Eles lhe darão alguns resultados, até alguns bons, mas têm muitos problemas. W++ é um dos maiores ofensores, onde não se parece com nada com o qual os modelos do NovelAI foram treinados, e seu uso constante de colchetes/chaves/aspas consome muitos tokens, inflando o tamanho dos cards sem nenhum benefício real.

Dos formatos existentes que não são incorporados ao NovelAI, AliChat é o mais provável de funcionar, pois depende do uso de mensagens de exemplo para transmitir tanto informações sobre o personagem quanto sua voz ao mesmo tempo, no formato do tipo de mensagem que você quer que a IA produza.

Para a maioria dos outros formatos, uma vez que geralmente são maneiras de listar diferentes características de um personagem específico, eles podem ser convertidos para o método de atributos de forma bastante direta.

### Qual módulo devo usar?

Provavelmente No Module. Prose Augmenter é útil se você quiser que um personagem fale de maneira mais florida, mas tenha cuidado para não exagerar. Text Adventure pode ser útil para um card/história no estilo de aventura de texto.

### Não o módulo instruct?

Você pode invocar o módulo Instruct quando precisar. Crie uma nova linha na sua mensagem e coloque suas instruções entre chaves assim: `{ CharName is offended by that seemingly innocuous statement }` (os espaços são _necessários_ entre o texto e as chaves). Fazer isso mudará automaticamente a IA para o módulo Instruct por um curto período. Você não quer usar o módulo Instruct o tempo todo porque ele tende a produzir uma saída menos criativa do que os outros módulos, apenas quando você precisa guiar a IA fortemente em uma direção específica.

### Por que minhas respostas continuam sendo cortadas?

NovelAI limita o comprimento da resposta a ~150 tokens no total, mesmo se você definir o controle deslizante mais alto que isso. Quando atinge o número de tokens no controle deslizante ou 150, o que for menor, ele gerará até 20 tokens a mais, procurando por uma sequência de parada ou o fim de uma sentença, então há um limite efetivo de 170 tokens para uma resposta, ponto em que ele simplesmente parará, fazendo com que seja cortado.

Se for cortado, você pode selecionar a opção continue (no menu de três linhas à esquerda da caixa de texto) para fazer o personagem continuar sua resposta.

Se você regularmente deseja respostas mais longas que 170 tokens, pode contornar o limite assim:

- Mantenha o comprimento da resposta em 150 tokens.
- Em Advanced Formatting, ative Auto-continue.
- Defina o "Target length" para o comprimento desejado.

Isso encadeará várias gerações para lhe dar mensagens mais longas, mas não garante que a resposta será 100% do comprimento desejado se o modelo decidir parar.

### Como fazer o bot escrever respostas mais longas?

Leia o acima sobre respostas sendo cortadas. Isso ajudará a garantir que as respostas não sejam cortadas prematuramente ao atingir o limite de comprimento de geração.

Se suas respostas não estão sendo cortadas, mas ainda são muito curtas, é provável que você esteja lidando com "garbage in, garbage out" - se você der ao modelo exemplos ruins, ele produzirá saída ruim. Se o card do personagem não tem diálogo de exemplo ou diálogo de exemplo curto e as mensagens que você envia ao bot são curtas, o modelo perceberá isso, tomará como a maneira aceita de fazer as coisas e as respostas serão curtas. Então, escreva diálogo de exemplo mais longo e mensagens mais longas para o bot. (Você sempre pode usar NovelAI para escrever algum diálogo de exemplo para você em vez de fazê-lo você mesmo.)

### Como fazer o bot parar de falar por mim?

- Verifique se a primeira mensagem e o diálogo de exemplo do card do personagem não incluem o personagem tomando ações por você - se incluírem, então reescreva-os para remover isso
- Certifique-se de que "Always add character's name to prompt" está marcado
- Certifique-se de que você está usando atualmente a mesma persona de usuário do resto do chat. Se você mudou de personas de usuário e não voltou (ou não tem uma persona bloqueada para esse chat), as regras usuais para parar de gerar para você falharão
- Adicione ["\n\{\{user\}\}:"] a Custom Stopping Strings (não deveria ser necessário, mas às vezes ajuda)

### Por que meu personagem não está respondendo?

Muitas coisas podem causar isso, então precisamos olhar em alguns lugares:

- Certifique-se de que "Always add character's name to prompt" está marcado em Advanced Formatting
- Verifique para ter certeza de que não há erros vindo da API. Embora você possa usar SillyTavern com o teste gratuito da NAI, uma vez que ele acabe, você só receberá erros
- Verifique o que você tem em "Custom Stopping Strings" - se essas estão sendo geradas no início da resposta, ela pode ser cortada prematuramente

### Como devo usar o Author's Note?

Em geral, você provavelmente não deveria. É inserido muito perto do final do contexto, e com os modelos da NAI, frequentemente sobrepuja tudo o mais no contexto. É principalmente um artefato de modelos mais antigos e fracos onde era mais necessário.

### Como fazer uma quebra de cena/salto no tempo?

Coloque o seguinte como uma mensagem do sistema ou em novas linhas no início da sua próxima mensagem:
```
***
[ 2 days later ]
```

Então coloque o resto da sua mensagem na próxima linha. O texto entre colchetes pode ser um salto no tempo, um novo local ou qualquer outra coisa. O "***" (hilariantemente chamado de "dinkus") diz à IA que a cena mudou, e o texto entre colchetes dá mais contexto a isso.

### A IA continua repetindo palavras/frases específicas, o que eu faço?

Como mencionado acima, você pode aumentar um pouco o controle deslizante de penalidade de repetição, embora empurrá-lo muito longe possa tornar a saída incoerente.
Para corrigir o problema mais completamente, volte através do contexto, especialmente mensagens recentes, e delete a palavra/frase repetida. Removê-la do contexto dá à IA menos razão para começar a dizê-la em primeiro lugar.
