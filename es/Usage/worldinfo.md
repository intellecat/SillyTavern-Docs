---
order: 130
icon: globe
route: /usage/core-concepts/worldinfo/
templating: false
---

# World Info

**World Info (también conocida como Lorebooks o Memory Books) es una herramienta poderosa disponible en ST para insertar dinámicamente avisos en tu chat para ayudar a guiar las respuestas de la IA.**

Comúnmente, World Info (abreviado WI) se utiliza para mejorar la comprensión de la IA sobre los detalles de tu mundo ficticio; sin embargo, podrías usar una entrada de World Info para insertar CUALQUIER COSA que desees incluir en el aviso.

Funciona como un diccionario dinámico que solo inserta información relevante de las entradas de World Info cuando las palabras clave asociadas con las entradas están presentes en el texto del mensaje.

El motor de SillyTavern activa e integra sin problemas el lore apropiado en el aviso, proporcionando información de contexto a la IA.

*Es importante tener en cuenta que aunque World Info ayuda a guiar a la IA hacia el contenido deseado, no garantiza su aparición en los mensajes de salida generados. ¡Eso depende de qué tan bien tu modelo pueda utilizar la información adicional!*

## Consejos Pro

* El motor de World Info es una herramienta muy poderosa de gestión de avisos. No te obsesiones solo con agregar lore de personajes; siéntete libre de experimentar.
* Las palabras clave de activación, títulos y otra información que no está en el campo **Content** no se insertan en el contexto, por lo que cada entrada de World Info debe tener una descripción completa e independiente.
* Para crear un lore mundial rico y detallado, las entradas pueden estar interconectadas y referenciarse entre sí utilizando activación recursiva. Consulta más en [Recursión](#recursive-scanning) a continuación.
* SillyTavern ofrece presupuesto de contexto flexible para la información de contexto insertada. Para conservar tokens de aviso, es aconsejable mantener los contenidos de entrada concisos.

## Lectura adicional

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): Guía exhaustiva y detallada sobre World Info y Lorebooks. Por kingbri, Alicat, Trappu.

## Character Lore

Opcionalmente, los archivos de World Info pueden asignarse a un personaje para servir como fuentes de lore dedicadas en todos los chats con ese personaje (incluyendo chats grupales).

Un World Info primario puede vincularse al personaje. Para hacerlo, navega al panel de Character Management y haz clic en el botón del globo, luego selecciona World Info de una lista desplegable y haz clic en "Ok". Al exportar el personaje, este archivo también se incrustará en los datos de la tarjeta del personaje.

Para desvinculación, cambio o asignación de archivos World Info adicionales como lore de personaje, shift-click en el botón del globo o haz clic en "More..." luego "Link World Info". Ten en cuenta que solo el archivo primario de World Info se exporta con el personaje.

### Character Lore Insertion Strategy

Al generar una respuesta de IA, las entradas del World Info del personaje se combinan con las entradas del selector de World Info global utilizando una de las siguientes estrategias:

#### Sorted Evenly (default)

Todas las entradas se ordenarán según su Insertion Order como si fueran parte de un archivo grande, ignorando la fuente.

#### Character Lore First

Las entradas del World Info del personaje se incluirían primero por su Insertion Order, luego las entradas del World Info Global.

#### Global Lore First

Las entradas del World Info Global se incluirían primero por su Insertion Order, luego las entradas del World Info del personaje.

### World Info Entry

#### Key

Una lista de palabras clave que activan la activación de una entrada de World Info. Las claves no distinguen entre mayúsculas y minúsculas por defecto (esto es [configurable](#case-sensitive-keys)).

##### Regular Expression (Regex) as Keys

Las claves permiten un enfoque más flexible para el emparejamiento al soportar regex. Esto hace posible emparejar contenido más dinámico con palabras u caracteres opcionales, espaciado, y todas las otras utilidades que regex proporciona.
Si una clave definida es una expresión regular válida (estilo regex de Javascript, con `/` como delimitadores. Se permiten todos los indicadores), se tratará como tal al verificar si una entrada debe ser activada. Se pueden ingresar múltiples expresiones regulares como claves separadas y funcionarán juntas. Dentro de una expresión regular, las comas son posibles. Las claves de texto sin formato no admiten comas, ya que se tratan como separadores de clave.

Un ejemplo de caso de uso para emparejamiento regex avanzado:
Una entrada/instrucción que debe insertarse cuando el personaje está realizando una acción relacionada con el clima

```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

Para más información sobre la sintaxis de Regex y posibilidades: [Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

###### Advanced Regex Per-Message Matching

ST prefija cada mensaje de chat en el búfer de escaneo de WI con `character name:` y después de v1.12.6, los concatena anteponiendo usando el carácter de valor 1 (`\x01`).
Esto significa que puedes emparejar entrada o salida específica de un cierto personaje usando una expresión regular vinculada a ese carácter de separación.

Por ejemplo, para emparejar solo al usuario diciendo "hello", podrías usar la siguiente expresión regular:

```js
/\x01{{user}}:[^\x01]*?hello/
```

##### Key Input

Hay dos modos para ingresar palabras clave, cada uno con una interfaz de usuario ligeramente diferente. En modo ⌨️ *plaintext mode* (por defecto), las claves se pueden ingresar como una lista separada por comas en un único campo de texto. Las expresiones regulares también se pueden incluir, pero no tienen ningún resaltado especial. En modo ✨ *fancy mode*, las claves aparecen como elementos separados y las expresiones regulares se resaltarán como tales. El control admite edición y eliminación de claves. El modo se puede cambiar a través del botón en línea dentro del control de entrada.

#### Optional Filter

Lista separada por comas de palabras clave adicionales en conjunción con la clave primaria.
Si no se proporcionan argumentos, este indicador se ignora.
Soporta lógica para AND ANY, NOT ANY, o NOT ALL

1. AND ANY = Activa la entrada solo si la clave primaria y Cualquiera de las claves de filtro opcional están en el contexto escaneado.
2. AND ALL = Activa la entrada solo si la clave primaria y TODAS las claves de filtro opcional están presentes.
3. NOT ANY = Activa la entrada solo si la clave primaria y Ninguna de las claves de filtro opcional están en el contexto escaneado.
4. NOT ALL = Evita la activación de la entrada a pesar del disparo de clave primaria, si todos los filtros opcionales están en el contexto escaneado.

Estas claves también soportan [regex](#regular-expression-regex-as-keys).

#### Entry Content

El texto que se inserta en el aviso cuando se activa la entrada.

#### Insertion Order

Valor numérico. Define una prioridad de la entrada si múltiples fueron activadas al mismo tiempo. Las entradas con números de orden más grandes se insertarán más cerca del final del contexto ya que tendrán más impacto en la salida. Por ejemplo, una entrada con número de Order 100 aparecerá en el contexto antes de una entrada con número de Order 250.

#### Insertion Position

* **Before Char Defs:** La entrada de World Info se inserta antes de la descripción y escenario del personaje. Tiene un impacto moderado en la conversación.
* **After Char Defs:** La entrada de World Info se inserta después de la descripción y escenario del personaje. Tiene un mayor impacto en la conversación.
* **Before Example Messages:** La entrada de World Info se analiza como un bloque de diálogo de ejemplo e se inserta antes de los ejemplos proporcionados por la tarjeta del personaje.
* **After Example Messages:** La entrada de World Info se analiza como un bloque de diálogo de ejemplo e se inserta después de los ejemplos proporcionados por la tarjeta del personaje.
* **Top of AN:** La entrada de World Info se inserta en la parte superior del contenido de Author's Note. Tiene un impacto variable dependiendo de la posición de Author's Note.
* **Bottom of AN:** La entrada de World Info se inserta en la parte inferior del contenido de Author's Note. Tiene un impacto variable dependiendo de la posición de Author's Note.
* **@ D:** La entrada de World Info se inserta a una profundidad específica en el chat (Depth 0 siendo el fondo del aviso).
  * ⚙️ - como un mensaje de función de sistema
  * 👤 - como un mensaje de función de usuario
  * 🤖 - como un mensaje de función de asistente
* **Outlet:** La entrada de World Info no se inyecta automáticamente. En su lugar, su contenido se almacena bajo una salida con nombre para que puedas decidir exactamente dónde aparece en el aviso llamándolo con la macro [`{{outlet::Name}}`](#outlet-name).

Las entradas de mensaje de ejemplo se formatearán según la configuración de construcción de avisos: Instruct Mode o Chat Completion prompt manager. También siguen las reglas de Ejemplo de Comportamiento de Mensajes: siendo gradualmente eliminadas en contexto completo, siempre mantenidas, o deshabilitadas completamente.

Si tu Author's Note está deshabilitado (Insertion Frequency = 0), las entradas de World Info en posiciones A/N serán ignoradas!

#### Outlet Name

Cuando se selecciona la posición de inserción **Outlet**, un campo adicional **Outlet Name** se vuelve disponible para la entrada. El nombre que proporcionas aquí agrupa las entradas juntas y define el símbolo que usarás para extraerlas al aviso manualmente.

Utiliza la macro `{{outlet::YourName}}` en [Prompt Manager](./Prompts/prompt-manager.md) o campos de avisos [Advanced Formatting](./Prompts/advancedformatting.md). Cuando se construye el aviso, la macro se reemplaza con el contenido combinado de cada entrada de World Info que comparte el mismo nombre de salida, separado por saltos de línea, ordenado por su valor de [Insertion Order](#insertion-order).

Si una entrada de salida no tiene un nombre, será omitida durante la generación, así que asegúrate de completar el campo. Los nombres de salida soportan autocompletar basado en los nombres que ya has usado para facilitar la reutilización de etiquetas consistentes.

##### Limitations and caveats

* Colocar macros de salida dentro de entradas de World Info no es soportado y no funcionará. Esto entra en conflicto con el orden de evaluación de World Info y puede llevar a bucles infinitos.
* Las salidas anidadas no son soportadas. No puedes colocar una macro de salida dentro del contenido de otra salida. Lo mismo que lo anterior, esto puede llevar a bucles infinitos.
* Los campos de tarjeta de personaje (Description, Personality, Scenario, etc.) no pueden expandir salidas. Estos campos se analizan temprano para que puedan actuar como [fuentes de emparejamiento adicionales](#additional-matching-sources) para desencadenamientos de World Info, lo que significa que las salidas no están disponibles cuando se procesa su texto. Utiliza otro campo que reconozca macros si necesitas colocar contenido de salida en el cuerpo del aviso en su lugar.
* El editor de Author's Note tampoco puede resolver salidas. Para colocar contenido de salida alrededor de Author's Note, asigna las entradas a posiciones de inserción **Top of AN** o **Bottom of AN** en su lugar de depender de la macro.
* Los nombres de salida distinguen entre mayúsculas y minúsculas. La macro `{{outlet::}}` debe usar exactamente la misma capitalización que el **Outlet Name** de la entrada, de lo contrario, no se devuelve contenido.
* Se ignoran los espacios iniciales o finales en un nombre de salida cuando llamas a la macro, así que los nombres guardados con espacios adicionales no coincidirán. Evita rellenar nombres para que puedan resolverse correctamente.
* Las macros de salida que no tienen contenido asignado a ellas se reemplazarán con una cadena vacía.

#### Entry Title / Memo

Un campo de texto para tu conveniencia para etiquetar tus entradas, que no es utilizado por la IA o ninguna de las lógicas de activación.

Si está vacío, puede rellenarse usando la primera clave de las entradas haciendo clic en el botón "Fill empty memos".

#### Strategy

1. 🔵 (Círculo Azul) = La entrada siempre estaría presente en el aviso.
2. 🟢 (Círculo Verde) = La entrada será activada solo en presencia de la palabra clave.
3. 🔗 (Enlace de Cadena) = La entrada se puede insertar mediante similitud vectorial.

Cada Entrada también tiene un conmutador que te permite habilitar o deshabilitar la entrada.

#### Probability (Trigger %)

Este valor actúa como un filtro adicional que añade una probabilidad de que la entrada NO se inserte cuando se activa por cualquier medio (constante, clave primaria, recursión).

1. Probability = 100 significa que la entrada se insertará en cada activación.
2. Probability = 50 significa que la entrada se insertará con una probabilidad de 1:1.
3. Probability = 0 significa que la entrada NO se insertará (deshabilitándola efectivamente).

Utiliza esto para crear eventos aleatorios en tus chats. Por ejemplo, cada mensaje podría tener una probabilidad del 1% de despertar a un Elder God si su nombre se menciona en el mensaje.

#### Inclusion Group

Los grupos de inclusión controlan cómo se seleccionan las entradas cuando múltiples entradas con la misma etiqueta de grupo se activan simultáneamente. Si múltiples entradas que tienen la misma etiqueta de grupo fueron activadas, solo una se insertará en el aviso.

Por defecto, la entrada elegida se selecciona aleatoriamente según su Group Weight (por defecto 100 puntos) — cuanto mayor sea el número, mayor será la probabilidad de selección. Esto permite una selección aleatoria entre las entradas activadas, añadiendo un elemento de sorpresa y variedad a las interacciones.

Una sola entrada puede ser parte de múltiples grupos de inclusión si se definen como una lista separada por comas. La misma lógica explicada arriba se aplicará. Si esa entrada se activa, *deshabilitará* todas las otras entradas que sean parte de cualquiera de sus grupos. Por lo tanto, si alguno de los grupos se activa, esta entrada no se activará.

#### Prioritize Inclusion

Para proporcionar más control sobre qué entradas se activan a través de [Inclusion Group](/Usage/worldinfo.md#inclusion-group), puedes usar la configuración 'Prioritize Inclusion'. Esta opción te permite especificar deterministicamente qué entrada elegir en lugar de seleccionar aleatoriamente las probabilidades de Group Weight.

Si múltiples entradas que tienen la misma etiqueta de grupo y esta configuración activada fueron activadas, se seleccionará la que tenga el valor 'Order' más alto. Esto es útil para crear secuencias de alternativa mediante grupos de inclusión. Por ejemplo, para priorizar entradas de poca profundidad con más énfasis, o para elegir una instrucción específica para establecer la escena sobre otra si ambas son válidas.

#### Use Group Scoring

Cuando esta configuración está habilitada globalmente o por entrada, el número de claves de entrada activadas determina la selección del ganador del grupo. Solo el subconjunto de un grupo con el número más alto de coincidencias de clave se dejará ser activado por Group Weight o Inclusion Priority - el resto será desactivado y eliminado del grupo.

Utiliza esto para dar más especificidad a entradas individuales en grupos grandes. Por ejemplo, pueden tener una clave común y una clave específica. Se insertará una entrada aleatoria cuando no se proporcione una clave específica, y viceversa.

La lógica del cálculo de puntuación para las claves primarias es 1 coincidencia = 1 punto.

Para claves secundarias, la interacción depende de la Selective Logic elegida:

1. AND ANY: 1 coincidencia secundaria = 1 punto.
2. AND ALL: 1 punto por cada clave secundaria si todas coinciden.
3. NOT ANY y NOT ALL: sin cambios.

Ejemplo:

* Entry 1. Keys: song, sing, Black Cat. Group: songs
* Entry 2. Keys: song, sing, Ghosts. Group: songs

La entrada `sing me a song` puede activar cualquiera de las entradas (ambas activadas 2 claves), pero `sing me a song about Ghosts` activará solo Entry 2 (activada 3 claves).

#### Automation ID

Permite integrar entradas de World Info con [STscripts](/For_Contributors/st-script.md) de la extensión Quick Replies. Si tanto el comando de respuesta rápida como la entrada de WI tienen el mismo Automation ID, el comando se ejecutará automáticamente cuando se active la entrada con una ID coincidente.

Las automatizaciones se ejecutan en el orden en que se activan, adhiriéndose a tu estrategia de ordenamiento designada, combinando [Character Lore Insertion Strategy](#character-lore-insertion-strategy) con el ordenamiento 'Priority'. Lo que lleva a que las entradas [Círculo Azul](#strategy) se procesen primero, seguidas por otras en su 'Order' especificado. Las entradas activadas recursivamente se procesarán después en el mismo orden.

El comando de script se ejecutará solo una vez si múltiples entradas con el mismo Automation ID se activan.

#### Character Filter

Una lista de nombres de personajes para los cuales esta entrada puede ser activada. Si esta lista no está vacía, la entrada solo será activada para los personajes cuyos nombres están en la lista. Cuando se selecciona una etiqueta, la entrada solo será activada para los personajes que tengan esa etiqueta específica.

El modo "Exclude" invierte el filtro, lo que significa que la entrada será activada para todos los personajes excepto los que se agreguen a la lista o que tengan la(s) etiqueta(s) seleccionada(s).

#### Triggers

Los tipos de generación para los cuales esta entrada de World Info puede ser activada. Si nada está seleccionado, la entrada se puede activar para todos los tipos de generación. Si uno o más están seleccionados, la entrada solo será activada para esos tipos de generación específicos:

* **Normal:** Solicitud de generación de mensaje normal.
* **Continue:** Cuando se presiona el botón Continue.
* **Impersonate:** Cuando se presiona el botón Impersonate.
* **Swipe:** Cuando la generación se activa al pasar.
* **Regenerate:** Cuando se presiona el botón Regenerate en chats en solitario.
* **Quiet:** Solicitudes de generación en segundo plano, generalmente activadas por [extensions](/extensions/index.md) o comandos [STscript](/For_Contributors/st-script.md).

!!!
El disparador "Regenerate" no está disponible en chats de grupo ya que usa lógica de regeneración diferente: todos los mensajes de la última respuesta se eliminan, y los mensajes se encolan usando el tipo de generación "Normal" según la [Group reply strategy](/Usage/Characters/groupchats.md#reply-order-strategies) elegida.
!!!

#### Additional matching sources

Por defecto, las Entradas de World Info se emparejan solo con contenido de la conversación actual. Estas opciones te permiten emparejar la entrada contra información de personaje diferente que no aparece en el chat, o incluso información de persona. Esto es útil cuando quieres tener un amplio rango de entradas que se usarán entre varios personajes pero no quieres tener que gestionar listas grandes de etiquetas, o no quieres tener que actualizar listas de filtro de personajes cada vez que crees uno nuevo. Esto también te permite emparejar entradas en función de la persona que tengas activa.

* **Character Description**: Se empareja contra la descripción del personaje.
* **Character Personality**: Se empareja contra el resumen de personalidad del personaje, encontrado bajo Advanced Definitions.
* **Scenario**: Se empareja contra el escenario especificado del personaje, encontrado bajo Advanced Definitions.
* **Persona Description**: Se empareja contra la descripción de la persona actualmente seleccionada.
* **Character's Note**: Se empareja contra la nota del personaje, que se puede encontrar bajo Advanced Definitions.
* **Creator's Notes**: Se empareja contra las notas del creador del personaje, que se pueden encontrar bajo Advanced Definitions. Las notas del creador generalmente no se incluyen en el aviso.

## Vector Storage Matching

La extensión Vector Storage proporciona una alternativa al emparejamiento de palabras clave mediante el uso de la similitud entre los mensajes de chat recientes y los contenidos de entradas de World Info.

Para habilitar y usar esto, se deben cumplir los siguientes requisitos previos:

1. La extensión Vector Storage está habilitada y está configurada para usar una de las fuentes de incrustación disponibles.
2. La casilla "Enable for World Info" está marcada en la configuración de la extensión Vector Storage.
3. Ya sea que las entradas de World Info que se permiten para emparejamiento sin clave tengan el estado "Vectorized" (🔗) o la opción "Enabled for all entries" esté marcada en la configuración de Vector Storage.

La elección del modelo de vectorización en la extensión y el significado teórico detrás del término "embeddings" no se cubrirá aquí. Consulta la guía [Data Bank](/Usage/Characters/data-bank.md#vector-storage) si necesitas más información sobre este tema.

El emparejamiento de Vector Storage se adhiere a este conjunto de reglas:

* El número máximo de entradas que se permiten ser emparejadas con Vector Storage se puede ajustar con la configuración "Max Entries". Este número solo establece el límite y no influye en el presupuesto de token establecido en la configuración de activación para World Info. Se aplican todas las reglas de presupuesto.
* Esta característica solo reemplaza la verificación de palabras clave. Todas las verificaciones adicionales deben cumplirse para que la entrada se inserte: trigger%, filtros de personaje, grupos de inclusión, etc.
* La configuración "Scan Depth" de la Configuración de Activación o anulaciones de entrada no se utiliza. Se utiliza el valor "Query messages" de Vector Storage en su lugar para obtener el texto para emparejar. Esto permite una configuración como "Scan Depth" establecida en 0, por lo que no se realizarán coincidencias de palabras clave regulares, pero las entradas aún pueden ser activadas por vectores.
* El estado "Vectorized" es solo un marcador adicional. La entrada seguiría comportándose como un registro normal, habilitado, no constante que será activado por palabras clave si se establecen. Elimina las palabras clave si deseas que se activen solo por vectores.

!!!info Nota
Como la calidad de recuperación depende enteramente de los resultados del modelo de incrustación, es imposible predecir exactamente qué entradas se insertarán. Si quieres resultados deterministas y predecibles, mantente con el emparejamiento de palabras clave.
!!!

## Timed Effects

Usualmente, la evaluación de World Info es sin estado, lo que significa que el resultado de la evaluación es el mismo, dependiendo solo del contexto de chat actual. Sin embargo, con la introducción de Timed Effects, puedes crear entradas que tienen un retraso de activación, permanecen activas después de ser activadas, o no pueden ser activadas después de la activación.

### Timed Effects Rules

1. Los marcos de tiempo para los efectos se miden en mensajes (no pares de mensajes/intercambios), siendo 0 significado de que no hay efecto.
2. Los efectos solo se aplican en el chat donde se activó la entrada. Las ramas heredan el estado del chat padre.
3. Los efectos cronometrados activos se eliminan si el chat no avanza, p. ej. si el último mensaje fue pasado o eliminado.
4. Hacer cualquier cambio a la entrada que está actualmente en efecto cronometrado causará que el efecto sea removido por la fuerza.
5. El disparo consecutivo de palabras clave no actualiza la duración del efecto si ya está activo.

### Types of Timed Effects

1. Sticky - la entrada permanece activa durante N mensajes después de ser activada. Las entradas pegajosas ignoran las verificaciones de probabilidad en escaneos posteriores hasta que expiren.
2. Cooldown - la entrada no se puede activar durante N mensajes después de ser activada. Se puede usar junto con sticky: la entrada entra en cooldown cuando termina la duración pegajosa.
3. Delay - la entrada no se puede activar a menos que haya al menos N mensajes en el chat en el momento de la evaluación.
    * Delay = 0 -> La entrada se puede activar en cualquier momento.
    * Delay = 1 -> La entrada no se puede activar si el chat está vacío (sin saludo).
    * Delay = 2 -> La entrada no se puede activar si hay cero o solo un mensaje en el chat, etc.

### Timed Effects Example

Configuración de entrada: sticky = 3, cooldown = 2, delay = 2.

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

Menú desplegable en la parte superior de la pantalla de World Info.

### Scan Depth

> Puede ser anulado en un nivel de entrada.

Define cuántos mensajes en el historial de chat deben ser escaneados para las claves de World Info.

* Si se establece en 0, entonces solo se evalúan las entradas recursadas y Author's Note.
* Si se establece en 1, entonces SillyTavern solo escanea el último mensaje.
* 2 = dos últimos mensajes, etc.

### Include Names

Define si los nombres de los participantes del chat deben incluirse en el búfer de texto escaneado como prefijos de mensaje. Esto permite activar entradas que usan nombres como palabras clave sin mencionar directamente los nombres en los mensajes.

Consulta un ejemplo del texto a ser escaneado a continuación, asumiendo que los participantes del chat se nombran Alice y Bob.

Habilitado (por defecto):

```txt
Alice: Hello! Good to see you.
Bob: How is the weather today?
```

Deshabilitado:

```txt
Hello! Good to see you.
How is the weather today?
```

### Context % / Budget

**Define cuántos tokens podrían ser utilizados por las entradas de World Info a la vez.**
Puedes definir un umbral relativo a la configuración máxima de contexto de tu API (Context %) o un umbral de token objetivo (Budget)

Si el presupuesto se agota, entonces no se activan más entradas incluso si las claves están presentes en el aviso.

Constant entries will be inserted first. Then entries with larger order numbers.

Las entradas insertadas al mencionar directamente sus claves tienen mayor prioridad que las que se mencionaron en los contenidos de otras entradas.

### Min Activations

**Esta configuración es mutuamente excluyente con Max Recursion Steps.**

Activaciones Mínimas: Si se establece en un valor distinto de cero, esto ignorará la limitación de "scan-depth", buscando todo el registro de chat hacia atrás desde el último mensaje para palabras clave hasta que se hayan activado tantas entradas como se especifique en activaciones mín. Esto seguirá siendo limitado por la configuración de Max Depth o tu límite de presupuesto general.

*Los escaneos adicionales activados por Min Activations no verificarán las entradas agregadas por recursión en pasos anteriores. Solo los mensajes de chat y los avisos de extensión pueden activar estas activaciones adicionales. Sin embargo, las entradas activadas por Min Activations pueden desencadenar otras entradas como es habitual.*

### Max Depth

Profundidad máxima para escanear cuando se utiliza la configuración Min Activations.

### Recursive scanning

El escaneo recursivo permite que las entradas activen otras entradas o sean activadas por otras, permitiendo interacciones complejas y dependencias entre diferentes entradas de World Info. Esta característica puede mejorar significativamente la naturaleza dinámica de tus escenarios creativos.
Si el escaneo recursivo está habilitado se puede controlar con la configuración global **Recursive Scan**.
Hay tres opciones disponibles para controlar la recursión para cada entrada:

* **Non-recursable**: Cuando se selecciona esta casilla, la entrada no será activada por otras entradas. Esto es útil para información estática que no debe cambiar o ser influenciada por otras entradas de información del mundo.

* **Prevent further recursion**: Seleccionar esta opción garantiza que una vez que esta entrada se active, no activará ninguna otra entrada. Esto es útil para evitar cadenas no intencionadas de activaciones.

* **Delay until recursion**: Esta entrada solo será activada durante verificaciones recursivas, lo que significa que no será activada en el primer paso pero puede ser activada por otras entradas que tienen recursión habilitada. Ahora, con el **Recursion Level** añadido para esos retrasos, las entradas se agrupan por niveles. Inicialmente, solo coincidirá el primer nivel (número más pequeño). Una vez que no se encuentren coincidencias, el siguiente nivel se vuelve elegible para el emparejamiento, repitiendo el proceso hasta que se verifiquen todos los niveles. Esto permite más control sobre cómo y cuándo se revelan capas más profundas de información durante la recursión, especialmente en combinación con criterios como combinación de NOT ANY o NOT ALL de coincidencias de clave.

**Las entradas pueden activar otras entradas mencionando sus palabras clave en el texto de contenido.**

Por ejemplo, si tu World Info contiene dos entradas:

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

**Ambas** serán extraídas al contexto si el texto del mensaje menciona **solo Bessie**.

### Max Recursion Steps

**Esta configuración es mutuamente excluyente con Min Activations.**

Cuando se establece en cero, el anidamiento de recursión solo está limitado por tu presupuesto de aviso. Cuando se establece en un valor distinto de cero, limita el número total de escaneos a la "nesting level" máxima deseada.

Valores de ejemplo:

* 1 efectivamente desactiva la recursión ya que la verificación se detiene después del primer paso.
* 2 solo puede activar entradas recursivas una vez.
* 3 puede activar recursión dos veces...

### Case-sensitive keys

> Puede ser anulado en un nivel de entrada.

**Para ser extraída al contexto, las claves de entrada deben coincidir con el caso tal como se definen en la entrada de World Info.**

Esto es útil cuando tus claves son palabras comunes o partes de palabras comunes.

Por ejemplo, cuando esta configuración está activa, las claves 'rose' y 'Rose' serán tratadas de manera diferente, dependiendo de las entradas.

### Match whole words

> Puede ser anulado en un nivel de entrada.

Las entradas con claves que contienen solo una palabra serán emparejadas solo si la palabra completa está presente en el texto de búsqueda. Habilitado por defecto.

Por ejemplo, si la configuración está habilitada y la clave de entrada es "king", entonces el texto como "long live the king" sería emparejado, pero "it's not to my liking" no sería.

**Importante:** esta configuración puede tener un efecto perjudicial cuando se usa con idiomas que no usan espacios en blanco para separar palabras (p. ej. Japonés o Chino). Si escribes entradas en estos idiomas, se aconseja mantenerlo apagado.

### Alert on overflow

Muestra una alerta si el World Info activado excede el presupuesto de token asignado.
