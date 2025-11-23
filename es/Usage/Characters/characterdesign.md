---
order: 100
route: /usage/core-concepts/characterdesign/
templating: false
---

# Diseño de Personajes

!!!tip
El Nombre del Personaje es el único campo requerido. Puede dejar el resto vacío y seguir usando el personaje en chats.
!!!

## Descripción del Personaje

Se utiliza para agregar la descripción del personaje y otra información relevante para la IA. Esta información siempre se incluye en el prompt, por lo que todos los hechos importantes deben incluirse aquí.

Por ejemplo, puede agregar información sobre el mundo en el que tiene lugar la acción, describir la apariencia, personalidad y trasfondo del personaje.

Puede ser de cualquier longitud (ya sea 200 o 2000 tokens) y formateada en cualquier estilo (texto libre, estilo de conversación seudocódigo, etc.).

### Métodos y formato

Los métodos de formateo de personajes son un tema complicado que va más allá del alcance de esta página de documentación.

Guías recomendadas que fueron probadas con o se basan en las características de SillyTavern:

* Guía Trappu's PLists + Ali:Chat: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* Guía AliCat's Ali:Chat: <https://rentry.co/alichat>
* Guía minimalista de kingbri: <https://rentry.co/kingbri-chara-guide>

## Tokens del Personaje

**TL;DR: Si está trabajando con un modelo de IA con un límite de tokens de contexto de 2048, una definición de personaje de 1000 tokens reduce la 'memoria' de la IA a la mitad.**

Para poner esto en perspectiva, una respuesta decente de una buena IA puede ser fácilmente alrededor de 200-300 tokens. En este caso, la IA solo podría 'recordar' alrededor de 3 intercambios del historial de chat.

### ¿Por qué el contador de tokens de mi personaje se volvió rojo?

Cuando vemos que su personaje tiene más de la mitad de la longitud de contexto definida por el modelo en términos de tokens en sus definiciones, lo resaltamos para usted porque esto puede reducir la capacidad de la IA para proporcionar una conversación agradable.

### ¿Qué sucede si mi Personaje tiene demasiados tokens?

No se preocupe, no romperá nada. En el peor de los casos, si los tokens permanentes del Personaje son demasiado grandes, simplemente significa que habrá menos espacio disponible en el contexto para otras cosas (vea a continuación).

El único efecto secundario negativo que esto puede tener es que la IA tendrá menos 'memoria', ya que tendrá menos historial de chat disponible para procesar.

Esto se debe a que cada modelo de IA tiene un límite en la cantidad de contexto que puede procesar a la vez.

## ¿'Contexto'?

Esta es la información que se envía a la IA cada vez que le pide que genere una respuesta. SillyTavern calcula automáticamente la mejor forma de asignar los tokens de contexto disponibles antes de enviar la información al modelo de IA.

Lea más sobre cómo se construye el contexto en la sección [Prompts](/Usage/Prompts/index.md).

### ¿Cuáles son los 'Tokens Permanentes' de un Personaje?

Estos siempre se enviarán a la IA con cada solicitud de generación:

* Nombre del Personaje
* Cuadro de Descripción del Personaje
* Cuadro de Personalidad del Personaje
* Cuadro de Escenario

### ¿Qué partes de las Definiciones de un Personaje NO son permanentes?

* El cuadro del primer mensaje: solo se envía una vez al inicio del chat.
* Cuadro de mensajes de ejemplo: solo se mantiene hasta que el historial de chat llena el contexto (opcionalmente, estos pueden forzarse a mantenerse en contexto)

### Límites Populares de Tokens de Contexto del Modelo de IA

* LLaMA 3 y sus finetunes - 8192
* OpenAI GPT-4 - hasta 128k
* Google Gemini - hasta 2M
* Claude de Anthropic - 200k (Claude 3)
* NovelAI - 8192 (Erato y Kayra, nivel Opus; Clio, todos los niveles), 6144 (Kayra, nivel Scroll), o 3072 (Kayra, nivel Tablet)

## Primer Mensaje

El Primer Mensaje es un elemento importante que define cómo y en qué estilo se comunicará el personaje. Es más probable que el modelo adopte el estilo y las restricciones de longitud del primer mensaje que de cualquier otra cosa, por lo que es importante escribirlo de una manera que desee que sean las respuestas (cortas y concisas, largas y detalladas, etc.).

Admite formato Markdown y HTML.

Por ejemplo:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Saludos Alternos

Los mensajes agregados aquí se muestran como 'swipes' adicionales para el primer mensaje del personaje al iniciar un nuevo chat. Si el personaje es parte de un chat grupal, el sistema selecciona aleatoriamente uno de estos saludos para iniciar la conversación.

## Personaje Favorito

Haga clic en el botón **<i class="fa-solid fa-star"></i> Agregar a Favoritos** para marcar el personaje como favorito para filtrarlos rápidamente en la barra lateral del menú seleccionando la opción de clasificación "Favoritos". Los personajes favoritos tienen un resaltado dorado en la lista. Esto también hará que el retrato del personaje aparezca en el área de hotswaps (si está habilitado en la Configuración del Usuario).

## Definiciones Avanzadas

!!!info
Los siguientes campos están ocultos por defecto. Para acceder y editarlos, debe hacer clic en el botón **<i class="fa-solid fa-book"></i> Definiciones Avanzadas** en la barra de menú de la página de definición del personaje.
!!!

### Anulaciones de Prompt

* **Prompt Principal**: Si la configuración de usuario "Prefer Char. Prompt" está habilitada, cualquier texto que coloque aquí anulará el [prompt principal/del sistema](/Usage/Prompts/index.md#main-prompt-system-prompt) para el personaje.
* **Instrucciones Posteriores al Historial**: Si la configuración de usuario "Prefer Char. Instructions" está habilitada, cualquier texto que coloque aquí se utilizará como las [instrucciones posteriores al historial](/Usage/Prompts/index.md#post-history-instructions) para el personaje.

!!!tip
Inserte `{{original}}` en cualquiera de los cuadros para incluir el prompt predeterminado respectivo de la configuración del sistema en un lugar designado.
!!!

### Metadatos del Creador

!!!info
No se utiliza para la construcción de prompts, pero proporciona metadatos adicionales sobre el personaje.
!!!

* **Creado por**: El nombre del creador del personaje. Puede mostrarse en la lista de personajes si la configuración de usuario "Char List Subheader" se establece en consecuencia.
* **Versión del Personaje**: La versión del personaje. Puede mostrarse en la lista de personajes si la configuración de usuario "Char List Subheader" se establece en consecuencia.
* **Notas del Creador**: Cualquier nota adicional sobre el personaje que el creador quiera compartir. Las primeras líneas se muestran en la lista de personajes, y el texto completo se muestra en la sección "Notas del Creador" en la página del personaje. Admite formato Markdown/HTML.
* **Etiquetas para Incrustar**: Una lista separada por comas de etiquetas que se incrustarán en la descripción del personaje. Estas etiquetas no se importan de forma predeterminada al importar el personaje, pero puede fusionarlas con sus etiquetas existentes seleccionando "Importar Etiquetas" en el menú "Más..." en la página del personaje.

### Resumen de Personalidad

Un breve resumen de la personalidad del personaje.

### Escenario

Las circunstancias y el contexto del diálogo.

### Nota del Personaje

Un texto a utilizar como inyección de prompt en el chat para el personaje en una profundidad de mensaje específica. Generalmente se utiliza para reforzar ciertos rasgos del personaje, ya que siempre se mantiene a una profundidad estática en el historial del chat, independientemente de su progresión.

* **@ Profundidad**: El número de mensajes en el historial del chat después de los cuales se inyectará esta nota (en orden del más nuevo al más antiguo). Si se establece en 0, se inyectará después del último mensaje.
* **Rol**: El rol del mensaje. Puede ser "Usuario", "Sistema" o "Asistente".

### Locuacidad

Determina la probabilidad de que la respuesta del personaje se active en chats grupales cuando se usa un orden de activación [Natural](/Usage/Characters/groupchats.md#natural-order). Oscila entre 0% y 100%, siendo 50% el valor predeterminado.

### Ejemplos de Diálogo

Describe cómo habla el personaje. Antes de cada ejemplo, debe agregar la etiqueta `<START>`. Los bloques de diálogo de ejemplo solo se insertan si hay espacio libre en el contexto para ellos y se expulsan del bloque de contexto por bloque. `<START>` no estará presente en el prompt ya que es solo un marcador; será reemplazado por el "Separador de Ejemplo" de Formato Avanzado para APIs de Finalización de Texto y el contenido del prompt de utilidad "Nuevo Chat de Ejemplo" para APIs de Finalización de Chat.

* Use el prefijo `{{char}}:` para denotar un mensaje del personaje.
* Use el prefijo `{{user}}:` para denotar un mensaje del usuario.

Ejemplo:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
