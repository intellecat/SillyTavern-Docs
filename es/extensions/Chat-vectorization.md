---
route: /extensions/chat-vectorization/
tags:
    [
        almacenamiento de vectores,
        RAG,
        generación aumentada por recuperación,
        vectores,
        summarización,
        chats,
        mensajes
    ]
---

# Vectorización de Chat

!!!warning Aviso de Responsabilidad
El uso de esta extensión no garantiza una mejor experiencia de chat ni memoria mejorada de ningún tipo. Solo úsela si entiende todas las implicaciones del uso de base de datos vectorial.
!!!

La vectorización de chat busca mensajes en el historial de chat actual que parecen relevantes para sus mensajes más recientes.
Reorganiza temporalmente los mensajes más relevantes al principio o al final del historial de chat.
Esto sucede cuando se genera la respuesta del modelo a su último mensaje.

Los mensajes al principio y al final del historial de chat tienden a tener el mayor impacto en la respuesta del modelo.
Por lo tanto, reorganizar mensajes relevantes a estas ubicaciones puede ayudar al modelo a enfocarse en información relevante en su respuesta.

En particular, la vectorización de chat puede encontrar mensajes relevantes que están demasiado atrás en el historial de mensajes para caber en el contexto de la solicitud. Reorganizar estos mensajes en el contexto proporciona al modelo información que de otro modo no tendría.

La vectorización de chat es un tipo de generación aumentada por recuperación (RAG). La generación aumentada por recuperación aumenta la calidad de las respuestas generadas por un modelo, proporcionando información adicional relevante en el mensaje.

* Recuperación: los mensajes más recientes se utilizan para recuperar mensajes pasados relevantes
* Aumentado: el contexto del modelo se aumenta insertando mensajes pasados de forma útil
* Generación: el modelo recibe instrucciones para usar los mensajes pasados al generar la respuesta

!!!info Algunos términos:
Un *vector* es un conjunto de números que podrían representar los temas, contenido, estilo u otras características de un fragmento de texto.

*Vectorización* es calcular el vector que representa un fragmento de texto. Esto se realiza mediante un modelo vectorizador.
Así como los modelos de generación de texto generan texto a partir de texto, los modelos vectorizadores generan vectores a partir de texto.

*Búsqueda vectorial* encuentra resultados relevantes comparando vectores en lugar de, por ejemplo, palabras clave. Si calculamos el vector para una consulta de búsqueda, podemos compararlo con los vectores almacenados para una colección de fragmentos de texto. Esto encuentra los textos en nuestra colección que son más similares al texto en la consulta de búsqueda. En el caso de vectorización de chat, la "consulta de búsqueda" son los últimos 2 mensajes, y los "textos en nuestra colección" son todos los demás mensajes en el chat.
!!!

## Configuración

!!!warning Compatibilidad con Almacenamiento en Caché de Mensajes
Como cualquier fuente de mensaje dinámico (World Info, Summarization, etc.), Chat Vectorization reestructura el prefijo de mensaje entre llamadas LLM, lo que puede llevar a fallos de caché frecuentes. Cuando se utiliza con almacenamiento en caché, la vectorización es a menudo contraproducente, ya que los mensajes modificados rara vez alcanzan el caché, haciendo que el almacenamiento en caché sea inútil. Tiene que elegir una u otra, pero no ambas.
!!!

Para habilitar la vectorización de chat, seleccione "Extensions" > "Vector Storage" > "Enabled for chat messages".

Configure una fuente de vectorización y modelo de vectorización. La vectorización de chat utiliza la misma fuente vectorial que Data Bank, por lo que es posible que ya haya configurado esto. Los ajustes para la Vectorization Source y Vectorization Model están documentados en [Data Bank](/Usage/Characters/data-bank.md).

La vectorización de chat utiliza el mismo almacenamiento vectorial que Data Bank, pero esto no necesita ser configurado u organizado.
También hay información sobre Vector Storage en [Data Bank](/Usage/Characters/data-bank.md).

La vectorización de chat no utiliza Data Bank para almacenar los mensajes de chat. Los mensajes se almacenan en el chat.

## Preparación de mensajes de chat para búsqueda (almacenamiento de vectores)

Para que se puedan buscar los mensajes de chat, se calcula un vector para cada mensaje y se almacena.

La vectorización ocurre en segundo plano, siempre que envíe o reciba un mensaje.

Cada mensaje se almacena individualmente, para que pueda ser encontrado y reorganizado individualmente durante la generación.

Los mensajes grandes se dividen en "fragmentos" para que el modelo pueda recibir la parte más relevante de un mensaje largo. El tamaño de fragmento es 400 caracteres.
Puede cambiar esto con "Chunk size (chars)".

Los mensajes se dividen en fragmentos encontrando un límite de fragmento como un salto de párrafo, salto de línea o espacio entre palabras. Esto es para que todos los fragmentos tengan sentido, en la medida de lo posible. Si sus mensajes de chat tienen otra manera de marcar puntos de división natural, como `----`, puede agregar esto a "Chunk boundary". La configuración para "Chunk boundary" se comparte con Data Bank.

### Controles de almacenamiento de vectores

Para calcular vectores para todos los mensajes en el chat actual, sin esperar a que se procesen en segundo plano, elija "Vectorize All" en la configuración.

Para ver cuántos mensajes en el chat actual han sido vectorizados, elija "View Stats". Esto muestra el número total de vectores almacenados.
También indica los mensajes de chat específicos que han sido vectorizados, marcándolos con un círculo verde.

Para eliminar todos los vectores para mensajes en el chat actual, elija "Purge Vectors".

!!!
Los controles para "Vectorize All" y "Purge Vectors" **dentro de Chat Vectorization** solo afectan los vectores almacenados para el chat actual.
Sin embargo, hay botones idénticos en File vectorization que afectan los vectores para archivos en Data Bank. Asegúrese de que está eliminando los vectores que pretende eliminar.
!!!

## Búsqueda de mensajes relevantes para reorganizar (recuperación vectorial)

Para encontrar los mensajes más relevantes en el historial de chat, los mensajes más recientes se convierten (vectorizan) en un vector de consulta. Por defecto, se utilizan los últimos 2 mensajes. Para cambiar esto, cambie el valor de "Query messages". Este valor también se utiliza al encontrar contenido relevante de Data Bank.

Los mensajes pasados deben tener una puntuación de relevancia de al menos 25% para ser incluidos. Puede cambiar esto con "Score threshold". La configuración para el umbral de puntuación se comparte con Data Bank.

Los 3 mensajes más relevantes del historial de chat se reorganizan. Puede cambiar esto con "Insert#".

Para evitar perturbar los eventos más recientes en el chat, los últimos 5 mensajes no se reorganizan. Para cambiar esto, cambie el valor de "Retain#".

## Reorganización de mensajes (generación aumentada)

Los mensajes se reorganizan en uno de 3 lugares:

* La parte superior del chat, después del Main Prompt / Story String (por defecto)
* La parte superior del chat y *antes del* Main Prompt / Story String
* El final del chat, antes de los últimos 2 mensajes ("In-chat @ Depth 2"). Como acaba de enviar un mensaje, esta posición es generalmente justo antes de la respuesta anterior del modelo.

Puede cambiar esto con "Injection Position" e "Depth".

Los mensajes se incluyen en orden de relevancia, con mensajes más relevantes mostrados después de mensajes menos relevantes.

El nombre de la persona o personaje que envió cada mensaje se incluye.

Los mensajes se muestran al modelo como "eventos pasados". Esto ayuda al modelo a entender que los mensajes contienen información desde un punto diferente en el historial de chat que el punto en el que se insertan. Puede cambiar esto con "Injection Template".

Puede ver el mensaje final al modelo utilizando la ventana emergente de Prompt Itemization, los registros de terminal o los registros de consola del navegador. Los registros de consola del navegador son útiles para entender qué están haciendo todos los pasos en la vectorización de chat.

## Summarización Vectorial

!!!warning Advertencia
La función de summarización vectorial es experimental.

**Vector summarization no crea resúmenes de su chat. No convierte los mensajes recuperados en resúmenes. No hace que el historial de chat sea más corto. No es "como [Summarize](/extensions/Summarize.md) pero mejor".**
!!!

La summarización vectorial está destinada a hacer que la búsqueda vectorial de mensajes de chat sea más efectiva. Lo hace introduciendo un paso de summarización antes de vectorizar. El paso de summarización extrae las partes más importantes del mensaje, de modo que el vector resultante sea un indicador mejor de lo que el mensaje se relaciona.

La summarización vectorial puede hacer que la búsqueda vectorial sea menos efectiva.

Para resumir los mensajes en el historial de chat, y generar un vector para cada mensaje resumido, elija "Summarize chat messages for vector generation".

El mensaje resumido no reemplaza al mensaje original en el chat. Si una búsqueda vectorial coincide con el vector de un mensaje resumido, el mensaje original se recupera del historial de chat y se reorganiza en el contexto. Las versiones resumidas de los mensajes se retienen en Vector Storage, lo que puede ser de interés para la depuración.

Para resumir el contenido de los mensajes utilizados para buscar en el historial de chat (los últimos 2 mensajes por defecto), elija "Summarize chat messages when sending".

Cada vez que un mensaje se resume para vectorizar, se realiza una solicitud separada al modelo de summarización. Puede elegir qué fuente de summarización se usa con "Summarize with". Elegir "Main API" generará los resúmenes utilizando el mismo modelo y configuración de conexión que utiliza para generar chat o completaciones de texto.

La solicitud consiste en el contenido del mensaje crudo y una instrucción sobre cómo el modelo debe producir el resumen. Puede cambiar la instrucción con "Summary Prompt".
