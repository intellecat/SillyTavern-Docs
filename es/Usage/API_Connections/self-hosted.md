---
order: 90
icon: desktop-download
route: /usage/how-to-use-a-self-hosted-model/
---

# Modelos de IA autohospedados

!!!warning
Esta guía se basa en la experiencia y conocimiento personal del autor y no es una verdad absoluta. Todas las afirmaciones deben tomarse con cautela. Si tienes correcciones o sugerencias, contáctanos en Discord o envía un PR al [repositorio de documentación de SillyTavern](https://github.com/SillyTavern/SillyTavern-Docs).
!!!

## Introducción

Esta guía tiene como objetivo ayudarte a configurar SillyTavern con una IA local ejecutándose en tu PC (a partir de ahora utilizaremos la terminología correcta y la llamaremos LLM). Léela antes de molestar a la gente con preguntas de soporte técnico.

### ¿Cuáles son los mejores Modelos de Lenguaje Grande?

Es imposible responder esta pregunta ya que no existe una escala estandarizada de "Mejor". La comunidad tiene suficientes recursos y discusiones en Reddit y Discord para formar al menos una opinión sobre qué modelo es el preferido o de referencia. Tu experiencia puede variar.

### ¿Cuál es la mejor configuración?

Si hubiera una configuración mejor u obvia, ¿habría necesidad de configuración? La mejor configuración es la que funciona para ti. Es un proceso de ensayo y error.

## Requisitos de hardware y orientación

Este es un tema complejo, así que me ciñeré a lo esencial y generalizaré.

* Hay miles de LLMs gratis que puedes descargar de Internet, similar a cómo Stable Diffusion tiene toneladas de modelos que puedes obtener para generar imágenes.
* Ejecutar un LLM sin modificar requiere una GPU monstruosa con una tonelada de VRAM (memoria GPU). Más de la que jamás tendrás.
* Es posible reducir los requisitos de VRAM comprimiendo el modelo utilizando técnicas de cuantización, como GPTQ o AWQ. Esto hace que el modelo sea algo menos capaz, pero reduce enormemente los requisitos de VRAM para ejecutarlo. De repente, esto permitió a personas con GPUs de juegos como una 3080 ejecutar un modelo de 13B. Aunque no es tan bueno como el modelo sin cuantizar, sigue siendo bueno.
* Mejora aún más: también existe un formato de modelo y cuantización llamado GGUF (anteriormente GGML) que se ha convertido en el formato de elección para personas normales sin GPUs monstruosas. Esto te permite usar un LLM sin GPU. Solo utilizará CPU y RAM. Es mucho más lento (probablemente 15 veces) que ejecutar el LLM en una GPU usando GPTQ/AWQ, especialmente durante el procesamiento de indicaciones, pero la capacidad del modelo es igual de buena. El creador de GGUF luego optimizó GGUF aún más agregando una opción de configuración que permite a las personas con una GPU de grado de juego descargar partes del modelo a la GPU, permitiéndoles ejecutar parte del modelo a velocidad de GPU (nota que esto no reduce los requisitos de RAM, solo mejora tu velocidad de generación).
* Hay diferentes tamaños de modelos, nombrados según el número de parámetros con los que fueron entrenados. Verás nombres como 7B, 13B, 30B, 70B, etc. Puedes pensar en estos como el tamaño del cerebro del modelo. Un modelo de 13B será más capaz que el de 7B de la misma familia de modelos: fueron entrenados en los mismos datos, pero el cerebro más grande puede retener mejor el conocimiento y pensar de manera más coherente. Los modelos más grandes también requieren más VRAM/RAM.
* Hay varios grados de cuantización (8 bits, 5 bits, 4 bits, etc). Cuanto más bajo vayas, más se degrada el modelo, pero menores son los requisitos de hardware. Así que incluso en hardware deficiente, podrías ejecutar una versión de 4 bits de tu modelo deseado. Incluso hay cuantización de 3 bits y 2 bits pero en este punto estás golpeando un caballo muerto. También hay subtipos de cuantización adicionales llamados k_s, k_m, k_l, etc. k_m es mejor que k_s pero requiere más recursos.
* El tamaño del contexto (cuánto tiempo puede durar tu conversación antes de que el modelo pierda partes de ella) también afecta los requisitos de VRAM/RAM. Afortunadamente, esta es una configuración configurable, permitiéndote usar un contexto más pequeño para reducir los requisitos de VRAM/RAM. (Nota: el tamaño del contexto de los modelos basados en Llama2 es de 4k. Mistral se anuncia como 8k, pero en la práctica es 4k.)
* En algún momento de 2023, NVIDIA cambió su controlador de GPU para que si necesitas más VRAM de la que tu GPU tiene, en lugar de que la tarea falle, comience a usar RAM regular como respaldo. Esto arruinará la velocidad de escritura del LLM, pero el modelo aún funcionará y dará la misma calidad de salida. Afortunadamente, este comportamiento [puede deshabilitarse](https://nvidia.custhelp.com/app/answers/detail/a_id/5490).

Dados todos los anteriores, los requisitos de hardware y rendimiento varían completamente dependiendo de la familia del modelo, el tipo de modelo, el tamaño del modelo, el método de cuantización, etc.

#### Calculadora de tamaño de modelo
Puedes usar [Calculadora de VRAM del Modelo LLM de Nyx](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) para determinar cuánta RAM/VRAM necesitas.

Recuerda, quieres ejecutar el modelo más grande y menos cuantizado que quepa en tu memoria, es decir, sin causar [intercambio de disco](https://serverfault.com/a/48487).

## Descargando un LLM

Para comenzar, necesitarás descargar un LLM. El lugar más común para encontrar y descargar LLMs es en HuggingFace. Hay miles de modelos disponibles. Una buena manera de encontrar modelos GGUF es verificar la página de la cuenta de bartowski: <https://huggingface.co/bartowski>. Si no deseas GGUF, proporciona enlaces a la página del modelo original donde podrías encontrar otros formatos para ese mismo modelo.

En la página de un modelo dado, encontrarás un montón de archivos.

* ¡Puede que no necesites todos ellos! Para GGUF, solo necesitas el archivo de modelo .gguf (generalmente 4-11GB). Si encuentras varios archivos grandes, generalmente son todas cuantizaciones diferentes del mismo modelo, solo necesitas elegir una.
* Para archivos .safetensors (que pueden ser GPTQ o AWQ o cuantizados HF o sin cuantizar), si ves una secuencia de números en el nombre de archivo como model-00001-of-00003.safetensors, entonces necesitas todos los 3 archivos .safetensors + todos los otros archivos en el repositorio (tokenizer, configs, etc.) para obtener el modelo completo.
* A partir de enero de 2024, Mixtral MOE 8x7B se considera ampliamente el estado del arte para LLMs locales. Si tienes los 32GB de RAM para ejecutarlo, definitivamente pruébalo. Si tienes menos de 32GB de RAM, entonces usa Kunoichi-DPO-v2-7B, que a pesar de su tamaño es excelente desde el principio.

### Tutorial para descargar Kunoichi-DPO-v2-7B

Usaremos el modelo Kunoichi-DPO-v2-7B para el resto de esta guía. Es un modelo excelente basado en Mistral 7B, que solo requiere 7GB de RAM, y ofrece un rendimiento muy superior al esperado. Nota: Kunoichi usa indicaciones de Alpaca.

* Ve a <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* Haz clic en 'Files and versions'. Verás un listado de varios archivos. Estos son todos el mismo modelo pero ofrecidos en diferentes opciones de cuantización. Haz clic en el archivo 'kunoichi-dpo-v2-7b.Q6_K.gguf', que nos da una cuantización de 6 bits.
* Haz clic en el botón 'download'. Tu descarga debería comenzar.

### Cómo identificar el tipo de modelo

Los buenos cargadores de modelos como TheBloke dan nombres descriptivos. Pero si no lo hacen:

* El nombre de archivo termina en .gguf: Modelo GGUF CPU (obvio)
* El nombre de archivo termina en .safetensors: puede ser sin cuantizar, o cuantizado HF, o GPTQ, o AWQ
* El nombre de archivo es pytorch-***.bin: igual que lo anterior, pero este es un formato de archivo de modelo más antiguo que permite que el modelo ejecute un script Python arbitrario cuando se carga el modelo, y se considera inseguro. Aún puedes usarlo si confías en el creador del modelo o estás desesperado, pero elige .safetensors si tienes la opción.
* ¿Existe config.json? Mira si tiene quant_method.
* q4 significa cuantización de 4 bits, q5 es cuantización de 5 bits, etc
* ¿Ves un número como -16k? ¡Ese es un tamaño de contexto aumentado (es decir, cuánto tiempo puede durar tu conversación antes de que el modelo olvide el principio de tu chat)! Ten en cuenta que los tamaños de contexto más grandes requieren más VRAM.

## Instalando un servidor LLM: Oobabooga o KoboldAI

Con el LLM ahora en tu PC, necesitamos descargar una herramienta que actúe como intermediaria entre SillyTavern y el modelo: cargará el modelo y expondrá su funcionalidad como una API web HTTP local con la que SillyTavern puede comunicarse, de la misma manera que SillyTavern se comunica con servicios web pagados como OpenAI GPT o Claude. La herramienta que uses debe ser KoboldAI u Oobabooga (u otras herramientas compatibles).

Esta guía cubre ambas opciones, solo necesitas una.

!!!warning
Si estás hospedando SillyTavern en Docker, usa **http://host.docker.internal:\<port\>** en lugar de **http://127.0.0.1:\<port\>**. Esto es porque SillyTavern se conecta al punto final de la API desde el servidor ejecutándose en el contenedor de Docker. La pila de red de Docker es separada de la del host, y por lo tanto las interfaces de loopback no se comparten.
!!!

### Descargando y usando KoboldCpp (Sin instalación requerida, modelos GGUF)

1. Visita https://koboldai.org/cpp donde verás la versión más reciente con varios archivos que puedes descargar.
En el momento de escribir, la versión más nueva de CUDA que enumeran es cu12 que funcionará mejor en GPUs de Nvidia modernas, si tienes una GPU más antigua o de una marca diferente puedes usar el koboldcpp.exe regular. Si tienes una CPU antigua es posible que KoboldCpp se bloquee cuando intentes cargar modelos, en ese caso intenta la versión _oldcpu para ver si resuelve tu problema.
2. KoboldCpp no necesita ser instalado, una vez que inicies KoboldCpp podrás inmediatamente seleccionar tu modelo GGUF como el vinculado arriba usando el botón Examinar junto al campo Modelo.
3. Por defecto KoboldCpp se ejecuta con un máximo de contexto de 4K incluso si configuras esto más alto en SillyTavern, si deseas ejecutar un modelo con un contexto más alto asegúrate de ajustar el regulador de contexto en esta pantalla antes de lanzar el modelo. Ten en cuenta que un tamaño de contexto más grande significa requisitos de memoria (video) más altos, si estableces esto demasiado alto o cargas un modelo que es demasiado grande para tu sistema, KoboldCpp automáticamente comenzará a usar tu CPU para las capas que no cabe en tu GPU, esto será mucho más lento.
4. Haz clic en Launch, si todo funciona correctamente se abrirá una nueva página web con KoboldAI Lite donde puedes probar si todo funciona correctamente.
5. Abre SillyTavern y haz clic en API Connections (2do botón en la barra superior)
6. Establece API en Text Completion y el Tipo de API en KoboldCpp.
7. Establece la URL del servidor en <http://127.0.0.1:5001/> o el enlace que KoboldCpp te dio en caso de que no se esté ejecutando en el mismo sistema (Puedes activar el modo Remote Tunnel de KoboldCpp para obtener un enlace que sea accesible desde cualquier lugar).
8. Haz clic en Connect. Debería conectarse exitosamente y detectar kunoichi-dpo-v2-7b.Q6_K.gguf como el modelo.
9. Chatea con un personaje para probar que funciona.

### Consejos para Optimizar la velocidad de KoboldCpp
1. Flash Attention ayudará a reducir los requisitos de memoria, puede ser más rápido o más lento dependiendo de tu sistema y te permitirá ajustar más capas en tu GPU que el predeterminado.
2. KoboldCpp dejará algo de espacio para otro software cuando adivine capas para prevenir problemas, si tienes pocos programas abiertos y no puedes ajustar el modelo completamente en la GPU es posible que puedas agregar algunas capas extras.
3. Si el modelo usa demasiada memoria para el tamaño del contexto puedes disminuir esto cuantificando el KV. Esto reducirá la calidad de la salida pero puede ayudarte a poner más capas en la GPU. Para hacer esto vas a la pestaña Tokens en KoboldCpp y luego desactivas Context Shifting y activas Flash Attention. Esto desbloqueará el regulador de Quantized KV Cache, un número más bajo significa menos memoria / inteligencia del modelo.
4. ¿Ejecutando KoboldCpp en un sistema más lento donde tarda mucho en procesar la indicación? Context Shifting funciona mejor cuando evitas usar Lorebooks, randomización u otras características que cambian dinámicamente la entrada. Dejando Context Shifting habilitado KoboldCpp te ayudará a evitar tiempos de reprocesamiento largos.

### Instalando Oobabooga

Aquí hay un procedimiento de instalación más correcto/a prueba de tontos:

1. git clone <https://github.com/oobabooga/text-generation-webui> (o descarga su repositorio como .zip en tu navegador, luego extráelo)
2. Ejecuta start_windows.bat o lo que sea para tu SO
3. Cuando se te pregunte, selecciona tu tipo de GPU. Incluso si tienes la intención de usar GGUF/CPU, si tu GPU está en la lista, selecciónala ahora, porque te dará la opción de usar una optimización de velocidad más tarde llamada GPU sharding (sin tener que reinstalar desde cero). Si no tienes una dGPU de grado de juego (NVIDIA, AMD), selecciona None.
4. Espera a que la instalación se complete
5. Coloca kunoichi-dpo-v2-7b.Q6_K.gguf en text-generation-webui/models
6. Abre text-generation-webui/CMD_FLAGS.txt, elimina todo adentro y escribe: --api
7. Reinicia Oobabooga
8. Visita <http://127.0.0.1:5000/docs>. ¿Carga una página de FastAPI? Si no, cometiste un error en algún lugar.

### Cargando nuestro modelo en Oobabooga

1. Abre <http://127.0.0.1:7860/> en tu navegador
2. Haz clic en la pestaña Model
3. En la lista desplegable, selecciona nuestro modelo Kunoichi DPO v2. Debería haber seleccionado automáticamente el cargador llama.cpp.
4. (Opcional) Mencionamos 'GPU offload' varias veces antes: esa es la configuración n-gpu-layers en esta página. Si deseas usarla, establece un valor antes de cargar el modelo. Como referencia básica, establecerlo en 30 usa un poco menos de 6GB VRAM para modelos de 13B e inferiores. (varía con la arquitectura y tamaño del modelo)
5. Haz clic en Load


### Configurando SillyTavern para hablar con Oobabooga

1. Haz clic en API Connections (2do botón en la barra superior)
2. Establece API en Text Completion
3. Establece Tipo de API en Default (Oobabooga)
4. Establece la URL del servidor en <http://127.0.0.1:5000/>
5. Haz clic en Connect. Debería conectarse exitosamente y detectar kunoichi-dpo-v2-7b.Q6_K.gguf como el modelo.
6. Chatea con un personaje para probar que funciona

## Conclusión

Felicitaciones, ahora deberías tener un LLM local en funcionamiento.
