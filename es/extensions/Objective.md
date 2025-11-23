---
route: /es/extensions/objective/
---

# Objetivo

### ¿Qué es?

La extensión Objective permite al usuario especificar un Objetivo hacia el cual la IA debe esforzarse durante la conversación. Este objetivo se divide en tareas paso a paso. Las tareas pueden ramificarse, donde las tareas secundarias pueden crearse automática o manualmente. Esto permite crear árboles de tareas complejos. El estado de finalización de cada tarea en la lista se verificará en ciertos intervalos.

Esto difiere de agregar dirección estática a través de indicaciones en que agrega directivas secuenciales y a ritmo para que la IA siga sin intervención del usuario. Ofrece una experiencia más genuina de la IA esforzándose autónomamente por alcanzar un objetivo.

### Requisitos previos

Antes de comenzar, asegúrese de cumplir con los siguientes requisitos previos:

- Asegúrese de estar en la última versión de SillyTavern.
- Instale la extensión "Objective" desde el menú "Download Extensions & Assets" en el panel Extensions (icono de bloques apilados).

### Casos de uso comunes

Tu imaginación es el límite, puedes darle a la IA cualquier objetivo que desees y ella planificará cómo lograrlo. Puedes pedirle que planifique cómo matar a un demonio, robar un templo, organizar una fiesta lujosa o incluso tomar el control del mundo.

![Objective Settings Panel](/static/extensions/objective-panel.png)

### Configuración

- La extensión se encuentra en el menú Extensions bajo Objective.

- Escriba un objetivo en el cuadro de texto superior y luego haga clic en `Auto-Generate Tasks`. Esto envía una solicitud a la API conectada y le pide que proporcione una lista de tareas que coincidan con el objetivo que ha escrito.

*Nota: Al hacer clic en Auto-Generate Tasks, se eliminarán todas las tareas existentes del Objetivo seleccionado actualmente antes de agregar las nuevas.*

- Una vez recibida la respuesta de la IA, se creará automáticamente una lista de tareas en el espacio debajo del cuadro de entrada Objective. Las tareas se pueden editar después de su creación.

- En la parte inferior del panel hay dos cuadros: `Position in Chat` y `Task Check Frequency`
  - `Position in Chat` - Esta es la profundidad en la que desea que la tarea actual se inserte en la sección de chat del indicador. Cuanto menor sea el número, más atención le prestará la IA a la tarea. Si se establece en 0, la tarea será lo principal en la mente de la IA. Si se establece en valores altos, la tarea se colocará en segundo plano y permitirá que la IA se enfoque en la conversación en cuestión, pero si se establece demasiado alto, puede causar que la IA nunca 'llegue' a la tarea en absoluto.
  - `Task Check Frequency` - Esta es la frecuencia con la que desea que la IA verifique si la tarea se ha completado. Si se establece en `3`, se le preguntará a la IA si la tarea actual se ha completado cada 3er mensaje.

-  Los Objectives, las tareas y sus descripciones se guardan en tiempo real en la sesión de chat actual. Los indicadores personalizados se guardan globalmente.

### Indicaciones personalizadas

Puede personalizar los indicadores enviados a la LLM para generar tareas, verificar la finalización de tareas e inyectar indicadores. La edición de indicadores los guardará para la sesión actual. Los indicadores personalizados se pueden guardar y cargar para persistencia.

- Haga clic en Edit Prompts para abrir la ventana del editor de indicadores. Puede editar sus indicadores como desee.
- Para guardar indicadores, ingrese un nombre y haga clic en Save Prompt.
- Para cargar indicadores, seleccione el indicador de la lista desplegable.
- Para eliminar un indicador guardado, selecciónelo de la lista desplegable y haga clic en Delete Prompt

**ADVERTENCIA: La verificación de tareas ocurre en una solicitud API separada. Establecer Task Check Frequency en 1 duplicará sus llamadas API al servicio LLM. Tenga cuidado con esto si está utilizando un servicio de pago.**

### Uso

Por defecto, la extensión Objective mantiene un registro automático de todas las tareas y su estado de finalización respectivo.

El usuario también puede crear, actualizar, eliminar y completar manualmente tareas en cualquier momento.


#### Selección de tarea actual

La tarea actual siempre será la primera tarea incompleta listada. Cualquier actualización manual de tareas activará una verificación de cuál debe ser la tarea actual. Entonces, si agrega una tarea encima de un montón de tareas completadas, se establecerá como la tarea actual. Una vez que se complete, las tareas completadas previamente se omitirán y la siguiente tarea incompleta se seleccionará como 'Current'.

Cuando se utilizan tareas padre/hijo en un árbol de tareas, las tareas se seleccionan en profundidad, lo que significa que todos los niños Las tareas se seleccionarán en orden primero, luego continúe con la lista de tareas para el Objective/Task actual.

#### Tareas de rama

Haga clic en el botón Branch Task para establecer la tarea actual como un Objective donde puede generar automáticamente o crear manualmente tareas como tareas secundarias. Puede continuar convirtiendo cualquier tarea secundaria en un Objective y continuar generando a su gusto.

Marcar una tarea principal como completada hará que la extensión omita todas las subtareas. Cuando todas las tareas secundarias se completen, la tarea principal se marcará como completada.

#### Completar manualmente tareas

Puede cambiar manualmente el estado de finalización de una tarea haciendo `clic en la casilla de verificación` junto a ella. Esto establecerá la siguiente tarea incompleta a ser seleccionada.

#### Verificación manual de tareas

Si desea activar manualmente que la IA verifique la finalización de la tarea, haga clic en el botón Extras Extension (la `varita mágica` en el lado derecho de la barra de entrada de chat) y seleccione `Manual Task Check`.

![Manual Task Check](/static/extensions/task-check.png)

#### Agregar manualmente tareas

Cuando no hay tareas presentes, un botón `Add Task` es visible, lo que permite crear manualmente la primera tarea.

Si ya hay otras tareas presentes, haga clic en el botón `+` a la derecha de cualquier tarea para insertar una nueva tarea después de ella.

#### Eliminar tareas

Haga clic en la `x` roja para eliminar una tarea existente. La siguiente tarea incompleta se seleccionará como la tarea actual automáticamente.

Eliminar una tarea con tareas secundarias eliminará todas las tareas secundarias y sus descendientes.

#### Ocultar tareas

Si desea mantenerse ajeno a qué tareas intenta completar la IA, marque la casilla `Hide Tasks` para ocultar la lista de tareas y hacer que las intenciones de la IA sean un misterio. ¡Para 100% de misterio, haga esto antes de hacer clic en `Auto-Generate Tasks`!
