---
tags: ['>=1.13.0']
icon: people
route: /usage/welcome-assistants/
---

# Asistentes de la Página de Bienvenida

SillyTavern cuenta con una pantalla de bienvenida que puede saludarte con un carácter "Asistente" designado. Esta pantalla aparece cuando lanzas SillyTavern sin un chat activo o después de cerrar tu última sesión de chat.

!!! Nota
Si no ves una pantalla de bienvenida al iniciar la aplicación, asegúrate de que la opción "Auto-Load Last Chat" (Cargar automáticamente último chat) esté deshabilitada en la sección "Chat/Message Handling" (Manejo de chat/mensajes) del panel **<i class="fa-solid fa-user-cog"></i> User Settings** (Configuración de usuario). Si esta opción está habilitada, SillyTavern cargará automáticamente tu último chat en lugar de mostrar la pantalla de bienvenida.
!!!

## La Pantalla de Bienvenida

Cuando no hay un chat activo, la pantalla de bienvenida proporciona varios elementos útiles:

* **Versión de SillyTavern:** Muestra el logo de la aplicación y la versión actual.
* **Enlaces rápidos:** Acceso fácil a:
  * **Docs:** Abre la documentación oficial de SillyTavern (¡ya estás aquí!).
  * **GitHub:** Te lleva al repositorio GitHub de SillyTavern (<https://github.com/SillyTavern/SillyTavern>).
  * **Discord:** Proporciona un enlace al servidor oficial de Discord de SillyTavern (<https://discord.gg/sillytavern>).
* **Botón de chat temporal:** Te permite iniciar rápidamente una nueva sesión de chat temporal con el asistente neutral predeterminado, que no se guardará en tu historial de chat a menos que lo guardes explícitamente.
* **Sección de chats recientes:** Lista tus conversaciones recientes para acceso rápido. Puedes:
  * Mostrar u ocultar esta sección.
  * Expandir la lista si hay más de 3 chats disponibles (hasta 15 chats recientes).

## Chat Temporal

!!! Nota
Debido a una limitación técnica, la función de chat temporal no utilizará tu Asistente de página de bienvenida personalizado. Siempre iniciará un chat vacío sin indicaciones adicionales ni información de caracteres.
!!!

El botón de chat temporal te permite iniciar rápidamente una nueva sesión de chat sin guardarla en tu historial de chat. Esto es útil para pruebas o conversaciones casuales sin saturar tus chats guardados. Este chat se eliminará tan pronto como lo cierres o cambies a otro chat.

* El botón **Save** (Guardar) te permitirá exportar el chat temporal como un archivo JSONL, que puedes importar más tarde.
* El botón **Load** (Cargar) te permitirá restaurar un archivo de chat temporal guardado previamente.

## ¿Qué es un Asistente de Página de Bienvenida?

Un Asistente de página de bienvenida es un carácter que eliges para que aparezca en la pantalla de bienvenida. Esto permite un saludo personalizado y una forma rápida de iniciar un chat con un carácter familiar desde el principio.

### Asignando y Desasignando un Asistente

Puedes elegir cualquiera de tus caracteres para que actúe como tu Asistente de página de bienvenida.

**Para asignar un asistente:**

1. Navega al panel de **Character Management** (Gestión de caracteres) (usualmente se encuentra en la barra lateral derecha mediante el icono <i class="fa-solid fa-address-card"></i>).
2. Encuentra el carácter que deseas asignar como tu asistente en la lista.
3. Haz clic en "More..." (Más...) y selecciona **"Set / Unset as Welcome Page Assistant"** (Asignar/desasignar como Asistente de página de bienvenida) del menú desplegable.
4. Un pequeño icono (<i class="fa-solid fa-user-graduate"></i>) aparecerá junto al nombre del carácter, indicando que ahora es tu Asistente de página de bienvenida activo.

**Para desasignar un asistente:**

1. Ve al panel de Character Management (Gestión de caracteres).
2. Localiza tu Asistente de página de bienvenida actual (tendrán el icono <i class="fa-solid fa-user-graduate"></i>).
3. Haz clic en "More..." (Más...) y selecciona **"Set / Unset as Welcome Page Assistant"** (Asignar/desasignar como Asistente de página de bienvenida) nuevamente.
4. El carácter ya no será tu asistente, y el icono <i class="fa-solid fa-user-graduate"></i> desaparecerá.
5. SillyTavern volverá a usar el Asistente predeterminado (ver abajo).

### Interactuando con el Asistente

Una vez que se muestre la pantalla de bienvenida con tu asistente elegido, simplemente escribe tu mensaje en la barra de entrada de chat en la parte inferior de la pantalla y presiona Enter o haz clic en el botón enviar. Esto iniciará una nueva sesión de chat con tu Asistente de página de bienvenida.

Para abrir un chat anterior con el asistente, usa la sección de chats recientes o encuentra el chat en el diálogo **Manage chat files** (Gestionar archivos de chat) (accesible a través del menú **<i class="fa-solid fa-bars"></i> Options** (Opciones)).

## Asistente Predeterminado

SillyTavern creará automáticamente un carácter predeterminado llamado "Assistant" (Asistente) cuando interactúes con la pantalla de bienvenida por primera vez. Este carácter sirve como opción de retroceso si no has asignado un carácter específico como tu Asistente de página de bienvenida.

El asistente predeterminado no tiene indicaciones específicas adjuntas, y eres libre de personalizarlo como desees (p. ej., cambiar su nombre, añadir una imagen o establecer una personalidad).

* Si no has asignado explícitamente un carácter como tu Asistente de página de bienvenida, se utilizará este asistente predeterminado.
* Si desasignas tu asistente elegido, el sistema volverá a este asistente predeterminado.
* Si se elimina un carácter que habías asignado como asistente, el sistema también volverá al asistente predeterminado.

**Nota:** No puedes "desasignar" el asistente del sistema predeterminado de la misma manera que desasignas un carácter que has elegido. Para cambiar desde el asistente predeterminado, debes asignar uno de tus otros caracteres como asistente.
