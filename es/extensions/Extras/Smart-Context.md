---
route: /extensions/smart-context/
---

# Smart Context

## **ESTA EXTENSIÓN YA NO SE MANTIENE Y NO SE RECOMIENDA USAR. CONSIDERE [CHAT VECTORIZATION](/extensions/Chat-vectorization.md) COMO UNA POSIBLE ALTERNATIVA.**

!!!warning Descargo de responsabilidad
El uso de esta extensión no garantiza una mejor experiencia de chat o memoria mejorada de ningún tipo. Use solo si comprende todas las implicaciones del uso de la base de datos vectorial.
!!!

### ¿Qué es?

Smart Context es una extensión de SillyTavern que usa la [biblioteca ChromaDB](https://www.trychroma.com) para dar a sus personajes de IA acceso a información que existe fuera del límite de contexto del historial de chat normal.

### ¿Cómo es eso útil?

Si tiene un chat muy largo, la mayoría del contenido está fuera de la ventana de contexto habitual y, por lo tanto, no está disponible para la IA al momento de escribir una respuesta.

Smart Context toma automáticamente todo el historial del archivo de chat y lo coloca en una base de datos vectorial. Esta base de datos se busca cada vez que introduce algo nuevo en el chat, y si se encuentran mensajes con palabras clave coincidentes, esos mensajes de chat se colocan en el contexto para que la IA pueda verlos cuando escriba su próxima respuesta.

***

### Instrucciones de Configuración

1. Actualice SillyTavern a la versión 1.10.6 o posterior.
2. Instale la extensión "Smart Context" desde el menú "Descargar Extensiones y Activos" en el panel de Extensiones (icono de bloques apilados).
3. Instale o Actualice [Extras](https://github.com/SillyTavern/SillyTavern-extras) a la versión más reciente. Alternativamente, use el [cuaderno de Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb).
4. *Solo instalaciones locales:* Instale requirements-complete.txt para Extras (aunque lo haya hecho una vez antes en una instalación anterior).
5. Ejecute Extras con el módulo chromadb habilitado: `python server.py --enable-modules=chromadb`

#### ¿Recibiendo un error al instalar ChromaDB?

```
ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

Instalar el paquete chromadb requiere uno de lo siguiente:

- Tener herramientas de compilación Visual C++ instaladas: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- Instalar hnswlib desde conda: `conda install -c conda-forge hnswlib`

***

### Configuración

Una vez que Smart Context esté habilitado, debe configurarlo en la interfaz de usuario de SillyTavern.
La configuración de Smart Context se puede hacer desde el menú Extensiones ![STExtensionMenuIcon](/static/extensions/menu-icon.png)

![Panel de Configuración de Smart Context](/static/extensions/smart-context.png)

Hay 4 conceptos principales a tener en cuenta:

- Preservación del Historial de Chat
- Cantidad de Inyección de Memoria
- Longitud Individual de Memoria
- Estrategia de Inyección

***

#### SmartContext comienza solo después de que haya 10 mensajes en el historial de chat

- Al inicio de un nuevo chat, ChromaDB está inactivo.
- Una vez que el chat ha acumulado 10 mensajes, comenzará a registrar todos los mensajes en la base de datos y recuperará mensajes según sea necesario.

#### Preservación del Historial de Chat ('mensajes mantenidos')

De forma predeterminada, ChromaDB mantendrá tantos mensajes recientes del historial de chat natural como se especifique en el control deslizante.
Cualquier mensaje más allá de esta cantidad se eliminará de su solicitud enviada, y si existen 'recuerdos' en la base de datos se agregarán en lugar de los mensajes del historial de chat más antiguos (vea la Estrategia a continuación).

***

#### Cantidad de Inyección de Memoria

El número máximo de 'recuerdos' que Smart Context insertará en el contexto.
No todos los intentos de inyección obtendrán esta cantidad completa.
Si envía una entrada relacionada con 'perros' y solo otro mensaje en la BD está relacionado con perros, solo se insertará 1 elemento.

***

#### Longitud Individual de Memoria

Esta es la longitud máxima permitida para cada 'recuerdo' inyectado.
Esto está en **CARACTERES** (no tokens).
Si se establece demasiado pequeño, el recuerdo podría cortarse a mitad de camino.

Ejemplo:

`Ross: I like dogs with long fur and fluffy tails. I dislike dogs with short fur and short tails.`

Este 'recuerdo' de la base de datos tiene 103 caracteres de largo, por lo que necesitaría establecer el control deslizante en al menos `103` para sacarlo completamente del contexto.

Si el control deslizante es menor de 103, el mensaje se cortaría e inyectaría así.

***

### Estrategia de Inyección

#### Reemplazar historial más antiguo

Esta estrategia mantiene X mensajes recientes, elimina todos los mensajes antes de eso, y los reemplaza con 'recuerdos'.

Ventaja

- menos probable que sobrepase su límite de contexto
- los recuerdos existentes cerca de la parte superior del contexto tendrán menos impacto inmediato en la respuesta mientras aún proporcionan 'información de fondo'.

Desventaja

- los mensajes antiguos se insertan directamente en el historial de chat sin demarcación especial, y generalmente no tienen relevancia natural inmediata para los mensajes del historial de chat natural preservados. Esto puede confundir a los modelos de IA menos inteligentes.

#### Agregar a la Parte Inferior

Esta estrategia deja el historial de chat en su estado natural y agrega 'recuerdos' **después** de él dentro de un [encabezado entre corchetes] formateado.
Esto significa que el control deslizante 'mensajes mantenidos' es efectivamente deshabilitado.

Ventaja

- no acorta ni altera el historial de chat natural actual
- los 'recuerdos' existen después del chat y tienen un impacto más fuerte en la próxima respuesta de IA

Desventaja

- debido a que no se están removiendo/reemplazando elementos de chat, hay una mayor probabilidad de que sobrepase su límite de contexto.
- debido a que los recuerdos existen muy cerca del final del aviso, pueden tener DEMASIADO efecto en la respuesta de la IA.

#### Profundidad Personalizada

Esta estrategia deja el historial de chat en su estado natural y agrega 'recuerdos' en la profundidad que determine dentro de la plantilla que especifique.
Esto significa que el control deslizante 'mensajes mantenidos' es efectivamente deshabilitado.
El mensaje de inyección personalizado debe incluir la palabra de plantilla `{{memories}}` que es donde se colocarán todos los recuerdos consultados.

Ventaja

- flexibilidad para experimentar con la colocación de memoria
- introducciones personalizables a memoria dentro del contexto

Desventaja

- debido a que no se están removiendo/reemplazando elementos de chat, hay una mayor probabilidad de que sobrepase su límite de contexto.


#### Usar Estrategia de %

Nota: Esto no es compatible con la estrategia 'Agregar a la Parte Inferior', que no elimina ningún mensaje en absoluto.

Mientras usa la estrategia 'Reemplazar Historial Más Antiguo', marcar esta casilla habilitará el control deslizante para seleccionar un porcentaje del historial de chat en contexto para reemplazar con recuerdos de SmartContext. También deshabilitará los dos controles deslizantes para seleccionar manualmente el número de mensajes.

Esta estrategia calcula automáticamente un porcentaje del historial de chat para ser reemplazado con recuerdos de SmartContext, en lugar de un número fijo de mensajes.

Ventaja

- más fácil que calcular manualmente el número de mensajes usted mismo
- se ajusta con el tamaño de contexto disponible, aplicando el mismo porcentaje a espacios de solicitud pequeños y grandes

Desventaja

- los cálculos de cuánto historial eliminar pueden ser ligeramente inexactos ya que se basan en tokens estimados por mensaje
- redondea el número de mensajes a eliminar al número más cercano divisible por 5 (0, 5, 10, 15, 20, etc), por lo que no es tan detallado como la selección numérica manual.

***

### Estrategia de Recuperación de Memoria

#### Recuperar solo de este chat

Este es el comportamiento predeterminado de smart-context y extrae 'recuerdos' solo de la colección ChromaDB para este chat específico.

#### Recuperar de todos los chats de personajes

Este es un comportamiento experimental de smart-context que extrae 'recuerdos' de todas las colecciones ChromaDB para el personaje seleccionado.
Hipotéticamente, esto debería permitir el desarrollo de un conjunto de memoria más robusto que abarque muchas interacciones.
Se recomienda que esto se use con estrategias 'Agregar a la Parte Inferior' o 'Profundidad Personalizada' y 'mensajes mantenidos' configurados en un número bajo para que ChromaDB extraiga de la memoria más pronto.

### Usar Smart Context

Una vez que esté habilitado y configurado, Smart Context sucede automáticamente.

ChromaDB crea una nueva base de datos para cada chat que se abre dentro de SillyTavern.
Esta base de datos se rellena automáticamente con el historial de chat completo.

También puede insertar manualmente archivos de texto en la base de datos.

Estos archivos de texto no tienen que ser chats. Pueden ser cualquier cosa (entradas de wikipedia, fanfic, etc).

#### Purgar la Base de Datos

Puede usar el botón 'Purgar BD' para limpiar la base de datos para el chat actual.

Esto puede ser útil si encuentra que se han almacenado recuerdos inexactos (como mensajes de chat que ha eliminado o editado desde entonces).

***

### Preguntas Frecuentes

#### ¿Qué sucede con las bases de datos cuando termino de chatear? ¿Puedo guardarlas?

Para servidores Extras instalados localmente, Smart Context guarda las bases de datos. No es necesario guardarlas manualmente en casos de uso habituales.

Para usuarios de colab, las bases de datos se borran cuando se apaga el servidor de extras. Use el botón de exportación para guardar la base de datos como un archivo JSON e impórtela la próxima vez que desee usarla.

**Generalmente no hay necesidad de guardar bases de datos de Smart Context.**

Actualmente tenemos una función Importar/Exportar, que le permite guardar la BD del chat y usarla nuevamente en una fecha posterior.

#### ¿Puedo hacer una gran base de datos para que todos mis chats hagan referencia?

No sería un buen uso de las capacidades de Smart Context.
Recomendamos usar World Info para este propósito.
