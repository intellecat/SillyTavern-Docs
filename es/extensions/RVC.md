---
route: /extensions/rvc/
---

# Conversión de Voz Basada en Recuperación (RVC)

Esta guía te mostrará cómo usar RVC, una técnica que permite transferir características de voz de un clip de audio a otro, permitiendo que las voces hablen en diferentes tonos y estilos.

¿Alguna vez has disfrutado de esos famosos vídeos de "Presidents Play X"? Fueron creados usando RVC. Con la extensión RVC, puedes hacer que tus personajes de SillyTavern hablen con cualquier voz que desees, ya sea anime, película, o incluso tu propia voz única.

RVC NO es TTS: es más como conversión de voz a voz (speech-to-speech). Toma un clip de audio como entrada. En segundo plano, lo que RVC hace es trabajar en conjunto con la extensión TTS de SillyTavern: espera a que TTS genere un archivo de audio (que TTS habría hecho independientemente de si usas RVC o no), luego RVC realiza un segundo pase que toma el archivo de audio TTS y lo transforma en la voz clonada de tu configuración de RVC.

## Configuración de RVC

El RVC de SillyTavern admite varias fuentes de API que realizan conversión de audio:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (deprecated)

### Requisitos previos comunes

Antes de comenzar, asegúrate de haber cumplido los siguientes requisitos previos.

#### ffmpeg

Asegúrate de tener el binario `ffmpeg` en tu variable de entorno PATH. Esta herramienta se usa para convertir audio entrante.

**Windows**:

* Usa el Toolbox en el script de SillyTavern Launcher para instalar ffmpeg automáticamente: <https://github.com/SillyTavern/SillyTavern-Launcher>
* O descarga la compilación aquí: <https://www.gyan.dev/ffmpeg/builds/>
* Cómo modificar la variable PATH: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* Para verificar si hiciste todo correctamente, abre el símbolo del sistema y ejecuta ```ffmpeg```. Debería imprimir la versión de ffmpeg e información.

**Linux**:

Instala ffmpeg usando tu gestor de paquetes.

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

Instala ffmpeg usando [Homebrew](https://brew.sh/):

```shell
brew install ffmpeg
```

#### Asegúrate de que TTS esté habilitado y funcione

RVC depende de TTS, necesitas habilitar una extensión TTS. ¡Tu TTS tiene que estar ya funcionando correctamente y narrando tus chats antes de que intentes agregar RVC!

Ten en cuenta que:

* El motor TTS del sistema no admite conversión de voz en absoluto.
* Streaming TTS esperará a que el flujo de audio termine antes de la conversión.

#### Instalar la extensión

Instala la extensión "RVC" desde el menú "Download Extensions & Assets" en el panel Extensions (icono de bloques apilados).

#### Habilitar RVC en SillyTavern*

En SillyTavern, navega a **Extensions** > **RVC** y habilítalo.

#### Seleccionar la fuente

En la configuración de la extensión, elige una fuente RVC para usar. Luego procede a las instrucciones de instalación específicas de la fuente.

### Configuración de rvc-python

#### 1. Instalar el paquete

Sigue las instrucciones de instalación desde la página de GitHub: [rvc-python Installation](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation). Se recomienda seguir las instrucciones de instalación de CUDA si tienes una GPU de Nvidia.

Si experimentas problemas al instalar en Windows (por ejemplo, el paso de compilación de fairseq falla), asegúrate de que el siguiente software esté instalado en tu PC:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. Preparar los modelos

Crea un directorio para almacenar modelos RVC. Por defecto se llama `rvc_models` y se recoge desde tu directorio actual al iniciar el servidor. Cada modelo es una subcarpeta (su nombre será visible en la UI) que debe contener archivos `.pth` (requerido) e `.index` (opcional).

Lee más: [rvc-python Model Management](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. Iniciar el servidor API

Inicia el servidor API ejecutando el siguiente comando:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**Argumentos:**

* `5050` - establece un puerto de escucha para el servidor. Cambia si deseas alojar en un puerto diferente.
* `models_path` - establece una ruta para modelos. Elimina si deseas usar el directorio predeterminado `rvc_models`.
* `-l` - configura el servidor para escuchar en todas las interfaces de red. Elimina para escuchar solo en localhost.

#### 4. Conectar al servidor

* En la configuración de la extensión RVC, establece una **URL de API rvc-python** apropiada. Por defecto, será `http://localhost:5050`.
* Marca la casilla **Use CUDA** si has instalado rvc-python para admitir aceleración CUDA.
* Presiona "Refresh" para cargar una lista de voces disponibles.

#### 5. Configurar un mapa de voces

**El mapa de voces define la configuración de conversión de voz para cada personaje o persona de usuario.**

* Para configurar un mapa de voces, elige el nombre de tu personaje o persona del menú desplegable "Character", luego elige una "Voice" RVC, luego haz clic en Apply.
* Opcionalmente, también puedes configurar otros parámetros relacionados como corrección de tono o filtrado.
* Si hiciste todo correctamente, el área de depuración del Mapa de voces mostrará algo como 'Betty:MyVoice(rvpme)'.

### Configuración de SillyTavern Extras

#### 1. Preparar archivos del modelo RVC

* En un explorador de archivos, navega a: `\SillyTavern-extras\data\models\rvc`.
* Crea una subcarpeta como 'Betty' y coloca los archivos `.pth` e `.index` en ella. (Sugerencia: puedes descargar archivos de voz desde https://voice-models.com, asegúrate de que el nombre de la voz diga que es RVPME.)

#### 2. Instalar requisitos

Instala los requisitos necesarios usando el comando:

```shell
pip install -r requirements-rvc.txt`
```

#### 3. Ejecutar SillyTavern-extras con RVC habilitado

Inicia SillyTavern-extras con el módulo RVC habilitado. Esta invocación de ejemplo asume que usaste Edge TTS que viene preinstalado con SillyTavern-extras:

```shell
python server.py --enable-modules=rvc,edge-tts
```

Opcionalmente, es posible que desees ejecutar RVC en tu GPU si tienes una capaz, agregando ```--cuda``` al comando de inicio. Según una prueba rápida, el uso de VRAM fue de 3.4GB para narrar 50 tokens (~36 palabras) y 7.6GB para 200 tokens (~150 palabras).

#### 4. Configurar mapeo de voces

Crea un mapa de voces para RVC. Establece tu Character en el nombre de personaje de SillyTavern que desees, establece Voice en la carpeta RVC que creaste en el paso 1, luego haz clic en Apply. Si hiciste las cosas correctamente, el Mapa de voces mostrará algo como 'Betty:MyVoice(rvpme)'.

#### 5. Seleccionar extracción de tono

* Elige "rmvpe" como método de extracción de tono.
* Si tienes problemas con "rmvpe", prueba otros métodos (por ejemplo, "harvest" o "torchcrepe").

#### 6. (Opcional) Configurar RVC para guardar tus generaciones en un archivo

Si por propósitos de prueba o resolución de problemas deseas guardar el audio RVC generado, agrega ```--rvc-save-file``` a tu comando de inicio. Esto guardará la última generación en `SillyTavern-extras/data/tmp/rvc_output.wav`:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### Voz dinámica basada en expresión

##### 1. Configurar modelos RVC

En tu carpeta de modelo RVC, ten archivos `.pth` e `.index` separados para cada expresión clasificada (por ejemplo, anger, fear, joy, love, sadness, surprise).

##### 2. Habilitar módulos

Habilita ambos módulos RVC y classify:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. Usar módulo RVC

La configuración restante es similar a usar el módulo RVC solo (como se explicó arriba).

## Entrena tu propio modelo RVC

### Usando RVC Easy Menu por Deffcolony (Solo Windows)

Instala automáticamente e inicia Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

#### 1. Clonar repositorio

Clona el repositorio a tu ubicación deseada:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. Lanzar RVC-Launcher.bat

* Abre el archivo `RVC-Launcher.bat`.
* Elige la opción 1 para instalar RVC.

#### 3. Completar instalación

Cuando se te pida, instala los paquetes y dependencias requeridos.

#### 4. Abrir WebUI para entrenamiento de voz

Después de la instalación, elige la opción 2 para abrir la WebUI para el entrenamiento de voz.

### Mangio-RVC: Entrenar un modelo de voz

***Preparación del conjunto de datos***:

**1. Preparar audio**:

* Coloca el audio que deseas entrenar en la carpeta `datasets`.
* Asegúrate de que el audio esté libre de ruido de fondo – solo se necesita voz sin procesar.
* El audio más largo produce una mejor calidad de salida.

***Entrenamiento en WebUI***:

**1. Acceder a la pestaña de entrenamiento**:

* Haz clic en la pestaña de entrenamiento en la WebUI.

**2. Configurar experimento**:

* Ingresa un nombre de experimento (por ejemplo, `my-epic-voice-model`).
* Establece la versión en v2.

**3. Procesar datos y extraer características**:

* Haz clic en "Process data" y "Feature extraction".
* Establece "Save frequency" en 50.

**4. Parámetros de entrenamiento**:

* Establece "Total training epochs" en 300.
* Haz clic en "Train feature index" y "Train model".
