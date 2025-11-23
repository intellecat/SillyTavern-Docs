---
route: /extensions/speech-recognition/
---

# Speech Recognition

Esta guía te guiará a través de la configuración del reconocimiento de voz para transcribir tu voz en texto dentro de SillyTavern.

## Requisitos previos

Antes de comenzar, asegúrate de haber cumplido con los siguientes requisitos previos:

- Asegúrate de estar en la última versión de SillyTavern.
- Instala la extensión "Speech Recognition" desde el menú "Download Extensions & Assets" en el panel de Extensiones (icono de bloques apilados).
- Ten instalado el binario ffmpeg. Consulta [RVC setup](RVC.md#rvc-setup) para más detalles.

## Speech Recognition Setup (Browser)

1. **Configurar SillyTavern**:
   - Inicia SillyTavern y ve a **Extensiones** > **Speech Recognition**.
   - Selecciona "Browser" de las opciones desplegables.
   - Si tu navegador no admite reconocimiento de voz, aparecerá una ventana emergente de error.

2. **Seleccionar modo de mensaje**:
   - Elige el "Modo de mensaje" que deseas:
     - **Append**: Tu mensaje se añadirá al área de texto del mensaje de usuario actual.
     - **Replace**: Tu mensaje reemplazará el mensaje de usuario actual en el área de texto.
     - **Auto send**: Tu mensaje se enviará automáticamente una vez se detecte el final del habla.

3. **Habilitar mapeo de mensajes** *(Opcional)*:
   - Configura el mapeo de frases para atajos de voz.
   - Por ejemplo, al agregar "command delete = /del2", el comando "/del2" reemplazará tu mensaje de voz cuando se detecte "command delete".
   - Útil cuando se combina con el modo de envío automático para control de voz completo. Habilita esto marcando "Enable messages mapping".

4. **Seleccionar idioma**:
   - Elige el idioma en el que deseas hablar (Nota: no todos los navegadores admiten todos los idiomas).

5. **Grabación**:
   - Para comenzar a grabar, haz clic en el botón de micrófono a la derecha del área de mensaje junto al botón de envío. Haz clic de nuevo para dejar de grabar. La grabación puede detenerse automáticamente si no se detecta voz.

## Speech Recognition Setup (Whisper/Vosk)

1. **Habilitar proveedor**:
   - Habilita el proveedor de reconocimiento de voz deseado en el servidor extras usando el siguiente comando:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     o
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - También puedes usar un modelo personalizado agregando la opción `--stt-vosk-model-path` o `--stt-whisper-model-path` con la ruta al modelo.

2. **Configurar SillyTavern**:
   - Inicia SillyTavern y ve a **Extensiones** > **Speech Recognition**.
   - Selecciona "Vosk" o "Whisper" de las opciones desplegables (whisper es más preciso).
   - La configuración es similar a la configuración del proveedor "Browser" (excepto por el idioma) ver arriba.

## Speech Recognition Setup (Streaming)

1. **Habilitar proveedor**:
   - Habilita el módulo de reconocimiento de voz por streaming en Sillytavern-extras con el siguiente comando:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **Configurar SillyTavern**:
   - (Opcional) Especifica un modelo Whisper personalizado como en la configuración de Whisper anterior.
   - (Opcional pero recomendado) Configura palabras de activación en SillyTavern. Solo los mensajes que comienzan con estas palabras de activación se enviarán a SillyTavern como mensajes reales. Esto evita que el habla aleatoria o el ruido se transcriban. Habilita esto con la casilla de verificación. Las palabras de activación se pueden incluir/excluir del mensaje real usando una casilla de verificación.
   - Otros ajustes son similares a otros proveedores.

Ya estás listo para transcribir tu voz en texto usando reconocimiento de voz en SillyTavern.
