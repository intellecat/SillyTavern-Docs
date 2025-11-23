---
order: 50
route: /usage/core-concepts/authors-note/
---

# Author's Note

## ¿Qué es?

Author's Note es una herramienta poderosa para personalizar respuestas de IA que inserta una sección de texto en el prompt en cualquier posición y con la frecuencia que desees.

## Uso

Author's Note se puede encontrar en el menú Options en el lado izquierdo de la barra de entrada de chat.

| Menú de Opciones                      | Panel de Author's Note                 |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## Configurando Author's Notes

### Author's Note específico del chat

El cuadro en la parte superior del panel de Author's Note contiene el Author's Note para tu chat actual.

**El contenido de este cuadro no se transfiere automáticamente a ningún chat nuevo.**

### Opciones de ubicación

#### Después de Scenario

Esto coloca el Author's Note hacia la parte superior del contexto después de la sección 'Scenario' de Character Definition. Si no se especifica ningún scenario, se colocará después de la última parte de Character Definition, y antes de los Example messages.

#### En el chat

Esto coloca el Author's Note en el historial de chat a la profundidad especificada.

Depth 0 = colocado al final del historial de chat.

Depth 4 = colocado antes de los 3 mensajes de historial de chat más recientes, convirtiéndose en la 4ª entidad en el historial de chat.

_Cuanto más cerca esté el Author's Note de la parte inferior del prompt, más impacto tiene en la próxima respuesta de IA._

### Frecuencia de inserción

Esta es la frecuencia con la que deseas que se incluya el Author's Note en el chat.

Frequency 0 = Author's Note nunca será insertado.

Frequency 1 = Author's Note será insertado con cada entrada de usuario en el prompt.

Frequency 4 = Author's Note será insertado en cada cuarto entrada de usuario en el prompt.

### Author's Note predeterminado

El cuadro en la parte inferior del panel contiene el Author's Note predeterminado que se aplicará a cada nuevo chat.

## Casos de uso comunes

### Recordar al IA el formato de respuesta

El Author's Note se puede utilizar para especificar cómo el IA debe escribir sus respuestas.

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### Reforzando instrucciones

- [Remember the instructions you were given at the beginning of this chat.]

### Como World Info temporal, Character Bias, o Instruct para modelos no-Instruct

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
