---
order: 150
icon: repo-forked
expanded: false
route: /usage/api-connections/
---

# Conexiones API

SillyTavern puede conectarse a una amplia gama de APIs de LLM.
A continuación se describe sus respectivas fortalezas, debilidades y casos de uso.

## ELI5: Chat Completions vs Text Completions

Cuando navegas por primera vez a la página "API Connections" en ST, notarás una opción desplegable para seleccionar entre opciones usando nomenclatura como "Chat Completion" y "Text Completion". Es útil entender qué significa esto.

Lo que no es: Es fácil pensar en "Text Completion" como modelos locales y "Chat Completion" como LLMs basados en la nube, pero ese no es el caso. Tampoco es, por ejemplo, "Novel AI" u "Kobold" realmente un tipo de modelo separado en absoluto, aunque sean opciones separadas en el menú desplegable de API en ST. Puedes forzar modelos en diferentes estructuras de API con el backend apropiado, pero ese no es el propósito de esta sección.

Cuando envías un mensaje usando ST, tu chat, descripción de personaje y otros avisos como libros de lore o notas del autor se construyen en una única "solicitud" para enviar al modelo. El "tipo" de API del modelo que estés usando decide exactamente cómo se construirá este aviso (algo de lo que ST se encarga automáticamente en segundo plano - puedes abrir tu terminal de ST y ver exactamente cómo se ve el aviso que se envía a la IA).

### Chat Completions

Un modelo Chat Completion, como su nombre sugiere, estructurará tu aviso en una serie de mensajes entre el Usuario (tú) y el Asistente (la IA) o Sistema (neutral). Los modelos entrenados para Chat Completion ayudan a crear la sensación de un "Chat", con la IA "respondiendo" al último mensaje. Cuando usas el sitio web de ChatGPT, estás tratando con una API Chat Completions en segundo plano.

### Text Completions (a.k.a solo "Completions")

Un Text Completion por otro lado, y nuevamente como su nombre sugiere, convertirá tu aviso en una cadena larga, y el modelo simplemente intentará continuar esto (como, literalmente imagina todo tu texto, tus cientos de mensajes, todo tu formato, saltos de línea, etc. comprimido en una oración muy larga).

Si tus mensajes en ST resultan estar formateados como una serie de mensajes entre YourPersona: y Character:, el modelo Text Completion intentará continuar este patrón y ST lo renderizará como un nuevo mensaje de chat para ti, pero realmente el modelo solo está intentando continuar el texto. Si proporcionaste una entrada de "The Sun rises in the", es probable que un modelo de completitud de texto termine ese mensaje con "East".

La mayoría de los modelos Text Completion tienen una "Instruct Template" recomendada (generalmente mencionada en la documentación o página de descarga del modelo) que los ayuda a "responder" a mensajes e instrucciones, como un modelo Chat Completion. ST generalmente tiene la mayoría (si no todos) de los Instruct Templates disponibles para que elijas en la página "Formato Avanzado".

## APIs Locales

- Estas APIs de LLM se pueden ejecutar en tu PC.
- Son gratuitas de usar y no tienen filtro de contenido.
- El proceso de instalación puede ser complejo (**El equipo de desarrollo de SillyTavern no proporciona soporte para esto**).
- Requiere descarga separada de modelos LLM desde [HuggingFace](https://huggingface.co/models?other=LLM) que pueden tener entre 5-50GB cada uno.
- La mayoría de los modelos no son tan poderosos como las APIs LLM en la nube.

### KoboldCpp

- API fácil de usar con descarga de CPU (útil para usuarios con VRAM bajo) y streaming
- Se ejecuta desde un único archivo binario en Windows, Mac y Linux
- Compatible con modelos GGUF
- Más lento que cargadores solo GPU como AutoGPTQ y Exllama/v2
- [GitHub](https://github.com/LostRuins/koboldcpp), [Setup Instructions](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- La fuente original de la que se bifurcaron KoboldCpp y Ollama
- Proporciona binarios precompilados y una opción para compilar desde la fuente
- Compatible con modelos GGUF
- Interfaz CLI ligera para llama-server
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- La más fácil de configurar y usar de todas las APIs basadas en llama.cpp
- Un útil [catálogo](https://ollama.com/library) de modelos disponibles para descargar con un clic
- Compatible con modelos GGUF envueltos en su propio formato
- [GitHub](https://github.com/ollama/ollama), [Website](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- UI Gradio todo en uno con streaming
- Soporte más amplio para modelos cuantificados (AWQ, Exl2, GGML, GGUF, GPTQ) y FP16
- Los instaladores de un clic están disponibles
- Actualizaciones regulares, que a veces pueden romper la compatibilidad con SillyTavern
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**Manera correcta de conectar SillyTavern a la nueva API OpenAI de Ooba:**

1. Asegúrate de estar en la última actualización de Oobabooga's TextGen (a partir del 14 de noviembre de 2023).
2. Edita el archivo CMD_FLAGS.txt e incluye la bandera `--api`. Luego reinicia el servidor de Ooba.
3. Conecta ST a `http://localhost:5000/` (por defecto) sin marcar la casilla de 'Legacy API'. Puedes eliminar el sufijo `/v1` de la URL que proporciona la consola de Ooba.

*Puedes cambiar el puerto de alojamiento de API con la bandera `--api-port 5001`, donde 5001 es tu puerto personalizado.*

### TabbyAPI

- API liviana basada en [Exllamav2](https://github.com/turboderp/exllamav2) con streaming
- Compatible con modelos Exl2, GPTQ y FP16
- La [extensión oficial](https://github.com/theroyallab/ST-tabbyAPI-loader) permite cargar/descargar modelos directamente desde SillyTavern
- No se recomienda para usuarios con VRAM bajo (sin descarga de CPU)
- [GitHub](https://github.com/theroyallab/tabbyAPI), [Setup Instructions](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic (deprecated, abandoned)

- Se ejecuta en tu PC, 100% privado, amplia gama de modelos disponibles
- Da el control más directo de la configuración de generación de la IA
- Requiere grandes cantidades de VRAM en tu GPU (6-24GB, dependiendo del modelo LLM)
- Modelos limitados a contexto de 2k
- Sin streaming
- Versiones populares de KoboldAI:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## APIs LLM en la Nube

- Estas APIs de LLM se ejecutan como servicios en la nube y no requieren recursos en tu PC
- Son más fuertes/inteligentes que la mayoría de los LLMs locales
- Sin embargo, todos tienen filtrado de contenido de diversos grados, y la mayoría requieren pago

### AI Horde

- SillyTavern puede acceder a esta API sin configuración adicional
- Utiliza la GPU de voluntarios individuales (Horde Workers) para procesar respuestas para tus entradas de chat
- A merced del Worker en términos de tiempos de espera de generación, configuración de IA y modelos disponibles
- [Website](https://aihorde.net/), [Setup Instructions](./horde.md)

### OpenAI (ChatGPT)

- Fácil de configurar y adquirir una clave API
- Requiere prepago de créditos y cobra por solicitud
- Muy lógico. El estilo creativo puede ser repetitivo y predecible
- La mayoría de los modelos más nuevos (gpt-4-turbo, gpt-4o) admiten multimodalidad
- [Website](https://platform.openai.com/), [Setup Instructions](/Usage/API_Connections/openai.md#openai)

### Claude (by Anthropic)

- Recomendado para usuarios que desean que sus chats de IA tengan un estilo de escritura creativo y único
- Requiere prepago de créditos y cobra por solicitud
- Los modelos más nuevos (Claude 3) admiten multimodalidad
- Requiere un estilo de solicitud específico y utilización de [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) para dirección de respuesta
- [Website](https://console.anthropic.com/), [Setup Instructions](/Usage/API_Connections/openai.md#claude)

### Google AI Studio and Vertex AI

- Tiene un nivel gratuito con límites de velocidad (Gemini Flash), puede requerir información de facturación
- [AI Studio](https://aistudio.google.com/) generalmente tiene los últimos modelos y características
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio) es más difícil de configurar, pero más estable
- [Setup Instructions](/Usage/API_Connections/google.md)

### Mistral (by Mistral AI)

- Modelos eficientes de varios tamaños y casos de uso. Puedes crear una cuenta y clave API en [su plataforma](https://console.mistral.ai/api-keys/).
- De 32k a 128k tamaños de contexto para uso general, y 32k a 256k tamaños de contexto para codificación.
- Nivel gratuito con límites de velocidad.
- Moderación razonable, siendo los principios principales de Mistral ser neutral y empoderar a los usuarios, más información [aquí](https://mistral.ai/terms/).
- [Website](https://console.mistral.ai/), [Setup Instructions](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- Proporciona una API unificada para acceder a todos los LLMs principales del mercado
- Sistema de crédito de pago por token, así como modelos gratuitos con solicitudes diarias limitadas
- Sin moderación impuesta, a menos que sea requerida por el proveedor de LLM
- [Website](https://openrouter.ai), [Setup Instructions](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- Proporciona acceso a las últimas versiones de modelos muy populares DeepSeek V3 (`deepseek-chat`) y DeepSeek R1 (`deepseek-reasoner`)
- Requiere pago de créditos ($2 mínimo), pero los modelos son bastante baratos por su calidad
- Sin moderación en la API, pero los modelos pueden rechazar ciertos avisos
- [Website](https://platform.deepseek.com/), [Setup Instructions](/Usage/API_Connections/openai.md#deepseek)

### AI21

- Proporciona acceso a modelos abiertos de Jamba Family
- Tiene una prueba gratuita ($10 por tres meses), luego requiere pagar mensualmente por token
- [Website](https://ai21.com/), [Setup Instructions](/Usage/API_Connections/openai.md#ai21)

### Cohere

- Proporciona acceso a los últimos modelos de Cohere (command-r, command-a, c4ai-aya, etc.)
- Tiene un nivel gratuito (Trial Keys) con suficientes límites de velocidad para uso casual
- [Website](https://cohere.com/), [Setup Instructions](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- Proporciona acceso a modelos Perplexity Sonar únicos habilitados en línea a través de su API
- Requiere tener facturación configurada y créditos comprados
- [Website](https://perplexity.ai/), [Setup Instructions](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- Servicio que aloja modelos sin restricciones de varias familias
- Utiliza 'créditos' para pagar por tokens en varios modelos
- No registra solicitudes por defecto, pero puedes habilitarlo para obtener descuentos de crédito en tokens.
- Utiliza una API similar a `Oobabooga TextGeneration WebUI`, consulta [Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters) para obtener detalles.
- [Website](https://mancer.tech/), [Setup Instructions](/Usage/API_Connections/mancer.md)

### DreamGen

- Modelos sin censura ajustados para escritura creativa controlable
- Créditos mensuales gratuitos, así como una suscripción de pago
- Modelos que van desde 7B a 70B
- [Setup Instructions](DreamGen.md)

### Pollinations

- No requiere configuración, se puede usar directamente
- Proporciona acceso a una amplia gama de modelos de forma gratuita
- Los resultados pueden ocasionalmente incluir anuncios con enlaces a servicios de terceros

### NovelAI

- Sin filtro de contenido, el último modelo se basa en Llama 3
- Se requiere suscripción de pago, el nivel determina la longitud máxima del contexto
- [Website](https://novelai.net/), [Setup Instructions](/Usage/API_Connections/novelai.md)

### Electron Hub

- Una clave API desbloquea modelos de múltiples proveedores (OpenAI, Anthropic, DeepSeek, etc.) para generación de texto e imágenes
- $0.25 de créditos gratuitos cada día, planes pagos disponibles
- [Website](https://www.electronhub.ai/), [Setup Instructions](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- API unificada para 300+ modelos incluyendo Claude, GPT-4o, Gemini, LLaMA 3, Mistral y otros
- Tiene un nivel gratuito con límites de velocidad, planes de suscripción y opciones de pago por uso
- [Website](https://aimlapi.com), [Docs](https://docs.aimlapi.com), [Models](https://aimlapi.com/models)
