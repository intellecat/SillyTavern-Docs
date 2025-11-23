---
order: 70
route: /usage/prompts/tokenizer/
---

# Tokenizer

Un Tokenizer es una herramienta que divide un fragmento de texto en unidades más pequeñas llamadas tokens. Estos tokens pueden ser palabras individuales o incluso partes de palabras, como prefijos, sufijos o puntuación. Una regla general es que un token corresponde aproximadamente a 3~4 caracteres de texto.

SillyTavern proporciona una opción "Mejor coincidencia" que intenta hacer coincidir el Tokenizer utilizando las siguientes reglas según el proveedor de API utilizado.

APIs de Finalización de Texto **(sobrescribibles)**:

1. NovelAI Clio: Tokenizer de NerdStash.
2. NovelAI Kayra: Tokenizer de NerdStash v2.
3. Finalización de Texto: Tokenizer de API (si es compatible) o Tokenizer de Llama.
4. KoboldAI Classic / AI Horde: Tokenizer de Llama.
5. KoboldCpp: Tokenizer de API del modelo.

Si obtiene resultados inexactos o desea experimentar, puede establecer un _Tokenizer de sobrescritura_ para que SillyTavern lo use al formar una solicitud al backend de IA:

1. Ninguno. Se estima que cada token sea de ~3.3 caracteres, redondeado al entero más cercano. **Pruebe esto si sus indicaciones se cortan en longitudes de contexto altas.** Este enfoque es utilizado por KoboldAI Lite.
2. Tokenizer de Llama. Utilizado por la familia de modelos Llama 1/2: Vicuna, Hermes, Airoboros, etc. **Seleccione si utiliza un modelo Llama 1/2.**
3. Tokenizer de Llama 3. Utilizado por modelos Llama 3/3.1. **Seleccione si utiliza un modelo Llama 3/3.1.**
4. Tokenizer de NerdStash. Utilizado por el modelo Clio de NovelAI. **Seleccione si utiliza el modelo Clio.**
5. Tokenizer de NerdStash v2. Utilizado por el modelo Kayra de NovelAI. **Seleccione si utiliza el modelo Kayra.**
6. Tokenizer de Mistral V1. Utilizado por la familia de modelos Mistral antiguos y sus ajustes. **Seleccione si utiliza un modelo Mistral antiguo.**
7. Tokenizer de Mistral Nemo. Utilizado por la familia de modelos Mistral Nemo y sus ajustes. **Seleccione si utiliza un modelo Mistral Nemo/Pixtral.**
8. Tokenizer de Yi. Utilizado por modelos Yi. **Seleccione si utiliza un modelo Yi.**
9. Tokenizer de Gemma. Utilizado por modelos Gemini/Gemma. **Seleccione si utiliza un modelo Gemma.**
10. Tokenizer de DeepSeek. Utilizado por modelos DeepSeek (como R1). **Seleccione si utiliza un modelo DeepSeek.**
11. Tokenizer de API. Consulta la API de generación para obtener el recuento de tokens directamente del modelo. Backends conocidos que lo soportan: Text Generation WebUI (ooba), koboldcpp, TabbyAPI, Aphrodite API. **Seleccione si utiliza un backend compatible.**

APIs de Finalización de Chat **(no sobrescribibles)**:

1. OpenAI: Tokenizer dependiente del modelo vía [tiktoken](https://github.com/openai/tiktoken).
2. Claude: Tokenizer dependiente del modelo vía [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: Tokenizers de Llama, Mistral, Gemma, Yi para sus respectivos modelos.
4. Google AI Studio: Tokenizer de Gemma.
5. AI21 API: Tokenizer de Jamba (requiere una descarga única).
6. Cohere API: Tokenizer de Command-R o Command-A (requiere una descarga única).
7. MistralAI API: Tokenizer de Mistral V1 o V3 (requiere una descarga única).
8. DeepSeek API: Tokenizer de DeepSeek (requiere una descarga única).
9. Tokenizer alternativo: Tokenizer de GPT-3.5 turbo.

#### Tokenizers Adicionales

Estos Tokenizers no se incluyen en la instalación predeterminada debido a su tamaño. Se requiere una descarga única cuando se utilizan por primera vez.

1. Tokenizer de Qwen2.
2. Tokenizers de Command-R / Command-A. Utilizados por la fuente Cohere en Finalización de Chat.
3. Tokenizer de Mistral V3 (Nemo). Utilizado por la fuente MistralAI en Finalización de Chat (modelos Nemo y Pixtral).
4. Tokenizer de DeepSeek (deepseek-chat). Utilizado por la fuente DeepSeek en Finalización de Chat.

Si no desea utilizar descargas de Internet, existe la opción de exclusión en config.yaml: `enableDownloadableTokenizers`. Establézcalo en `false` para deshabilitar las descargas.

También puede descargar Tokenizers manualmente desde el repositorio [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers). Descargue los archivos JSON y colóquelos en el subdirectorio `_cache` de su raíz de datos, la ruta es `./data/_cache` de forma predeterminada. Cree el directorio `_cache` si no existe. Después de eso, reinicie el servidor de SillyTavern para reinicializar los Tokenizers.

Si el modelo de Tokenizer requerido no está almacenado en caché y las descargas están deshabilitadas, se utilizará un Tokenizer alternativo (Llama 3) para contar.

### Relleno de Tokens

!!! Aplica a: APIs de Finalización de Texto
SillyTavern siempre utilizará el Tokenizer coincidente para modelos de Finalización de Chat, por lo que no hay necesidad de relleno de tokens.
!!!

A menos que SillyTavern utilice un Tokenizer proporcionado por la API del backend remoto que ejecuta el modelo, todos los recuentos de tokens asumidos durante la generación de indicaciones se estiman en función del tipo de [Tokenizer](#tokenizer) seleccionado.

Dado que los resultados de la tokenización pueden ser inexactos en tamaños de contexto cercanos al máximo definido por el modelo, algunas partes del prompt pueden ser recortadas o eliminadas, lo que puede afectar negativamente la coherencia de las definiciones de personajes.

Para evitar esto, SillyTavern asigna una porción del tamaño del contexto como relleno para evitar agregar más elementos de chat de los que el modelo puede acomodar. Si encuentra que alguna parte del prompt se recorta incluso con el Tokenizer más coincidente seleccionado, ajuste el relleno para que la descripción no se trunce.

Puede introducir valores negativos para relleno inverso, que permite asignar más que la cantidad máxima de tokens establecida.
