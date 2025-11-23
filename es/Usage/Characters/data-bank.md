---
order: 30
route: /usage/core-concepts/data-bank/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# Data Bank (RAG)

La generación aumentada por recuperación (RAG) es una técnica para proporcionar fuentes externas de conocimiento al LLM. Ayuda a mejorar la precisión de las respuestas de la IA accediendo a información fuera de los datos de entrenamiento del modelo.

SillyTavern proporciona un conjunto de herramientas para construir una base de conocimiento multipropósito a partir de diversas fuentes, así como para utilizar los datos recopilados en los prompts del LLM.

## Acceso al Data Bank

La extensión Chat Attachments integrada (incluida por defecto en las versiones >= 1.12.0) añade una nueva opción en el menú "Magic Wand" - Data Bank. Este es tu centro para gestionar los documentos disponibles para RAG en SillyTavern.

## Sobre los Documentos

El Data Bank almacena adjuntos de archivo, también conocidos como documentos. Los documentos se dividen en tres ámbitos de disponibilidad.

1. Adjuntos globales - disponibles en cada chat, ya sea en solitario o en grupo.
2. Adjuntos de caracteres - disponibles solo para el personaje elegido actualmente, incluso cuando responden en un grupo. _¡Los adjuntos se guardan localmente y no se exportan con la tarjeta de personaje!_
3. Adjuntos de chat - disponibles solo en el chat actualmente abierto. Cada personaje en el chat puede extraer de él.

!!!info Nota
Aunque no es formalmente parte del data bank, puedes adjuntar archivos incluso a mensajes individuales. Usa la opción Attach File del menú "Magic Wand", o un icono de clip en la fila de acciones del mensaje.
!!!

¿Qué puede ser un documento? ¡Prácticamente cualquier cosa que sea representable en forma de texto plano!

Los ejemplos incluyen, pero no se limitan a:

- Archivos locales (libros, artículos científicos, etc.)
- Páginas web (Wikipedia, artículos, noticias)
- Transcripciones de vídeos

Varias extensiones y plugins también pueden proporcionar nuevas formas de recopilar y procesar datos, más sobre eso a continuación.

## Fuentes de Datos

Para añadir un documento a cualquiera de los ámbitos, haz clic en "Add" y elige una de las fuentes disponibles.

### Notepad

Crea un archivo de texto desde cero o edita un adjunto existente.

### File

Carga un archivo desde el disco duro de tu computadora. SillyTavern proporciona convertidores integrados para formatos de archivo populares:

- PDF (solo texto)
- HTML
- Markdown
- ePUB
- TXT

También puedes adjuntar cualquier archivo de texto con extensiones no estándar, como JSON, YAML, código fuente, etc. Si no hay conversiones conocidas del tipo de archivo seleccionado y el archivo no se puede analizar como un documento de texto plano, la carga de archivo será rechazada, lo que significa que no se permiten archivos binarios sin procesar.

!!!info Nota
La importación de documentos de Microsoft Office (DOCX, PPTX, XLSX) y LibreOffice (ODT, ODP, ODS) requiere que un [Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser) esté instalado y cargado. Consulta la página README del plugin para obtener instrucciones de instalación.
!!!

### Web

Extrae texto de una página web por su URL. El documento HTML se procesa luego a través de la biblioteca [Readability](https://github.com/mozilla/readability) para extraer solo texto utilizable.

Algunos servidores web pueden rechazar solicitudes de búsqueda, estar protegidos por Cloudflare o depender en gran medida de JavaScript para funcionar. Si enfrenta problemas con algún sitio en particular, descargue la página manualmente a través del navegador web y adjúntela usando el cargador de archivos.

### YouTube

Descarga la transcripción de un vídeo de YouTube por su ID o URL, cargada por el creador o generada automáticamente por Google. Algunos vídeos pueden tener las transcripciones deshabilitadas, también el análisis de vídeos con restricción de edad no está disponible ya que requiere inicio de sesión.

El script se carga en el idioma predeterminado del vídeo. Opcionalmente, puedes especificar el código de idioma de dos letras para intentar obtener la transcripción en un idioma específico. Esta función no siempre está disponible y puede fallar, así que úsala con precaución.

### Web Search

!!!info Nota
Esta fuente requiere tener una extensión [Web Search](/extensions/WebSearch.md) instalada y correctamente configurada. Consulta la página vinculada para obtener más detalles.
!!!

Realiza una búsqueda web y descarga el texto de las páginas de resultados de búsqueda. Esto es similar a la fuente Web pero completamente automatizado. El motor de búsqueda elegido se heredará de la configuración de la extensión, así que configúralo con anticipación.

Para comenzar, especifica la consulta de búsqueda, el número máximo de enlaces a visitar y el tipo de salida: un archivo combinado (formateado según las reglas de la extensión) o un archivo individual para cada página. Puedes elegir guardar también los fragmentos de página.

### Fandom

!!!info Nota
Esta fuente requiere tener un [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) instalado y cargado. Consulta la página README del plugin para obtener instrucciones de instalación.
!!!

Extrae artículos de un wiki de [Fandom](https://www.fandom.com/) por su ID o URL. Como algunos wikis son muy grandes, puede ser beneficioso limitar el alcance usando la expresión regular del filtro, se probará contra el título del artículo. Si no se proporciona filtro, entonces todas las páginas están sujetas a ser exportadas. Puedes guardarlas como archivos individuales para cada página o unidas en un solo documento.

### Bronie Parser Extension (Third-Party)

!!!warning Nota
Esta fuente proviene de terceros y **no está afiliada** con el equipo de SillyTavern. Esta fuente requiere que tengas instalada la [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension) de Bronya Rand así como Server Plugins que requieran el analizador para funcionar.
!!!

La Bronie Parser Extension de Bronya Rand permite el uso de scrapers de terceros, como [HoYoLab](https://wiki.hoyolab.com) de miHoYo/HoYoverse en SillyTavern, similar a las otras fuentes de datos.

Actualmente, la Bronie Parser Extension de Bronya Rand admite lo siguiente:

- Wiki de HoYoLab de miHoYo/HoYoverse (para Genshin Impact/Honkai: Star Rail) vía [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

Para comenzar, instala la Bronie Parser Extension de Bronya Rand siguiendo su [guía de instalación](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation) e instala un Server Plugin compatible en SillyTavern. Reinicia SillyTavern y ve al menú _Data Bank_. Haz clic en `+ Add` y deberías ver que tus scrapers recientemente instalados se han añadido a la posible lista de fuentes para obtener información.

## Vector Storage

Entonces, te has construido una bonita y completa biblioteca de información sobre tu tema específico. ¿Qué viene después?

Para usar los documentos para RAG, necesitas usar una extensión compatible que inserte datos relacionados en el prompt del LLM.

Vector Storage, que viene incluido con SillyTavern, es una implementación de referencia de tal extensión. Utiliza embeddings (también conocidos como vectores) para buscar documentos que se relacionen con tus chats en curso.

!!!info Datos curiosos

1. Los embeddings son matrices de números que representan de forma abstracta un fragmento de texto, producidos por modelos de lenguaje especializados. Los textos más similares tienen una distancia más corta entre sus vectores respectivos.
2. La extensión Vector Storage utiliza la biblioteca [Vectra](https://github.com/Stevenic/vectra) para realizar un seguimiento de los embeddings de archivos. Se almacenan en archivos JSON en la carpeta `/vectors` de tu directorio de datos de usuario. Cada documento se representa internamente por su propio archivo de índice/colección.
   !!!

Como la funcionalidad de Vectors está deshabilitada por defecto, necesitas abrir el panel de extensiones (icono "Stacked Cubes" en la barra superior), luego navega a la sección "Vector Storage" y marca la casilla "Enabled for files" en "File vectorization settings".

Por sí solo, Vector Storage no produce vectores, necesitas usar un proveedor de embedding compatible.

## Vector Providers

!!!warning Advertencia
Los embeddings solo son utilizables cuando se recuperan usando el mismo modelo que los generó. Cuando cambias un modelo de embedding o fuente, los vectores necesitan ser recalculados.
!!!

### Local

Estas fuentes son gratuitas e ilimitadas y utilizan tu CPU/GPU para calcular embeddings.

1. Local (Transformers) - se ejecuta en un servidor Node. SillyTavern descargará automáticamente un modelo compatible en formato ONNX desde HuggingFace. Modelo predeterminado: [jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en).
2. WebLLM - requiere que se instale una extensión y un navegador web que [soporte WebGPU](https://caniuse.com/webgpu). Se ejecuta directamente en tu navegador, puede usar aceleración de hardware. Descarga automáticamente modelos compatibles desde HuggingFace. Instala la extensión desde aquí: <https://github.com/SillyTavern/Extension-WebLLM>.
3. Ollama - obtenlo desde <https://ollama.com/>. Establece la URL de la API en el menú de conexión de API (bajo Text Completion, por defecto: `http://localhost:11434`). Debe descargar primero un modelo compatible, luego establecer su nombre en la configuración de la extensión. Modelo de ejemplo: [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Opcionalmente, marca la opción para mantener el modelo cargado en memoria.
4. llama.cpp server - obtenlo desde [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) y ejecuta el ejecutable del servidor con la bandera `--embedding`. Carga modelos de embedding GGUF compatibles desde HuggingFace, por ejemplo, [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF).
5. vLLM - obtenlo desde [vllm-project/vllm](https://github.com/vllm-project/vllm). Establece la URL de la API y la clave de API en el menú de conexión de API primero.
6. Extras (deprecated) - se ejecuta bajo [Extras API](https://github.com/SillyTavern/SillyTavern-extras) usando el cargador de SentenceTransformers. Modelo predeterminado: [all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2). Esta fuente no se mantiene y eventualmente se eliminará en el futuro.

### API sources

Todas estas fuentes requieren una clave de API del servicio respectivo y generalmente tienen un costo de uso, pero generalmente calcular embeddings es bastante económico.

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI

## Vectorization Settings

Después de haber seleccionado tu proveedor de embedding, no olvides configurar otros ajustes que definirán las reglas para procesar y recuperar documentos.

!!!info Nota
La división, vectorización y recuperación de información de los adjuntos lleva algo de tiempo. Aunque la ingestión inicial del archivo puede llevar un tiempo, las consultas de búsqueda RAG suelen ser lo suficientemente rápidas como para no crear un retraso significativo.
!!!

### Message attachments

Estos ajustes controlan los archivos que se adjuntan directamente a los mensajes.

Se aplican las siguientes reglas:

1. Solo los mensajes que caben en la ventana de contexto del LLM pueden tener sus adjuntos recuperados.
2. Cuando la extensión de almacenamiento de vectores está deshabilitada, los adjuntos de archivo y su mensaje acompañante se insertan completamente en el prompt.
3. Cuando la vectorización de archivos está habilitada, el archivo se dividirá en fragmentos y solo se insertarán las partes más relevantes, ahorrando espacio de contexto y permitiendo que el modelo se mantenga enfocado.

- Size threshold (KB) - establece un umbral de división de fragmentos. Solo los archivos más grandes que el tamaño especificado se dividirán.
- Chunk size (chars) - establece el tamaño objetivo de un fragmento individual (en caracteres textuales, ¡no tokens del modelo!).
- Chunk overlap (%) - establece el porcentaje del tamaño de un fragmento que se compartirá entre fragmentos adyacentes. Esto permite una transición más suave entre los fragmentos, pero también puede introducir algo de redundancia.
- Retrieve chunks - establece la cantidad máxima de los fragmentos de archivo más relevantes a recuperar. Se insertarán en su orden original.

### Data Bank files

Estos ajustes controlan cómo se manejan los documentos del Data Bank.

Se aplican las siguientes reglas:

1. Cuando la vectorización de archivos está deshabilitada, el Data Bank no se usa.
2. De lo contrario, todos los documentos disponibles del ámbito actual (ver arriba) se consideran para la consulta. Solo se recuperan los fragmentos más relevantes en todos los archivos. Los múltiples fragmentos del mismo archivo se insertan en su orden original.
3. Los fragmentos insertados reservarán una parte del contexto antes de ajustar los mensajes del chat.

- Size threshold (KB) - establece un umbral de división de fragmentos. Solo los archivos más grandes que el tamaño especificado se dividirán.
- Chunk size (chars) - establece el tamaño objetivo de un fragmento individual (en caracteres textuales, ¡no tokens del modelo!).
- Chunk overlap (%) - establece el porcentaje del tamaño de un fragmento que se compartirá entre fragmentos adyacentes. Esto permite una transición más suave entre los fragmentos, pero también puede introducir algo de redundancia.
- Retrieve chunks - establece la cantidad máxima de fragmentos de archivo a recuperar. Esta asignación se comparte entre todos los archivos.
- Injection Template - define cómo se insertará la información recuperada en el prompt. Puedes usar una macro especial \{\{text\}\} para especificar la posición del texto recuperado, así como cualquier otra macro.
- Injection Position - establece dónde insertar la inyección de prompt. Se aplican las mismas reglas que para Author's Note y World Info.

### Shared settings

- Query messages - cuántos de los últimos mensajes del chat se usarán para consultar fragmentos de documentos.
- Score threshold - ajusta para permitir descartar la recuperación de fragmentos según su puntuación de relevancia (0 - sin coincidencia, 1 - coincidencia perfecta). Los valores más altos permiten recuperación más precisa y evitan que información completamente aleatoria entre en el contexto. Los valores razonables están en un rango entre 0.2 (más suelto) y 0.5 (más enfocado).
- Chunk boundary - una cadena personalizada que se priorizará al dividir los archivos en fragmentos. Si no se especifica, el predeterminado es dividir por (en orden) saltos de línea dobles, saltos de línea simples y espacios entre palabras.
- Only chunk on custom boundary - si está habilitado, la división de fragmentos solo ocurrirá en el límite de fragmento especificado. De lo contrario, la división también ocurrirá en los límites predeterminados.
- Translate files into English before processing - si está habilitado, utilizará la API de traducción configurada en la extensión [Chat Translation](/extensions/Translation.md) para traducir los archivos al inglés antes de procesarlos. Esto es útil cuando se utilizan modelos de embedding que solo soportan texto en inglés.
- Include in World Info Scanning - marca si deseas que el contenido inyectado active entradas del libro de lore.
- Vectorize All - fuerza la ingesta de embeddings para todos los archivos no procesados.
- Purge Vectors - borra los embeddings de archivo, permitiendo recalcular sus vectores.

!!!info Nota
Para la configuración de "Chat vectorization", ve [Chat Vectorization](/extensions/Chat-vectorization.md).
!!!

## Conclusión

¡Felicitaciones! Tu experiencia de chat ahora está mejorada con el poder de RAG. Sus capacidades solo están limitadas por tu imaginación. Como siempre, ¡no tengas miedo de experimentar!
