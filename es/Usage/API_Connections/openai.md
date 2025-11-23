---
order: 20
route: /usage/api-connections/openai/
---

# Completaciones de Chat

## Instrucciones específicas de la fuente

!!!warning **¡Importante!**
La mayoría de plataformas API permiten ver la clave API generada solo una vez, en el momento de su creación. Si la pierdes, tendrás que generar una nueva clave. ¡Asegúrate de mantenerla segura!
!!!

### OpenAI

Usa la plataforma de desarrolladores de OpenAI para acceder a varios modelos de OpenAI, incluyendo gpt-4o, gpt-4.1, o3, etc.

**Cómo obtener una clave API:**

1. Ve a [OpenAI](https://platform.openai.com/) e inicia sesión.
2. Usa la opción "[Ver claves API](https://platform.openai.com/account/api-keys)" para crear una nueva clave API.

### Claude

Claude es una familia de modelos de IA desarrollados por Anthropic. Puedes acceder a los modelos Claude a través de la consola de Anthropic.

**Cómo obtener una clave API:**

1. Ve a [Anthropic Console](https://console.anthropic.com/) e inicia sesión.
2. Usa la sección "[Obtener clave API](https://console.anthropic.com/settings/keys)" para crear una nueva clave API.

### Mistral AI

Mistral AI es un equipo que desarrolla modelos abiertos y propietarios con altos estándares científicos y un enfoque en la apertura. Puedes ejecutar sus modelos localmente o a través de su servicio API, La Plateforme.

**Cómo obtener una clave API:**

1. El primer paso es crear una cuenta en [La Plateforme](https://console.mistral.ai/).
2. Una vez hecho, puedes elegir un [plan](https://console.mistral.ai/billing/plans) y configurar tu información de pago u optar por el Nivel Gratuito.
3. A continuación, puedes crear tu [clave API](https://console.mistral.ai/api-keys/). ¡Puede que tengas que esperar un par de minutos antes de que la clave sea válida!

### DeepSeek

DeepSeek Platform proporciona acceso a los últimos modelos de DeepSeek a través de una API. Ofrecen una variedad de modelos, incluyendo DeepSeek V3 y DeepSeek R1.

**Cómo obtener una clave API:**

1. Regístrate en [DeepSeek Platform](https://platform.deepseek.com/).
2. Después de registrarte y recargar tu cuenta, puedes crear una clave API en la sección "[Claves API](https://platform.deepseek.com/api_keys)".

### AI21

AI21 Labs ofrece una variedad de modelos de IA, incluyendo su serie Jamba insignia. Puedes acceder a sus modelos a través de la API de AI21 Studio.

**Cómo obtener una clave API:**

1. Ve a [AI21 Studio](https://studio.ai21.com/) e inicia sesión.
2. Navega a la sección "Configuración => Claves API" para crear una nueva clave API.

### Cohere

Cohere proporciona un conjunto de modelos de IA para varias tareas, incluyendo generación de texto e incrustaciones. Puedes acceder a sus modelos a través de la API de Cohere.

**Cómo obtener una clave API:**

1. Ve a [Cohere](https://cohere.com/) e inicia sesión.
2. Navega a la sección "[Claves API](https://dashboard.cohere.com/api-keys)" en la configuración de tu cuenta para crear una nueva clave API.

### Perplexity

Perplexity AI ofrece acceso a modelos Sonar habilitados en línea a través de su API para investigación en tiempo real y recuperación de información.

Guía oficial de inicio: [Guía de inicio rápido de Perplexity](https://docs.perplexity.ai/getting-started/quickstart)

**Cómo obtener una clave API:**

1. Ve a [Perplexity](https://perplexity.ai/) e inicia sesión.
2. Ve a la sección "[Facturación de API](https://www.perplexity.ai/account/api/billing)" para comprar créditos para el uso de la API.
3. Navega a la sección "[Claves API](https://www.perplexity.ai/account/api/keys)" en la configuración para crear una nueva clave API.

### Fireworks AI

Fireworks AI es una plataforma de alto rendimiento que proporciona acceso rápido y económico a modelos de lenguaje de código abierto de última generación. La plataforma ofrece implementación sin servidor con API compatibles con OpenAI y soporta ventanas de contexto de hasta 256,000 tokens.

**Cómo obtener una clave API:**

1. Ve a [Fireworks AI](https://fireworks.ai/) y crea una cuenta o inicia sesión.
2. Navega a la [página de Claves API](https://app.fireworks.ai/settings/users/api-keys) en la configuración de tu cuenta.
3. Haz clic en "Crear clave API" y proporciona un nombre descriptivo (p. ej., "SillyTavern").

## Electron Hub

Electron Hub es una plataforma unificada compatible con OpenAI que proporciona acceso a modelos de múltiples proveedores a través de una única clave API.

**Cómo obtener una clave API:**

1. Crea una cuenta en [Electron Hub](https://playground.electronhub.ai/console).
2. Genera una clave API desde la página **Consola → Claves API**.

## Punto final compatible con OpenAI personalizado

!!!warning
¡Es importante notar que no proporcionamos soporte para posibles problemas que puedas tener!
¡No garantizamos compatibilidad con todos los puntos finales de API posibles!
!!!

!!!
Si tienes la intención de usar esta función para usar un punto final local, como TabbyAPI, Oobabooga, Aphrodite, o cualquiera similar, podrías querer consultar la [compatibilidad integrada para esos](/Usage/API_Connections/index.md) en su lugar. La función de punto final personalizado está principalmente destinada a usarse con otros servicios y programas que exponen un punto final de Completación de Chat compatible con OpenAI.

Las API de Completación de Texto más compatibles proporcionan opciones de personalización mucho mayores que las que permiten los estándares de OpenAI. Estas mayores opciones de personalización, como el muestreo Min-P, podrían ser valiosas para que los usuarios de SillyTavern las consulten, lo que puede mejorar mucho la calidad de las generaciones.
!!!

Puedes configurar un punto final alternativo para el backend de Completaciones de Chat. Este punto final personalizado puede conectarse a cualquier servidor que admita el esquema API genérico de OpenAI.

Ejemplos de backends compatibles incluyen:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### Conectando

Para acceder a esta función:

1. Cambia al tipo de API 'Completación de Chat'
2. Selecciona 'Personalizado (compatible con OpenAI)' para 'Fuente de Completación de Chat'

Ingresa la URL del punto final personalizado y una clave API si es necesaria. Por ejemplo, TabbyAPI requiere una clave API para la autenticación.

!!!tip
**Sugerencia:** Si experimentas problemas de conexión, intenta agregar `/v1` al final de la URL del punto final. NO agregues el sufijo `/chat/completions`.
!!!

### Seleccionar un modelo

Si la API personalizada implementa el punto final `/v1/models` para proporcionar una lista de modelos disponibles, puedes elegir entre una lista desplegable. De lo contrario, usa el campo de texto para ingresar manualmente un ID de modelo.

Marca 'Omitir verificación de estado de la API' para evitar que SillyTavern te alerte sobre un punto final de API que no funciona. Habilita esta opción si tu punto final de API funciona correctamente pero SillyTavern continúa mostrando advertencias.

Haz clic en "Mensaje de prueba" para verificar la conectividad enviando un mensaje simple al modelo.

## Post-procesamiento de solicitud

!!!warning
**Nota:** ¡La llamada de herramientas no se admite cuando se utiliza la opción Post-procesamiento con "sin herramientas"!
!!!

Algunos puntos finales pueden imponer restricciones específicas sobre el formato de las solicitudes entrantes, como requerir solo un mensaje de sistema o roles estrictamente alternados.

SillyTavern proporciona convertidores de solicitud integrados para ayudar a cumplir con estos requisitos (de menos a más restrictivo):

1. Ninguno - no se aplica procesamiento explícito a menos que sea estrictamente requerido por la API
2. Fusionar mensajes consecutivos del mismo rol
3. Semi-estricto - fusionar roles y permitir solo un mensaje de sistema opcional
4. Estricto - fusionar roles, permitir solo un mensaje de sistema opcional y requerir que un mensaje de usuario sea el primero
5. Mensaje de usuario único - fusionar todos los mensajes de todos los roles en un único mensaje de usuario

Fusionar, semi-estricto y estricto además eliminan las llamadas de herramientas de la solicitud, a menos que se seleccione la variante "con herramientas". Esto es útil para API que no admiten la llamada de herramientas y tus solicitudes existentes contienen llamadas de herramientas.

Las opciones menos restrictivas no tienen efecto en puntos finales más restrictivos implementados en SillyTavern que no sean "OpenAI personalizado"; OpenAI personalizado puede generar un error con una solicitud inválida.

En modo estricto, si no existe un mensaje de usuario antes del primer mensaje del asistente, entonces se insertará `promptPlaceholder` de `config.yaml`, que por defecto es "\[Iniciar un nuevo chat]".
