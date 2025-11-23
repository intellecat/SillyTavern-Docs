---
route: /extensions/expression-images/
---

# Expresiones de Personaje

## ¿Qué es?

Las imágenes de expresión son imágenes (también llamadas 'sprites') de tu personaje de IA que se muestran junto a (o detrás de) la ventana de chat.

Las imágenes de expresión pueden cambiar automáticamente en función de una clasificación, ajustándose al sentimiento expresado en la respuesta de chat más reciente de la IA.

## Agregando Imágenes de Expresión de Personaje

1. Abre el Panel de Extensiones y expande la sección 'Character Expressions'. Si tienes el chat de personaje abierto, verás una cuadrícula de espacios reservados de imágenes.
![Expression Drawer](/static/extensions/expression-drawer.png)
2. Haz clic en el botón 'Upload image' en la esquina superior izquierda de cada imagen en la cuadrícula, y selecciona la imagen que deseas aplicar a esa emoción. Esto guardará la imagen con el nombre de archivo correcto dentro de la carpeta `/data/<user-handle>/characters/(character_name_here)/`.
3. Repite esto para todas las expresiones a las que desees asignar una imagen.

### Importando un archivo ZIP de imágenes de expresión

Usando el botón '<i class="fa-solid fa-file-zipper"></i> Upload sprite pack (ZIP)', puedes importar un archivo zip que contenga una colección de imágenes de expresión, y esas imágenes se agregarán automáticamente a la carpeta correcta para tu **personaje seleccionado actualmente**. El archivo ZIP debe contener todas las imágenes en una estructura plana (sin subcarpetas) y archivos con nombres correctos. Importar un zip no renombrará automáticamente las imágenes para que coincidan con las emociones.

## Cambiar Expresiones Manualmente

1. Haz clic en cualquiera de las imágenes de expresión cargadas (sprites) para mostrarlas cerca de la interfaz de chat (con modo de interfaz predeterminado) o en el centro de la pantalla (en modo Visual Novel).
2. Usa el comando de barra oblicua `/expression-set (name)` o Quick Reply coincidente para establecer el sprite sin abrir el menú de extensiones.

## Cambiar Expresiones Automáticamente

Para establecer automáticamente las expresiones cuando el personaje responde, tienes varias opciones.
Las expresiones cambian por mensaje o a intervalos regulares cuando la transmisión de mensajes está habilitada.

### ¿Cómo funciona el módulo classify?

El módulo `classify` utiliza un pequeño modelo de 'análisis de sentimientos' que se ejecuta junto al servidor de SillyTavern. Este modelo toma la nueva salida de la IA y detecta qué tipo de sentimiento, o emoción, está expresando el texto. Aunque múltiples sentimientos pueden expresarse en un solo mensaje, el modelo solo elige el más probable y lo devuelve a SillyTavern. La extensión frontend luego muestra la imagen que está asociada con ese sentimiento.

### Instrucciones de Configuración (Local)

1. Abre el panel de extensiones y expande el menú de extensión "Character Expressions".
2. Selecciona "Local" en el menú desplegable de fuente de clasificación.
3. Esto iniciará una descarga única del modelo de clasificación desde HuggingFace Hub (aproximadamente ~100 Mb).
4. Genera cualquier mensaje para verificar que la clasificación funciona y el sprite aparece. También puedes verificar la consola del servidor en busca de registros de depuración.

La clasificación local predeterminada tiene 28 etiquetas de imagen posibles: [Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

Para usar el modelo de clasificación de 6 opciones, cambia el valor de la variable `extensions.models.classification` en el archivo `config.yaml` a: [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

### Instrucciones de Configuración (con LLM)

1. Conéctate a cualquiera de las API compatibles y correctamente configuradas a través de **<i class="fa-solid fa-plug"></i> API Connections**.
2. Importa las imágenes de expresión de la misma manera que se menciona arriba.
3. Selecciona "Main API" en el menú desplegable de fuente de clasificación.
4. Opcionalmente, configura el mensaje de instrucción de clasificación.
5. Genera cualquier mensaje para verificar que la clasificación funciona y el sprite aparece. También puedes verificar la consola del servidor en busca de registros de depuración.

#### Estrategias de Construcción de Avisos

La fuente LLM principal permite elegir cómo se construirá el aviso de clasificación:

* **Contexto Limitado**: Solo se envían el último mensaje y un aviso de instrucción del sistema.
* **Contexto Completo**: Se envía todo el historial de chat, incluida la tarjeta de personaje.

### Instrucciones de Configuración (WebLLM)

1. Instala la extensión oficial [WebLLM extension](https://github.com/SillyTavern/Extension-WebLLM).
2. Importa las imágenes de expresión de la misma manera que se menciona arriba.
3. Selecciona "WebLLM" en el menú desplegable de fuente de clasificación.
4. Opcionalmente, configura el mensaje de instrucción de clasificación.
5. Genera cualquier mensaje para verificar que la clasificación funciona y el sprite aparece. También puedes verificar la consola del servidor en busca de registros de depuración.

### Instrucciones de Configuración (con Extras)

> [!WARNING]
> Extras está deprecado y puede ser removido en futuras actualizaciones.

1. Ten Extras instalado y ejecutándose con el módulo `classify` habilitado: `python server.py --enable-modules=classify`
2. Importa las imágenes de expresión de la misma manera que se menciona arriba.
3. Selecciona "Extras" en el menú desplegable de fuente de clasificación.
4. La imagen de expresión apropiada se mostrará automáticamente cada vez que la IA te envíe una respuesta.

La API de Extras usa un modelo de clasificación con 6 opciones por defecto: [nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

También hay un modelo con 28 opciones: [joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

Para usar este modelo, debes cambiar tu línea de comandos de Extras para incluir el siguiente argumento (con un espacio antes y después): `--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

## Expresiones Personalizadas

¿Cómo obtener más opciones de expresión de las proporcionadas de forma predeterminada? Puedes configurar **Expresiones Personalizadas** en la configuración de la extensión. Puedes asignar cualquier nombre a las Expresiones Personalizadas. Aparecerán en la lista de imágenes de expresión y se les pueden asignar imágenes como otras expresiones. Tendrán un indicador que muestra que son personalizadas.

> [!TIP]
> Tanto Local como Extras solo soportan una lista limitada de expresiones.
>
> Si deseas que se muestren Expresiones Personalizadas, necesitas entrenar un modelo de clasificación con etiquetas soportadas (fuera del alcance de esta guía), o puedes usar LLM o WebLLM como fuente de clasificación, que ambos usarán automáticamente todas las expresiones existentes - tanto las predeterminadas como cualquier personalizada.

## ¿Qué formatos de imagen se admiten para Expresiones?

Se permite cualquier formato de imagen, incluidos webp y gifs animados.

El formato más común es un archivo PNG con fondo transparente.

## Usando Expresiones Predeterminadas

Si no tienes imágenes de expresión para todas las expresiones de un personaje, o no tienes imágenes en absoluto, hay múltiples opciones sobre qué mostrar por defecto.
Todas esas opciones se pueden seleccionar a través del menú desplegable en 'Default / Fallback Expression'.

1. **Elige una Expresión de Respaldo**: Si se elige una expresión para la que no tienes una imagen, se muestra la expresión de respaldo. Simplemente selecciona una de las expresiones disponibles en el menú desplegable.
2. **[No Fallback]**: Cuando no existe ninguna imagen, no muestra nada.
3. **[Default emojis]**: Puedes usar las expresiones predeterminadas integradas que se incluyen en SillyTavern. Estas son imágenes de estilo emoji simples.

## Usando Múltiples Imágenes por Expresión

Es posible agregar múltiples imágenes por expresión para permitir más variedad en las expresiones mostradas.
Para habilitarlo, simplemente activa **Allow multiple sprites per expression**.
Ahora puedes cargar más de una imagen, y cualquier imagen adicional se mostrará con un pequeño marcador.

Las imágenes individuales se pueden elegir manualmente haciéndoles clic, o a través de `/expression-set type=sprite`, que enumerará las imágenes de sprite disponibles, en lugar de expresiones.

Cada vez que se elige automáticamente una expresión con múltiples imágenes, se seleccionará una de las imágenes existentes al azar.
Si deseas forzar la selección de una nueva imagen de esa expresión cuando se usa la misma expresión varias veces, puedes habilitar **Re-roll if same sprite is used again**.

### Convención de Nomenclatura para Múltiples Imágenes por Expresión

En caso de múltiples imágenes por expresiones, los archivos deben ser nombrados de una manera específica.
Los archivos deben comenzar con el nombre de las expresiones, seguido de un sufijo, separado por un punto o un guión. Ejemplos: `joy.png`, `joy-1.png`, `joy.expressive.png`
Los nombres de archivo deben seguir este formato tanto para cargas directas como para importaciones ZIP.

## Anulación de Carpeta de Sprites

> [!NOTE]
> Los nombres de pantalla (no los nombres de archivo de tarjetas de personaje) determinan qué conjunto de imágenes se usa

Si tienes más de un personaje con el mismo nombre de pantalla, ambos usarán el mismo conjunto de imágenes de expresión.

Si deseas que se use un conjunto de imágenes diferente para cada versión del personaje con el mismo nombre, puedes usar la anulación de la carpeta de sprites.
Las anulaciones de carpeta también se pueden usar para definir diferentes conjuntos de sprites (atuendos, etc.) del mismo personaje.

### Cómo establecer una anulación

1. Crea una carpeta en `/data/<user-handle>/characters` con cualquier nombre y pon imágenes allí, por ejemplo `/data/<user-handle>/characters/Boris`.
2. Abre el chat con el personaje cuyos sprites deseas anular.
3. Ingresa el nombre de la carpeta de anulación en la entrada "Sprite Folder Override" y haz clic en "Submit".
4. La lista de Sprites se recargará y el indicador "Sprite set" debe mostrar la carpeta de anulación.
5. Alternativamente, puedes usar el comando de barra oblicua `/costume` para lograr el mismo resultado: `/costume Boris`.
6. Al anteponer una barra invertida al nombre de la carpeta de anulación, se resolverá a una subcarpeta en la carpeta de sprites del personaje actual, por ejemplo `/costume \tracksuit` para el personaje llamado Boris se resolverá a la carpeta `/data/<user-handle>/characters/Boris/tracksuit`.
