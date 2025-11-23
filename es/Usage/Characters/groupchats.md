---
order: 70
route: /usage/core-concepts/groupchats/
---

# Chats Grupales

## Estrategias de orden de respuesta

Decide cómo se redactan los personajes en chats grupales para sus respuestas.

### Manual

Puedes seleccionar el personaje que responda manualmente desde el menú o con el comando `/trigger`. El miembro del grupo seleccionado será el único que responda. Los mensajes del usuario no desencadenarán respuestas automáticas. Desencadenar una generación con una entrada de usuario vacía desencadenará que un miembro del grupo desmutado al azar responda.

### Natural Order

Intenta simular el flujo de una conversación humana real. El algoritmo es el siguiente:

1. Las menciones de los nombres de los miembros del grupo se extraen del último mensaje en el chat.

    ¡Solo se reconocen palabras completas como menciones! Si el nombre de tu personaje es "Misaka Mikoto", responderán solo a "Misaka" o "Mikoto", pero nunca a "Misa", "Railgun", etc.

    A menos que la configuración "Allow Self Responses" esté habilitada, los personajes no responderán a menciones de su nombre en su propio mensaje.

2. Los personajes se activan por el factor "Talkativeness".

    Talkativeness define con qué frecuencia el personaje habla si no fue mencionado. Ajusta este valor en la pantalla "Advanced Definitions" en el editor de personajes. Los valores del deslizador están en una escala lineal de **0% / Shy** (el personaje nunca habla a menos que sea mencionado) a **100% / Chatty** (el personaje siempre responde). El valor predeterminado para nuevos personajes es 50% de probabilidad.

3. Se selecciona un personaje al azar.

    Si ningún personaje fue activado en los pasos anteriores, se selecciona un orador al azar, ignorando todas las demás condiciones.

### List Order

Los personajes se redactan según el orden en que se presentan en la lista de miembros del grupo. No se aplican otras reglas.

### Pooled Order

Activa un personaje al azar que no ha hablado desde el último mensaje del usuario. Si todos los personajes han hablado, selecciona uno al azar hasta el próximo mensaje del usuario.

## Modo de manejo de generación de grupo

Esta configuración decide cómo manejar la información del personaje de los miembros del chat grupal. Sin importar la opción elegida, el historial del chat grupal siempre se comparte entre todos los miembros.

### Swap character cards

Modo predeterminado. Cada vez que se genera el mensaje, solo se incluye la información de la tarjeta de personaje del orador activo en el contexto.

### Join character cards

La información de todos los miembros del grupo se combina en un solo aviso conjunto en su orden de lista. Esto puede ayudar en los casos en que alterar grandes fragmentos del contexto no sea deseable, por ejemplo, con el almacenamiento en caché de indicaciones de llama.cpp.

Este modo tiene dos submodos (debes elegir uno):

* Include muted - los personajes silenciados siempre se incluirán en el aviso conjunto.
* Exclude muted - los personajes silenciados no se incluirán si no son el orador actual.

Los siguientes campos se están combinando:

1. Description
2. Scenario, si no se anula para el chat
3. Personality
4. Message examples
5. Character notes / Depth prompts

¡Importante! Ten en cuenta que debido a cómo se estructura típicamente la tarjeta de personaje, el uso de este modo puede llevar a un comportamiento inesperado, incluyendo pero no limitado a: personajes confundidos sobre sí mismos, teniendo personalidades fusionadas, rasgos inciertos, etc.

### Join Prefix and Suffix

Cuando se selecciona 'Join character cards', todos los campos respectivos de los personajes se unen. Esto significa que en el aviso resultante todas las descripciones de personajes se unirán en una gran gota de texto. Si deseas que esos campos estén separados, puedes definir un prefijo y/o sufijo.

Estas opciones admiten macros normales y también reemplazarán {{char}} con el nombre del personaje relevante y <FIELDNAME> con el nombre de la parte (por ejemplo: description, personality, scenario, etc.)

## Otras opciones del menú de Chat Grupal

### Mute Character

El icono de burbuja de voz tachado junto al avatar del personaje en el menú de chat grupal puede deshabilitar o habilitar respuestas de un personaje en particular en el chat.

### Force Talk

El icono de burbuja de voz junto al avatar del personaje en el menú de chat grupal desencadenará una respuesta solo de un personaje en particular, omitiendo la estrategia de orden de respuesta. Funcionará incluso si el miembro del grupo está silenciado.

### Auto-mode

Mientras el modo automático está habilitado, el chat grupal seguirá el orden de respuesta y desencadenará la generación de mensajes sin interacción del usuario. El siguiente turno del modo automático se desencadena después de un retraso de 5 segundos cuando el personaje redactado envía su mensaje. Cuando el usuario comienza a escribir en el área de texto de envío de mensajes, el modo automático se deshabilitará, pero las generaciones ya en cola no se detienen automáticamente.

### Allow Self Responses

Permitirá respuestas consecutivas del personaje que envió el último mensaje de cada turno si resultan ser desencadenadas por ser auto-mencionadas cuando se selecciona Natural Order. No tiene efecto en List order.

### Group Chat Scenario Override

Todos los miembros del grupo usarán el texto de escenario ingresado en lugar de lo especificado en sus tarjetas de personaje. Los chats ramificados heredan la anulación del escenario de su padre y pueden cambiarse individualmente después.

### Peek Character Definitions

Al hacer clic en el icono de tarjeta de personaje junto al avatar en el menú de chat grupal, navegará rápidamente a la pantalla de definiciones de personaje habitual. Cualquier cambio realizado aquí se guardará en la tarjeta misma.

Para volver al chat grupal, haz clic en el enlace del título Group Name.

### Member Management

Cualquiera de tus personajes existentes puede agregarse, eliminarse, silenciarse o reordenarse dentro del chat grupal. Por defecto, un nuevo miembro se agrega a la parte superior de la lista de miembros del grupo y luego puede reordenarse utilizando los iconos de flecha.

### Group Chat pop-out

El pop-out del menú de chat grupal se puede activar haciendo clic en el icono junto al campo "Current Members". Esto crea un pop-out del menú de chat grupal. Al habilitar MovingUI desde la configuración de usuario, este menú se puede redimensionar y arrastrar a cualquier posición dentro de la interfaz y funciona como el menú de chat grupal habitual.
