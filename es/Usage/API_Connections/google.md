---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini es el LLM multimodal de última generación de Google, que está disponible a través de varias APIs, incluyendo Google Vertex AI y Google AI Studio (anteriormente MakerSuite). Esta guía te ayudará a configurar las conexiones de la API de Gemini en SillyTavern.

## Google AI Studio

AI Studio es la forma más rápida y fácil de probar los últimos modelos de AI de Google sin necesidad de configurar un proyecto de Google Cloud Platform (GCP). Proporciona una clave API simple que puedes usar para acceder a los modelos de Gemini.

### Paso 1: Crear una clave de Google AI Studio

1. Ve a la página [Google AI Studio](https://aistudio.google.com/apikey) e inicia sesión con tu cuenta de Google.
2. Haz clic en "Get API Key", acepta los términos y condiciones.
3. Haz clic en "Create API Key" para generar tu clave de API.
4. Copia la clave de API al portapapeles.

### Paso 2: Poner la clave de API en SillyTavern

1. En SillyTavern, ve a la página de "API Connections".
2. Selecciona "Chat Completion" como tipo de API.
3. Selecciona "Google AI Studio" del menú desplegable.
4. Ingresa la clave de API que copiaste anteriormente en el cuadro de texto "API Key".
5. Haz clic en el botón "Connect" para guardar la clave.

Ahora deberías poder usar la API de Google AI Studio con SillyTavern.

## Google Vertex AI

Vertex AI es un servicio proporcionado por Google Cloud Platform (GCP). Proporciona acceso a varios modelos de AI, incluyendo la serie Gemini.

Hay varias formas de configurar una API de Vertex AI, y los modelos disponibles pueden variar según el método utilizado.

### Service Account

Google Cloud Platform (GCP) requiere una cuenta de servicio para acceder a Vertex AI, las claves API simples no funcionarán. Se generará un token a partir del archivo JSON de la cuenta de servicio, que luego se usará para autenticar las solicitudes a la API de Vertex AI.

Puedes crear una cuenta de servicio siguiendo estos pasos:

**Requisitos previos:**

1. Debes tener una cuenta de Google Cloud Platform (GCP).
2. Debes tener un proyecto creado dentro de tu cuenta de GCP.
3. Debes tener la facturación habilitada para ese proyecto.

#### Paso 1: Habilitar la API de Vertex AI

Antes de que tu clave pueda funcionar, la API debe estar habilitada para tu proyecto.

1. Ve a Google Cloud Console: <https://console.cloud.google.com/>
2. Asegúrate de que el proyecto correcto esté seleccionado en la barra superior.
3. Navega a la página de la API de Vertex AI: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. Si aún no está habilitada, haz clic en el botón "Enable".

#### Paso 2: Crear la cuenta de servicio

Esta es la identidad que se utilizará para acceder a la API de Vertex AI.

1. En Google Cloud Console, navega a la página "Service Accounts". Puedes buscarla en la barra de búsqueda superior o usar este enlace directo: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. Selecciona tu proyecto de GCP y haz clic en "+ CREATE SERVICE ACCOUNT".
3. Nombre de la cuenta de servicio: Dale un nombre descriptivo, como `my-vertex-ai-client`.
4. Haz clic en "CREATE AND CONTINUE".
5. Otorga a esta cuenta de servicio acceso al proyecto: En el menú desplegable "Role", busca y selecciona Vertex AI User. Este rol otorga los permisos necesarios para ejecutar modelos sin dar demasiado acceso.
6. Haz clic en "CONTINUE" y luego en "DONE".

#### Paso 3: Generar la clave JSON

Este es el archivo "contraseña" que necesitas. Contiene información confidencial, así que no la compartas ni la cargues en ningún lugar público.

1. Ahora deberías estar de vuelta en la lista de cuentas de servicio. Encuentra la cuenta que acabas de crear (por ejemplo, sillytavern-vertex-ai).
2. Haz clic en el menú de tres puntos (⋮) en el extremo derecho de esa fila y selecciona "Manage keys".
3. Haz clic en "ADD KEY" -> "Create new key".
4. Asegúrate de que el tipo de clave esté configurado en JSON.
5. Haz clic en "CREATE".

Un archivo .json se descargará inmediatamente en tu computadora. Mantenlo seguro, porque esta clave no se puede recuperar si se pierde.

#### Paso 4: Poner el contenido JSON en SillyTavern

El archivo JSON que descargaste contiene toda la información necesaria para autenticarse con la API de Vertex AI. Se verá algo como esto:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. Abre el archivo .json que acabas de descargar con un editor de texto simple (como Notepad en Windows, TextEdit en Mac o VS Code).
2. Selecciona todo el texto en el archivo (Ctrl+A o Cmd+A).
3. Copia el texto al portapapeles (Ctrl+C o Cmd+C).
4. En SillyTavern, ve a la página de "API Connections", selecciona "Chat Completion" como tipo de API y luego selecciona "Google Vertex AI" del menú desplegable. Cambia el método de autenticación a "Service Account".
5. Pega todo el contenido copiado en el cuadro de texto "Service Account JSON Content".
6. Haz clic en el botón "Validate JSON" para asegurarte de que lo copiaste correctamente.
7. Finalmente, desplázate hacia abajo y haz clic en "Connect" en la parte inferior de la página de configuración de la API.

Ahora deberías poder usar la API de Google Vertex AI con SillyTavern.

### Express Mode

Express Mode es la forma más rápida de comenzar a usar Generative AI en Google Cloud. Te permite usar la API de Gemini sin necesidad de configurar una cuenta de servicio. En su lugar, puedes usar directamente una clave de API.

Consulta la documentación oficial para más detalles: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### Paso 1: Asegúrate de que tu cuenta sea elegible para Express Mode

Debes tener una cuenta de Google que no haya sido utilizada anteriormente para crear un proyecto de Google Cloud.
Si tienes un proyecto de Google Cloud existente (incluyendo pruebas gratuitas), puedes crear uno nuevo para este propósito.

#### Paso 2: Activar Vertex AI Express Mode

1. Ve a la siguiente página web: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. Haz clic en "Try it free".
3. Acepta los términos y condiciones e inicia sesión con tu cuenta de Google.
4. Elige tu país y haz clic en "Agree & start free". Espera a que se complete la configuración.

#### Paso 3: Crear una clave de API

1. Verifica que tu Google Cloud Console se esté ejecutando en Express Mode. Deberías ver un banner en la esquina superior izquierda de la página.
2. Haz clic en el enlace "API Keys" en la barra lateral izquierda.
3. Haz clic en el botón "Create API Key".
4. Se generará una nueva clave de API. Copia esta clave al portapapeles.

#### Paso 4: Poner la clave de API en SillyTavern

1. En SillyTavern, ve a la página de "API Connections".
2. Selecciona "Chat Completion" como tipo de API.
3. Selecciona "Google Vertex AI" del menú desplegable.
4. Cambia el método de autenticación a "Express Mode (API Key)".
5. Pega la clave de API que copiaste anteriormente en el cuadro de texto "API Key".
6. Haz clic en el botón "Connect" para guardar la clave.

Ahora deberías poder usar la API de Google Vertex AI en Express Mode con SillyTavern.
