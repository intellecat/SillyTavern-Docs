---
route: /usage/api-connections/novelai/
title: NovelAI
---

# NovelAI

NovelAI es un servicio de suscripción de pago que permite acceso ilimitado mensual a su generación de texto de alta calidad, generación de imágenes y modelos de conversión de texto a voz. Registra una cuenta aquí para comenzar: <https://novelai.net/>

Solo obtendrás *50 generaciones* gratis para evaluar el modelo. Cuando aparece el error **"Not eligible for this model"**, significa que has agotado tu período de prueba y necesitas suscribirte a un plan de pago.

## Clave API

Para obtener tu clave API de NovelAI, sigue estos pasos:

1. Selecciona el icono de engranaje en la parte superior de la barra lateral izquierda.
![Left Sidebar](/static/novel-side.png)

2. Selecciona "Account" bajo "User Settings".
![User Settings](/static/novel-user.png)

3. Selecciona "Get Persistent API Token".
![Account](/static/novel-account.png)

4. Selecciona el icono de copiar para copiar tu token API de NovelAI al portapapeles.
![Persistent API Token](/static/novel-token.png)

## Modelos

Si tienes Opus, entonces Erato es el modelo a usar. Si no tienes Opus, entonces Kayra es el mejor modelo disponible.

Clio tiene un tamaño de contexto más grande en los niveles Tablet/Scroll, pero la solidez de Kayra generalmente compensa esa diferencia.

## Configuración

Los archivos con la configuración están aquí (`SillyTavern/data/<user-handle>/NovelAI Settings`).
También puedes agregar manualmente tus propios archivos de configuración.

### Longitud de Respuesta

Cuánto texto deseas generar por mensaje. Ten en cuenta que NovelAI tiene un límite de 150 tokens por respuesta.

### Tamaño del Contexto

Cuántos tokens del chat se mantienen en el contexto en cualquier momento. El tamaño máximo de contexto que puedes usar depende del modelo y tu nivel de suscripción:

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (Opus exclusive), Kayra (Opus) y Clio (all tiers) - 8192 tokens

### Preámbulo

Texto que se inserta justo encima del chat para modificar el estilo de escritura. El formato recomendado es una lista de etiquetas cortas, como "[ Style: chat, detailed, sensory ]".

## Descripciones de Presets

Esto es, según NovelAI, para qué son buenos los presets predeterminados.

### Erato

* Golden Arrow - Un buen todoterreno.
* Wilder - Mayor variedad de elección de palabras, más diferencias entre reintentos, más propenso a errores.
* Zany Scribe - Evita errores y repeticiones. Prioriza palabras más complejas.
* Dragonfruit - Lenguaje variado y complejo con poca repetición. Errores y contradicciones más frecuentes.
* Shosetsu - Diseñado para escribir en japonés. También funciona bien en inglés.

### Kayra

* Asper - Para escritura creativa. Espera giros inesperados.
* Carefree - Un buen todoterreno
* Fresh-Coffee - Mantiene las cosas en su lugar. Maneja bien las instrucciones.
* Pro_Writer - Imita el ritmo y la sensación de la ficción más vendida
* Stelenes - Más probable que elija alternativas razonables. Variedad en reintentos.
* Tea_Time - Se pone bien cuando comienza.
* Writers-Daemon - Extremadamente imaginativo, a veces demasiado.

### Clio

* Edgewise - Maneja bien una variedad de estilos de generación
* Fresh Coffee - Mantiene las cosas en su lugar.
* Long-Press - Destinado a prosa creativa.
* Talker Chat - Diseñado para generación estilo chat.
* Vingt-Un - Un buen predeterminado versátil con tendencia hacia la prosa.

## Consejos y Preguntas Frecuentes para usar NovelAI con SillyTavern

Hay muchos problemas comunes y preguntas que surgen cuando cambias a NovelAI desde otra API de backend de ST. La diferencia se reduce a para qué fueron entrenados los modelos. Lo más probable es que hayas usado un modelo de OpenAI o Anthropic (o un modelo local hecho para parecerse a esos), que está construido para seguir las instrucciones del usuario. Los modelos de NovelAI se construyeron puramente alrededor de la finalización de texto: en lugar de tomar tu entrada como un mensaje y formular una respuesta, los modelos NAI intentan continuar el prompt entrante. Debido a esta diferencia, muchos consejos y conocimientos comunes que funcionan para otras API no funcionarán para NAI.

### Ajuste de Configuración para NovelAI

Bajo Advanced Formatting (el icono A):
- Establece "Context Template" a "NovelAI"
- Establece "Tokenizer" a "Best match"
- Marca "Always add character's name to prompt"
- Marca "Collapse Consecutive Newlines"
- Desmarca la casilla "Enabled" bajo "Instruct Mode"

Bajo User Settings (la persona con un engranaje)
- Activa "Swipes" (No es específico de NAI, pero es tan útil que simplemente deberías hacerlo)

### Crear/Adaptar Tarjetas de Personaje para NovelAI

Para optimizar tus tarjetas de personaje para NovelAI, hay un par de métodos recomendados para escribir la descripción de tu personaje: prosa y atributos.

La prosa es tan simple que no parece que debería funcionar: "Sylpheed is a young-looking but actually 900 year old nymph. She's short and petite, with long white hair that fades into a green gradient in her braided side ponytail, and emerald green eyes shaped like crosses.[...]" No, en serio, eso es todo. Solo escribe, en oraciones normales, cómo se ve el personaje, cómo actúa, etc., y la IA lo captará.

Si no confías en tus habilidades de escritura o quieres una forma más estructurada de hacerlo, puedes usar el método de atributos, que está presente en los datos de entrenamiento de NovelAI. Esto funciona como una lista simple de características de personaje de diferentes tipos. Aquí hay una lista de atributos posibles que han sido probados como efectivos con los modelos de NovelAI:

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

"Type: character" está ahí para decirle a la IA que esto describe un personaje (a diferencia de una ubicación, objeto u otro tipo de cosa). El resto de los atributos son opcionales, y algunos son redundantes (por ejemplo, Personality, Mind y Mental todos significan básicamente lo mismo), pero estos han sido probados y funcionan bien con los modelos de NovelAI. Rellena los que sean relevantes para tu personaje. Los atributos deben escribirse en minúsculas y separados por comas, sin necesidad de comillas alrededor de las palabras. Por ejemplo:

```
Skills: lockpicking, stealth, running away very fast
```

Estos métodos se recomiendan porque están presentes en los datos de entrenamiento de NovelAI, por lo que funcionan específicamente bien con el modelo.

#### Tarjetas de Ejemplo

Aquí hay un par de tarjetas de ejemplo, hechas para NovelAI, que muestran diferentes formas de crear tarjetas específicamente para NovelAI. La primera tarjeta, Valka, utiliza el método de atributos para la descripción del personaje, mientras que Eris, la segunda tarjeta, utiliza descripciones en prosa, junto con una gran cantidad de diálogos de ejemplo.

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### Qué No Hacer

La mayoría de los formatos de tarjeta de personaje existentes no son una buena opción para NovelAI. Te darán algunos resultados, incluso algunos buenos, pero tienen muchos problemas. W++ es uno de los mayores infractores, donde no se parece a nada en lo que fueron entrenados los modelos de NovelAI, y su uso constante de corchetes/llaves/comillas consume una tonelada de tokens, inflando el tamaño de las tarjetas sin ningún beneficio real.

De los formatos existentes que no están integrados en NovelAI, AliChat es el que más probablemente funcione, ya que se basa en el uso de mensajes de ejemplo para transmitir tanto información sobre el personaje como su voz al mismo tiempo, en el formato del tipo de mensaje que deseas que emita la IA.

Para la mayoría de los otros formatos, dado que generalmente son formas de enumerar diferentes características de un personaje particular, pueden convertirse al método de atributos de manera bastante directa.

### ¿Qué Módulo Debo Usar?

Probablemente No Module. Prose Augmenter es útil si deseas que un personaje hable de una manera más florida, pero ten cuidado de no excederte. Text Adventure podría ser útil para una tarjeta/historia de estilo aventura de texto.

### ¿No el Módulo Instruct?

Puedes invocar el módulo Instruct cuando lo necesites. Crea una nueva línea en tu mensaje e inserta tus instrucciones entre llaves así: `{ CharName is offended by that seemingly innocuous statement }` (los espacios son _necesarios_ entre el texto y las llaves). Si haces eso, la IA cambiará automáticamente al módulo Instruct por un corto tiempo. No quieres usar el módulo Instruct todo el tiempo porque tiende a producir una salida menos creativa que los otros módulos, solo cuando necesites guiar a la IA fuertemente en una dirección particular.

### ¿Por Qué mis Respuestas se Cortan?

NovelAI limita la longitud de la respuesta a ~150 tokens en total, incluso si estableces el control deslizante más alto que eso. Cuando alcanza la cantidad de tokens en el control deslizante o 150, lo que sea menor, generará hasta 20 tokens más, buscando una secuencia de parada o el final de una oración, por lo que hay un límite efectivo de 170 tokens para una respuesta, en cuyo punto simplemente se detendrá, lo que causa que se corte.

Si se corta, puedes seleccionar la opción de continuar (en el menú de tres líneas a la izquierda del cuadro de texto) para que el personaje continúe su respuesta.

Si regularmente deseas respuestas más largas que 170 tokens, puedes eludir el límite así:

- Mantén la longitud de respuesta en 150 tokens.
- Bajo Advanced Formatting, habilita Auto-continue.
- Establece la "Target length" a la longitud deseada.

Esto encadenará múltiples generaciones para darte mensajes más largos pero no garantiza que la respuesta sea el 100% de la longitud deseada si el modelo decide detenerse.

### ¿Cómo Hago que el Bot Escriba Respuestas Más Largas?

Lee lo anterior sobre respuestas que se cortan. Eso te ayudará a asegurar que las respuestas no se corten prematuramente al ejecutar el límite de la longitud de generación.

Si tus respuestas no se están cortando pero aún son demasiado cortas, probablemente estés lidiando con "garbage in, garbage out" - si le das malos ejemplos al modelo, producirá una salida deficiente. Si la tarjeta de personaje no tiene diálogos de ejemplo o diálogos de ejemplo cortos y los mensajes que envías al bot son cortos, el modelo lo captará, lo tomará como la forma aceptada de hacer las cosas y las respuestas serán cortas. Entonces, escribe diálogos de ejemplo más largos y mensajes más largos al bot. (Siempre puedes usar NovelAI para escribir algunos diálogos de ejemplo para ti en lugar de hacerlo tú mismo.)

### ¿Cómo Hago que el Bot Deje de Hablar por Mí?

- Verifica que el primer mensaje y el diálogo de ejemplo de la tarjeta del personaje no incluyan al personaje realizando acciones por ti - si lo hacen, reescribelos para eliminar esa actuación por ti
- Asegúrate de que "Always add character's name to prompt" esté marcado
- Asegúrate de que estés usando actualmente el mismo persona de usuario que el resto del chat. Si cambió personas de usuario y no volvió (o no tiene una persona bloqueada en ese chat), las reglas habituales para dejar de generar para ti fallarán
- Agrega ["\n\{\{user\}\}:"] a Custom Stopping Strings (no debería ser necesario, pero a veces ayuda)

### ¿Por Qué mi Personaje no Responde?

Muchas cosas pueden causar esto, así que necesitamos buscar en algunos lugares:

- Asegúrate de que "Always add character's name to prompt" esté marcado en Advanced Formatting
- Verifica que no haya errores provenientes de la API. Aunque puedes usar SillyTavern con la prueba gratuita de NAI, una vez que se agota, solo obtendrás errores
- Comprueba lo que tienes en "Custom Stopping Strings" - si se generan al principio de la respuesta, podría cortarse prematuramente

### ¿Cómo Debo Usar la Nota del Autor?

En general, probablemente no deberías. Se inserta muy cerca del final del contexto, y con los modelos de NAI, frecuentemente superan todo lo demás en el contexto. Es principalmente un artefacto de modelos más antiguos y débiles donde era más necesario.

### ¿Cómo Hago un Salto de Escena/Salto de Tiempo?

Coloca lo siguiente como un mensaje del sistema o en nuevas líneas al principio de tu siguiente mensaje:
```
***
[ 2 days later ]
```

Luego coloca el resto de tu mensaje en la siguiente línea. El texto entre corchetes puede ser un salto de tiempo, una nueva ubicación o cualquier otra cosa. El "***" (hilarantemente llamado "dinkus") le dice a la IA que la escena ha cambiado, y el texto entre corchetes proporciona más contexto.

### La IA Sigue Repitiendo Palabras/Frases Específicas, ¿Qué Hago?

Como se mencionó anteriormente, puedes empujar el control deslizante de la penalización de repetición un poco más, aunque empujarlo demasiado lejos puede hacer que la salida sea incoherente.

Para solucionar el problema de manera más exhaustiva, vuelve a través del contexto, especialmente los mensajes recientes, y elimina la palabra/frase repetida. Eliminarla del contexto le da menos razones a la IA para comenzar a decirla en primer lugar.
