---
order: 0
icon: file-symlink-file
route: /usage/st-script/
templating: false
---

# Referencia del Lenguaje STscript

## ¿Qué es STscript?

Es un lenguaje de scripting simple pero poderoso que se puede usar para expandir la funcionalidad de SillyTavern sin necesidad de programación seria, permitiéndote:

- Crear minijuegos o desafíos de velocidad
- Construir análisis de chat impulsados por IA
- Desata tu creatividad y comparte con otros

STscript se construye usando el motor de comandos de barra diagonal, utilizando ejecución por lotes, tuberías de datos, macros y variables.
Estos conceptos se describirán en el siguiente documento.

### Precaución de seguridad

Con un gran poder viene una gran responsabilidad. Ten cuidado e inspecciona siempre los scripts antes de ejecutarlos.

## ¡Hola, Mundo!

Para ejecutar tu primer script, abre cualquier chat de SillyTavern e ingresa lo siguiente en la barra de entrada de chat:

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

Deberías ver el mensaje en la notificación en la parte superior de la pantalla. Ahora desglosemos esto poco a poco.

Un script es un lote de comandos, cada uno comenzando con una barra diagonal, con o sin argumentos nombrados y sin nombre, y terminados con el carácter separador de comandos: `|`.

Los comandos se ejecutan secuencialmente, uno tras otro, y transfieren datos entre sí.

1. El comando `/pass` acepta un valor constante de "Hello, World!" como argumento sin nombre y lo escribe en la tubería.
2. El comando `/echo` recibe el valor a través de la tubería del comando anterior y lo muestra como una notificación de toast.

!!!tip
**Consejo:** Para ver una lista de todos los comandos disponibles, escribe `/help slash` en el chat.
!!!

Como los argumentos constantes sin nombre y las tuberías son intercambiables, podríamos reescribir este script simplemente como:

```stscript
/echo Hello, World!
```

## Entrada del usuario

Ahora agreguemos un poco de interactividad al script. Aceptaremos el valor de entrada del usuario y lo mostraremos en la notificación.

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. El comando `/input` se usa para mostrar un cuadro de entrada con el indicador especificado en el argumento sin nombre y luego escribe la salida en la tubería.
2. Dado que `/echo` ya tiene un argumento sin nombre que establece la plantilla para la salida, usamos la macro `{{pipe}}` para especificar un lugar donde se renderizará el valor de la tubería.

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### Otros comandos de entrada/salida

- `/popup (text)` — muestra una ventana emergente de bloqueo, soporta formato HTML lite, p. ej.: `/popup <font color=red>I'm red!</font>`.
- `/setinput (text)` — reemplaza el contenido de la barra de entrada del usuario con el texto proporcionado.
- `/speak voice="name" (text)` — narra el texto usando el motor de síntesis de voz seleccionado y el nombre del personaje del mapa de voces, p. ej.: `/speak name="Donald Duck" Quack!`.
- `/buttons labels=["a","b"] (text)` — muestra una ventana emergente de bloqueo con el texto especificado y etiquetas de botón. `labels` debe ser una matriz de cadenas serializada en JSON o un nombre de variable que contenga tal matriz. Devuelve la etiqueta del botón haciendo clic en la tubería o una cadena vacía si se cancela. El texto soporta formato HTML lite.

#### Argumentos para `/popup` e `/input`

`/popup` e `/input` soportan los siguientes argumentos nombrados adicionales:
- `large=on/off` - aumenta el tamaño vertical de la ventana emergente. Predeterminado: `off`.
- `wide=on/off` - aumenta el tamaño horizontal de la ventana emergente. Predeterminado: `off`.
- `okButton=string` - agrega la capacidad de personalizar el texto del botón "Ok". Predeterminado: `Ok`.
- `rows=number` - (solo para `/input`) aumenta el tamaño del control de entrada. Predeterminado: 1.

Ejemplo:
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### Argumentos para `/echo`

`/echo` soporta los siguientes valores para el argumento adicional de `severity` que establece el estilo del mensaje mostrado.
  - `warning`
  - `error`
  - `info` (predeterminado)
  - `success`

Ejemplo:

```stscript
/echo severity=error Something really bad happened.
```

## Variables

Las variables se usan para almacenar y manipular datos en scripts, usando comandos o macros. Las variables podrían ser uno de los siguientes tipos:

- Variables locales — guardadas en los metadatos del chat actual, y únicas para él.
- Variables globales — guardadas en settings.json y existen en todo la aplicación.

1. `/getvar name` o `{{getvar::name}}` — obtiene el valor de la variable local.
2. `/setvar key=name value` o `{{setvar::name::value}}` — establece el valor de la variable local.
3. `/addvar key=name increment` o `{{addvar::name::increment}}` — agrega el `increment` al valor de la variable local.
4. `/incvar name` o `{{incvar::name}}` — incrementa un valor de la variable local por 1.
5. `/decvar name` o `{{decvar::name}}` — disminuye un valor de la variable local por 1.
6. `/getglobalvar name` o `{{getglobalvar::name}}` — obtiene el valor de la variable global.
7. `/setglobalvar key=name` o `{{setglobalvar::name::value}}` — establece el valor de la variable global.
8. `/addglobalvar key=name` o `{{addglobalvar::name:increment}}` — agrega el `increment` al valor de la variable global.
9. `/incglobalvar name` o `{{incglobalvar::name}}` — incrementa un valor de la variable global por 1.
10. `/decglobalvar name` o `{{decglobalvar::name}}` — disminuye un valor de la variable global por 1.
11. `/flushvar name` — elimina el valor de la variable local.
12. `/flushglobalvar name` — elimina el valor de la variable global.

- El valor predeterminado de variables previamente indefinidas es una cadena vacía o un cero si se usa por primera vez en el comando `/addvar`, `/incvar`, `/decvar`.
- El incremento en el comando `/addvar` realiza una suma o resta del valor si tanto el incremento como el valor de la variable se pueden convertir a un número, o de lo contrario realiza una concatenación de cadenas.
- Si un argumento de comando acepta un nombre de variable y existen variables locales y globales con el mismo nombre, entonces la *variable local* tiene prioridad.
- Todos los *comandos de barra diagonal* para manipulación de variables escriben el valor resultante en la tubería para que el siguiente comando lo use.
- Para las *macros*, solo la macro de tipo "get", "inc" y "dec" devuelve el valor, "add" y "set" se reemplazan con una cadena vacía en su lugar.

Ahora, consideremos el siguiente ejemplo:

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. El valor de la entrada del usuario se guarda en la variable local llamada `SDinput`.
2. La macro `getvar` se usa para mostrar el valor en el comando `/echo`.
3. El comando `getvar` se usa para recuperar el valor de la variable y pasarlo a través de la tubería.
4. El valor se pasa al comando `/imagine` (proporcionado por el complemento de Generación de Imágenes) para ser usado como su indicador de entrada.

Dado que las variables se guardan y no se vacían entre las ejecuciones del script, puedes referenciar la variable en otros scripts y a través de macros, y se resolverá al mismo valor que durante la ejecución del script de ejemplo. Para garantizar que el valor se descarte, agrega el comando `/flushvar` al script.

### Matrices y objetos

Los valores de las variables pueden contener matrices serializadas en JSON o pares de clave-valor (objetos).

Ejemplos:
- Matriz: `["apple","banana","orange"]`
- Objeto: `{"fruits":["apple","banana","orange"]}`

Las siguientes modificaciones se pueden aplicar a los comandos para trabajar con estas variables:

- `/len` comando obtiene la cantidad de elementos en la matriz.
- El argumento nombrado `index=number/string` se puede agregar `/getvar` o `/setvar` y sus contrapartes globales para obtener o establecer subvalores por índice de base cero para matrices o clave de cadena para objetos.
  - Si se usa un índice numérico en una variable no existente, la variable se creará como una matriz vacía `[]`.
  - Si se usa un índice de cadena en una variable no existente, la variable se creará como un objeto vacío `{}`.
- Los comandos `/addvar` y `/addglobalvar` soportan insertar un nuevo valor en variables de tipo matriz.

## Control de flujo - condicionales

Puedes usar el comando `/if` para crear expresiones condicionales que ramifiquen la ejecución basándose en las reglas definidas.

```stscript
/if left=valueA right=valueB rule=comparison else={: /echo (command on false) :} {: /echo (command on true) :}
```

Nota que

```stscript
/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"
```

la sintaxis también es compatible, sin embargo `{: closures :}` te ayudará a escribir scripts más limpios.

Revisemos el siguiente ejemplo:

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else={: /echo You shall not pass | /abort :} {: /echo Welcome to the club, {{user}} :}
```

Este script evalúa la entrada del usuario contra un valor requerido y muestra diferentes mensajes, dependiendo del valor de entrada.

### Argumentos para `/if`

1. `left` es el primer operando. Llamémoslo A.
2. `right` es el segundo operando. Llamémoslo B.
3. `rule` es la operación a ser aplicada a los operandos.
4. `else` es la cadena opcional de subcomandos a ser ejecutada si el resultado de la comparación booleana es falso.
5. El argumento sin nombre es el subcomando a ser ejecutado si el resultado de la comparación booleana es verdadero.

Los valores de los operandos se evalúan en el siguiente orden:

1. Literales numéricos
2. Nombres de variables locales
3. Nombres de variables globales
4. Literales de cadena

Los valores de cadena de argumentos nombrados pueden escaparse con comillas para permitir cadenas de varias palabras. Las comillas se descartan después.

### Operaciones booleanas

Las reglas soportadas para comparación booleana son las siguientes. Una operación aplicada a los operandos resulta en un valor verdadero o falso.

1. `eq` (igual) => A = B
2. `neq` (no igual) => A != B
3. `lt` (menos que) => A < B
4. `gt` (mayor que) => A > B
5. `lte` (menos que o igual) => A <= B
6. `gte` (mayor que o igual) => A >= B
7. `not` (negación unaria) => !A
8. `in` (incluye subcadena) => A incluye B, sin distinción de mayúsculas y minúsculas
9. `nin` (no incluye subcadena) => A no incluye B, sin distinción de mayúsculas y minúsculas

### Subcomandos

Un subcomando es una cadena que contiene una lista de comandos de barra diagonal a ejecutar.

1. Para usar ejecución por lotes en subcomandos, el carácter separador de comandos debe escaparse (ver abajo).
2. Dado que los valores de macro se ejecutan cuando se ingresa el condicional, no cuando se ejecuta el subcomando, una macro puede escaparse adicionalmente para retrasar su evaluación al tiempo de ejecución del subcomando.
3. El resultado de la ejecución de los subcomandos se canaliza al comando después de `/if`.
4. El comando `/abort` interrumpe la ejecución del script cuando se encuentra.

Los comandos `/if` se pueden usar como un operador ternario.
El siguiente ejemplo pasará una cadena "true" al siguiente comando si la variable `a` es igual a 5, y una cadena "false" en caso contrario.

```stscript
/if left=a right=5 rule=eq else={: /pass false:} {: /pass true :} |
/echo
```

## Secuencias de Escape

### Macros

El escapamiento de macros funciona como antes. Sin embargo, con clausuras, necesitarás escapar macros mucho menos a menudo que antes. Escapa las dos llaves de apertura, o tanto el par de apertura como el de cierre.

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### Tuberías

Las tuberías no necesitan escaparse en clausuras (cuando se usan como separadores de comandos). En todas partes donde quieras usar un carácter de tubería literal en lugar de un separador de comandos, necesitas escaparlo.

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

Con la bandera de parser `STRICT_ESCAPING` no necesitas escapar tuberías en valores entrecomillados.

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### Comillas

Para usar un carácter de comilla literal dentro de un valor entrecomillado, el carácter debe escaparse.

```stscript
/echo title="a \"b\" c" d "e" f
```

### Espacios

Para usar un espacio en el valor de un argumento nombrado, debes rodear el valor entre comillas o escapar el carácter de espacio.

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### Delimitadores de Clausura

Si quieres usar las combinaciones de caracteres utilizadas para marcar el comienzo o el final de una clausura, debes escapar la secuencia con una barra invertida simple.

```stscript
/echo \{: |
/echo \:}
```

## Bloqueadores de Tubería

```stscript
||
```

Para prevenir que la salida del comando anterior se inyecte automáticamente como el argumento sin nombre en el siguiente comando, coloca tuberías dobles entre los dos comandos.

```stscript
/echo we don't want to pass this on ||
/world
```

## Clausuras

```stscript
{: ... :}
```

Las clausuras (sentencias de bloque, lambdas, funciones anónimas, como quieras llamarlas) son una serie de comandos envueltos entre `{:` y `:}`, que solo se evalúan una vez que se ejecuta esa parte del código.

### Sub-Comandos

Las clausuras hacen que usar sub-comandos sea mucho más fácil y eliminan la necesidad de escapar tuberías y macros.

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

### Ámbitos

Las clausuras tienen su propio ámbito y soportan variables con ámbito. Las variables con ámbito se declaran con `/let`, sus valores se establecen y recuperan con `/var`. Otra forma de obtener una variable con ámbito es la macro `{{var::}}`.

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

Dentro de una clausura, tienes acceso a todas las variables declaradas dentro de esa misma clausura o en uno de sus ancestros. No tienes acceso a variables declaradas en los descendientes de una clausura.
Si una variable se declara con el mismo nombre que una variable que fue declarada en uno de los ancestros de la clausura, no tienes acceso a la variable ancestro en esta clausura y sus descendientes.

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

### Clausuras Nombradas

```stscript
/let x {: ... :} | /:x
```

Las clausuras se pueden asignar a variables (solo variables con ámbito) para ser llamadas en un punto posterior o para ser usadas como sub-comandos.

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

`/:` también se puede usar para ejecutar Respuestas Rápidas, ya que es solo una abreviatura para `/run`.

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### Argumentos de Clausura

Las clausuras nombradas pueden tomar argumentos nombrados, al igual que los comandos de barra diagonal. Los argumentos pueden tener valores predeterminados.

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### Clausuras y Argumentos Canalizados

El valor canalizado de una clausura padre no se inyectará automáticamente en el primer comando de una clausura hijo.
Aún puedes referenciar explícitamente el valor canalizado del padre con `{{pipe}}`, pero si dejas el argumento sin nombre del primer comando dentro de una clausura en blanco, el valor *no* será inyectado automáticamente.

```stscript
/* This used to attempt to change the model to "foo"
   because the value "foo" from the /echo outside of
   the loop was injected into the /model command
   inside of the loop.
   Now it will simply echo the current model without
   attempting to change it.
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* You can still recreate the old behavior by
   explicitly using the {{pipe}} macro.
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### Clausuras Ejecutadas Inmediatamente

```stscript
{: ... :}()
```

Las clausuras se pueden ejecutar inmediatamente, lo que significa que serán reemplazadas con su valor de retorno. Esto es útil en lugares donde no hay soporte explícito para clausuras, y para acortar algunos comandos que de otro modo requerirían muchas variables intermedias.

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

Además de ejecutar clausuras nombradas guardadas dentro de variables con ámbito, el comando `/run` también se puede usar para ejecutar clausuras inmediatamente.

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## Comentarios

```stscript
// ... | /# ...
```

Un comentario es una explicación o anotación legible por humanos en el código del script. Los comentarios no rompen tuberías.

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### Comentarios de Bloque

Los comentarios de bloque se pueden usar para comentar rápidamente múltiples comandos a la vez. No terminarán en una tubería.

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```


## Control de Flujo

### Bucles: `/while` y `/times`

Si necesitas ejecutar algún comando en un bucle hasta que se cumpla una cierta condición, usa el comando `/while`.

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

En cada paso del bucle compara el valor de la variable A con el valor de la variable B, y si la condición es verdadera, ejecuta cualquier comando de barra diagonal válido encerrado entre comillas, de lo contrario sale del bucle. Este comando no escribe nada en la tubería de salida.

#### Argumentos para `/while`

**El conjunto de comparaciones booleanas disponibles, el manejo de variables, valores literales y subcomandos es el mismo que para el comando `/if`.**

El argumento nombrado opcional `guard` (`on` por defecto) se usa para proteger contra bucles infinitos, limitando el número de iteraciones a 100.
Para desactivar y permitir bucles infinitos, establece `guard=off`.

Este ejemplo agrega 1 al valor de `i` hasta que alcance 10, luego genera el valor resultante (10 en este caso).

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### Argumentos para `/times`

Ejecuta un subcomando un número especificado de veces.

`/times (repeats) "(command)"` – cualquier comando de barra diagonal válido encerrado entre comillas se repite un número de veces, p. ej. `/setvar key=i 1 | /times 5 "/addvar key=i 1"` agrega 1 al valor de "i" 5 veces.
- {{timesIndex}} se reemplaza con el número de iteración (basado en cero), p. ej. `/times 4 {:/echo {{timesIndex}}:}` hace eco de los números 0 al 4.
- Los bucles se limitan a 100 iteraciones por defecto, pasa `guard=off` para desactivar.

### Salir de Bucles y Clausuras

```stscript
/break |
```

El comando `/break` se puede usar para salir de un bucle (`/while` o `/times`) o de una clausura temprano. El argumento sin nombre de `/break` se puede usar para pasar un valor diferente del actual de la tubería.
`/break` actualmente está implementado en los siguientes comandos:
- `/while` - sale del bucle temprano
- `/times` - sale del bucle temprano
- `/run` (con una clausura o clausura vía variable) - sale de la clausura temprano
- `/:` (con una clausura) - sale de la clausura temprano

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

## Operaciones Matemáticas

- Todas las siguientes operaciones aceptan una serie de números o nombres de variables y producen el resultado en la tubería.
- Las operaciones inválidas (como división por cero), y operaciones que resultan en un valor NaN o infinito devuelven cero.
- La multiplicación, suma, mínimo y máximo aceptan un número ilimitado de argumentos separados por espacios.
- La resta, división, exponenciación y módulo aceptan dos argumentos separados por espacios.
- El seno, coseno, logaritmo natural, raíz cuadrada, valor absoluto y redondeo aceptan un argumento.

**Lista de operaciones:**

1. `/add (a b c d)` – realiza una suma del conjunto de valores, p. ej. `/add 10 i 30 j`
2. `/mul (a b c d)` – realiza una multiplicación del conjunto de valores, p. ej. `/mul 10 i 30 j`
3. `/max (a b c d)` – devuelve el máximo del conjunto de valores, p. ej. `/max 1 0 4 k`
4. `/min (a b c d)` – devuelve el mínimo del conjunto de valores, p. ej. `/min 5 4 i 2`
5. `/sub (a b)` – realiza una resta de dos valores, p. ej. `/sub i 5`
6. `/div (a b)` – realiza una división de dos valores, p. ej. `/div 10 i`
7. `/mod (a b)` – realiza una operación de módulo de dos valores, p. ej. `/mod i 2`
8. `/pow (a b)` – realiza una operación de potencia de dos valores, p. ej. `/pow i 2`
9. `/sin (a)` – realiza una operación de seno de un valor, p. ej. `/sin i`
10. `/cos (a)` – realiza una operación de coseno de un valor, p. ej. `/cos i`
11. `/log (a)` – realiza una operación de logaritmo natural de un valor, p. ej. `/log i`
12. `/abs (a)` – realiza una operación de valor absoluto de un valor, p. ej. `/abs -10`
13. `/sqrt (a)`– realiza una operación de raíz cuadrada de un valor, p. ej. `/sqrt 9`
14. `/round (a)` – realiza una redondeo al entero más cercano de un valor, p. ej. `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – devuelve un número aleatorio entre from y to, p. ej. `/rand` o `/rand 10` o `/rand from=5 to=10`. Los rangos son inclusivos. El valor devuelto contendrá una parte fraccional. Usa el argumento nombrado `round` para obtener un valor integral, p. ej. `/rand round=ceil` para redondear hacia arriba, `round=floor` para redondear hacia abajo, y `round=round` para redondear al más cercano.

### Ejemplo 1: obtener un área de un círculo con un radio de 50.

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### Ejemplo 2: calcular un factorial de 5.

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

## Uso del LLM

Los scripts pueden hacer solicitudes a tu API LLM actualmente conectada usando los siguientes comandos:

- `/gen (prompt)` — genera texto usando el indicador proporcionado para el personaje seleccionado e incluye mensajes de chat.
- `/genraw (prompt)` — genera texto usando solo el indicador proporcionado, ignorando el personaje actual y el chat.
- `/trigger` — desencadena una generación normal (equivalente a hacer clic en un botón "Enviar"). Si estás en un chat grupal, opcionalmente puedes proporcionar un índice de miembro de grupo basado en 1 o un nombre de personaje para que respondan, de lo contrario desencadena una ronda de grupo según la configuración del grupo.

### Argumentos para `/gen` y `/genraw`

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — puede ser `on` u `off`. Especifica si la entrada del usuario debe bloquearse mientras la generación está en progreso. Predeterminado: `off`.
- `stop` — matriz de cadenas serializada en JSON. Agrega una cadena de parada personalizada (si la API lo soporta) solo para esta generación. Predeterminado: ninguno.
- `instruct` (solo `/genraw`) — puede ser `on` u `off`. Permite usar formato de instrucción en el indicador de entrada (si el modo de instrucción está habilitado y la API lo soporta). Establece en `off` para forzar indicadores puros. Predeterminado: `on`.
- `as` (para API de Completación de Texto) — puede ser `system` (predeterminado) o `char`. Define cómo se formateará la última línea del indicador. `char` usará un nombre de personaje, `system` usará un nombre neutral o ninguno.

El texto generado se pasa luego a través de la tubería al siguiente comando y se puede guardar en una variable o mostrar usando las capacidades de E/S:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

o para insertar el mensaje generado como una respuesta de tu personaje:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## Personaje Temporal

Si no estás en un chat grupal, los scripts pueden hacer temporalmente una solicitud al LLM actualmente conectado como un personaje diferente.

- `/ask (prompt)` — genera texto usando el indicador proporcionado para un personaje especificado e incluye mensajes de chat. Tenga en cuenta que los swipes de la respuesta de este personaje volverán al personaje actual.

```stscript
/ask name=... (prompt)
```
### Argumentos para `/ask`

- `name` — **Requerido**. El nombre del personaje a preguntar (o un identificador único de personaje, como una clave de avatar). Esto debe proporcionarse como un argumento nombrado.
- `return` — Especifica cómo debe proporcionarse el valor de retorno. Por defecto es `pipe` (salida a través de la tubería de comandos). Se pueden especificar otras opciones si la API lo soporta.


```stscript
/ask name=Alice What is your favorite color?
```

## Inyecciones de Indicador

Los scripts pueden agregar inyecciones de indicador LLM personalizadas, lo que lo hace esencialmente equivalente a Notas del Autor ilimitadas.

- `/inject (text)` — inserta cualquier texto en el indicador LLM normal para el chat actual, y requiere un identificador único. Guardado en metadatos de chat.
- `/listinjects` — muestra una lista de todas las inyecciones de indicador agregadas por scripts para el chat actual en un mensaje del sistema.
- `/flushinjects` — elimina todas las inyecciones de indicador agregadas por scripts para el chat actual.
- `/note (text)` — establece el valor de Nota del Autor para el chat actual. Guardado en metadatos de chat.
- `/interval` — establece el intervalo de inserción de Nota del Autor para el chat actual.
- `/depth` — establece la profundidad de inserción de Nota del Autor para la posición en el chat.
- `/position`  — establece la posición de Nota del Autor para el chat actual.

### Argumentos para `/inject`

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — una cadena de identificador o una referencia a una variable. Las llamadas posteriores de `/inject` con el mismo ID sobrescribirán la inyección de texto anterior. **Argumento requerido.**
- `position` — establece una posición para la inyección. Predeterminado: `after`. Valores posibles:
  - `after`: después del indicador principal.
  - `before`: antes del indicador principal.
  - `chat`: en el chat.
- `depth` — establece una profundidad de inyección para la posición en el chat. 0 significa inserción después del último mensaje, 1 - antes del último mensaje, etc. Predeterminado: 4.
- El argumento sin nombre es un texto a ser inyectado. Una cadena vacía desestablecerá el valor anterior para el identificador proporcionado.

## Acceder a Mensajes de Chat

### Leer Mensajes

Puedes acceder a mensajes en el chat actualmente seleccionado usando el comando `/messages`.

```stscript
/messages names=on/off start-finish
```

- El argumento `names` se usa para especificar si deseas incluir nombres de personajes o no, predeterminado: `on`.
- En un argumento sin nombre, acepta un índice de mensaje o rango en el formato `start-finish`. ¡Los rangos son inclusivos!
- Si el rango es insatisfacible, es decir, un índice inválido o se solicitan más mensajes de los que existen, se devuelve una cadena vacía.
- Los mensajes que están ocultos en el indicador (denotados por el icono de fantasma) se excluyen de la salida.
- Si deseas saber el índice del último mensaje, usa la macro `{{lastMessageId}}`, y `{{lastMessage}}` te obtendrá el mensaje en sí.

Para calcular el índice de inicio de un rango, por ejemplo, cuando necesitas obtener los últimos N mensajes, usa la resta de variables.
Este ejemplo te obtendrá los últimos 3 mensajes en el chat:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### Enviar Mensajes

Un script puede enviar mensajes como un usuario, personaje, persona, narrador neutral, o agregar comentarios.

1. `/send (text)` — agrega un mensaje como la persona actualmente seleccionada.
2. `/sendas name=charname (text)` — agrega un mensaje como cualquier personaje, coincidiendo por su nombre. El argumento `name` es requerido. Usa la macro `{{char}}` para enviar como el personaje actual.
3. `/sys (text)` — agrega un mensaje del narrador neutral que no pertenece al usuario ni al personaje. El nombre mostrado es puramente cosmético y se puede personalizar con el comando `/sysname`.
4. `/comment (text)` — agrega un comentario oculto que se muestra en el chat pero no es visible en el indicador.
5. `/addswipe (text)` — agrega un swipe al último mensaje del personaje. No puede agregar un swipe a mensajes del usuario u ocultos.
6. `/hide (message id or range)` — oculta uno o varios mensajes del indicador según el índice de mensaje proporcionado o rango inclusivo en el formato `start-finish`.
7. `/unhide (message id or range)` — devuelve uno o varios mensajes al indicador según el índice de mensaje proporcionado o rango inclusivo en el formato `start-finish`.

Los comandos `/send`, `/sendas`, `/sys` y `/comment` aceptan opcionalmente un argumento nombrado `at` con un valor numérico basado en cero (o un nombre de variable que contenga tal valor) que especifica una posición exacta de inserción de mensaje. Por defecto los nuevos mensajes se insertan al final del registro de chat.

Esto insertará un mensaje del usuario al comienzo del historial de conversación:

```stscript
/send at=0 Hi, I use Linux.
```

### Eliminar Mensajes

**Estos comandos son potencialmente destructivos y no tienen función "deshacer". Verifica la carpeta /backups/ si accidentalmente eliminaste algo importante.**

1. `/cut (message id or range)` — corta uno o varios mensajes del chat según el índice de mensaje proporcionado o rango inclusivo en el formato `start-finish`.
2. `/del (number)` — elimina los últimos N mensajes del chat.
3. `/delswipe (1-based swipe id)` — elimina un swipe del último mensaje del personaje según el ID de swipe 1-basado proporcionado.
4. `/delname (character name)` — elimina todos los mensajes en el chat actual que pertenezcan a un personaje con el nombre especificado.
5. `/delchat` — elimina el chat actual.

## Comandos de World Info

World Info (también conocida como Lorebook) es una herramienta altamente utilitaria para insertar dinámicamente datos en el indicador. Consulta la página dedicada para una explicación más detallada: [World Info](/Usage/worldinfo.md).

1. `/getchatbook` – obtiene el nombre del archivo World Info vinculado al chat o crea uno nuevo si no estaba vinculado, y lo pasa por la tubería.
2. `/findentry file=bookName field=fieldName [text]` – encuentra un UID del registro del archivo especificado (o una variable que apunta a un nombre de archivo) usando coincidencia difusa del valor de un campo con el texto proporcionado (campo predeterminado: `key`) y pasa el UID por la tubería, p. ej. `/findentry file=chatLore field=key Shadowfang`.
3. `/getentryfield file=bookName field=field [UID]` – obtiene el valor de un campo (campo predeterminado: `content`) del registro con el UID del archivo World Info especificado (o una variable que apunta a un nombre de archivo) y pasa el valor por la tubería, p. ej. `/getentryfield file=chatLore field=content 123`.
4. `/setentryfield file=bookName uid=UID field=field [text]` – establece el valor de un campo (campo predeterminado: `content`) del registro con el UID (o una variable que apunta a un UID) del archivo World Info especificado (o una variable que apunta a un nombre de archivo). Para establecer múltiples valores para campos clave, usa una lista delimitada por comas como valor de texto, p. ej. `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`.
5. `/createentry file=bookName key=keyValue [content text]` – crea un nuevo registro en el archivo especificado (o una variable que apunta a un nombre de archivo) con la clave y el contenido (ambos argumentos son *opcionales*) y pasa el UID por la tubería, p. ej. `/createentry file=chatLore key=Shadowfang The sword of the king`.

### Campos de Entrada Válidos

| Campo              | Elemento UI       | Tipo de Valor   |
|:-------------------|:------------------|:----------------|
| `content`          | Contenido         | Cadena          |
| `comment`          | Título / Memo     | Cadena          |
| `key`              | Palabras Clave Primarias | Lista de cadenas |
| `keysecondary`     | Filtro Opcional   | Lista de cadenas |
| `constant`         | Estado Constante  | Booleano (1/0)  |
| `disable`          | Estado Deshabilitado | Booleano (1/0)  |
| `order`            | Orden             | Número          |
| `selectiveLogic`   | Lógica            | (ver abajo)     |
| `excludeRecursion` | No-Recursable     | Booleano (1/0)  |
| `probability`      | Disparador%       | Número (0-100)  |
| `depth`            | Profundidad       | Número (0-999)  |
| `position`         | Posición          | (ver abajo)     |
| `role`             | Rol de Profundidad | (ver abajo)     |
| `scanDepth`        | Profundidad de Escaneo | Número (0-100)  |
| `caseSensitive`    | Sensible a Mayúsculas | Booleano (1/0)  |
| `matchWholeWords`  | Coincidir Palabras Completas | Booleano (1/0)  |
| `vectorized`       | Estado Vectorizado | Booleano (1/0)  |
| `automationId`     | ID de Automatización | Cadena          |
| `group`            | Grupo de Inclusión | Cadena          |
| `groupOverride`    | Priorizar Grupo de Inclusión | Booleano (1/0) |
| `groupWeight`      | Peso del Grupo de Inclusión | Número (0-100) |
| `useGroupScoring`  | Puntuación de Grupo | Booleano (1/0)  |
| `characterFilterExclude` | Modo de Exclusión de Filtro de Personaje | Lista de cadenas |
| `characterFilterNames` | Nombres de Filtro de Personaje | Lista de cadenas |
| `characterFilterTags` | Etiquetas de Filtro de Personaje | Lista de cadenas |
| `matchCharacterDepthPrompt` | Coincidir Indicador de Profundidad de Personaje | Booleano (1/0) |
| `matchCharacterDescription` | Coincidir Descripción de Personaje | Booleano (1/0) |
| `matchCharacterPersonality` | Coincidir Personalidad de Personaje | Booleano (1/0) |
| `matchCreatorNotes` | Coincidir Notas del Creador | Booleano (1/0) |
| `matchPersonaDescription` | Coincidir Descripción de Persona | Booleano (1/0) |
| `matchScenario` | Coincidir Escenario | Booleano (1/0) |

**Valores de Lógica**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Valores de Posición**

- 0 = antes del indicador principal
- 1 = después del indicador principal
- 2 = superior de la Nota del Autor
- 3 = inferior de la Nota del Autor
- 4 = en el chat a profundidad
- 5 = superior de mensajes de ejemplo
- 6 = inferior de mensajes de ejemplo

**Valores de Rol** (Posición = 4 solo)
- 0 = Sistema
- 1 = Usuario
- 2 = Asistente

### Ejemplo 1: Leer contenido del lorebook del chat por clave

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### Ejemplo 2: Crear una entrada de lorebook del chat con clave y contenido

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### Ejemplo 3: Expandir una entrada de lorebook existente con nueva información del chat

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

## Manipulación de Texto

Hay una variedad de comandos de utilidad útiles para manipulación de texto para ser usados en varios escenarios de scripts.

1. `/trimtokens` — recorta la entrada al número especificado de tokens de texto desde el inicio o desde el final y produce el resultado en la tubería.
2. `/trimstart` — recorta la entrada al inicio de la primera oración completa y produce el resultado en la tubería.
3. `/trimend` — recorta la entrada al final de la última oración completa y produce el resultado en la tubería.
4. `/fuzzy` — realiza coincidencia difusa del texto de entrada a la lista de cadenas, produciendo la mejor coincidencia de cadena en la tubería.
5. `/regex name=scriptName [text]` — ejecuta un script regex de la extensión Regex para el texto especificado. El script debe estar habilitado.

### Argumentos para `/trimtokens`

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` establece la dirección para recortar, que puede ser `start` o `end`. Predeterminado: `end`.
2. `limit` establece la cantidad de tokens a dejar en la salida. También puede especificar un nombre de variable que contenga el número. **Argumento requerido.**
3. El argumento sin nombre es el texto de entrada a ser recortado.

### Argumentos para `/fuzzy`

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` es una matriz de cadenas serializada en JSON que contiene los candidatos. También puede especificar un nombre de variable que contenga la lista. **Argumento requerido.**
2. El argumento sin nombre es el texto de entrada a ser coincidido. La salida es uno de los candidatos que coincide más estrechamente con la entrada.

## Autocompletado

- El autocompletado está habilitado tanto en la entrada de chat como en el editor de Respuesta Rápida grande.
- El autocompletado funciona en cualquier lugar de tu entrada. Incluso con múltiples comandos canalizados y clausuras anidadas.
- El autocompletado soporta tres formas de buscar comandos que coincidan (*Configuración del Usuario* -> *Coincidencia de STscript*).

1. **Comienza con** La forma "antigua". Solo aparecerán comandos que comiencen exactamente con el valor escrito.
2. **Incluye**  Todos los comandos que *incluyan* el valor escrito aparecerán. Ejemplo: Al escribir `/delete`, los comandos `/qr-delete` y `/qr-set-delete` aparecerán en la lista de autocompletado (/qr-**delete**, /qr-set-**delete**).
3. **Difuso**  Todos los comandos que se pueden hacer coincidir de forma difusa con el valor escrito aparecerán. Ejemplo: Al escribir `/seas`, el comando `/sendas` aparecerá en la lista de autocompletado (/**se**nd**as**).

- Los argumentos de comandos también son soportados por el autocompletado. La lista aparecerá automáticamente para argumentos requeridos. Para argumentos opcionales, presiona *Ctrl*+*Espacio* para abrir la lista de opciones disponibles.
- Al escribir `/:` para ejecutar una clausura o QR, el autocompletado mostrará una lista de variables con ámbito y QRs.
- El autocompletado tiene soporte limitado para macros (en comandos de barra diagonal). Escribe `{{` para obtener una lista de macros disponibles.
- Usa las teclas de *flecha arriba* y *flecha abajo* para seleccionar una opción de la lista de opciones de autocompletado.
- Presiona *Entrar* o *Tabulación* o *haz clic* en una opción para colocar la opción en el cursor.
- Presiona *Escape* para cerrar la lista de autocompletado.
- Presiona *Ctrl*+*Espacio* para abrir la lista de autocompletado o alternar los detalles de la opción seleccionada.

## Banderas del Parser

```stscript
/parser-flag
```

El parser acepta banderas para modificar su comportamiento. Estas banderas se pueden activar y desactivar en cualquier punto de un script y toda la entrada siguiente será evaluada en consecuencia.
Puedes establecer tus banderas predeterminadas en la configuración del usuario.

### Escapamiento Estricto

```stscript
/parser-flag STRICT_ESCAPING on |
```

Los cambios con `STRICT_ESCAPING` habilitado son los siguientes.

#### Tuberías

Las tuberías no necesitan escaparse en valores entrecomillados.

```stscript
/echo title="a|b" c\|d
```

#### Barras Invertidas

Una barra invertida delante de un símbolo puede escaparse para proporcionar la barra invertida literal seguida del símbolo funcional.

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Reemplazar Macros de Variable

```stscript
/parser-flag REPLACE_GETVAR on |
```

Esta bandera ayuda a evitar doble-sustituciones cuando los valores de variable contienen texto que podría interpretarse como macros. Las macros `{{var::}}` se sustituyen en último lugar y no ocurren más sustituciones en el texto resultante / valor de variable.

Reemplaza todas las macros `{{getvar::}}` y `{{getglobalvar::}}` con `{{var::}}`.
Detrás de escenas, el parser insertará una serie de ejecutores de comando antes del comando con las macros reemplazadas:

- llamar a `/let` para guardar el actual `{{pipe}}` a una variable con ámbito
- llamar a `/getvar` o `/getglobalvar` para obtener la variable usada en la macro
- llamar a `/let` para guardar la variable recuperada a una variable con ámbito
- llamar a `/return` con el valor `{{pipe}}` guardado para restaurar el valor correcto de la tubería para el siguiente comando

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

## Respuestas Rápidas: biblioteca de scripts y auto-ejecución

Quick Replies es una extensión SillyTavern integrada que proporciona una forma fácil de almacenar y ejecutar tus scripts.

### Configurando Respuestas Rápidas

Para comenzar, abre el panel de extensiones (icono de bloques apilados) y expande el menú Respuestas Rápidas.

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Las Respuestas Rápidas están deshabilitadas por defecto, necesitas habilitarlas primero.** Luego verás una barra apareciendo encima de tu barra de entrada de chat.

Puedes establecer la etiqueta de texto del botón mostrado (recomendamos usar emojis para brevedad) y el script que será ejecutado cuando hagas clic en el botón.

El número de botones se controla por la configuración **Número de espacios** (máx = 100), ajústalo según tus necesidades y haz clic en "Aplicar" cuando hayas terminado.

**Inyectar entrada del usuario automáticamente** se recomienda deshabilitarla cuando usas STscript, de lo contrario puede interferir con tus entradas, usa la macro `{{input}}` para obtener el valor actual de la barra de entrada en scripts en su lugar.

**Presets de Respuesta Rápida** permiten tener múltiples conjuntos predefinidos de Respuestas Rápidas e intercambiar entre ellos manualmente o usando el comando `/qrset (nombre del conjunto)`.
¡No olvides hacer clic en "Actualizar" antes de cambiar a un conjunto diferente para escribir tus cambios en el preset actualmente usado!

### Ejecución Manual

Ahora puedes agregar tu primer script a la biblioteca. Elige cualquier espacio libre (o crea uno), escribe "Click me" en la caja izquierda para establecer la etiqueta, luego pega esto en la caja derecha:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

Luego haz clic 5 veces en el botón que apareció encima de la barra de chat.
Cada clic incrementa la variable `clicks` por uno y muestra un mensaje diferente cuando el valor es igual a 5 y reinicia la variable.

### Ejecución Automática

Abre el menú modal haciendo clic en el botón `⋮` para el comando creado.

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

En este menú puedes hacer lo siguiente:

- Editar el script en un editor conveniente de pantalla completa
- Ocultar el botón de la barra de chat, haciéndolo accesible solo para auto-ejecución.
- Habilitar la ejecución automática en una o más de las siguientes condiciones:
  * Inicio de la aplicación
  * Envío de un mensaje del usuario al chat
  * Recepción de un mensaje de IA en el chat
  * Abriendo un personaje o chat grupal
  * Desencadenando una respuesta de un miembro del grupo
  * Activando una entrada de World Info usando el mismo ID de Automatización
- Proporcionar una información de herramienta personalizada para la respuesta rápida (texto mostrado al pasar el ratón sobre la respuesta rápida en tu interfaz de usuario)
- Ejecutar el script con fines de prueba

Los comandos se ejecutan automáticamente solo si la extensión Respuestas Rápidas está habilitada.

Por ejemplo, puedes mostrar un mensaje después de enviar cinco mensajes del usuario agregando el siguiente script y configurándolo para auto-ejecutarse en el mensaje del usuario.

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### Depurador

Un depurador básico existe dentro del editor expandido de Respuesta Rápida. Establece puntos de interrupción con `/breakpoint |` en cualquier lugar en tu script. Cuando ejecutas el script desde el editor de QR, la ejecución se interrumpirá en ese punto, permitiéndote examinar las variables actualmente disponibles, tubería, argumentos de comando, y más, e ir a través del resto del código uno por uno.

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

### Llamar Procedimientos

Un comando `/run` puede llamar a scripts definidos en Respuestas Rápidas por su etiqueta, básicamente proporcionando la capacidad de definir procedimientos y devolver resultados de ellos. Esto permite tener bloques de script reutilizables que otros scripts podrían referenciar. El último resultado de la tubería del procedimiento se pasa al comando siguiente después de él.

```stscript
/run ScriptLabel
```

Vamos a crear dos Respuestas Rápidas:

***
**Etiqueta:**

`GetRandom`

**Comando:**

```stscript
/pass {{roll:d100}}
```
***
**Etiqueta:**

`GetMessage`

**Comando:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

Haciendo clic en el botón `GetMessage` llamará al procedimiento `GetRandom` que resolverá la macro `{{roll}}` y pasará el número al llamador, mostrándolo al usuario.

- Los procedimientos no aceptan argumentos nombrados o sin nombre, pero pueden referenciar las mismas variables que el llamador.
- Evita la recursión cuando llames procedimientos ya que puede producir el error "call stack exceeded" si no se maneja con cuidado.

#### Llamar procedimientos desde un preset de Respuesta Rápida diferente

Puedes llamar a un procedimiento desde un preset de respuesta rápida diferente usando la sintaxis `a.b`, donde a = nombre del preset de QR y b = nombre de la etiqueta de QR

```stscript
/run QRpreset1.QRlabel1
```

Por defecto, el sistema primero buscará una etiqueta de respuesta rápida `a.b`, así que si una de tus etiquetas es literalmente "QRpreset1.QRlabel1" intentará ejecutar esa. Si no se encuentra tal etiqueta, buscará un nombre de preset de QR "QRpreset1" con un QR etiquetado "QRlabel1".

### Comandos de Gestión de Respuestas Rápidas

#### Crear Respuesta Rápida

* `/qr-create (arguments, [message])` – crea una nueva Respuesta Rápida, ejemplo: `/qr-create set=MyPreset label=MyButton /echo 123`

Argumentos:
- `label`    - cadena - texto en el botón, p. ej., `label=MyButton`
- `set`      - cadena - nombre del conjunto de QR, p. ej., `set=PresetName1`
- `hidden`   - booleano   - si el botón debe estar oculto, p. ej., `hidden=true`
- `startup`  - booleano   - auto ejecutar en inicio de aplicación, p. ej., `startup=true`
- `user`     - booleano   - auto ejecutar en mensaje del usuario, p. ej., `user=true`
- `bot`      - booleano   - auto ejecutar en mensaje de IA, p. ej., `bot=true`
- `load`     - booleano   - auto ejecutar en carga de chat, p. ej., `load=true`
- `title`    - booleano   - título / información de herramienta a ser mostrado en botón, p. ej., `title="My Fancy Button"`

#### Eliminar Respuesta Rápida

* `/qr-delete (set=string [label])` – elimina Respuesta Rápida

#### Actualizar Respuesta Rápida

* `/qr-update (arguments, [message])` – actualiza Respuesta Rápida, ejemplo: `/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

Argumentos:
- `newlabel` - cadena - nuevo texto para el botón, p. ej. `newlabel=MyRenamedButton`
- `label`    - cadena - texto en el botón, p. ej., `label=MyButton`
- `set`      - cadena - nombre del conjunto de QR, p. ej., `set=PresetName1`
- `hidden`   - booleano   - si el botón debe estar oculto, p. ej., `hidden=true`
- `startup`  - booleano   - auto ejecutar en inicio de aplicación, p. ej., `startup=true`
- `user`     - booleano   - auto ejecutar en mensaje del usuario, p. ej., `user=true`
- `bot`      - booleano   - auto ejecutar en mensaje de IA, p. ej., `bot=true`
- `load`     - booleano   - auto ejecutar en carga de chat, p. ej., `load=true`
- `title`    - booleano   - título / información de herramienta a ser mostrado en botón, p. ej., `title="My Fancy Button"`

####

* `qr-get` - recupera todas las propiedades de una Respuesta Rápida, ejemplo: `/qr-get set=myQrSet id=42`

#### Crear o Actualizar Preset de QR

* `/qr-presetupdate (arguments [label])` o `/qr-presetadd (arguments [label])`

Argumentos:
- `enabled` - booleano - habilitar o deshabilitar el preset
- `nosend`  - booleano - deshabilitación de envío / inserción en entrada del usuario (inválido para comandos de barra diagonal)
- `before`  - booleano - colocar QR antes de la entrada del usuario
- `slots`   - entero  - número de espacios
- `inject`  - booleano - inyectar entrada del usuario automáticamente (si está deshabilitado usa `{{input}}`)

Crea un nuevo preset (anula los existentes), ejemplo: `/qr-presetadd slots=3 MyNewPreset`

#### Agregar Menú de Contexto de QR

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – agregar preset de menú de contexto a un QR, ejemplo: `/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### Eliminar todos los Menús de Contexto

* `/qr-contextclear (set=string [label])` – eliminar todos los presets de menú de contexto de un QR, ejemplo: `/qr-contextclear set=MyPreset MyButton`

#### Eliminar un Menú de Contexto

* `/qr-contextdel (set=string label=string [preset name])` – eliminar preset de menú de contexto de un QR, ejemplo: `/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Escapamiento de Valor de Respuesta Rápida

`|{}` puede escaparse con barra invertida en el mensaje / comando de QR.

Por ejemplo, usa `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}` para crear un QR que llame a `/getvar myvar | /echo {{pipe}}`.

## Comandos de Extensiones

Las extensiones de SillyTavern (integradas, descargables y de terceros) pueden agregar sus propios comandos de barra diagonal. A continuación se muestra solo un ejemplo de las capacidades en las extensiones oficiales. La lista puede estar incompleta, asegúrate de revisar `/help slash` para la lista más completa de comandos disponibles.

1. `/websearch (query)` — busca fragmentos de páginas web en línea para la consulta especificada y devuelve el resultado en la tubería. Proporcionada por la extensión de Búsqueda Web.
2. `/imagine (prompt)` — genera una imagen usando el indicador proporcionado. Proporcionada por la extensión de Generación de Imágenes.
3. `/emote (sprite)` — establece un sprite para el personaje activo haciendo coincidir su nombre de forma difusa. Proporcionada por la extensión de Expresiones de Personaje.
4. `/costume (subfolder)` — establece una anulación del conjunto de sprites para el personaje activo. Proporcionada por la extensión de Expresiones de Personaje.
5. `/music (name)` — fuerza un cambio de archivo de música de fondo reproducido por su nombre. Proporcionada por la extensión de Audio Dinámico.
6. `/ambient (name)` — fuerza un cambio de archivo de sonido ambiente reproducido por su nombre. Proporcionada por la extensión de Audio Dinámico.
7. `/roll (dice formula)` — agrega un mensaje oculto al chat con el resultado de una tirada de dados. Proporcionada por la extensión de Dados D&D.

## Interacción con la Interfaz de Usuario

Los scripts también pueden interactuar con la interfaz de usuario de SillyTavern: navegar a través de los chats o cambiar parámetros de estilo.

### Navegación de Personajes

1. `/random` — abre un chat con el personaje aleatorio.
2. `/go (name)` — abre un chat con el personaje del nombre especificado. Primero busca una coincidencia exacta de nombre, luego por un prefijo, luego por una subcadena.

### Estilo de Interfaz de Usuario

1. `/bubble` — establece el estilo de mensaje al estilo "chat de burbuja".
2. `/flat` — establece el estilo de mensaje al estilo "chat plano".
3. `/single` — establece el estilo de mensaje al estilo "documento único".
4. `/movingui (name)` — activa un preset MovingUI por nombre.
5. `/resetui` — restablece el estado de los paneles MovingUI a sus posiciones originales.
6. `/panels` — alterna la visibilidad de los paneles de la interfaz de usuario: barra superior, cajones izquierdo y derecho.
7. `/bg (name)` — encuentra y establece un fondo usando coincidencia de nombres difusa. Respeta el estado de bloqueo de fondo del chat.
8. `/lockbg` — bloquea la imagen de fondo para el chat actual.
9. `/unlockbg` — desbloquea la imagen de fondo para el chat actual.

## Más Ejemplos

### Generar Resumen de Chat (por @IkariDevGIT)

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

### Uso de Ventana Emergente de Botones

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### Obtener el Número N de Fibonacci (usando la Fórmula de Binet)

!!!tip
**Consejo**: Establece el valor de `fib_no` al número deseado
!!!

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### Factorial Recursivo (usando clausuras)

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
