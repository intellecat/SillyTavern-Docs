---
order: 180
icon: question
route: /usage/faq/
---

# FAQ

## Explique sobre o que é o SillyTavern

Modelos de linguagem de IA modernos como o ChatGPT ficaram tão poderosos que alguns deles agora são capazes de simular de forma convincente um personagem que você cria, e com quem você pode conversar, escrever ficção, etc. Por exemplo, você pode dizer à IA para fingir ser um instrutor de Go chamado Jubei do Japão medieval, e ela agirá e responderá de acordo. Você pode ter uma longa conversa com Jubei, ir ao pub juntos, decidir entrar em uma luta com samurais, o que você puder imaginar, e a IA acompanhará e escreverá/reagirá a este conteúdo, agindo como seu contraste e mestre de masmorras. Sua imaginação é o limite. Você pode dizer à IA para fingir que é a Mulher Maravilha. Você também pode especificar um cenário ("A Mulher Maravilha e eu estamos roubando um banco"), um estilo de escrita ("A Mulher Maravilha fala em ebonics"), ou qualquer outra coisa que você possa pensar.

O SillyTavern é um aplicativo para facilitar esses usos:

* É uma interface de usuário que lida com a comunicação com modelos de linguagem de IA.
* Permite que você crie novos cartões de personagem (prompts), e alterne entre eles facilmente.
* Permite que você importe personagens criados por outras pessoas.
* Mantém seu histórico de chat com um personagem, permitindo que você retome a qualquer momento, inicie um novo chat, revise chats antigos, etc.
* Nos bastidores, faz as coisas necessárias para preparar o prompt da IA para você. Especificamente, enviará um prompt de sistema (instruções para a IA) que prepara a IA para seguir certas regras para melhorar a precisão da resposta.

## Me dê uma visão geral das minhas opções de modelo de IA

O SillyTavern pode interagir com dois tipos de IA:

1. [Serviços web](/Usage/API_Connections/openai.md) (Baseados em nuvem, geralmente pagos, proprietários, fechados)
2. [Auto-hospedados](/Usage/API_Connections/self-hosted.md) (local, gratuito, código aberto)

### IAs de serviço web pago

Modelos web pagos são caixas pretas. Você paga uma empresa para usar o serviço de IA deles. Você coloca as informações da sua conta no SillyTavern e ele se conectará ao seu provedor para usar a IA em seu nome.

Prós:

* Muito fácil de começar.
* Escrita de IA de mais alta qualidade.

Contras:

* Custam dinheiro para usar.
* Tudo é registrado no servidor deles. Preocupações de privacidade.
* Frequentemente são censurados e recusarão conversar com você sobre certos assuntos.

### IAs Auto-hospedadas

Modelos auto-hospedados são modelos gratuitos que você pode executar no seu PC, mas requerem um PC poderoso e mais trabalho para configurar.

Prós:

* Uma vez configurados, podem ser usados gratuitamente mesmo sem acesso à Internet.
* Privacidade total. Tudo que você escreve permanece no seu próprio PC.
* Há uma grande variedade de modelos. Como uma tecnologia orientada pela comunidade, você pode encontrar modelos que se adequam a certas tarefas ou comportamentos que você deseja.

Contras:

* Não são tão capazes quanto modelos <abbr title="State of the art">SOTA</abbr> (ou seja, escrevem diálogos piores, são menos criativos, etc).
* Executar modelos locais requer uma GPU com pelo menos 6GB de VRAM.

Se você está interessado em usar estes, consulte o guia dedicado aqui: [Como Usar um Modelo Auto-hospedado](/Usage/API_Connections/self-hosted.md).

## Posso usar o SillyTavern no meu telefone ou tablet?

iPhones e iPads não são capazes de executar o aplicativo SillyTavern completo, mas como é apenas uma interface web, você pode executá-lo em outro computador na sua rede Wi-Fi doméstica, e então acessá-lo no seu navegador móvel. Consulte [Conexões Remotas](/Administration/remote-connections.md) para mais informações.

Para usuários Android, além do exposto acima, você pode executar o SillyTavern completo diretamente no seu telefone, sem precisar de um PC, usando o aplicativo Termux. Consulte [Instalação (Android)](/Installation/Android.md). (NOTA: Instalações Termux não são oficialmente suportadas, e não podemos garantir que funcionará.)

## Tentei importar um cartão de personagem PNG mas recebi um erro de que é inválido. Por quê?

Duas possibilidades:

1. O cartão não tinha as definições incorporadas dentro dele e era apenas um arquivo de imagem normal. Alguns programas ou gerenciadores de arquivos removerão as definições incorporadas do cartão quando você os salvar. Certifique-se de que está usando o arquivo PNG bruto como foi postado pela pessoa que o compartilhou.
2. O arquivo PNG era na verdade um arquivo WEBP com um nome de arquivo `.png`. Você pode tentar renomear o cartão para `.webp` antes de importar, ou procurar por uma versão PNG adequada da imagem.

## Como posso criar meu próprio personagem de IA?

1. Clique no botão Character Management
2. Clique em Create New Character
3. Em Character Name, dê um nome, como Amanda
4. Opcionalmente, clique no botão Select Avatar para escolher uma imagem de retrato para este personagem
5. Em Description, descreva o personagem, e inclua qualquer informação que você queira que seja relevante para o chat. Por exemplo: ```Amanda é uma estudante viajando durante seu ano sabático. Ela tem 1,80m de altura, e é jogadora de vôlei. Ela tem um físico atlético. Ela tem cabelos longos e castanhos. Ela ama o período da Inglaterra Vitoriana, e assistir TV e ler romances relacionados a esse período.```
Por exemplo, se você quer que Amanda seja amigável, então você adicionaria: ```Amanda é extremamente alegre e extrovertida.```
6. Em First Message, escreva a saudação do personagem quando você começar um novo chat. Por exemplo: ```*Amanda acena para você* Ei! Você também é um mochileiro?```
7. Clique no botão Create Character

Agora você tem um personagem básico com quem pode conversar. Selecione Amanda da lista de personagens, e um novo chat começará.

Note que você pode usar a Description e/ou First Message para criar um cenário mais específico, e/ou incluir-se na descrição. Por exemplo:

```txt
Description:
Amanda é uma estudante viajando durante seu ano sabático. Ela tem 1,80m de altura, e é jogadora de vôlei. Ela tem um físico atlético. Ela tem cabelos longos e castanhos. Ela ama o período da Inglaterra Vitoriana, e assistir TV e ler romances relacionados a esse período. Ela tem guardado um segredo que pesa muito em sua alma. Ela está esperando pela pessoa certa para se abrir, mas isso pode levar a um jogo de gato e rato contra uma poderosa sociedade secreta. Ela chegou recentemente em Calcutá.

Você é Rajesh Nahasmapetilon, um superastro indiano de vôlei mundialmente famoso. Você está caminhando em Calcutá. Amanda te vê e grita de empolgação.

First Message:
*Amanda corre até você, radiante.* Rajesh! Não acredito! Sou uma grande fã. Tenho seu pôster no meu quarto.
```

Qualquer informação relevante que você incluir pode ser usada. Quão bem ela é usada depende do nível de poder do modelo de IA.

NOTA: você pode voltar e editar qualquer uma dessas informações depois que o personagem for criado, exceto o nome.

## Onde minhas chaves de API são armazenadas? Por que não consigo vê-las?

O SillyTavern salva suas chaves de API em um arquivo `secrets.json` no diretório de dados do usuário (`/data/default-user/secrets.json` é o caminho padrão).

Por padrão, as chaves de API não serão visíveis na interface depois que você as tiver salvo e atualizado a página.

Para habilitar a visualização de suas chaves:

1. Defina o valor de `allowKeysExposure` para `true` no arquivo `config.yaml`.
2. Reinicie o servidor SillyTavern.
3. Clique no link 'View hidden API keys' no canto inferior direito do API Connection Panel.

## Dicas de Performance

### Por que a UI está tão lenta/instável?

* Tente habilitar o modo No Blur Effect (Fast UI) no painel User settings.
* Habilite Reduced motion nas configurações de tema da UI para remover animações cosméticas.
* Certifique-se de que seu navegador está usando Hardware Acceleration.
* Se estiver usando streaming de resposta, defina o FPS de streaming para um valor mais baixo (10-15 FPS é recomendado).

### Estou experimentando lag de entrada. O que posso fazer?

A degradação de performance, particularmente lag de entrada, é mais comumente atribuída a extensões do navegador. Extensões problemáticas conhecidas incluem:

* iCloud Password Manager
* DeepL Translation
* Ferramentas de correção gramatical baseadas em IA
* Várias extensões de bloqueio de anúncios

Se você tiver problemas de performance e não conseguir identificar a causa, ou suspeitar de um problema com o próprio SillyTavern, por favor:

1. [Grave um perfil de performance](https://developer.chrome.com/docs/devtools/performance/reference)
2. Exporte o perfil como um arquivo JSON
3. Envie-o para a equipe de desenvolvimento para análise

Recomendamos primeiro testar com todas as extensões do navegador e extensões de terceiros do SillyTavern desabilitadas para isolar a origem da degradação de performance.

### Quando importo muitos personagens, o aplicativo fica lento. Por quê?

Infelizmente, o SillyTavern não foi projetado para lidar com bibliotecas enormes de personagens. Quanto mais você tiver, mais tempo levará para carregar a lista de personagens. Dados evidenciais sugerem que a degradação de performance começa a se tornar perceptível quando você tem mais de 1000 personagens.

No entanto, há algumas coisas que você pode fazer para mitigar o problema:

**1. Use carregamento preguiçoso.**

Habilite o carregamento preguiçoso de personagens definindo o valor `performance.lazyLoadCharacters` como true no arquivo `config.yaml`. Após a próxima reinicialização do servidor, a lista de personagens carregará apenas os dados completos dos personagens com os quais você interagir. Esteja ciente de que algumas extensões de terceiros podem não funcionar corretamente com esta configuração habilitada se não foram atualizadas para suportá-la (entre em contato com o desenvolvedor da extensão para mais informações).

**2. Use cache de memória.**

Aumente a capacidade do cache de memória se você tiver alguma RAM sobrando. Isso permitirá que o servidor mantenha mais personagens na memória, reduzindo o tempo necessário para carregá-los. Você pode fazer isso ajustando o valor de `performance.memoryCacheCapacity` para um número maior no arquivo `config.yaml`. O valor padrão é `100mb`. Regra geral aproximada: aumente o valor em 100mb para cada 3000 personagens que você tiver.

**Limitações:**

1. A busca avançada (fuzzy) de personagens não funcionará com carregamento preguiçoso habilitado. Apenas nomes de personagens serão pesquisados.
2. O cache de memória está desabilitado em dispositivos Android devido à quantidade limitada de memória disponível.

## Como fazer a IA escrever mais?

Às vezes a IA responderá com apenas uma única frase quando você gostaria que fosse mais verbosa.
Isso geralmente é um problema com modelos executados localmente.

Se você simplesmente quer que o bot continue escrevendo de onde parou no final de sua resposta mais recente, você pode enviar uma mensagem de usuário vazia digitando nada na Barra de Entrada e clicando em Send. Isso forçará o bot a continuar a história.

Estratégias para corrigir isso:

* Aumente o valor da configuração `Response Length`
* Projete uma boa `First Message` para o Personagem, que os mostre falando de maneira prolixada. Modelos de IA podem melhorar muito quando recebem orientação sobre o estilo de escrita que você espera.
* Adicione uma frase na Caixa de Description do personagem como "gosta de falar muito" ou "falante muito verboso"
* Faça a mesma coisa para sua `Author's Note`, ou `Post-History Instruction Prompt`
* Como último recurso, você pode tentar ativar `Auto-Continue` (no painel User Settings), mas isso fará as respostas saírem mais lentas porque está fazendo a IA produzir pequenas respostas uma após a outra, e então combinando-as todas em uma grande resposta. Também pode ser incompatível com algumas opções de API.

## Como fazer a IA escrever menos?

Isso é principalmente apenas um problema para modelos como ChatGPT ou Claude. As mesmas estratégias podem ser aplicadas mas ao contrário.

* Diminua o valor da configuração `Response Length`
* Dê ao personagem uma frase como 'fala pouco', ou 'não fala muito' na sua Description.
* Dê ao personagem uma First Message breve para definir o tom e expectativa para o chat.
* Certifique-se de que `Auto-Continue` está desligado.

## Como fazer a IA parar de escrever as ações do meu personagem, e conduzir o enredo sozinha?

Isso deve ser tratado na `Author's Note` com uma combinação de frases como:

* \{\{char\}\}'s responses shall only be passive and reactive to \{\{user\}\}'s actions.
* Your next response shall be solely from the POV of \{\{char\}\}.
* You are never allowed to dictate actions or speech for \{\{user\}\}
