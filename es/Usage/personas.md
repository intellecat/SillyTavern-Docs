---
order: 110
icon: smiley
route: /usage/core-concepts/personas/
templating: false
---

# Personas

## ¿Qué es una Persona?

Una persona en SillyTavern es la identidad que usas para participar en chats — esencialmente una combinación de tu nombre de pantalla, avatar y texto descriptivo opcional. Las personas te permiten cambiar fácilmente de roles o "personajes" con los que hablas, sin tener que actualizar manualmente tu nombre de usuario/avatar cada vez.

!!!
**Nota:** Los avatares/nombres de usuario heredados que no estaban vinculados a una persona han sido eliminados. Los datos existentes se migrarán a personas. Si no se especificaba un nombre, la persona se llamará "[Persona sin nombre]".
!!!

## ¿Cómo crear una Persona?

1. Abre el panel **Persona Management** (botón <i class="fa-solid fa-face-smile"></i> en el menú superior).
2. Crea una persona en blanco con el botón **Create** y dale un nombre.
3. En la lista de personas, selecciona la persona recién creada.
4. En el lado derecho, puedes rellenar tu descripción y establecer un avatar a través del botón "Change Persona Image". Ambos son opcionales.
5. Ahora tu persona está lista para usar en chats.

### Convertir un Personaje a Persona

Las personas también se pueden crear convirtiendo cualquier personaje existente. Simplemente abre el personaje, selecciona "More..." y haz clic en "Convert to Persona". Se creará una persona con el mismo nombre y descripción. Otros campos de la tarjeta de personaje como Scenario o Personality no se usarán. El personaje no será eliminado.

!!! Note
Como los macros `{{user}}` y `{{char}}` tienen significados opuestos cuando se usan en descripciones de Persona y Personaje, se te pedirá que los cambies si la descripción convertida contiene alguno de ellos.
!!!

## Descripción de la Persona

Cada persona puede almacenar una descripción de texto personalizado — características mentales y físicas, edad, ocupación o cualquier detalle personal. Estos también pueden incluir macros de plantilla como `{{char}}` o `{{user}}` (ver [Macros](/Usage/Characters/macros.md)).

Dónde se inyecta la descripción de tu persona en el prompt de IA depende de la configuración **Position** en el panel Persona Management:

- **None (disabled)**
- **In Story String / Prompt Manager** (la predeterminada)
- **Top of Author's Note** / **Bottom of Author's Note** (solo se agregará cuando exista una Author's Note)
- **In Chat @ Depth** (esto abrirá opciones de configuración para establecer profundidad y rol)

La posición se guarda **por persona**.

## Título de la Persona

El título es un campo de texto opcional que se puede usar para almacenar información adicional sobre la persona y no se usa en el prompt, pero se muestra en el panel Persona Management.

Para establecer un título, haz clic en el botón **<i class="fa-solid fa-pencil"></i> Rename Persona** en el panel Persona Management e ingresa el título en el campo "Persona Title", o especifícalo durante la creación de la persona. Establecer un valor vacío cuando el título ya existe lo eliminará.

## Conexiones de Persona / Bloqueo

Las conexiones de persona aseguran que una persona determinada se seleccione automáticamente en ciertas situaciones. Si no hay ninguna persona conectada, la persona elegida actualmente permanecerá seleccionada.

Hay tres tipos de bloqueo:

1. **<i class="fa-solid fa-unlock"></i> Chat lock** – La persona está bloqueada al chat actual.
2. **<i class="fa-solid fa-unlock"></i> Character lock** – La persona está bloqueada a un personaje específico.
3. **<i class="fa-solid fa-crown"></i> Default persona** – Una persona que se usa siempre que no se apliquen otros bloqueos.

### 1. Bloquear a un Chat

Si una persona está bloqueada a un chat, abrir ese chat en el futuro cambiará automáticamente tu persona activa a la bloqueada.

- **Para bloquear**: Selecciona la persona deseada, luego haz clic en el botón **<i class="fa-solid fa-unlock"></i> Chat** bajo la sección "Connections" (o usa `/persona-lock type=chat on`).
- **Para desbloquear**: Haz clic en el botón de nuevo (o usa `/persona-lock type=chat off`).

### 2. Bloquear a un Personaje

También puedes vincular una persona a un personaje específico. Abrir cualquier chat con ese personaje selecciona automáticamente tu persona bloqueada.

- **Para bloquear**: Selecciona la persona deseada, luego haz clic en el botón **<i class="fa-solid fa-unlock"></i> Character** bajo la sección "Connections" (o usa `/persona-lock type=character on`).
- **Para desbloquear**: Haz clic en el botón de nuevo (o usa `/persona-lock type=character off`).

El panel Persona Management también muestra qué personajes están vinculados a esa persona (mostrados como pequeños avatares). Al hacer clic en ellos, navegas directamente al chat de ese personaje.

#### Bloquear Múltiples Personas al Mismo Personaje

Si otra persona ya estaba vinculada con ese personaje, se desvincularà automáticamente de forma predeterminada.

Para tener múltiples personas vinculadas a la vez, se puede usar la configuración global **Allow multiple persona connections per character**.
Si múltiples personas están vinculadas al mismo personaje, verás un popup pidiendo qué persona usar cada vez que abras o inicies un nuevo chat con ese personaje (a menos que una persona esté vinculada al chat).

### 3. Persona Predeterminada

Tu **persona predeterminada** se usa siempre que no haya otro bloqueo relevante. La persona predeterminada se reconoce por un borde amarillo alrededor de su avatar.

- **Para establecer/desestablecer predeterminado**: Selecciona la persona deseada, luego haz clic en el botón **<i class="fa-solid fa-crown"></i> Default** bajo la sección "Connections" (o usa `/persona-lock type=default`).

Solo una persona puede ser elegida como la persona predeterminada.

### Persona Temporal

Si cualquiera de las tres opciones de conexión conecta una persona al personaje/chat actual, aún puedes elegir usar una persona diferente. Esta persona se marcará en el panel de personas como "Temporary Persona". Cualquier recarga de la ventana del navegador o cambio a un chat diferente y de vuelta lo restablecerá a la persona vinculada nuevamente.

Puedes *convertir* manualmente una Persona Temporal para que esté conectada de forma persistente vinculándola al chat.

## Configuración Global de Personas

Todas las configuraciones bajo **Current Persona** se guardan por persona. Existen algunas configuraciones globales también, que se pueden encontrar bajo **Global Persona Settings** en el panel Persona Management.

1. **Show notifications on switching personas**
   - Habilita mensajes de notificación relacionados con personas (p. ej., "Persona Auto Selected", "Temporary Persona").

2. **Allow multiple persona connections per character**
   - Cuando se **habilita**, puedes vincular múltiples personas a un único personaje. Abrir el chat de ese personaje te pedirá qué persona usar. Si se deshabilita, solo una persona puede estar conectada a un personaje a la vez.

3. **Auto-lock a chosen persona to the chat**
   - Cuando se **habilita**, cada vez que seleccionas una persona (manual o por auto-selección) o creas un nuevo chat, bloquea esa persona al chat.
   Esto combinado con "Allow multiple" proporciona la opción de tener una selección de persona por personaje, pero mantenerla vinculada una vez elegida para un chat.

## Comandos Slash para Personas

### `/persona-lock type=<type?>`

- `chat` bloquea la persona actual a tu chat activo.
- `character` bloquea la persona actual al personaje en uso.
- `none` (o sin argumento) desbloquea/limpia el bloqueo de persona para el contexto actual.
- Si se usa sin argumentos, devuelve el estado de bloqueo actual (o un error si no se establece ninguno).
- El estado de bloqueo se puede elegir a través de `on`, `off` o `toggle`. El predeterminado es toggle.

### `/persona <name>`

- Cambia rápidamente tu persona activa por nombre sin abrir el panel Persona Management.
- Ejemplo: `/persona Blaze`.
- Usar `mode=temp` permite establecer temporalmente el nombre de tu persona **actual**, aunque una persona con el mismo nombre ya pueda existir (preservando tu avatar y descripción actuales).

### `/persona-sync`

- Re-atribuye todos los mensajes del usuario en el chat activo a la persona **actual** y su nombre.

> **Nota:** Los comandos más antiguos `/lock` y `/unlock` permanecen por compatibilidad hacia atrás pero pueden eliminarse en el futuro. Usa `/persona-lock` en su lugar.

## Consejos Profesionales

1. **Cambiar personas a mitad del chat** no re-atribuye tus mensajes de usuario anteriores a la nueva persona; estos permanecen atribuidos a la persona que estabas usando en ese momento.
2. **Re-atribución en lote**: Si alguna vez necesitas que todos los mensajes anteriores coincidan con una nueva persona, presiona el botón **sync** o usa `/persona-sync`.
3. **Reemplazar imágenes de persona** sin perder descripción o bloqueos eligiendo tu persona y haciendo clic en el botón **<i class="fa-solid fa-images"></i> Change Persona Image**.
4. **Popups de vinculación de personaje**: Si múltiples personas están vinculadas al mismo personaje, obtendrás un popup para elegir qué persona cada vez que abras el chat. Esta es una forma práctica de tener una pequeña selección de personas para elegir entre personajes específicos.
5. **Copias de seguridad**: Puedes hacer una copia de seguridad de toda tu lista de personas (nombres, conexiones de personaje, descripciones) con el botón **Backup** en Persona Management, y restaurarla más tarde si es necesario.

!!!tip Observaciones sobre Copias de Seguridad

- Las imágenes y conexiones de chat no se guardan junto con las personas y no se realizará una copia de seguridad de ellas mediante esto.
- Estas copias de seguridad no están diseñadas para ser compartidas, ya que contienen enlaces internos.

!!!
