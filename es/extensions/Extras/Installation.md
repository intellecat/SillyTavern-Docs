---
icon: gear
label: Instalación Local
route: /extensions/extras/installation/
---

# Instalación de Extras

Esta página contiene instrucciones para instalar SillyTavern Extras en tu dispositivo local.

!!! Descontinuado
El proyecto Extras se descontinuó en abril de 2024 y no recibirá nuevas actualizaciones o módulos. La gran mayoría de los módulos están disponibles de forma nativa en la aplicación principal de SillyTavern. Aún puedes instalar y usar, pero no esperes obtener soporte inmediato si enfrentas problemas.
!!!

La instalación local de Extras puede ser difícil o imposible en tu SO (especialmente Termux).

## Utiliza el [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* Simple de configurar
* Libre de usar
* No se requieren créditos de GPU de Colab (usa las opciones de `use_cpu`)
* Consulta la [Colab Guide Page](/extensions/Extras/Installation.md#running-extras-in-colab) para obtener más detalles.

### Ejecutar Extras en Colab

* Abre el [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* Selecciona las opciones de "Extra" deseadas
* selecciona `use_cpu` para ejecutar Extras sin requerir crédito de GPU
  * esto hará que Stable Diffusion sea más lento, pero todo lo demás funcionará normalmente
* No es obligatorio, pero se recomienda: selecciona la opción `secure` para generar la clave API para proteger tu instancia compartida.
* Haz clic en el botón Inicio de la izquierda (parece un botón de reproducción de triángulo)
* Espera a que termine de cargar todo
* Busca el enlace `trycloudflare.com` al final del resultado. Ignora el enlace localhost, no funcionará (¡lo intentamos!).
* Comenzará con el texto `Running on`
* Copia el enlace de URL de la API que aparece debajo de esa línea. (**NO copies la URL 'localhost', usa la otra**)
* Inicia SillyTavern con soporte de extensiones: (establece `enableExtensions` en `true` en tu `config.yaml` si es necesario)
* Navega al menú de Extensiones de SillyTavern (haz clic en el icono de 'bloques apilados' en la parte superior de la página).
* Pega la URL de la API en la casilla de la parte superior. (**NO la casilla de la Clave API**)
* Si NO has habilitado la opción `secure`, asegúrate de que la casilla de la Clave API esté completamente vacía cuando uses el colab oficial.
* Si has habilitado la opción `secure`, pega la clave API generada en la casilla de la Clave API.
* La clave API aparecerá en el resultado de la consola del colab, por ejemplo: `Your API key is fee2f3f559`
* Haz clic en "Conectar"

---

## Métodos de Instalación Local

### MiniConda (recomendado)

Este método se recomienda porque Conda crea un 'entorno virtual' para que los paquetes requeridos de Extras vivan dentro, por lo que no afectan tu configuración de Python en todo el sistema.

1. Instala [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(¡Importante!) Lee [cómo usar Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. Instala [git](https://git-scm.com/downloads)

    _(¡Los campeones que instalaron SillyTavern con git desde el principio pueden omitir este paso!)_

    Después de tener ambos instalados...

    Escribe/pega los comandos a continuación `UNO POR UNO` EN LA `VENTANA DE SÍMBOLO DEL SISTEMA DE CONDA` y presiona `Enter` después de cada uno.

3. Crea un nuevo entorno de Conda (llamémoslo `extras`):

    `conda create -n extras`

4. Activa el nuevo entorno

    `conda activate extras` (deberías ver `(extras)` aparecer en el lado izquierdo de tu símbolo del sistema)

5. Instala los paquetes del sistema requeridos (esto tomará algo de tiempo)

    `conda install python=3.11 git`

6. Clona el repositorio de Extras GitHub

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. Navega a tu repositorio de Extras clonado

    `cd SillyTavern-extras`

8. Instala los requisitos de Extras usando **uno** de los siguientes comandos (tomará tiempo, de nuevo):

   * `pip install -r requirements.txt` - para características básicas
   * `pip install -r requirements-rvc.txt` - para clonación de voz en tiempo real
   * `pip install -r requirements-coqui.txt` - para Coqui TTS (no recomendado)

    ¡Consulta la página [Problemas Comunes de Instalación de Extras](/extensions/Extras/Installation.md#extras-install-common-problems) si obtienes errores en este paso!

9. Consulta a continuación 'Ejecutar Extras Después de Instalar'

---

### Instalación en Todo el Sistema

Esto es más fácil, pero afectará tu instalación de Python en todo el sistema.

Esto puede causar conflictos si trabajas con muchos programas Python que tienen diferentes requisitos.

Si esta es tu primera vez tocando algo relacionado con Python, eso no debería ser un problema.

1. Instala Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. Instala git: <https://git-scm.com/downloads>
3. Abre una ventana de símbolo del sistema y ve a una carpeta en la que tengas permisos de acceso completo.
4. Clona el repositorio: `git clone https://github.com/SillyTavern/SillyTavern-extras`, presiona Enter.
5. Después de que se complete la clonación, escribe `cd SillyTavern-extras`, presiona Enter.
6. Escribe `python -m pip install -r requirements.txt`
7. Consulta a continuación 'Ejecutar Extras Después de Instalar'

---

## Ejecutar Extras Después de Instalar

### Confirma que las extensiones estén habilitadas

1. Abre el archivo llamado `config.yaml` en un editor de texto. El archivo se encuentra en la carpeta de instalación base de ST.
2. Busca la línea que dice `enableExtensions`.
3. Asegúrate de que esa línea tenga `true`, no `false`.

### Decide qué módulo usar

(Esto solo necesita hacerse una vez)

* Extras siempre se inicia con una línea de comandos de Python.
* `python server.py` es el mínimo absoluto, pero no habilita ningún módulo útil.
* para habilitar módulos debes usar el modificador `--enable-modules=`, con una lista de nombres de módulos separados por comas

Ejemplo: `python server.py --enable-modules=caption,summarize,classify`

Esto habilitaría Subtítulos de Imagen, Resumen de Chat y Expresiones de Personajes con actualización en vivo.

A continuación se muestra una tabla que describe cada módulo.

| Nombre       | Descripción                                                         |
|--------------|---------------------------------------------------------------------|
| `caption`    | Subtítulos de imagen                                                |
| `summarize`  | Resumen de texto                                                    |
| `classify`   | Clasificación de sentimientos de texto                              |
| `sd`         | Generación de imágenes Stable Diffusion                             |
| `silero-tts` | [Servidor Silero TTS](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Cliente Microsoft Edge TTS](https://github.com/rany2/edge-tts)     |
| `chromadb`   | Servidor de almacenamiento vectorial                                |
| `coqui-tts`  | Coqui TTS                                                           |
| `rvc`        | Clonación de voz en tiempo real                                     |

* Decide qué módulos deseas agregar a tu línea de comandos de Python.
* Se utilizarán en el siguiente paso.

**NOTA: ¡No debe haber `espacios en absoluto en la lista de módulos de tu comando de Python!`**

### Inicia el Servidor Extras

Mientras aún estés en tu ventana de símbolo del sistema dentro de la carpeta de instalación de Extras...

1. Asegúrate de que tu entorno de conda esté activo (si usaste el método de instalación de Conda)
2. Escribe `activate extras` si el entorno no está activo.
3. Escribe `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. El servidor de extras se cargará.
5. Después de un tiempo, te mostrará una URL al final. Para instalaciones locales, esto por defecto es `http://localhost:5100`.
6. Copia la URL de la API.

### Conecta ST al servidor Extras

1. Inicia tu servidor SillyTavern y visualiza la interfaz de SillyTavern en tu navegador.
2. Abre el panel de Extensiones (a través del icono de 'Bloques Apilados' en la parte superior de la página)
3. Pega la URL de la API en la casilla de entrada.
4. Haz clic en `Conectar`.

Para ejecutar Extras de nuevo, simplemente activa el entorno y ejecuta estos comandos en un símbolo del sistema.

`conda activate extras`, Presiona Enter.
`python server.py`, Presiona Enter.

Asegúrate de tener las opciones adicionales para server.py (consulta a continuación) que tu configuración requiere.

## Crea un Archivo .bat para un Inicio Fácil

Esto es opcional y solo se aplica a Windows, pero algo similar debería ser posible en MacOS.

1. Ve tu Escritorio de Windows
2. Haz clic derecho, selecciona `Nuevo` y luego haz clic en `Documento de Texto`
3. Un nuevo archivo aparecerá en tu Escritorio, pidiendo un nombre.
4. Nombra el archivo `STExtras.txt`
5. Abre el archivo recién creado en un editor de texto.
6. Pega el siguiente código en él:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. Reemplaza la ruta de carpeta de marcador de posición con tu ruta de carpeta de instalación de Extras real.
8. Reemplaza la línea de comandos de Python con tu línea de comandos real
9. Guarda el archivo con un nuevo nombre `STExtras.bat` (Usa `Archivo` >> `Guardar Como` en la mayoría de editores de texto)

Ahora simplemente puedes hacer doble clic en este archivo .bat para iniciar fácilmente Extras.

Si alguna vez deseas cambiar la lista de módulos (o cualquier otro modificador de línea de comandos para el servidor de extras), simplemente edita el comando de Python dentro del archivo .bat.

## Problemas Comunes de Instalación de Extras

Esta sección enumera preguntas comunes y problemas encontrados al instalar SillyTavern Extras.

### Error: No se pudo importar el módulo 'talkinghead' en Linux

Requiere la instalación de un paquete adicional porque no se instala automáticamente debido a incompatibilidad con Colab. Ejecuta esto después de instalar otros requisitos:

`pip install wxpython`

### El servidor Extras no puede conectarse a la interfaz web de Stable Diffusion de AUTOMATIC1111

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Asegúrate de que webui-user.bat con el que inicias Stable Diffusion contiene la opción de línea de comandos --api en la variable COMMANDLINE_ARGS.**

Encuentra y reemplaza esa línea en tu "webui-user.bat": `set COMMANDLINE_ARGS=--api`

![How it should look](/static/extensions/sd-user.png)

Si el modo API está deshabilitado para SD Web UI, el servidor Extras no podrá hacer una conexión y no podrás generar imágenes!

#### ¿Aún no funciona?

Asegúrate de iniciar todo en el orden correcto, esperando a que cada programa termine de cargar antes de pasar al siguiente paso:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

El servidor de extras no puede reconectarse a la API de Stable Diffusion si se cargó después.

### Error de compilación de rueda hnswlib al instalar ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Antes de instalar el módulo ChromaDB, primero debes hacer `uno de los siguientes`:

* Instala Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Instala el paquete `hnswlib` con conda: `conda install -c conda-forge hnswlib`

---

### Error al instalar requisitos de Python en Mac

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac no soporta CUDA, por lo que los paquetes torch deben instalarse sin soporte CUDA.

Instala los requisitos usando el archivo `requirements-silicon.txt` en su lugar.

---

### ¿Faltan módulos?

* Debes especificar una lista de nombres de módulos en tu línea de comandos de Python, con el modificador `--enable-modules`.
* Consulta la sección [Módulos](/extensions/Extras/Installation.md#decide-which-module-to-use).

---

### ¿Para qué sirve la casilla de Clave API?

* La casilla de Clave API en el panel de Extensiones de SillyTavern solo se usa cuando tienes:
  * creado un archivo de texto llamado `api_key.txt` en tu carpeta de instalación de Extras, que contiene tu contraseña de Extras elegida.
  * iniciado extras con el argumento de línea de comandos `--secure`.
* Esto hace que la API de Extras esté 'bloqueada con contraseña', por lo que solo los usuarios que tengan esa clave en su casilla de Clave API pueden acceder a ella.
* Esto es principalmente útil para personas que desean hacer su propia implementación pública de Extras (colab, etc).
* Los usuarios que ejecutan Extras en su propia PC para uso personal no deben escribir nada en la casilla de Clave API.

### ¿Qué hay de mobile/Android/Termux? 🤔

* Hay algunos en la comunidad que tienen éxito ejecutando Extras en sus teléfonos a través de Ubuntu en Termux.
* Sin embargo, Extras no fueron creados con soporte para dispositivos móviles en mente.
* No se proporcionará soporte para personas que ejecutan Extras en sus dispositivos Android.
* Dirige todas tus preguntas al creador de la guía vinculada a continuación.

#### ❗ Esto NO ES COMPATIBLE

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
