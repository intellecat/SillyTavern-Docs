---
route: /extensions/emulatorjs/
templating: false
---

# EmulatorJS

Esta extensión te permite jugar juegos de consolas retro directamente desde el chat de SillyTavern.

## Instalación

**Requisitos previos:**

- Última versión de lanzamiento de SillyTavern.
- Archivos ROM descargados de internet. Puedes encontrarlos [en cualquier lugar](https://archive.org/details/ni-romsets).

**Cómo instalar:**

1. Instala usando el descargador de extensiones de SillyTavern.
2. O usa este enlace: `https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## Uso

- Abre el menú de la extensión "EmulatorJS".
- Haz clic en "Agregar archivo ROM". Los ROMs se guardan en el almacenamiento de tu navegador y no se almacenan en un servidor.
- Selecciona el archivo del juego a añadir. Ingresa el nombre y núcleo (si no fue detectado automáticamente). Si el núcleo requiere un archivo BIOS, añádelo también.
- Haz clic en el botón "Reproducir" en la lista o inicia desde el menú de varita.
- Puedes personalizar los controles y otras configuraciones en la ventana del emulador después de iniciar el juego.
- Usa las funciones de guardar/cargar estado si necesitas un descanso.

Consulta la documentación de EmulatorJS para ver la lista de núcleos disponibles y sus requisitos: [Núcleos](https://emulatorjs.org/docs4devs/cores).

## Modo de comentarios

Con el poder de los modelos multimodales, tus bots de IA pueden ver tu juego y proporcionar comentarios ingeniosos en personaje.

### Requisitos

1. Un navegador que admita [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility). Probado en Chrome de escritorio. Firefox requiere habilitarlo con config. Safari no funcionará.
2. Se recomienda la API de Finalización de Chat con modo de inserción de imágenes. Consulta la documentación de la API para ver si el modelo elegido admite indicaciones multimodales.
3. Si la inserción de imágenes está deshabilitada, asegúrate de que la extensión [Subtítulos de Imagen](./captioning.md#multimodal-source) esté habilitada y luego selecciona la fuente de subtítulos "Multimodal".

### Cómo habilitar comentarios

1. Asegúrate de establecer el intervalo de proporcionar comentarios en la configuración de la extensión EmulatorJS. Esta configuración define con qué frecuencia se consulta al personaje para obtener comentarios usando una imagen de tu juego actual. Un valor de 0 indica que no se proporcionan comentarios.
2. Selecciona un chat de personaje e inicia el juego. Para obtener el mejor rendimiento, asegúrate de que el archivo ROM esté correctamente nombrado para que la IA pueda tener más contexto de fondo.
3. Comienza a jugar como normalmente lo harías. El modelo de visión será consultado periódicamente para escribir un comentario basado en la última captura de pantalla que "ve".

### Configuración

1. Plantilla de subtítulos - un aviso usado para describir la captura de pantalla del juego. Se admiten macros adicionales `{{game}}` y `{{core}}`.
2. Plantilla de comentarios - un aviso usado para escribir un comentario basado en el subtítulo generado. Se admiten macros adicionales `{{game}}`, `{{core}}`, `{{caption}}`. Para modo de inserción de imágenes, `{{caption}}` se reemplaza con `ver imagen incluida`.
3. Forzar subtítulos - forzará el uso de subtítulos multimodales incluso si la inserción de imágenes es compatible y está habilitada.

### ¿Por qué no veo ningún comentario?

Los comentarios se pausan temporalmente (se omite el paso de intervalo) si:

1. El emulador está pausado (con un botón de pausa, no en el juego).
2. La ventana del navegador está fuera de foco.
3. El área de entrada del usuario no está vacía. Esto es para permitirte escribir tu respuesta en paz.
4. Otra generación de respuesta está en progreso.
5. Se está leyendo en voz alta la voz TTS. El comentario se detiene (máximo 30 segundos) hasta que termine, pero no se omite.
6. Una tarjeta de personaje o grupo está actualmente abierta. El modo de comentario está deshabilitado al iniciar el juego desde una pantalla de bienvenida.

Otros problemas comunes:

1. Asegúrate de haber establecido un intervalo de comentarios antes de iniciar el juego.
2. Asegúrate de haber establecido una clave de API multimodal y no haya errores en la consola del servidor ST.

¿Aún no funciona? Envíanos los registros de la consola de depuración de tu navegador (presiona F12).

## Créditos

- EmulatorJS engine (GPLv3): <https://github.com/EmulatorJS/EmulatorJS>
