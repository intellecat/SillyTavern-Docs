---
route: /extensions/dynamic-audio/
---

# Audio Dinámico

Esta guía te ayudará a configurar y personalizar recursos de audio dinámico para tu experiencia en SillyTavern.

## Requisitos previos

Antes de comenzar, asegúrate de cumplir los siguientes requisitos previos:

- Asegúrate de que estés en la versión más reciente de SillyTavern.
- Instala la extensión "Dynamic Audio" desde el menú "Download Extensions & Assets" en el panel Extensions (icono de bloques apilados).

## Configuración de Audio Dinámico (Navegador)

1. **Conectarse al Repositorio de Recursos**:
   - Lanza SillyTavern y navega a **Extensions** > **Assets**.
   - Haz clic en el botón "Connect" para establecer una conexión con el repositorio de recursos oficial.
   - Descarga los recursos de audio deseados, como música de fondo (BGM) o sonidos ambientes, que correspondan con los fondos que tengas la intención de usar.

2. **Habilitar la Extensión de Audio Dinámico**:
   - En SillyTavern, ve a **Extensions** > **Dynamic Audio**.
   - Habilita la extensión, desactiva el silencio y ajusta el volumen del BGM y los sonidos ambientes a tu preferencia.
   - Cuando el BGM termine, otro se reproducirá aleatoriamente. Haz clic en el botón de bucle para mantener el BGM actual reproduciéndose.
   - Haz clic en el botón de tirada para seleccionar otro BGM aleatoriamente.

3. **BGM Basado en Expresión**:
   - Habilita el interruptor de BGM de expresión si deseas que el BGM siga la expresión del personaje (requiere BGM en la carpeta del personaje, ver a continuación).
   - Ajusta el temporizador de enfriamiento (en segundos) entre actualizaciones de BGM. Auméntalo si encuentras que el BGM cambia con demasiada frecuencia en chats de grupo o cuando usas BGM específico de personaje con detección de emociones.

## Importar Música para Personajes

Para configurar música personalizada para las emociones de tus personajes, sigue estos pasos:

1. **Navega a la Carpeta de Personajes**:
   - Ve a la carpeta de personajes, p. ej., `\SillyTavern\data\<user-handle>\characters\Seraphina`.

2. **Crear Carpeta BGM**:
   - Dentro de la carpeta del personaje, crea una subcarpeta llamada `bgm`.

3. **Importar Música de Emoción**:
   - Dentro de la carpeta `bgm`, importa los archivos de música para cada emoción. Las extensiones de audio compatibles incluyen `.mp3`, `.ogg` y `.wav`.
   - Convención de nombres: `[emotion]_[number].mp3`, p. ej., `anger_0.mp3`, `joy_0.mp3`.

4. **Pistas Múltiples para Emociones**:
   - Puedes importar múltiples pistas para la misma emoción incrementando el número, p. ej., `neutral_1.mp3`, `neutral_2.mp3`.

5. **Selección de Música Predeterminada**:
   - Cuando no se detecta emoción, se reproducirá una pista neutra aleatoria como predeterminada. Las emociones se detectan de manera similar a la actualización de sprites; consulta la [documentación de imágenes de expresión](/extensions/Expression-Images.md) para más detalles.

## Cambiar Música BGM Predeterminada

Si un personaje no tiene BGM personalizado en su carpeta, se reproducirá una pista predeterminada. Así es como puedes cambiarla:

1. **Navega a la Carpeta BGM**:
   - Ve a la siguiente carpeta: `\SillyTavern\data\<user-handle>\assets\bgm`.

2. **Reemplazar/Añadir Música**:
   - Reemplaza o añade archivos de música (`.mp3`, `.ogg`, `.wav`) a esta carpeta.
   - Estos son los recursos de audio oficiales descargados utilizando la extensión de recursos.
   - Una de estas pistas se reproducirá aleatoriamente cuando no se encuentre BGM específico del personaje (chat solo o grupal).

## Cambiar Sonidos Ambientes

Los sonidos ambientes añaden profundidad a tus escenas. Así es como puedes personalizarlos:

1. **Navega a la Carpeta Ambient**:
   - Ve a la siguiente carpeta: `\SillyTavern\data\<user-handle>\assets\ambient`.

2. **Convención de Nombres de Archivos**:
   - Los nombres de archivos de audio ambiental corresponden a los nombres de archivos de imagen de fondo, reemplazando espacios con guiones.
   - Ejemplo: `"bedroom-clean.mp3"` corresponde al fondo "bedroom clean.jpg".
   - Si el botón de bloqueo está desbloqueado, se reproducirá el archivo de audio correspondiente al fondo. Activar el bloqueo mantendrá el ambiente actual reproduciéndose.

3. **Ambientes Personalizados**:
   - Puedes añadir tus propios sonidos ambientales para fondos personalizados o existentes siguiendo el mismo patrón de nombres.

¡Gracias por seguir esta guía! Tu experiencia en SillyTavern ahora está enriquecida con audio dinámico.
