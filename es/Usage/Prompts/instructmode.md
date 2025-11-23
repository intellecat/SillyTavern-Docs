---
order: 80
route: /usage/core-concepts/instructmode/
---

# Modo de Instrucción

Modo de Instrucción te permite ajustar el indicador para modelos que siguen instrucciones entrenados en varios formatos de indicador, como Alpaca, ChatML, Llama2, etc.

!!! Se aplica a: Text Completion APIs
Para configuraciones equivalentes en Chat Completion APIs, usa [Prompt Manager](prompt-manager.md).
!!!

## Compatibilidad con API

### Text Completion API

Totalmente compatible. Esto incluye:

* Todas las fuentes bajo Text Completion
* KoboldAI Classic
* AI Horde

#### Elegir un formato

Una plantilla instruct elegida debe coincidir con las expectativas de un modelo actual que se ejecuta en un backend.

Esto generalmente se refleja en una tarjeta de modelo en HuggingFace, y algunas incluso proporcionan archivos JSON compatibles con SillyTavern.

Ejemplo: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude, etc)

Esto no es compatible **(y no es necesario)** para Chat Completion APIs. Utilizan un constructor de indicadores completamente diferente.

### NovelAI

Si bien *técnicamente* compatible con NovelAI, ninguno de sus modelos fue entrenado para entender el formato instruct. Los modelos de NovelAI pueden usar un módulo instruct especial que se activa *automáticamente* cuando se encuentra una instrucción envuelta entre llaves en los mensajes de chat, por lo que usar Modo de Instrucción para todo el indicador resultará en **calidad degradada** de los resultados.

Aquí hay un ejemplo que activa automáticamente el módulo instruct para NovelAI:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Configuración de Modo de Instrucción

### System Prompt

!!!warning Cambio reciente
System Prompt es ahora una entidad separada. Consulta la página [Advanced Formatting](advancedformatting.md#system-prompt) para más detalles.
!!!

### Plantillas

Proporciona plantillas prefabricadas con secuencias para algunos modelos instruct bien conocidos.

*Cambiar una plantilla reinicia la configuración sin guardar al último estado guardado. ¡No olvides guardar tu plantilla si hiciste cambios que no quieres perder!*

### Expresión Regular de Activación

Si se define como una expresión regular válida, cuando se conecta a un modelo y su nombre coincide con esta expresión regular, seleccionará automáticamente esta plantilla.

Modo de Instrucción debe estar habilitado previamente. Solo se seleccionará la primera coincidencia de expresión regular entre plantillas (evaluadas en orden alfabético).

### Envolver Secuencias con Nueva Línea

Cada texto de secuencia se envolverá con caracteres de nueva línea cuando se inserte en el indicador. Requerido para Alpaca y sus derivados.

Deshabilita si deseas tener control total sobre los terminadores de línea.

### Reemplazar Macro en Secuencias

Si está habilitado, se reemplazarán las sustituciones de macro conocidas \{\{macro\}\} si se definen en secuencias de envoltura de mensaje.

Además, se puede usar una macro especial \{\{name\}\} en prefijos de mensaje para hacer referencia al nombre real adjunto a un mensaje (en lugar de un \{\{char\}\} o \{\{user\}\} actualmente activo), lo que puede ser útil al usar chats grupales o comando /sendas. Si el nombre no se puede determinar, "System" se usa como marcador de posición de respaldo.

### Incluir Nombres

Si está habilitado, antepone los nombres de caracteres y usuario a los registros de historial de chat después de la secuencia de prefijo.

Las siguientes opciones están disponibles:

* **Never**: No agregues prefijos de nombre antes del contenido del mensaje.
* **Groups and Past Personas**: Solo agrega prefijos de nombre a mensajes de caracteres de grupo y personas pasadas.
* **Always**: Siempre agrega prefijos de nombre antes del contenido del mensaje.

### Secuencias: Envoltura de Story String

!!!warning Cambio reciente
Se han eliminado los ajustes de envoltura de System Prompt y se han reemplazado con ajustes de Story String.
!!!

Define cómo se envolverá Story String cuando la Posición se establezca en "Default (top of context)"

#### Prefijo de Story String

Insertado antes de Story String.

#### Sufijo de Story String

Insertado después de Story String.

### Secuencias: Envoltura de Mensajes de Chat

Estos ajustes definen cómo se envolverán los mensajes pertenecientes a diferentes roles al construir un indicador.

Todas las secuencias de prefijo también se utilizarán automáticamente como cadenas de parada.

#### Prefijo de Mensaje de Usuario

Insertado antes de un mensaje de Usuario y como última línea de indicador al suplantar.

#### Sufijo de Mensaje de Usuario

Insertado después de un mensaje de Usuario.

#### Prefijo de Mensaje de Asistente

Insertado antes de un mensaje de Asistente y como última línea de indicador al generar una respuesta de IA.

#### Sufijo de Mensaje de Asistente

Insertado después de un mensaje de Asistente

#### Prefijo de Mensaje del Sistema

Insertado antes de un mensaje del Sistema (añadido por comandos de barra inclinada o extensiones).

#### Sufijo de Mensaje del Sistema

Insertado después de un mensaje del Sistema.

#### Sistema igual que Usuario

Si se marca como verdadero, los mensajes del Sistema usarán secuencias de rol de Usuario.

De lo contrario, los mensajes del Sistema usan sus propias secuencias (si no están vacías) o no harán ningún ajuste en absoluto (si están vacías).

### Secuencias Varias

Varias configuraciones avanzadas para un ajuste fino de la construcción del indicador

#### Prefijo del Primer Asistente

Insertado antes del primer mensaje del Asistente.

!!!info
¡Solo el primer mensaje del **historial de chat** cuenta, no el mensaje que realmente va al indicador primero!
!!!

#### Prefijo del Último Asistente

Insertado antes del último mensaje del Asistente o como última línea de indicador al generar una respuesta de IA.

!!!info
No se usa cuando se genera texto en segundo plano (por ejemplo, indicadores de Stable Diffusion o Resúmenes). Se usará el Prefijo de Instrucción del Sistema o el Prefijo de Asistente regular en su lugar.
!!!

#### Prefijo de Instrucción del Sistema

Insertado como última línea de indicador al generar texto neutral/sistema en segundo plano (por ejemplo, indicadores de Stable Diffusion o Resúmenes).

#### Mensaje de Relleno de Usuario

Se insertará al inicio del historial de chat si no comienza con un mensaje de Usuario.

**Caso de uso:** cuando un formato instruct *estrictamente requiere* que los indicadores sean primero de usuario y tengan mensajes con roles alternos solamente, ejemplos: Llama 2 Chat, Mistral Instruct.

#### Secuencia de Parada

Texto que denota el final de la respuesta. También enviado como una cadena de parada a la API del backend.

Si se genera una secuencia de parada, todo lo que esté después de ella se eliminará de la salida (incluida la secuencia misma).
