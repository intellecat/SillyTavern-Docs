---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# Indicaciones de Prompt

Cuando envías un mensaje a tu IA, el texto que escribes se combina con otro texto para formar una única solicitud que se envía a la IA. Este texto combinado se llama "indicación de prompt" o a veces "solicitud" o "contexto".

La indicación de prompt puede incluir una variedad de diferentes tipos de texto, incluyendo:

* [Instrucciones principales](#main-prompt-system-prompt) a la IA sobre cómo generar una respuesta
* Definiciones de los [roles que la IA debe tomar](/Usage/Characters/characterdesign.md)
* Definiciones de [el rol que estás asumiendo](/Usage/personas.md)
* [Información sobre el "mundo"](/Usage/worldinfo.md) con el que la IA está interactuando
* Documentos relevantes o información del [Banco de datos](/Usage/Characters/data-bank.md)
* [Resúmenes](/extensions/Summarize.md) de la conversación anterior
* Resultados de [búsquedas web](/extensions/WebSearch.md) u otras [fuentes de datos externas](/For_Contributors/Function-Calling.md)
* Mensajes anteriores en la conversación
* **Tu mensaje a la IA**
* [Instrucciones finales](#post-history-instructions) para la IA sobre cómo generar una respuesta

¡Esto puede ser mucho para manejar! Para ayudarte a entender cómo estructurar y modificar la solicitud que se envía a la IA, SillyTavern identifica diferentes elementos que es posible que desees incluir en tu indicación de prompt. Luego puedes estructurar tu indicación de prompt para incluir las cosas que tienen sentido según la forma en que desees interactuar con la IA.

Muchos de estos elementos se explican en las secciones donde los cambiarás. Por ejemplo, para describir el rol que desearías que la IA tome, podrías usar el campo [Descripción](/Usage/Characters/characterdesign.md#personality-summary) en [Diseño de Personaje](/Usage/Characters/characterdesign.md).

## Ver la Indicación de Prompt

Leer la indicación de prompt final que se envía a la IA es muy útil para entender qué se le dijo a la IA y por qué generó la respuesta que generó. Puedes ver la indicación de prompt de varias formas:

* Usando el icono de Itemización de indicaciones de prompt en el mensaje de respuesta de la IA
* Usando la extensión [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)
* Revisando los registros en la ventana de terminal donde estés ejecutando SillyTavern
* Revisando la consola en las herramientas de desarrollo de tu navegador

## Cambiar Cómo se Construye la Indicación de Prompt

Presentar todas las partes de tu indicación de prompt a la IA de la manera correcta es crucial para obtener las mejores respuestas. Puedes controlar cómo se construye la indicación de prompt.

+++ Text Completion APIs

Usa el panel [Advanced Formatting](advancedformatting.md) para personalizar la construcción de indicaciones de prompt para las APIs de Text Completion.

+++ Chat Completion APIs

Usa el [Prompt Manager](prompt-manager.md) para personalizar la construcción de indicaciones de prompt para las APIs de Chat Completion.

+++

## Indicación de Prompt Principal (Indicación del Sistema)

La indicación de Prompt Principal (o Indicación del Sistema) define las instrucciones generales que el modelo debe seguir. Establece el tono y contexto para la conversación. Por ejemplo, le dice al modelo que actúe como un asistente de IA, un compañero de escritura o un personaje ficticio.

+++ Text Completion APIs

La [Indicación del Sistema](advancedformatting.md#system-prompt) es parte de la [Cadena de Historia](context-template.md#story-string) y generalmente es la primera parte de la indicación de prompt que el modelo recibe.

+++ Chat Completion APIs

La indicación de Prompt Principal es una de las indicaciones de prompt predeterminadas en [Prompt Manager](prompt-manager.md). Generalmente es el primer mensaje en el contexto que el modelo recibe, atribuido al rol del sistema.

+++

La indicación de Prompt Principal predeterminada es:

> Escribe la siguiente respuesta de \{\{char\}\} en un chat ficticio entre \{\{char\}\} y \{\{user\}\}.

Los marcadores de posición \{\{char\}\} y \{\{user\}\} se reemplazan con los nombres del personaje y la persona que has definido en la conversación.

Puedes usar cualquiera de las etiquetas de [macros](/Usage/Characters/macros.md) admitidas en la indicación de Prompt Principal para incluir información que podría variar entre conversaciones o cambiar a medida que avanza la conversación.

### Ajustar la Indicación de Prompt Principal

La indicación de prompt principal predeterminada ayuda al modelo a entender qué se espera que haga con la información del personaje y la persona que sigue, cómo interpretar la conversación anterior y qué tipo de respuesta generar. Es una indicación de propósito general flexible que funciona bien en muchas situaciones, porque establece que la IA está escribiendo como un personaje en una conversación con tu persona.

Sin embargo, puedes ajustar la indicación de prompt principal para que se adapte mejor a tus necesidades. Aquí hay algunas razones comunes para ajustar la indicación de prompt principal:

* **Proporcionar instrucciones adicionales**: por ejemplo, quieres que la IA explique su razonamiento, siga reglas específicas o evite ciertos temas
* **Aclarar el rol de la IA**: por ejemplo, quieres que la IA actúe como narrador, narrador de historias o guía
* **Cambiar el contexto de la conversación**: por ejemplo, quieres que la IA responda como si fuera un asistente de IA, un juego de aventura de texto o un compañero de escritura

!!! Prueba cosas y ve qué funciona mejor para ti
Todos los ejemplos en esta guía han funcionado bien para otros usuarios, pero la indicación de prompt que funciona para tus necesidades y el modelo que estés usando podría ser diferente. Experimenta con diferentes instrucciones y estilos de indicación de prompt para ver qué funciona mejor para ti. Si no estás seguro de qué intentar, siempre puedes pedir ayuda en el [Discord de SillyTavern](https://discord.gg/sillytavern).
!!!

Dar instrucciones adicionales a la IA en la indicación de Prompt Principal puede ayudarle a entender lo que quieres de la conversación.

> Escribe una única respuesta. Escribe al menos un párrafo, hasta cuatro.

> Markdown está habilitado. Úsalo para formatear tu respuesta. Encierra fragmentos de código en comillas invertidas triples.

> Escribe el diálogo de personajes entre comillas. Escribe los pensamientos de \{\{char\}\} entre paréntesis.

> Eres un modelo de generación de juego de rol de anime para usuarios de 13 a 17 años. Siempre generas respuestas divertidas y apropiadas para la edad.

> Responde con veracidad y escribe tu pensamiento paso a paso para asegurarte de que obtienes la respuesta correcta.

La IA seguirá más fácilmente las instrucciones sobre qué debería hacer que sobre qué no debería hacer. Por ejemplo, si quieres que la IA evite escribir de cierta manera, es mejor decirle cómo quieres que escriba en su lugar. Y aunque *"No decidas qué \{\{user\}\} dice o hace"* se incluye comúnmente en las indicaciones de prompt para evitar que la IA controle tu persona, algunos usuarios encuentran que *"Escribe las respuestas de \{\{char\}\} de una manera que respete la autonomía de \{\{user\}\}"* es más efectivo.

A menudo hay un mejor lugar que la indicación de Prompt Principal para incluir información sobre el usuario o los personajes, modificar el estilo de escritura y habla de un personaje o dar otras instrucciones específicas. La indicación de Prompt Principal se usa mejor para instrucciones generales sobre la conversación en su conjunto o sobre un tipo de conversación que deseas tener.

### Efecto del Historial de Mensajes

Cuando ajustas la indicación de prompt principal para mejorar las respuestas de la IA, ten en cuenta que la IA recoge mucho del historial de mensajes. El historial es su memoria de eventos anteriores, interacciones y relaciones de personajes, y su guía de estilo para la elección de palabras y el estilo de escritura.

Usa esto a tu favor proporcionando también [mensajes de ejemplo](/Usage/Characters/characterdesign.md#examples-of-dialogue) mostrando cómo quieres que la IA responda. ¡Mostrar lo que quieres es a menudo más fácil que intentar explicarlo!

Cuando tu conversación ya tiene historial, cambiar la indicación de prompt principal tiene un efecto limitado en las respuestas de la IA. En términos de eventos y relaciones, la IA asume que la indicación de prompt principal ocurrió en el pasado lejano y el historial de mensajes la actualiza. En términos de estilo de escritura y elección de palabras, la IA asume que todos los mensajes en el historial fueron generados de acuerdo a las reglas en la indicación de prompt principal *actual* y que debería continuar generando mensajes de la misma manera. Algunas sugerencias para lidiar con esto son:

* insertar instrucciones actuales cerca o después del final del historial de mensajes, por ejemplo usando una [Nota del Autor](/Usage/Characters/Author's-Note.md)
* probar tus cambios en la indicación de prompt principal iniciando una nueva conversación
* editar el historial de mensajes para eliminar o corregir ejemplos de comportamiento no deseado
* usar las [Instrucciones de Poste-Historial](#post-history-instructions) para proporcionar instrucciones finales a la IA

!!! ¡Hazlo bien la primera vez!
Nunca dejes que la IA "se salga con la suya" con algo que no quieres que haga. Si no te gusta la respuesta de la IA, no continúes la conversación como si fuera correcta. En su lugar, modifica las indicaciones de prompt, regenera el mensaje y continúa desde allí. Esto ayudará a la IA a aprender lo que quieres.
!!!

### Eliminar el Contexto de "Chat Ficticio"

Hay situaciones donde "chat ficticio" podría no ser el contexto correcto para tu conversación.

Puedes eliminar el contexto "ficticio" de la indicación de Prompt Principal:

> Escribe la siguiente respuesta de \{\{char\}\} en una conversación con \{\{user\}\}.

Es posible que no quieras que la IA se piense a sí misma como interpretación de roles en absoluto. En lugar de eliminar la idea de un personaje, puedes eliminar la idea de una IA:

> Eres \{\{char\}\}, un asistente útil. Proporcionas información útil y ayudas a \{\{user\}\} con sus preguntas.

### IA como Narrador o Narrador de Historias

¿Qué pasa si quieres que la IA actúe como narrador, describiendo eventos desde una perspectiva omnisciente, inventando sus propios personajes y escenarios?

Un enfoque es crear un personaje nombrado para que la IA lo use como narrador. Este personaje podría llamarse "Narrador" o "IA", sugiriendo que la IA es un narrador de propósito general, o podría ser nombrado según un escenario o entorno específico, dándole a la IA la tarea de narrar una historia en ese entorno. Los detalles del entorno se pueden definir en el [Personaje](/Usage/Characters/characterdesign.md) o en [Información del Mundo](/Usage/worldinfo.md).

Necesitarás ajustar la indicación de prompt principal predeterminada para reflejar el rol de la IA. Para un narrador de propósito general, podrías usar:

> Eres \{\{char\}\}, un narrador hábil y versátil. Narra la historia.

o para un entorno específico:

> Eres el narrador de un escenario de fantasía. Juega como los personajes que visitan \{\{char\}\}.

Ayuda a aclarar el rol del usuario en la conversación. ¿Son tus mensajes parte de la historia o son instrucciones para el narrador sobre lo que tu personaje hace o dice? Un ejemplo que incluye al usuario en la historia:

> La historia debe progresar respondiendo a las acciones y el diálogo de \{\{user\}\}. Narra la historia en tercera persona.

Un ejemplo que mantiene al usuario fuera de la historia:

> Ingresa al Modo Aventura. Narra la historia basada en el diálogo y las acciones de \{\{user\}\} después de ">". Describe los alrededores en detalle vívido. Sé detallado, creativo, verboso y proactivo. Avanza la historia introduciendo elementos de fantasía y personajes interesantes.

Definir el rol del usuario no solo ayuda a la IA a entender cómo responder a tus mensajes, sino también en qué medida se le permite controlar tu persona. Esto evita situaciones donde la IA toma decisiones para tu persona que preferirías tomar tú mismo.

## Instrucciones de Poste-Historial

Las instrucciones de Poste-Historial (PHI) son instrucciones adicionales enviadas a la IA después de la indicación de prompt principal y el mensaje del usuario. Se pueden usar para proporcionar contexto adicional o instrucciones a la IA basadas en el historial de mensajes.

Dado que las instrucciones de Poste-Historial se envían después del mensaje del usuario, son las instrucciones finales que recibe la IA antes de generar una respuesta. La IA generalmente les da una prioridad más alta que la indicación de prompt principal y pueden anular las instrucciones de la indicación de prompt principal.

Para usar instrucciones de Poste-Historial por carácter, agrégalas a las [Instrucciones de Poste-Historial](/Usage/Characters/characterdesign.md) del personaje y habilita [Preferir Instrucciones del Carácter](/Usage/User_Settings/index.md). Para preservar la PHI definida globalmente mientras usas instrucciones específicas del personaje, puedes usar la macro `{{original}}` en el campo de instrucciones de Poste-Historial del personaje.

+++ Text Completion APIs

Las instrucciones de Poste-Historial se definen en el panel [Advanced Formatting](/Usage/Prompts/advancedformatting.md) bajo la categoría de Indicación del Sistema. Las instrucciones de Poste-Historial se agregan como una inyección de rol de usuario invisible que precede la última línea de la indicación de prompt (generalmente conteniendo un "encabezado" de mensaje de respuesta). Tenga en cuenta que el interruptor "Habilitar Indicación del Sistema" debe estar habilitado para que se apliquen las instrucciones de Poste-Historial (incluso si la indicación del sistema en sí está vacía).

+++ Chat Completion APIs

Las instrucciones de Poste-Historial es una de las indicaciones de prompt predeterminadas en [Prompt Manager](prompt-manager.md). Generalmente es el último mensaje en el contexto que recibe el modelo, atribuido al rol del sistema. Si tu API de Chat Completion no admite el rol del sistema, generalmente se atribuirá al rol del usuario en su lugar.

+++

## Agregar a la Indicación de Prompt (Información del Mundo)

Puedes insertar información adicional en cualquier lugar de la indicación de prompt usando la característica [Información del Mundo](/Usage/worldinfo.md). Al establecer las condiciones para cuándo se debe insertar la información, puedes guiar a la IA para incluir detalles específicos, cambiar cómo responde o agregar nuevos elementos a la conversación.

Algunos usos comunes de la Información del Mundo incluyen:

* un "libro de lore" o "enciclopedia" con información sobre el mundo o entorno
* una forma de manejar diferentes indicaciones del sistema para varios personajes y situaciones
* un lugar para almacenar recuerdos que la IA debería "recordar" en la conversación
* un sistema más modular para crear, editar y compartir detalles de personajes
* una fuente de eventos aleatorios y sorpresas para que la IA reaccione o ¡para hacerte reaccionar!
