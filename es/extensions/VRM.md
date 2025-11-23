---
route: /extensions/vrm/
---

# VRM

Esta guía te guiará a través del proceso de configurar y personalizar la extensión VRM para tu experiencia de SillyTavern. Esta extensión te permite usar modelos animados VRM para tu personaje, proporcionando un elemento dinámico e interactivo para tu personaje virtual.

## Requisitos previos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos previos:

1. **Selección de rama**: Asegúrate de usar la rama de la última versión de SillyTavern para acceder a las últimas características y actualizaciones.

2. **Instalación de extensión**: Instala la extensión "VRM" desde el menú "Descargar extensiones y activos" en el panel de Extensiones (representado por el icono de bloques apilados).

3. **Ubicación de carpeta de modelo**: Coloca tus archivos de modelo VRM (.vrm) en el directorio `/data/<user-handle>/assets/vrm/model` y tus archivos de animación en el directorio `/data/<user-handle>/assets/vrm/animation`. Los formatos de archivo de animación actualmente compatibles son .fbx y .bvh que son compatibles con modelos VRM. Esto incluye cualquier animación que puedas obtener de Mixamo (https://www.mixamo.com/) y cualquier animación que puedas exportar desde herramientas como XR Animator (https://github.com/ButzYung/SystemAnimatorOnline).

## Configuración de extensión

La extensión VRM ofrece varias configuraciones para personalizar el comportamiento de tu modelo animado. Aquí están las configuraciones clave:

![UI global settings](/static/extensions/vrm-global.png)

### Configuración global

1. **Habilitado**:
   - Habilita esta casilla para activar la extensión, permitiendo que tu modelo VRM interactúe dentro de SillyTavern.
   - Puedes desactivar la extensión si deseas usar solo sprites normales.

2. **Mirar a la cámara**:
   - Habilita esta casilla para hacer que los ojos del modelo VRM miren a la cámara.

3. **Parpadeo**:
   - Habilita esta casilla para hacer que los ojos del modelo VRM parpadeen en intervalos aleatorios. Las expresiones del modelo deben definir correctamente la propiedad de peso de parpadeo; de lo contrario, el modelo puede parpadear con los ojos cerrados, por ejemplo. Si eso sucede, puedes:
    - corregir el modelo si tienes el archivo .vroid
    - no usar esa expresión facial incorrecta
    - desactivar el parpadeo completamente con esta casilla

4. **Sincronización de labios TTS**
    - Habilita esta casilla para que el movimiento de la boca VRM siga el sonido de tu TTS cuando se reproduce. Solo funciona con TTS cuyo sonido es reproducido por Sillytavern mismo, como XTTS (no en modo de transmisión). Si está deshabilitado, la boca será animada de acuerdo con la longitud del texto del mensaje cuando se reciba un nuevo mensaje de personaje.

5. **Auto-enviar interacción**:
   - Habilita esta casilla para desencadenar automáticamente interacciones de personaje cuando hagas clic en áreas con mensajes mapeados (consulta la sección de áreas de golpe para más detalles).

### Configuración de rendimiento

1. **Hitboxes del cuerpo**
    - Habilita esta casilla para activar la detección de clics en varias partes del modelo VRM. Dependiendo del modelo, se pueden detectar las siguientes áreas: cabeza/pecho/manos/ingle/glúteos/piernas/pies. La ubicación de los hitboxes se calcula en cada fotograma y sigue la animación del cuerpo; desactivar esta opción puede mejorar el rendimiento.

2. **Usar caché de modelo**
    - Habilita esta casilla para mantener en memoria el modelo VRM al cambiar de modelos, lo que permite volver al modelo anterior más rápidamente. Útil si usas diferentes modelos para el mismo personaje para cambiar de traje o forma, por ejemplo. Puede afectar el rendimiento.

3. **Usar caché de animación**
    - Habilita esta casilla para mantener en memoria todas las animaciones reproducidas durante la sesión. Todas las animaciones asignadas a un modelo también se cargarán la primera vez que el modelo aparezca. Aumentará el tiempo que cargas el modelo la primera vez pero hará que todos los cambios de animación sean instantáneos. Puede afectar el rendimiento.

### Configuración de depuración

1. **Mostrar cuadrícula**
    - Habilita esta casilla para visualizar la cuadrícula 3d, el cuadro de arrastre del modelo y los hitboxes del cuerpo.

2. **Botón de recarga**
    - Haz clic en este botón para recargar la escena 3d, borrar la caché y todos los modelos VRM. Úsalo si ocurre algún error o si la caché comienza a afectar el rendimiento.

### Configuración de escena

![UI scene settings](/static/extensions/vrm-scene.png)

1. **Color de luz**
    - Establece el color de la luz en la escena 3d. Haz clic en el botón de reinicio para establecerlo de vuelta al color blanco predeterminado. Dependiendo de tu navegador, puedes usar un selector de color. Por ejemplo, puedes seleccionar el color del color de tu imagen de fondo para añadir más inmersión.

2. **Intensidad de luz**
    - Establece la intensidad de la luz en porcentaje usando el control deslizante. Haz clic en el botón de reinicio para establecerlo de vuelta al valor predeterminado del 100%. El modelo VRM puede reaccionar diferente a la luz dependiendo de los shaders integrados en el modelo; juega con el valor y observa cómo va.

![UI model settings](/static/extensions/vrm-model.png)

## Selección de personaje

Estas configuraciones te permiten gestionar personajes y asignar modelos VRM a ellos.

1. **Botón de actualización**:
   - Haz clic en el botón de actualización para actualizar la lista de personajes en el chat actual.

2. **Seleccionar personaje**:
   - Usa la lista desplegable para elegir un personaje al que asignar un modelo VRM.

3. **Botón Eliminar**:
   - Haz clic en este botón para eliminar el modelo asignado para un personaje.

## Selección de modelo

1. **Botón de actualización**:
   - Haz clic en el botón de actualización si tu modelo VRM no aparece en la lista.

2. **Seleccionar modelo**:
   - Elige un modelo de la lista para asignarlo al personaje seleccionado.
   - El modelo debe estar ubicado en el directorio `/data/<user-handle>/assets/vrm/model`.

3. **Botón de reinicio**
    - Haz clic en este botón para reiniciar la configuración del modelo a su valor predeterminado. Si tienes archivos de animación que corresponden al valor predeterminado, se asignarán automáticamente. Consulta el mapeo de nombres al final de este README.

## Configuración de modelo

1. **Escala de modelo**:
   - Usa el control deslizante para ajustar el tamaño del modelo, haciéndolo más grande o más pequeño.

2. **Desplazamiento del centro del modelo X/Y**:
   - Usa estos controles deslizantes para cambiar la posición horizontal/vertical del modelo relativa al centro de la ventana.

3. **Rotación del modelo X/Y**
    - Usa estos controles deslizantes para cambiar la rotación horizontal/vertical del modelo relativa a las caderas del modelo.

### Observaciones
    - La configuración se guarda por modelo, no por personaje, y se traslada entre diferentes chats.
    - Si deseas usar el mismo modelo para dos personajes diferentes con configuraciones diferentes, crea una copia del archivo .vrm.
    - También puedes arrastrar el modelo con tu ratón, y estas configuraciones se actualizarán y guardarán. Haz clic izquierdo y mantén presionado para arrastrar un modelo alrededor de la pantalla. Haz clic con el botón central del ratón y mantén presionado para rotar el modelo o usa shift+clic izquierdo. Usa la rueda del ratón con el cursor sobre el modelo para agrandarlo o reducirlo, o usa ctrl+clic izquierdo.
    - Usa estas configuraciones de la interfaz de usuario para devolver tu modelo a la pantalla si de alguna manera se salió de vista. Además, marca la casilla "Mostrar marco" para ver claramente dónde puedes hacer clic para arrastrar el modelo.

![UI hitboxes settings](/static/extensions/vrm-hitboxes.png)

## Mapeo de hitboxes

    - Dependiendo de la definición de huesos del modelo, se pueden generar algunas áreas de hitbox; se enumerarán en esta parte de la interfaz de usuario, y puedes asignar una expresión/animación/mensaje a cada una de ellas que se activará cuando hagas clic en el área.

![UI classify settings](/static/extensions/vrm-classify.png)

## Mapeo de expresiones clasificadas

1. **Requisitos**
    - Requiere el uso de la extensión de expresión clasificar; de lo contrario, volverá a la animación predeterminada.

2. **Mapeo**
    - Para cada emoción detectada por la extensión classify, puedes asignar una expresión/movimiento/mensaje. El mensaje puede contener comandos.

## Comandos

1. **/vrmlightcolor**
    - establece el color de la luz
    - argumentos: color
    - ejemplo: "/vrmlightcolor white" o "/vrmlightcolor purple".
2. **/vrmlightintensity**
    - establece la intensidad de la luz en porcentaje
    - argumentos: intensidad
    - ejemplo: "/vrmlightintensity 0" o "/vrmlightintensity 100
3. **/vrmmodel**
    - asigna el modelo vrm al personaje
    - argumentos: personaje, modelo
    - ejemplo: "/vrmmodel Seraphina.vrm" en chat solo o "/vrmmodel character=Seraphina model=Seraphina.vrm" en chat grupal
4. **/vrmexpression**
    - cambia la expresión del modelo
    - argumentos: personaje, expresión
    - ejemplo: "/vrmexpression happy" en chat solo o "/vrmexpression character=Seraphina expression=happy" en chat grupal

5. **/vrmmotion**
    - cambia la animación del modelo
    - argumentos: personaje, movimiento, bucle, aleatorio
    - "/vrmmotion idle" o "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## Mapeo predeterminado de animaciones
Si tus archivos de animación se nombran de la siguiente manera, se asignarán automáticamente al reiniciar la configuración del modelo. Por ejemplo, los archivos llamados "assets/vrm/animation/neutral.bvh" y "assets/vrm/animation/neutral1.fbx" se asignarán automáticamente como un grupo para la animación clasificada neutral y predeterminada. Lo mismo ocurre con los hitboxes.

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

¡Gracias por seguir esta guía! Tu experiencia de SillyTavern está ahora enriquecida con modelos 3D animados e interactivos.

## Observaciones
    - Los modelos VRM cargados por esta extensión son los archivos .vrm, no los archivos .vroid.
    - Los archivos de animación deben ser compatibles con VRM. Puedes usar una herramienta como XR animation (https://github.com/ButzYung/SystemAnimatorOnline) para convertir archivos de animación fbx/bvh.
    - Puedes crear grupos de animación teniendo archivos con el mismo nombre terminado en números diferentes. Por ejemplo: "idle1.bvh", "idle2.bhv", "idle3.bvh" se considerarán como un grupo "idle" y cuando se seleccione en un mapeo, uno aleatorio se reproducirá cuando se active. Se puede usar para añadir variedad a las animaciones.
    - Puedes obtener animaciones seleccionadas de este repositorio: https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral tiene algunos videos de tutorial sobre cómo usar la extensión y el repositorio de animaciones: https://www.youtube.com/@nitralai
