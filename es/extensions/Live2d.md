---
route: /extensions/live2d/
---

# Live2D

Esta guía te guiará a través del proceso de configuración y personalización de la extensión Live2D para tu experiencia de SillyTavern. Esta extensión te permite usar modelos animados Live2D para tu personaje, proporcionando un elemento dinámico e interactivo para tu personaje virtual.

## Requisitos previos

Antes de comenzar, asegúrate de cumplir los siguientes requisitos previos:

1. **Selección de rama**: Asegúrate de estar utilizando la versión más reciente de SillyTavern para acceder a las últimas funcionalidades y actualizaciones.

2. **Instalación de extensión**: Instala la extensión "Live2D" desde el menú "Descargar extensiones y activos" en el panel de extensiones (representado por el icono de bloques apilados).

3. **Ubicación de carpeta de modelos**: Coloca tus carpetas de modelos Live2D en el directorio `/data/<user-handle>/assets/live2d`. Una carpeta de activos `live2d` adecuadamente organizada podría verse así:

    ![Ejemplo de carpeta de activos](/static/extensions/live2d-folder.png)

    - Una carpeta de modelo Live2D debe incluir todos los componentes necesarios para el modelo Live2D, como expresiones, movimientos, texturas, sonidos y archivos de configuración. Notablemente, el archivo `***.model.json` debe estar en la raíz de la carpeta del modelo Live2D para que el modelo sea detectado por la extensión. En este ejemplo, la carpeta del modelo live2d `shizuku` podría verse así:

    ![Ejemplo de carpeta de modelo Live2D](/static/extensions/live2d-model.png)

    - Nota: Los modelos también pueden colocarse en carpetas específicas del personaje, como `/data/<user-handle>/characters/Shizuku/live2d/`. Sin embargo, los modelos en carpetas de personaje solo serán accesibles para ese personaje específico.

## Configuración de extensión

La extensión Live2D ofrece varias configuraciones para personalizar el comportamiento de tu modelo animado. Aquí hay las configuraciones clave:

![Configuración global de interfaz](/static/extensions/live2d-global.png)

### Configuración global

1. **Habilitado**:
   - Habilita esta casilla para activar la extensión, permitiendo que tu modelo Live2D interactúe dentro de SillyTavern.
   - Puedes desactivar la extensión si deseas usar solo sprites normales.
   - Puedes desactivar la extensión cuando desees mover sprites normales en un chat grupal y habilitarla nuevamente cuando estés listo para usar modelos Live2D.

2. **Seguir cursor**:
   - Habilita esta casilla para que el modelo Live2D siga tu cursor, siempre que el modelo admita esta función.

3. **Auto-enviar interacción**:
   - Habilita esta casilla para activar automáticamente las interacciones de personaje cuando hagas clic en áreas con mensajes asignados (consulta la sección de áreas de golpe para obtener detalles).


## Configuración de depuración

Estas configuraciones te ayudan a controlar el comportamiento y la visibilidad de tu modelo Live2D con fines de depuración.

1. **Reiniciar modelo antes de animación**:
   - Habilita esta casilla para recargar el modelo antes de cualquier animación. Esto fuerza que la animación comience y te permite spamear clics si es necesario. Algunos modelos pueden requerir esto para asegurar que las animaciones comiencen desde un estado compatible.

2. **Mostrar marcos de modelo**:
   - Habilita esta casilla para mostrar el marco del modelo, facilitando la identificación de dónde hacer clic para arrastrar el modelo. También muestra el área de golpe, si está disponible. Pasar el ratón sobre un área de golpe mostrará su nombre.

3. **Botón Recargar**
    - Haz clic en este botón para recargar todos los modelos live2d. Úsalo en caso de que algo falle.

## Selección de personaje

Estas configuraciones te permiten gestionar personajes y asignar modelos Live2D a ellos.

1. **Botón Actualizar**:
   - Haz clic en el botón actualizar para actualizar la lista de personajes en el chat actual.

2. **Seleccionar personaje**:
   - Usa la lista desplegable para elegir un personaje al que asignar un modelo Live2D.

3. **Botón Eliminar**:
   - Haz clic en este botón para eliminar todos los modelos asignados para un personaje. Aparecerá un mensaje de confirmación para confirmar la eliminación.

## Selección de modelo

![Lista de interfaz de modelo](/static/extensions/live2d-list.png)

1. **Botón Actualizar**:
   - Haz clic en el botón actualizar si tu modelo Live2D no aparece en la lista.

2. **Seleccionar modelo**:
   - Elige un modelo de la lista para asignarlo al personaje seleccionado.
   - El modelo puede ubicarse en la carpeta de activos o en la carpeta del personaje actual.
   - La lista muestra el nombre de la carpeta del modelo, su origen (activo o personaje) y el nombre del archivo de configuración del modelo detectado.
   - Ten en cuenta que algunas carpetas de modelo pueden contener diferentes versiones del mismo modelo. Puedes probar diferentes archivos de modelo para ver cuál funciona mejor.
   - Seleccionar ninguno usará sprites normales si los hay
   - La configuración se guarda por personaje y modelo

## Configuración de modelo

![Configuración de interfaz de modelo](/static/extensions/live2d-settings.png)

1. **Escala de modelo**:
   - Usa el deslizador para ajustar el tamaño del modelo, haciéndolo más grande o más pequeño.

2. **Desplazamiento del centro X del modelo**:
   - Usa el deslizador para cambiar la posición horizontal del modelo relativa al centro de la ventana.

3. **Desplazamiento del centro Y del modelo**:
   - Usa el deslizador para ajustar la posición vertical del modelo relativa al centro de la ventana.


### Observaciones
- La configuración se guarda y se mantiene en diferentes chats.
- También puedes arrastrar el modelo con tu ratón, y esa configuración se actualizará y guardará.
- Usa estas configuraciones de interfaz para traer tu modelo de vuelta a la pantalla si de alguna manera lo sacaste de vista. También, marca la casilla "Mostrar marco" para ver claramente dónde puedes hacer clic para arrastrar el modelo.

## Habla de modelo

![Interfaz de habla de modelo](/static/extensions/live2d-talk.png)

1. **ID de param de boca abierta Y**
    - Selecciona de la lista el ID del parámetro correspondiente al valor Y de la boca del modelo. No todos los modelos tienen uno, y los nombres pueden variar de modelo a modelo. Generalmente algo como "PARAM_MOUTH_OPEN_Y" o "ParamMouthOpenY". Verifica el modelo al seleccionar un elemento de la lista; intentará ejecutar la animación de habla. Si la boca se mueve, ¡lo tienes!

2. **Velocidad de movimiento de boca**
    - Ajusta el deslizador para cambiar la velocidad de movimiento de la animación de boca.

3. **Tiempo por carácter**
    - Establece la duración del tiempo de cada carácter. La duración de la animación de habla será este tiempo multiplicado por el número de caracteres del mensaje.

### Observaciones
- Esta animación de boca no funciona en todos los modelos y en todas las animaciones. Incluso si tu modelo tiene animaciones donde la boca se mueve, no significa que la animación de boca pueda ser controlada por esta extensión. Si no hay nada en la lista de parámetros, tu modelo probablemente fue hecho con una versión demasiado antigua de Live2D para acceder a los parámetros correctamente.

## Animaciones de modelo

![Interfaz de animaciones de modelo](/static/extensions/live2d-animations.png)

1. **Animación de inicio**
    - Selecciona una expresión y movimiento de las listas que se reproducirán cuando comiences un chat con el personaje. También puedes agregar un retraso durante el cual el modelo será invisible si necesitas ocultar el personaje durante un tiempo para lograr un efecto perfecto.

2. **Animación predeterminada**
    - Selecciona una expresión y movimiento de la lista que se reproducirá cuando el personaje envíe un mensaje. Usa una animación alternativa cuando uses la extensión de clasificación de expresión.

### Observaciones
- Las animaciones se reproducirán cuando selecciones una en las listas.
- Usa el botón de reproducción para reproducir la animación seleccionada.
- Algunos modelos tienen expresiones definidas como movimientos.
- Si no hay nada en las listas, es probable que el archivo de configuración de tu modelo no tenga expresiones/movimientos definidos.

## Mapeo de áreas de golpe

![Interfaz de mapeo de modelo](/static/extensions/live2d-mapping.png)

1. **Animación de clic predeterminada**
    - Selecciona una expresión y movimiento de la lista que se reproducirá cuando hagas clic en el modelo. También puedes establecer un mensaje que se enviará como un mensaje de usuario.

2. **Áreas de golpe**
    - Si el modelo tiene áreas de golpe, se enumerarán y podrás asignar una animación/mensaje a cada una.


### Observaciones
- Algunos modelos no tienen áreas de golpe, pero el clic predeterminado se detecta para todos.
- El clic predeterminado se activará si haces clic en un área de golpe sin nada asignado o si haces clic fuera de cualquier área de golpe.
- Las áreas de golpe tienen prioridad definida en el modelo; por ejemplo, "boca" está dentro de "cabeza." Si no se comporta adecuadamente, puede deberse al archivo de modelo.
- Para algunos modelos, las animaciones deben terminarse antes de comenzar otra. Usa la casilla de depuración si deseas forzar la actualización y spamear animaciones.

## Mapeo de expresiones clasificadas

![Interfaz de clasificación de modelo](/static/extensions/live2d-classify.png)

1. **Requisitos**
    - Requiere el uso de la extensión de clasificación de expresión; de lo contrario, volverá a la animación predeterminada.

2. **Mapeo**
    - Para cada emoción detectada por la extensión de clasificación, puedes asignar una animación de expresión/movimiento.

### Observaciones
- Si la animación anterior no terminó cuando se recibió un nuevo mensaje, es posible que la nueva animación no se reproduzca. Este comportamiento depende del modelo Live2D. Usa la casilla de depuración si deseas forzar la reproducción de la animación.

¡Gracias por seguir esta guía! Tu experiencia de SillyTavern está ahora enriquecida con modelos Live2D animados e interactivos.
