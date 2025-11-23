---
order: 100
route: /usage/core-concepts/advancedformatting/
---

# Formato Avanzado

La configuración proporcionada en esta sección permite un mayor control sobre la estrategia de [construcción de solicitudes](index.md), principalmente para Text Completion APIs.

La mayoría de la configuración en este panel no se aplica a Chat Completion APIs ya que están gobernadas por el sistema de administrador de solicitudes en su lugar.

+++ Text Completion APIs
* [Solicitud del Sistema](#system-prompt)
* [Plantilla de Contexto](#context-template)
* [Tokenizer](#tokenizer)
* [Cadenas de Parada Personalizadas](#custom-stopping-strings)
+++ Chat Completion APIs
* Solicitud del Sistema: no aplicable, use [Administrador de Solicitudes](prompt-manager.md)
* Plantilla de Contexto: no aplicable, use [Administrador de Solicitudes](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Cadenas de Parada Personalizadas](#custom-stopping-strings)
+++

## Restableciendo Plantillas

Puede restaurar las plantillas predeterminadas a su estado original. Esto se puede hacer a través de la interfaz de usuario o eliminando manualmente los archivos de datos relevantes.

### Restablecimiento de UI

1. Abra el menú **<i class="fa-solid fa-font"></i> Formato Avanzado**.
2. Elija la plantilla que desea restablecer.
3. Haga clic en el botón **<i class="fa-solid fa-recycle"></i> Restablecer plantilla actual**.
4. Confirme la acción cuando se le solicite.

### Restablecimiento Manual

!!!
Asegúrese de que la configuración `skipContentCheck` esté establecida en `false` en [config.yaml](/Administration/config-yaml.md#data-configuration), de lo contrario, la verificación de contenido no se activará.
!!!

1. Navegue a su directorio de datos de usuario (consulte [Rutas de datos](/Installation/index.md#data-paths) para obtener más detalles).
2. Elimine el archivo `content.log` de la raíz de su directorio de datos de usuario. Este archivo realiza un seguimiento de los archivos predeterminados copiados para su usuario.
3. Elimine los archivos JSON de plantilla de los subdirectorios relevantes (`context`, `instruct`, `sysprompt`, etc.).
4. Reinicie el servidor SillyTavern. La aplicación volverá a llenar el contenido predeterminado, restaurando cualquier plantilla predeterminada eliminada.

## Plantillas Definidas por Backend

!!! Se aplica a: Text Completion APIs
No aplicable a Chat Completion APIs ya que utilizan un constructor de solicitudes diferente.
!!!

Algunas fuentes de Text Completion proporcionan la capacidad de elegir automáticamente plantillas recomendadas por el autor del modelo. Esto funciona comparando un hash de la plantilla de chat definida en el archivo `tokenizer_config.json` del modelo con una de las plantillas predeterminadas de SillyTavern.

1. **<i class="fa-solid fa-bolt"></i> Derivar plantillas** debe estar habilitada en el menú **<i class="fa-solid fa-font"></i> Formato Avanzado**. Esto se puede aplicar a Context, Instruct, o ambos.
2. Se debe elegir un backend compatible como una fuente de Text Completion. Actualmente solo llama.cpp y KoboldCpp soportan derivar plantillas.
3. El modelo debe reportar correctamente sus metadatos cuando se establece la conexión con la API. Si esto no funcionó, intente actualizar el backend a la última versión.
4. El hash de la plantilla de chat reportado debe coincidir con el de las [plantillas conocidas de SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js). Esto solo cubre plantillas predeterminadas, como Llama 3, Gemma 2, Mistral V7, etc.
5. Si el hash coincide, la plantilla se seleccionará automáticamente si existe en la lista de plantillas (es decir, no fue renombrada o eliminada).

## Solicitud del Sistema

!!! Se aplica a: Text Completion APIs
Para configuración equivalente en Chat Completion APIs, use [Administrador de Solicitudes](prompt-manager.md). La **Solicitud Principal** es el equivalente de la Solicitud del Sistema en Chat Completion APIs.
!!!

La Solicitud del Sistema define las instrucciones generales que el modelo debe seguir. Establece el tono y el contexto de la conversación. Por ejemplo, le dice al modelo que actúe como un asistente de IA, un socio de escritura, o un personaje ficticio.

La Solicitud del Sistema es parte de la [Cadena de Historial](context-template.md#story-string) y generalmente la primera parte de la solicitud que recibe el modelo.

Consulte la [guía de solicitudes](index.md#main-prompt-system-prompt) para obtener más información sobre la Solicitud del Sistema.

## Plantilla de Contexto

!!! Se aplica a: Text Completion APIs
Para configuración equivalente en Chat Completion APIs, use [Administrador de Solicitudes](prompt-manager.md).
!!!

Por lo general, los modelos de IA requieren que proporcione los datos del personaje de una manera específica. SillyTavern incluye una lista de reglas de conversión prefabricadas para diferentes modelos, pero puede personalizarlas como desee.

Las opciones para esta sección se explican en [Plantilla de Contexto](context-template.md).

## Tokenizer

Un Tokenizer es una herramienta que divide un texto en unidades más pequeñas llamadas tokens. Estos tokens pueden ser palabras individuales o incluso partes de palabras, como prefijos, sufijos o puntuación. Una regla general es que un token generalmente corresponde a 3~4 caracteres de texto.

Las opciones para esta sección se explican en [Tokenizer](tokenizer.md).

## Cadenas de Parada Personalizadas

Acepta una matriz serializada en JSON de cadenas de parada. Ejemplo: `["\n", "\nUser:", "\nChar:"]`. Si no está seguro sobre el formato, use un [validador JSON en línea](https://jsonlint.com/). Si la salida del modelo **termina** con alguna de las cadenas de parada, serán eliminadas de la salida.

APIs Soportadas:

1. KoboldAI Classic (versiones 1.2.2 y superior) o KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama, etc.
4. NovelAI
5. OpenAI (máx 4 cadenas) y APIs compatibles
6. OpenRouter (tanto Text como Chat Completion)
7. Claude
8. Google AI Studio
9. MistralAI

## Comenzar Respuesta Con

!!! Nota
De forma predeterminada, el prefijo Comenzar Respuesta Con no se mostrará en el mensaje resultante. Habilite "Mostrar prefijo de respuesta en chat" para mostrarlo.
!!!

### Text Completion APIs

Precarga la última línea de la solicitud, obligando al modelo a continuar desde ese punto. Esto es útil para hacer cumplir el contenido, como empujar hacia el [Razonamiento del Modelo](/Usage/Prompts/reasoning.md) con el prefijo definido:

```txt
<think>
Sure!
```

### Chat Completion APIs

Agrega un mensaje de rol de asistente al final de la solicitud. Para algunos modelos de backend, esto es equivalente a precarga de la respuesta del modelo, pero algunos pueden no soportar eso en absoluto y fallarán con un error de validación. Si no está seguro, deje este campo vacío.
