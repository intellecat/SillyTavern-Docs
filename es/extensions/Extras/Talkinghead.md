---
route: /extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning
**EL SOPORTE PARA TALKINGHEAD FUE DESCONTINUADO EN SILLYTAVERN 1.12.13. ESTA PÁGINA SE MANTIENE CON FINES HISTÓRICOS.**
!!!

### ¿Qué es?

Una implementación de la Demostración de Talking Head Anime 3 para AITuber. Posee las siguientes características:

- Genera acciones de movimiento aleatorias similares a Live 2D a partir de una única imagen estática.
- Sincroniza los labios con la salida de sonido de cualquier salida de TTS.

Esta extensión contiene los programas de demostración originales del proyecto Talking Head(?) Anime from a Single Image 3: Now the Body Too. Como su nombre indica, el proyecto te permite animar personajes anime, y solo necesitas una única imagen de ese personaje para hacerlo. Hay dos programas de demostración:

El manual_poser te permite manipular la expresión facial de un personaje, la rotación de la cabeza, la rotación del cuerpo y la expansión del pecho debido a la respiración a través de una interfaz gráfica de usuario, para que puedas guardarlas como expresiones predeterminadas como Feliz, triste, alegre, etc.
ifacialmocap_puppeteer te permite transferir tu movimiento facial a un personaje anime.

### Requisitos de Hardware

Puedes usar modos CPU o GPU (CPU es el predeterminado). Sin embargo, en modo CPU espera alrededor de 1 FPS, y en modo GPU en una RTX3060 estoy obteniendo alrededor de 9-10 FPS.

ifacialmocap_puppeteer requiere un dispositivo iOS que sea capaz de calcular parámetros de forma de mezcla a partir de una fuente de video. Esto significa que el dispositivo debe ser capaz de ejecutar iOS 11.0 o superior y debe tener una cámara frontal TrueDepth. (Consulta esta página para obtener más información.) En otras palabras, si tienes un iPhone X o algo mejor, deberías estar listo.

### Cómo usar

Debes lanzar extras con los siguientes módulos para que talkinghead funcione: `classify` y `talkinghead`!
classify es requerido para el manejo del archivo talkinghead.png. Además, también puedes usar `--talkinghead-gpu` para cargar los modelos de mezcla en la memoria GPU y hacer las animaciones 10 veces más rápidas. ¡Se recomienda encarecidamente usar aceleración de GPU! De forma predeterminada, una vez que se inicia el programa, cargará una imagen predeterminada SillyTavern-extras\talkinghead\tha3\images\lambda_00.png. Puedes verificar que funciona yendo a http://localhost:5100/api/talkinghead/result_feed o `YOUR EXT URL:PORT/api/talkinghead/result_feed`.

- Una vez que el servidor haya iniciado, ve a la pestaña Extension API y conéctate. Luego simplemente selecciona una tarjeta de personaje para cargar. (`--enable-modules=classify,talkinghead --talkinghead-gpu` al iniciar server.py)

- Ahora selecciona Expresiones de Personaje, si marcas la casilla de tipo de imagen talkinghead, el script reemplazará tu expresión de personaje actual con el resultado de `YOUR EXT URL:PORT/api/talkinghead/result_feed` desmarcar la casilla DEBERÍA devolver la imagen a la expresión original, sin embargo a veces tienes que enviar un nuevo mensaje al chat para "recargar" la imagen.

- Si no tienes un archivo talkinghead.png en el directorio de caracteres, simplemente mostrará la imagen predeterminada o la última tarjeta de personaje que tenía un archivo talkinghead.png. La imagen de origen de la animación se cambia cuando se cambia la tarjeta de personaje.

- Ahora abre las expresiones del personaje, desplázate hacia abajo hasta la imagen de talkinghead y carga un archivo de imagen que cumpla con los requisitos en la sección siguiente llamada "Restricciones en Imágenes de Entrada".

- Luego marca y desmarca la casilla de talkinghead para recargar el personaje. Si la imagen se ve extraña, probablemente sea porque no es transparente / no tiene una capa alfa. De lo contrario, sigue las instrucciones y plantilla a continuación.

### Restricciones en Imágenes de Entrada
Para que el sistema funcione bien, la imagen de entrada debe obedecer las siguientes restricciones:

Debe ser de resolución 512 x 512. (Si el programa recibe una imagen de entrada de cualquier otro tamaño, redimensionará la imagen a esta resolución y también la mostrará a esta resolución.)
Debe tener un canal alfa.
Debe contener solo un personaje humanoide.
El personaje debe estar de pie y mirando hacia adelante.
Las manos del personaje deben estar debajo y lejos de la cabeza.
La cabeza del personaje debe estar aproximadamente contenida en la caja de 128 x 128 en el medio de la mitad superior de la imagen.
Los canales alfa de todos los píxeles que no pertenecen al personaje (es decir, píxeles de fondo) deben ser 0.

![Input Constraints](/static/input_spec.png)

### SECCIÓN AVANZADA

### Entorno de Python

Además de la característica base (app.py), tanto manual_poser como ifacialmocap_puppeteer están disponibles como aplicaciones de escritorio. Para ejecutarlas, necesitas configurar un entorno para ejecutar programas escritos en el lenguaje de programación Python. El entorno necesita tener los siguientes paquetes de software:

* Python >= 3.8
* PyTorch >= 1.11.0 con soporte CUDA
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

Una forma de hacerlo es instalar Anaconda y ejecutar los siguientes comandos en tu shell:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### Modelos de Mezcla Adicionales

Solo se incluye un modelo (el más ligero), si deseas los modelos de mezcla adicionales necesitas descargar los archivos de modelo desde https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 y descomprimirlos en la carpeta SillyTavern-extras\talkinghead\tha3\models. Al final, la carpeta de datos debería verse así:

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

Los archivos de modelo se distribuyen bajo la Licencia Creative Commons Atribución 4.0 Internacional, lo que significa que puedes usarlos con fines comerciales. Sin embargo, Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>, es el creador.

### Ejecutar la Aplicación de Escritorio manual_poser
Abre un shell. Cambia tu directorio de trabajo al directorio raíz del repositorio. Luego, ejecuta:

> python tha3/app/manual_poser.py
Ten en cuenta que antes de ejecutar el comando anterior, es posible que tengas que activar el entorno de Python que contiene los paquetes requeridos.

> conda activate extras
si aún no has activado el entorno.
