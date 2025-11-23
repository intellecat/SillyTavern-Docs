---
order: 0
icon: file-symlink-file
route: /usage/st-script/
templating: false
---

# Referência da Linguagem STscript

## O que é STscript?

É uma linguagem de script simples, porém poderosa, que pode ser usada para expandir a funcionalidade do SillyTavern sem programação séria, permitindo que você:

- Crie mini-jogos ou desafios de speed run
- Construa insights de chat alimentados por IA
- Libere sua criatividade e compartilhe com outros

STscript é construído usando o motor de comandos slash, utilizando lote de comandos, piping de dados, macros e variáveis.
Esses conceitos serão descritos no documento a seguir.

### Precaução de segurança

Com grande poder vem grande responsabilidade. Seja cuidadoso e sempre inspecione os scripts antes de executá-los.

## Hello, World!

Para executar seu primeiro script, abra qualquer chat do SillyTavern e digite o seguinte na barra de entrada de chat:

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

Você deve ver a mensagem na notificação no topo da tela. Agora vamos decompor bit por bit.

Um script é um lote de comandos, cada um começando com a barra, com ou sem argumentos nomeados e não nomeados, e terminado com o caractere separador de comando: `|`.

Os comandos são executados sequencialmente, um após o outro, e transferem dados entre si.

1. O comando `/pass` aceita um valor constante de "Hello, World!" como argumento não nomeado e o escreve no pipe.
2. O comando `/echo` recebe o valor através do pipe do comando anterior e o exibe como uma notificação toast.

!!!tip
**Dica:** Para ver uma lista de todos os comandos disponíveis, digite `/help slash` no chat.
!!!

Como argumentos não nomeados constantes e pipes são intercambiáveis, poderíamos reescrever este script simplesmente como:

```stscript
/echo Hello, World!
```

## Entrada do usuário

Agora vamos adicionar um pouco de interatividade ao script. Vamos aceitar o valor de entrada do usuário e exibi-lo na notificação.

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. O comando `/input` é usado para exibir uma caixa de entrada com o prompt especificado no argumento não nomeado e então escreve a saída no pipe.
2. Como `/echo` já tem um argumento não nomeado que define o template para a saída, usamos o macro `{{pipe}}` para especificar um local onde o valor do pipe será renderizado.

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### Outros comandos de entrada/saída

- `/popup (text)` — mostra um popup bloqueante, suporta formatação HTML lite, por exemplo: `/popup <font color=red>I'm red!</font>`.
- `/setinput (text)` — substitui o conteúdo da barra de entrada do usuário com o texto fornecido.
- `/speak voice="name" (text)` — narra o texto usando o motor TTS selecionado e o nome do personagem do mapa de voz, por exemplo `/speak name="Donald Duck" Quack!`.
- `/buttons labels=["a","b"] (text)` — mostra um popup bloqueante com o texto especificado e rótulos de botões. `labels` deve ser um array serializado em JSON de strings ou um nome de variável contendo tal array. Retorna o rótulo do botão clicado no pipe ou string vazia se cancelado. O texto suporta formatação HTML lite.

#### Argumentos para `/popup` e `/input`

`/popup` e `/input` suportam os seguintes argumentos nomeados adicionais:
- `large=on/off` - aumenta o tamanho vertical do popup. Padrão: `off`.
- `wide=on/off` - aumenta o tamanho horizontal do popup. Padrão: `off`.
- `okButton=string` - adiciona capacidade de personalizar o texto no botão "Ok". Padrão: `Ok`.
- `rows=number` - (somente para `/input`) aumenta o tamanho do controle de entrada. Padrão: 1.

Exemplo:
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### Argumentos para `/echo`

`/echo` suporta os seguintes valores para o argumento adicional `severity` que define o estilo da mensagem exibida.
  - `warning`
  - `error`
  - `info` (padrão)
  - `success`

Exemplo:

```stscript
/echo severity=error Something really bad happened.
```

## Variáveis

Variáveis são usadas para armazenar e manipular dados em scripts, usando comandos ou macros. As variáveis podem ser um dos seguintes tipos:

- Variáveis locais — salvas nos metadados do chat atual e únicas para ele.
- Variáveis globais — salvas no settings.json e existem em todo o aplicativo.

1. `/getvar name` ou `{{getvar::name}}` — obtém o valor da variável local.
2. `/setvar key=name value` ou `{{setvar::name::value}}` — define o valor da variável local.
3. `/addvar key=name increment` ou `{{addvar::name::increment}}` — adiciona o `increment` ao valor da variável local.
4. `/incvar name` ou `{{incvar::name}}` — incrementa um valor da variável local em 1.
5. `/decvar name` ou `{{decvar::name}}` — decrementa um valor da variável local em 1.
6. `/getglobalvar name` ou `{{getglobalvar::name}}` — obtém o valor da variável global.
7. `/setglobalvar key=name` ou `{{setglobalvar::name::value}}` — define o valor da variável global.
8. `/addglobalvar key=name` ou `{{addglobalvar::name:increment}}` — adiciona o `increment` ao valor da variável global.
9. `/incglobalvar name` ou `{{incglobalvar::name}}` — incrementa um valor da variável global em 1.
10. `/decglobalvar name` ou `{{decglobalvar::name}}` — decrementa um valor da variável global em 1.
11. `/flushvar name` — deleta o valor da variável local.
12. `/flushglobalvar name` — deleta o valor da variável global.

- O valor padrão de variáveis anteriormente indefinidas é uma string vazia ou um zero se for usado primeiro no comando `/addvar`, `/incvar`, `/decvar`.
- O incremento no comando `/addvar` realiza uma adição ou subtração do valor se tanto o incremento quanto o valor da variável puderem ser convertidos para um número, ou caso contrário faz a concatenação de string.
- Se um argumento de comando aceita um nome de variável e existem variáveis locais e globais com o mesmo nome, então a *variável local* tem prioridade.
- Todos os *comandos slash* para manipulação de variáveis escrevem o valor resultante no pipe para o próximo comando usar.
- Para *macros*, apenas os macros tipo "get", "inc" e "dec" retornam o valor, "add" e "set" são substituídos por uma string vazia.

Agora, vamos considerar o seguinte exemplo:

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. O valor da entrada do usuário é salvo na variável local chamada `SDinput`.
2. O macro `getvar` é usado para exibir o valor no comando `/echo`.
3. O comando `getvar` é usado para recuperar o valor da variável e passá-lo através do pipe.
4. O valor é passado para o comando `/imagine` (fornecido pelo plugin Image Generation) para ser usado como seu prompt de entrada.

Como as variáveis são salvas e não liberadas entre as execuções do script, você pode referenciar a variável em outros scripts e via macros, e ela será resolvida para o mesmo valor que durante a execução do script de exemplo. Para garantir que o valor seja descartado, adicione o comando `/flushvar` ao script.

### Arrays e objetos

Os valores das variáveis podem conter arrays serializados em JSON ou pares chave-valor (objetos).

Exemplos:
- Array: `["apple","banana","orange"]`
- Objeto: `{"fruits":["apple","banana","orange"]}`

As seguintes modificações podem ser aplicadas aos comandos para trabalhar com essas variáveis:

- O comando `/len` obtém o número de itens no array.
- O argumento nomeado `index=number/string` pode ser adicionado a `/getvar` ou `/setvar` e seus equivalentes globais para obter ou definir sub-valores por um índice baseado em zero para arrays ou uma chave de string para objetos.
  - Se um índice numérico for usado em uma variável inexistente, a variável será criada como um array vazio `[]`.
  - Se um índice de string for usado em uma variável inexistente, a variável será criada como um objeto vazio `{}`.
- Os comandos `/addvar` e `/addglobalvar` suportam adicionar um novo valor a variáveis do tipo array.

## Controle de fluxo - condicionais

Você pode usar o comando `/if` para criar expressões condicionais que ramificam a execução com base nas regras definidas.

```stscript
/if left=valueA right=valueB rule=comparison else={: /echo (command on false) :} {: /echo (command on true) :}
```

Note que

```stscript
/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"
```

a sintaxe também é suportada, porém `{: closures :}` ajudará você a escrever scripts mais limpos.

Vamos revisar o seguinte exemplo:

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else={: /echo You shall not pass | /abort :} {: /echo Welcome to the club, {{user}} :}
```

Este script avalia a entrada do usuário contra um valor requerido e exibe mensagens diferentes, dependendo do valor de entrada.

### Argumentos para `/if`

1. `left` é o primeiro operando. Vamos chamá-lo de A.
2. `right` é o segundo operando. Vamos chamá-lo de B.
3. `rule` é a operação a ser aplicada aos operandos.
4. `else` é a string opcional de subcomandos a serem executados se o resultado da comparação booleana for falso.
5. O argumento não nomeado é o subcomando a ser executado se o resultado da comparação booleana for verdadeiro.

Os valores dos operandos são avaliados na seguinte ordem:

1. Literais numéricos
2. Nomes de variáveis locais
3. Nomes de variáveis globais
4. Literais de string

Os valores de string dos argumentos nomeados podem ser escapados com aspas para permitir strings de várias palavras. As aspas são então descartadas.

### Operações booleanas

As regras suportadas para comparação booleana são as seguintes. Uma operação aplicada aos operandos resulta em um valor verdadeiro ou falso.

1. `eq` (equals) => A = B
2. `neq` (not equals) => A != B
3. `lt` (less than) => A < B
4. `gt` (greater than) => A > B
5. `lte` (less than or equals) => A <= B
6. `gte` (greater than or equals) => A >= B
7. `not` (unary negation) => !A
8. `in` (includes substring) => A includes B, insensível a maiúsculas/minúsculas
9. `nin` (not includes substring) => A not includes B, insensível a maiúsculas/minúsculas

### Subcomandos

Um subcomando é uma string contendo uma lista de comandos slash para executar.

1. Para usar lote de comandos em subcomandos, o caractere separador de comando deve ser escapado (veja abaixo).
2. Como os valores de macro são executados quando o condicional é inserido, não quando o subcomando é executado, um macro pode ser adicionalmente escapado para atrasar sua avaliação para o tempo de execução do subcomando.
3. O resultado da execução dos subcomandos é passado via pipe para o comando após `/if`.
4. O comando `/abort` interrompe a execução do script quando encontrado.

Comandos `/if` podem ser usados como um operador ternário.
O seguinte exemplo passará uma string "true" para o próximo comando se a variável `a` for igual a 5, e uma string "false" caso contrário.

```stscript
/if left=a right=5 rule=eq else={: /pass false:} {: /pass true :} |
/echo
```

## Sequências de Escape

### Macros

O escape de macros funciona como antes. No entanto, com closures, você precisará escapar macros muito menos frequentemente do que antes. Escape as duas chaves de abertura, ou tanto o par de abertura quanto o de fechamento.

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### Pipes

Pipes não precisam ser escapados em closures (quando usados como separadores de comando). Em todos os lugares onde você quiser usar um caractere pipe literal em vez de um separador de comando, você precisa escapá-lo.

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

Com a flag do parser `STRICT_ESCAPING` você não precisa escapar pipes em valores entre aspas.

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### Aspas

Para usar um caractere de aspas literal dentro de um valor entre aspas, o caractere deve ser escapado.

```stscript
/echo title="a \"b\" c" d "e" f
```

### Espaços

Para usar espaço no valor de um argumento nomeado, você deve envolver o valor em aspas ou escapar o caractere de espaço.

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### Delimitadores de Closure

Se você quiser usar as combinações de caracteres usadas para marcar o início ou fim de um closure, você deve escapar a sequência com uma única barra invertida.

```stscript
/echo \{: |
/echo \:}
```

## Quebradores de Pipe

```stscript
||
```

Para evitar que a saída do comando anterior seja automaticamente injetada como o argumento não nomeado no próximo comando, coloque pipes duplos entre os dois comandos.

```stscript
/echo we don't want to pass this on ||
/world
```

## Closures

```stscript
{: ... :}
```

Closures (blocos de instrução, lambdas, funções anônimas, como quiser chamá-los) são uma série de comandos envoltos entre `{:` e `:}`, que só são avaliados quando essa parte do código é executada.

### Sub-Comandos

Closures tornam o uso de sub-comandos muito mais fácil e eliminam a necessidade de escapar pipes e macros.

```stscript
// if without closures |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// if with closures |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### Escopos

Closures têm seu próprio escopo e suportam variáveis com escopo. Variáveis com escopo são declaradas com `/let`, seus valores definidos e recuperados com `/var`. Outra maneira de obter uma variável com escopo é o macro `{{var::}}`.

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

Dentro de um closure, você tem acesso a todas as variáveis declaradas dentro desse mesmo closure ou em um de seus ancestrais. Você não tem acesso a variáveis declaradas nos descendentes de um closure.  
Se uma variável for declarada com o mesmo nome que uma variável que foi declarada em um dos ancestrais do closure, você não tem acesso à variável ancestral neste closure e seus descendentes.

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### Closures Nomeados

```stscript
/let x {: ... :} | /:x
```

Closures podem ser atribuídos a variáveis (apenas variáveis com escopo) para serem chamados em um ponto posterior ou para serem usados como sub-comandos.

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:` também pode ser usado para executar Quick Replies, pois é apenas um atalho para `/run`.

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### Argumentos de Closure

Closures nomeados podem ter argumentos nomeados, assim como comandos slash. Os argumentos podem ter valores padrão.

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### Closures e Argumentos Pipados

O valor pipado de um closure pai não será automaticamente injetado no primeiro comando de um closure filho.  
Você ainda pode referenciar explicitamente o valor pipado do pai com `{{pipe}}`, mas se você deixar o argumento não nomeado do primeiro comando dentro de um closure em branco, o valor *não* será automaticamente injetado.

```stscript
/* This used to attempt to change the model to "foo"
   because the value "foo" from the /echo outside of
   the loop was injected into the /model command
   inside of the loop.
   Now it will simply echo the current model without 
   attempting to change it.
*/
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* You can still recreate the old behavior by
   explicitly using the {{pipe}} macro.
*/
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### Closures Imediatamente Executados

```stscript
{: ... :}()
```

Closures podem ser imediatamente executados, o que significa que serão substituídos pelo seu valor de retorno. Isso é útil em lugares onde não há suporte explícito para closures e para encurtar alguns comandos que de outra forma exigiriam muitas variáveis intermediárias.

```stscript
// a simple length comparison of two strings without closures |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// the same comparison with immediately executed closures |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

Além de executar closures nomeados salvos dentro de variáveis com escopo, o comando `/run` também pode ser usado para executar closures imediatamente.

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## Comentários

```stscript
// ... | /# ...
```

Um comentário é uma explicação ou anotação legível por humanos no código do script. Comentários não quebram pipes.

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### Comentários em Bloco

Comentários em bloco podem ser usados para comentar rapidamente múltiplos comandos de uma vez. Eles não terminarão em um pipe.

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*/
/echo foo again |
```


## Controle de Fluxo

### Loops: `/while` e `/times`

Se você precisa executar algum comando em um loop até que uma certa condição seja atendida, use o comando `/while`.

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

Em cada passo do loop ele compara o valor da variável A com o valor da variável B, e se a condição for verdadeira, então executa qualquer comando slash válido entre aspas, caso contrário sai do loop. Este comando não escreve nada no pipe de saída.

#### Argumentos para `/while`

**O conjunto de comparações booleanas disponíveis, tratamento de variáveis, valores literais e subcomandos é o mesmo que para o comando `/if`.**

O argumento nomeado opcional `guard` (`on` por padrão) é usado para proteger contra loops infinitos, limitando o número de iterações a 100.
Para desabilitar e permitir loops infinitos, defina `guard=off`.

Este exemplo adiciona 1 ao valor de `i` até atingir 10, então produz o valor resultante (10 neste caso).

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### Argumentos para `/times`

Executa um subcomando um número especificado de vezes.

`/times (repeats) "(command)"` – qualquer comando slash válido entre aspas se repete um número de vezes, por exemplo `/setvar key=i 1 | /times 5 "/addvar key=i 1"` adiciona 1 ao valor de "i" 5 vezes.
- {{timesIndex}} é substituído pelo número da iteração (baseado em zero), por exemplo `/times 4 {:/echo {{timesIndex}}:}` exibe os números de 0 a 4.
- Loops são limitados a 100 iterações por padrão, passe `guard=off` para desabilitar.

### Saindo de Loops e Closures

```stscript
/break |
```

O comando `/break` pode ser usado para sair de um loop (`/while` ou `/times`) ou de um closure mais cedo. O argumento não nomeado de `/break` pode ser usado para passar um valor diferente do pipe atual.  
`/break` está atualmente implementado nos seguintes comandos:
- `/while` - sai do loop mais cedo
- `/times` - sai do loop mais cedo
- `/run` (com um closure ou closure via variável) - sai do closure mais cedo
- `/:` (com um closure) - sai do closure mais cedo

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```
## Operações matemáticas

- Todas as seguintes operações aceitam uma série de números ou nomes de variáveis e produzem o resultado no pipe.
- Operações inválidas (como divisão por zero) e operações que resultam em um valor NaN ou infinito retornam zero.
- Multiplicação, adição, mínimo e máximo aceitam um número ilimitado de argumentos separados por espaços.
- Subtração, divisão, exponenciação e módulo aceitam dois argumentos separados por espaços.
- Seno, cosseno, logaritmo natural, raiz quadrada, valor absoluto e arredondamento aceitam um argumento.

**Lista de operações:**

1. `/add (a b c d)` – realiza uma adição do conjunto de valores, por exemplo `/add 10 i 30 j`
2. `/mul (a b c d)` – realiza uma multiplicação do conjunto de valores, por exemplo `/mul 10 i 30 j`
3. `/max (a b c d)` – retorna um máximo do conjunto de valores, por exemplo `/max 1 0 4 k`
4. `/min (a b c d)` – retorna um mínimo do conjunto de valores, por exemplo `/min 5 4 i 2`
5. `/sub (a b)` – realiza uma subtração de dois valores, por exemplo `/sub i 5`
6. `/div (a b)` – realiza uma divisão de dois valores, por exemplo `/div 10 i`
7. `/mod (a b)` – realiza uma operação de módulo de dois valores, por exemplo `/mod i 2`
8. `/pow (a b)` – realiza uma operação de potência de dois valores, por exemplo `/pow i 2`
9. `/sin (a)` – realiza uma operação de seno de um valor, por exemplo `/sin i`
10. `/cos (a)` – realiza uma operação de cosseno de um valor, por exemplo `/cos i`
11. `/log (a)` – realiza uma operação de logaritmo natural de um valor, por exemplo `/log i`
12. `/abs (a)` – realiza uma operação de valor absoluto de um valor, por exemplo `/abs -10`
13. `/sqrt (a)`– realiza uma operação de raiz quadrada de um valor, por exemplo `/sqrt 9`
14. `/round (a)` – realiza uma operação de arredondamento para o inteiro mais próximo de um valor, por exemplo `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – retorna um número aleatório entre from e to, por exemplo `/rand` ou `/rand 10` ou `/rand from=5 to=10`. Os intervalos são inclusivos. O valor retornado conterá uma parte fracionária. Use o argumento nomeado `round` para obter um valor integral, por exemplo `/rand round=ceil` para arredondar para cima, `round=floor` para arredondar para baixo, e `round=round` para arredondar para o mais próximo.

### Exemplo 1: obter a área de um círculo com raio de 50.

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### Exemplo 2: calcular um fatorial de 5.

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## Usando o LLM

Scripts podem fazer solicitações à sua API LLM atualmente conectada usando os seguintes comandos:

- `/gen (prompt)` — gera texto usando o prompt fornecido para o personagem selecionado e incluindo mensagens de chat.
- `/genraw (prompt)` — gera texto usando apenas o prompt fornecido, ignorando o personagem atual e o chat.
- `/trigger` — aciona uma geração normal (equivalente a clicar no botão "Send"). Se em chat de grupo, você pode opcionalmente fornecer um índice de membro do grupo baseado em 1 ou um nome de personagem para tê-los respondendo, caso contrário aciona uma rodada de grupo de acordo com as configurações do grupo.

### Argumentos para `/gen` e `/genraw`

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — pode ser `on` ou `off`. Especifica se a entrada do usuário deve ser bloqueada enquanto a geração está em andamento. Padrão: `off`.
- `stop` — array serializado em JSON de strings. Adiciona uma string de parada customizada (se a API suportar) apenas para esta geração. Padrão: nenhum.
- `instruct` (somente `/genraw`) — pode ser `on` ou `off`. Permite usar formatação de instrução no prompt de entrada (se o modo de instrução estiver habilitado e a API suportar). Defina como `off` para forçar prompts puros. Padrão: `on`.
- `as` (para APIs Text Completion) — pode ser `system` (padrão) ou `char`. Define como a última linha do prompt será formatada. `char` usará um nome de personagem, `system` usará nenhum nome ou nome neutro.

O texto gerado é então passado através do pipe para o próximo comando e pode ser salvo em uma variável ou exibido usando as capacidades de I/O:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

ou para inserir a mensagem gerada como uma resposta do seu personagem:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## Personagem temporal

Se você não estiver em um chat de grupo, scripts podem temporariamente fazer uma solicitação ao LLM atualmente conectado como um personagem diferente.

- `/ask (prompt)` — gera texto usando o prompt fornecido para um personagem especificado e incluindo mensagens de chat. Note que swipes da resposta deste personagem reverterão de volta ao personagem atual.

```stscript
/ask name=... (prompt)
```
### Argumentos para `/ask`

- `name` — **Obrigatório**. O nome do personagem para perguntar (ou um identificador único de personagem, como uma chave de avatar). Isso deve ser fornecido como um argumento nomeado.
- `return` — Especifica como o valor de retorno deve ser fornecido. Padrão é `pipe` (saída via pipe de comando). Outras opções podem ser especificadas se suportadas pela API.


```stscript
/ask name=Alice What is your favorite color?
```

## Injeções de prompt

Scripts podem adicionar injeções de prompt LLM customizadas, tornando-se essencialmente equivalente a Author's Notes ilimitadas.

- `/inject (text)` — insere qualquer texto no prompt LLM normal para o chat atual, e requer um identificador único. Salvo nos metadados do chat.
- `/listinjects` — mostra uma lista de todas as injeções de prompt adicionadas por scripts para o chat atual em uma mensagem do sistema.
- `/flushinjects` — deleta todas as injeções de prompt adicionadas por scripts para o chat atual.
- `/note (text)` — define o valor do Author's Note para o chat atual. Salvo nos metadados do chat.
- `/interval` — define o intervalo de inserção do Author's Note para o chat atual.
- `/depth` — define a profundidade de inserção do Author's Note para a posição in-chat.
- `/position`  — define a posição do Author's Note para o chat atual.

### Argumentos para `/inject`

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — uma string identificadora ou uma referência a uma variável. Chamadas consequentes de `/inject` com o mesmo ID sobrescreverão a injeção de texto anterior. **Argumento obrigatório.**
- `position` — define uma posição para a injeção. Padrão: `after`. Valores possíveis:
  - `after`: depois do prompt principal.
  - `before`: antes do prompt principal.
  - `chat`: in-chat.
- `depth` — define uma profundidade de injeção para a posição in-chat. 0 significa inserção após a última mensagem, 1 - antes da última mensagem, etc. Padrão: 4.
- Argumento não nomeado é um texto a ser injetado. Uma string vazia desativará o valor anterior para o identificador fornecido.

## Acessar mensagens de chat

### Ler mensagens

Você pode acessar mensagens no chat atualmente selecionado usando o comando `/messages`.

```stscript
/messages names=on/off start-finish
```

- O argumento `names` é usado para especificar se você deseja incluir nomes de personagens ou não, padrão: `on`.
- Em um argumento não nomeado, ele aceita um índice de mensagem ou intervalo no formato `start-finish`. Os intervalos são inclusivos!
- Se o intervalo não for satisfatível, ou seja, um índice inválido ou mais mensagens do que existem são solicitadas, então uma string vazia é retornada.
- Mensagens que estão ocultas do prompt (denotadas pelo ícone de fantasma) são excluídas da saída.
- Se você quiser saber o índice da última mensagem, use o macro `{{lastMessageId}}`, e `{{lastMessage}}` obterá a mensagem em si.

Para calcular o índice inicial para um intervalo, por exemplo, quando você precisa obter as últimas N mensagens, use subtração de variável.
Este exemplo obterá as 3 últimas mensagens no chat:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### Enviar mensagens

Um script pode enviar mensagens como usuário, personagem, persona, narrador neutro ou adicionar comentários.

1. `/send (text)` — adiciona uma mensagem como a persona atualmente selecionada.
2. `/sendas name=charname (text)` — adiciona uma mensagem como qualquer personagem, correspondendo pelo nome. O argumento `name` é obrigatório. Use o macro `{{char}}` para enviar como o personagem atual.
3. `/sys (text)` — adiciona uma mensagem do narrador neutro que não pertence ao usuário ou personagem. O nome exibido é puramente cosmético e pode ser customizado com o comando `/sysname`.
4. `/comment (text)` — adiciona um comentário oculto que é exibido no chat mas não é visível para o prompt.
5. `/addswipe (text)` — adiciona um swipe à última mensagem do personagem. Não pode adicionar um swipe a mensagens de usuário ou ocultas.
6. `/hide (message id or range)` — oculta uma ou várias mensagens do prompt com base no índice de mensagem fornecido ou intervalo inclusivo no formato `start-finish`.
7. `/unhide (message id or range)` — retorna uma ou várias mensagens ao prompt com base no índice de mensagem fornecido ou intervalo inclusivo no formato `start-finish`.

Os comandos `/send`, `/sendas`, `/sys` e `/comment` opcionalmente aceitam um argumento nomeado `at` com um valor numérico baseado em zero (ou um nome de variável que contém tal valor) que especifica uma posição exata de inserção da mensagem. Por padrão, novas mensagens são inseridas no final do log de chat.

Isso inserirá uma mensagem de usuário no início do histórico de conversação:

```stscript
/send at=0 Hi, I use Linux.
```

### Deletar mensagens

**Esses comandos são potencialmente destrutivos e não têm função de "desfazer". Verifique a pasta /backups/ se você deletou acidentalmente algo importante.**

1. `/cut (message id or range)` — recorta uma ou várias mensagens do chat com base no índice de mensagem fornecido ou intervalo inclusivo no formato `start-finish`.
2. `/del (number)` — deleta as últimas N mensagens do chat.
3. `/delswipe (1-based swipe id)` — deleta um swipe da última mensagem do personagem com base no ID de swipe baseado em 1 fornecido.
4. `/delname (character name)` — deleta todas as mensagens no chat atual que pertencem a um personagem com o nome especificado.
5. `/delchat` — deleta o chat atual.

## Comandos World Info

World Info (também conhecido como Lorebook) é uma ferramenta altamente utilitária para inserir dados dinamicamente no prompt. Veja a página dedicada para uma explicação mais detalhada: [World Info](/Usage/worldinfo.md).

1. `/getchatbook` – obtém o nome do arquivo World Info vinculado ao chat ou cria um novo se não estiver vinculado, e o passa pelo pipe.
2. `/findentry file=bookName field=fieldName [text]` – encontra um UID do registro do arquivo especificado (ou uma variável apontando para um nome de arquivo) usando correspondência fuzzy de um valor de campo com o texto fornecido (campo padrão: `key`) e passa o UID pelo pipe, por exemplo `/findentry file=chatLore field=key Shadowfang`.
3. `/getentryfield file=bookName field=field [UID]` – obtém um valor de campo (campo padrão: `content`) do registro com o UID do arquivo World Info especificado (ou uma variável apontando para um nome de arquivo) e passa o valor pelo pipe, por exemplo `/getentryfield file=chatLore field=content 123`.
4. `/setentryfield file=bookName uid=UID field=field [text]` – define um valor de campo (campo padrão: `content`) do registro com o UID (ou uma variável apontando para UID) do arquivo World Info especificado (ou uma variável apontando para um nome de arquivo). Para definir múltiplos valores para campos de chave, use uma lista delimitada por vírgulas como valor de texto, por exemplo `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`.
5. `/createentry file=bookName key=keyValue [content text]` – cria um novo registro no arquivo especificado (ou uma variável apontando para um nome de arquivo) com a chave e conteúdo (ambos esses argumentos são *opcionais*) e passa o UID pelo pipe, por exemplo `/createentry file=chatLore key=Shadowfang The sword of the king`.

### Campos de entrada válidos

| Campo              | Elemento UI       | Tipo de valor   |
|:-------------------|:------------------|:----------------|
| `content`          | Content           | String          |
| `comment`          | Title / Memo      | String          |
| `key`              | Primary Keywords  | Lista de strings |
| `keysecondary`     | Optional Filter   | Lista de strings |
| `constant`         | Constant Status   | Boolean (1/0)   |
| `disable`          | Disabled Status   | Boolean (1/0)   |
| `order`            | Order             | Number          |
| `selectiveLogic`   | Logic             | (veja abaixo)   |
| `excludeRecursion` | Non-recursable    | Boolean (1/0)   |
| `probability`      | Trigger%          | Number (0-100)  |
| `depth`            | Depth             | Number (0-999)  |
| `position`         | Position          | (veja abaixo)   |
| `role`             | Depth Role        | (veja abaixo)   |
| `scanDepth`        | Scan Depth        | Number (0-100)  |
| `caseSensitive`    | Case-Sensitive    | Boolean (1/0)   |
| `matchWholeWords`  | Match Whole Words | Boolean (1/0)   |
| `vectorized`       | Vectorized Status | Boolean (1/0)   |
| `automationId`     | Automation ID     | String          |
| `group`            | Inclusion Group   | String          |
| `groupOverride`    | Inclusion Group Prioritize | Boolean (1/0) |
| `groupWeight`      | Inclusion Group Weight | Number (0-100) |
| `useGroupScoring`  | Group Scoring     | Boolean (1/0)   |
| `characterFilterExclude` | Character Filter Exclude Mode | Lista de strings |
| `characterFilterNames` | Character Filter Names | Lista de strings |
| `characterFilterTags` | Character Filter Tags | Lista de strings |
| `matchCharacterDepthPrompt` | Match Character Depth Prompt | Boolean (1/0) |
| `matchCharacterDescription` | Match Character Description | Boolean (1/0) |
| `matchCharacterPersonality` | Match Character Personality | Boolean (1/0) |
| `matchCreatorNotes` | Match Creator Notes | Boolean (1/0) |
| `matchPersonaDescription` | Match Persona Description | Boolean (1/0) |
| `matchScenario` | Match Scenario | Boolean (1/0) |

**Valores de Logic**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Valores de Position**

- 0 = before main prompt
- 1 = after main prompt
- 2 = top of Author's Note
- 3 = bottom of Author's Note
- 4 = in-chat at depth
- 5 = top of example messages
- 6 = bottom of example messages

**Valores de Role** (Position = 4 apenas)
- 0 = System
- 1 = User
- 2 = Assistant

### Exemplo 1: Ler um conteúdo do lorebook de chat por chave

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### Exemplo 2: Criar uma entrada de lorebook de chat com chave e conteúdo

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### Exemplo 3: Expandir uma entrada de lorebook existente com novas informações do chat

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Milla |
/setvar key=millaUid |
/getentryfield file={{getvar::chatLore}} field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file={{getvar::chatLore}} uid=millaUid field=content {{getvar::millaContent}}
```

## Manipulação de texto

Há uma variedade de comandos utilitários de manipulação de texto úteis para serem usados em vários cenários de script.

1. `/trimtokens` — corta a entrada para o número especificado de tokens de texto do início ou do fim e produz o resultado no pipe.
2. `/trimstart` — corta a entrada para o início da primeira frase completa e produz o resultado no pipe.
3. `/trimend` — corta a entrada para o final da última frase completa e produz o resultado no pipe.
4. `/fuzzy` — realiza correspondência fuzzy do texto de entrada com a lista de strings, produzindo a melhor correspondência de string no pipe.
5. `/regex name=scriptName [text]` — executa um script regex da extensão Regex para o texto especificado. O script deve estar habilitado.

### Argumentos para `/trimtokens`

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` define a direção para cortar, que pode ser `start` ou `end`. Padrão: `end`.
2. `limit` define a quantidade de tokens a serem deixados na saída. Também pode especificar um nome de variável contendo o número. **Argumento obrigatório.**
3. Argumento não nomeado é o texto de entrada a ser cortado.

### Argumentos para `/fuzzy`

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` é um array serializado em JSON de strings contendo os candidatos. Também pode especificar um nome de variável contendo a lista. **Argumento obrigatório.**
2. Argumento não nomeado é o texto de entrada a ser correspondido. A saída é um dos candidatos correspondendo à entrada mais próxima.

## Autocomplete

- Autocomplete está habilitado tanto na entrada de chat quanto no editor grande de Quick Reply.
- Autocomplete funciona em qualquer lugar na sua entrada. Mesmo com múltiplos comandos pipados e closures aninhados.
- Autocomplete suporta três maneiras de procurar comandos correspondentes (*User Settings* -> *STscript Matching*).

1. **Starts with** A maneira "antiga". Apenas comandos que começam exatamente com o valor digitado serão exibidos.
2. **Includes**  Todos os comandos que *incluem* o valor digitado serão exibidos. Exemplo: Ao digitar `/delete`, os comandos `/qr-delete` e `/qr-set-delete` aparecerão na lista de autocomplete (/qr-**delete**, /qr-set-**delete**).
3. **Fuzzy**  Todos os comandos que podem ser correspondidos de forma fuzzy com o valor digitado serão exibidos. Exemplo: Ao digitar `/seas`, o comando `/sendas` aparecerá na lista de autocomplete (/**se**nd**as**).

- Argumentos de comando também são suportados pelo autocomplete. A lista será exibida para argumentos obrigatórios automaticamente. Para argumentos opcionais, pressione *Ctrl*+*Espaço* para abrir a lista de opções disponíveis.
- Ao digitar `/:` para executar um closure ou QR, autocomplete mostrará uma lista de variáveis com escopo e QRs.
- Autocomplete tem suporte limitado para macros (em comandos slash). Digite `{{` para obter uma lista de macros disponíveis.
- Use as *teclas de seta para cima* e *para baixo* para selecionar uma opção da lista de opções de autocomplete.
- Pressione *Enter* ou *Tab* ou *clique* em uma opção para colocar a opção no cursor.
- Pressione *Escape* para fechar a lista de autocomplete.
- Pressione *Ctrl*+*Espaço* para abrir a lista de autocomplete ou alternar os detalhes da opção selecionada.

## Flags do Parser

```stscript
/parser-flag
```

O parser aceita flags para modificar seu comportamento. Essas flags podem ser alternadas on e off em qualquer ponto em um script e toda a entrada seguinte será avaliada de acordo.  
Você pode definir suas flags padrão nas configurações de usuário.

### Strict Escaping

```stscript
/parser-flag STRICT_ESCAPING on |
```

Mudanças com `STRICT_ESCAPING` habilitado são as seguintes.

#### Pipes

Pipes não precisam ser escapados em valores entre aspas.

```stscript
/echo title="a|b" c\|d
```

#### Barras invertidas

Uma barra invertida na frente de um símbolo pode ser escapada para fornecer a barra invertida literal seguida pelo símbolo funcional.

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Replace Variable Macros

```stscript
/parser-flag REPLACE_GETVAR on |
```

Esta flag ajuda a evitar substituições duplas quando os valores das variáveis contêm texto que poderia ser interpretado como macros. Os macros `{{var::}}` são substituídos por último e nenhuma substituição adicional acontece no texto/valor da variável resultante.

Substitui todos os macros `{{getvar::}}` e `{{getglobalvar::}}` por `{{var::}}`.
Nos bastidores, o parser inserirá uma série de executores de comando antes do comando com os macros substituídos:

- chama `/let` para salvar o `{{pipe}}` atual em uma variável com escopo
- chama `/getvar` ou `/getglobalvar` para obter a variável usada no macro
- chama `/let` para salvar a variável recuperada em uma variável com escopo
- chama `/return` com o valor `{{pipe}}` salvo para restaurar o valor pipado correto para o próximo comando

```stscript
// the following will echo the last message's id / number |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// this will echo the literal text {{lastMessageId}} |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## Quick Replies: biblioteca de scripts e auto-execução

Quick Replies é uma extensão integrada do SillyTavern que fornece uma maneira fácil de armazenar e executar seus scripts.

### Configurando Quick Replies

Para começar, habilite e abra o painel de extensões (ícone de blocos empilhados) e expanda o menu Quick Replies.

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Quick Replies estão desabilitados por padrão, você precisa habilitá-los primeiro.** Então você verá uma barra aparecendo acima da sua barra de entrada de chat.

Você pode definir o rótulo de texto do botão exibido (recomendamos usar emojis para brevidade) e o script que será executado quando você clicar no botão.

O número de botões é controlado pela configuração **Number of slots** (máx = 100), ajuste de acordo com suas necessidades e clique em "Apply" quando terminar.

**Inject user input automatically** é recomendado estar desabilitado ao usar STscript, caso contrário pode interferir com suas entradas, use o macro `{{input}}` para obter o valor atual da barra de entrada em scripts.

**Quick Reply presets** permitem ter múltiplos conjuntos de Quick Replies predefinidos e alternar entre eles manualmente ou usando o comando `/qrset (name of set)`.
Não se esqueça de clicar em "Update" antes de mudar para um conjunto diferente para gravar suas mudanças no preset atualmente usado!

### Execução manual

Agora você pode adicionar seu primeiro script à biblioteca. Escolha qualquer slot livre (ou crie um), digite "Click me" na caixa esquerda para definir o rótulo, então cole isto na caixa direita:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

Então clique 5 vezes no botão que apareceu acima da barra de chat.
Cada clique incrementa a variável `clicks` em um e exibe uma mensagem diferente quando o valor é igual a 5 e reseta a variável.

### Execução automática

Abra o menu modal clicando no botão `⋮` para o comando criado.

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

Neste menu você pode fazer o seguinte:

- Editar o script em um editor conveniente em tela cheia
- Ocultar o botão da barra de chat, tornando-o acessível apenas para auto-execução.
- Habilitar execução automática em uma ou mais das seguintes condições:
  * Inicialização do aplicativo
  * Envio de uma mensagem de usuário para o chat
  * Recebimento de uma mensagem de IA no chat
  * Abertura de um chat de personagem ou grupo
  * Acionamento de uma resposta de um membro do grupo
  * Ativação de uma entrada World Info usando o mesmo Automation ID
- Fornecer uma dica de ferramenta customizada para a quick reply (texto exibido ao passar o mouse sobre a quick reply na sua UI)
- Executar o script para fins de teste

Comandos são executados automaticamente apenas se a extensão Quick Replies estiver habilitada.

Por exemplo, você pode exibir uma mensagem após enviar cinco mensagens de usuário adicionando o seguinte script e configurando-o para auto-executar na mensagem de usuário.

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### Debugger

Um debugger básico existe dentro do editor expandido de Quick Reply. Defina breakpoints com `/breakpoint |` em qualquer lugar no seu script. Ao executar o script do editor QR, a execução será interrompida nesse ponto, permitindo que você examine as variáveis disponíveis atualmente, pipe, argumentos de comando e mais, e percorra o resto do código um por um.

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
|--------------------------------------------------------|

### Chamando procedimentos

Um comando `/run` pode chamar scripts definidos nos Quick Replies por seu rótulo, basicamente fornecendo a capacidade de definir procedimentos e retornar resultados deles. Isso permite ter blocos de script reutilizáveis que outros scripts podem referenciar. O último resultado do pipe do procedimento é passado para o próximo comando após ele.

```stscript
/run ScriptLabel
```

Vamos criar dois Quick Replies:

***
**Label:**

`GetRandom`

**Command:**

```stscript
/pass {{roll:d100}}
```
***
**Label:**

`GetMessage`

**Command:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

Clicar no botão `GetMessage` chamará o procedimento `GetRandom` que resolverá o macro `{{roll}}` e passará o número para o chamador, exibindo-o para o usuário.

- Procedimentos não aceitam argumentos nomeados ou não nomeados, mas podem referenciar as mesmas variáveis que o chamador.
- Evite recursão ao chamar procedimentos, pois isso pode produzir o erro "call stack exceeded" se tratado de forma imprudente.

#### Chamando procedimentos de um preset diferente de Quick Reply

Você pode chamar um procedimento de um preset diferente de quick reply usando a sintaxe `a.b`, onde a = nome do preset QR e b = nome do rótulo QR

```stscript
/run QRpreset1.QRlabel1
```

Por padrão, o sistema primeiro procurará um rótulo de quick reply `a.b`, então se um de seus rótulos for literalmente "QRpreset1.QRlabel1" ele tentará executar isso. Se nenhum rótulo for encontrado, ele procurará um nome de preset QR "QRpreset1" com um QR rotulado "QRlabel1".

### Comandos de gerenciamento de Quick Replies

#### Criar Quick Reply

* `/qr-create (arguments, [message])` – cria um novo Quick Reply, exemplo: `/qr-create set=MyPreset label=MyButton /echo 123`

Argumentos:
- `label`    - string - texto no botão, por exemplo, `label=MyButton`
- `set`      - string - nome do conjunto QR, por exemplo, `set=PresetName1`
- `hidden`   - bool   - se o botão deve estar oculto, por exemplo, `hidden=true`
- `startup`  - bool   - executar automaticamente na inicialização do aplicativo, por exemplo, `startup=true`
- `user`     - bool   - executar automaticamente na mensagem do usuário, por exemplo, `user=true`
- `bot`      - bool   - executar automaticamente na mensagem da IA, por exemplo, `bot=true`
- `load`     - bool   - executar automaticamente no carregamento do chat, por exemplo, `load=true`
- `title`    - bool   - título / tooltip a ser mostrado no botão, por exemplo, `title="My Fancy Button"`

#### Deletar Quick Reply

* `/qr-delete (set=string [label])` – deleta Quick Reply

#### Atualizar Quick Reply

* `/qr-update (arguments, [message])` – atualiza Quick Reply, exemplo: `/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

Argumentos:
- `newlabel` - string - novo texto para o botão, por exemplo `newlabel=MyRenamedButton`
- `label`    - string - texto no botão, por exemplo, `label=MyButton`
- `set`      - string - nome do conjunto QR, por exemplo, `set=PresetName1`
- `hidden`   - bool   - se o botão deve estar oculto, por exemplo, `hidden=true`
- `startup`  - bool   - executar automaticamente na inicialização do aplicativo, por exemplo, `startup=true`
- `user`     - bool   - executar automaticamente na mensagem do usuário, por exemplo, `user=true`
- `bot`      - bool   - executar automaticamente na mensagem da IA, por exemplo, `bot=true`
- `load`     - bool   - executar automaticamente no carregamento do chat, por exemplo, `load=true`
- `title`    - bool   - título / tooltip a ser mostrado no botão, por exemplo, `title="My Fancy Button"`

####

* `qr-get` - recupera todas as propriedades de um Quick Reply, exemplo: `/qr-get set=myQrSet id=42`

#### Criar ou atualizar preset QR

* `/qr-presetupdate (arguments [label])` ou `/qr-presetadd (arguments [label])`

Argumentos:
- `enabled` - bool - habilitar ou desabilitar o preset
- `nosend`  - bool - desabilitar enviar / inserir na entrada do usuário (inválido para comandos slash)
- `before`  - bool - colocar QR antes da entrada do usuário
- `slots`   - int  - número de slots
- `inject`  - bool - injetar entrada do usuário automaticamente (se desabilitado use `{{input}}`)

Criar um novo preset (sobrescreve os existentes), exemplo: `/qr-presetadd slots=3 MyNewPreset`

#### Adicionar menu de contexto QR

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – adicionar preset de menu de contexto a um QR, exemplo: `/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### Remover todos os menus de contexto

* `/qr-contextclear (set=string [label])` – remover todos os presets de menu de contexto de um QR, exemplo: `/qr-contextclear set=MyPreset MyButton`

#### Remover um menu de contexto

* `/qr-contextdel (set=string label=string [preset name])` – remover preset de menu de contexto de um QR, exemplo: `/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Escape de valor de Quick Reply

`|{}` pode ser escapado com barra invertida na mensagem/comando QR.

Por exemplo, use `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}` para criar um QR que chama `/getvar myvar | /echo {{pipe}}`.

## Comandos de extensões

Extensões do SillyTavern (tanto integradas, baixáveis quanto de terceiros) podem adicionar seus próprios comandos slash. Abaixo está apenas um exemplo das capacidades nas extensões oficiais. A lista pode estar incompleta, certifique-se de verificar `/help slash` para a lista mais completa de comandos disponíveis.

1. `/websearch (query)` — pesquisa trechos de páginas web online pela consulta especificada e retorna o resultado no pipe. Fornecido pela extensão Web Search.
2. `/imagine (prompt)` — gera uma imagem usando o prompt fornecido. Fornecido pela extensão Image Generation.
3. `/emote (sprite)` — define um sprite para o personagem ativo por correspondência fuzzy de seu nome. Fornecido pela extensão Character Expressions.
4. `/costume (subfolder)` — define uma substituição de conjunto de sprites para o personagem ativo. Fornecido pela extensão Character Expressions.
5. `/music (name)` — força a mudança de um arquivo de música de fundo tocado por seu nome. Fornecido pela extensão Dynamic Audio.
6. `/ambient (name)` — força a mudança de um arquivo de som ambiente tocado por seu nome. Fornecido pela extensão Dynamic Audio.
7. `/roll (dice formula)` — adiciona uma mensagem oculta ao chat com o resultado de uma rolagem de dados. Fornecido pela extensão D&D Dice.

## Interação com UI

Scripts também podem interagir com a UI do SillyTavern: navegar pelos chats ou alterar parâmetros de estilo.

### Navegação de personagem

1. `/random` — abre um chat com o personagem aleatório.
2. `/go (name)` — abre um chat com o personagem do nome especificado. Primeiro, procura pela correspondência exata de nome, depois por prefixo, depois por substring.

### Estilização de UI

1. `/bubble` — define o estilo de mensagem para o estilo "bubble chat".
2. `/flat` — define o estilo de mensagem para o estilo "flat chat".
3. `/single` — define o estilo de mensagem para o estilo "single document".
4. `/movingui (name)` — ativa um preset MovingUI por nome.
5. `/resetui` — reseta o estado dos painéis MovingUI para suas posições originais.
6. `/panels` — alterna a visibilidade dos painéis da UI: barra superior, gavetas esquerda e direita.
7. `/bg (name)` — encontra e define um plano de fundo usando correspondência fuzzy de nomes. Respeita o estado de bloqueio do plano de fundo do chat.
8. `/lockbg` — bloqueia a imagem de fundo para o chat atual.
9. `/unlockbg` — desbloqueia a imagem de fundo para o chat atual.

## Mais exemplos

### Gerar resumo de chat (por @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### Uso de popup de botões

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### Obter o N-ésimo número de Fibonacci (usando a fórmula de Binet)

!!!tip
**Dica**: Defina o valor de `fib_no` para o número desejado
!!!

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### Fatorial Recursivo (usando closures)

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input Calculate factorial of: |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo factorial of {{var::n}} is {{pipe}}
```
