---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Llamadas de Función

Las llamadas de función permiten agregar funcionalidad dinámica a tus extensiones al permitir que el LLM use datos estructurados que luego puedes usar para desencadenar una funcionalidad específica de la extensión.

## Casos de uso de ejemplo

1. Consultar APIs externas para obtener información adicional (noticias, clima, búsqueda web, etc.).
2. Realizar cálculos o conversiones basados en la entrada del usuario.
3. Almacenar y recuperar recuerdos o hechos importantes, incluidas consultas de RAG y base de datos.
4. Introducir verdadera aleatoriedad en la conversación (tiradas de dados, lanzamientos de monedas, etc.).

## Extensiones oficialmente compatibles que utilizan llamadas de función

1. [Generación de Imágenes](/extensions/Stable-Diffusion.md) (integrada) - genera imágenes basadas en indicaciones del usuario.
2. [Búsqueda Web](/extensions/WebSearch.md) - desencadena una búsqueda web para una consulta.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - obtiene las noticias más recientes de los feeds RSS.
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - obtiene información meteorológica de AccuWeather.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - lanza dados para juegos de D&D.

## Requisitos previos y limitaciones

1. Esta función solo está disponible para ciertos orígenes de Chat Completion: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI, DeepSeek, AI/ML API y fuentes de API personalizadas.
2. Las APIs de Text Completion no admiten llamadas de función, pero algunos backends alojados localmente como Ollama y TabbyAPI pueden ejecutarse en modo compatible con Custom OpenAI bajo Chat Completion.
3. El soporte para llamadas de función debe ser permitido explícitamente por el usuario primero. Esto se realiza habilitando la opción "Habilitar llamadas de función" en el panel de Configuración de Respuesta de IA.
4. No hay garantía de que un LLM realice alguna llamada de función. La mayoría de ellos requiere una "activación" explícita a través del prompt (por ejemplo, el usuario pidiendo "Lanza un dado", "Obtén el clima", etc.).
5. No todos los prompts pueden desencadenar una llamada de herramienta. Las continuaciones, suplantación, prompts de fondo ('silenciosos') no pueden desencadenar una llamada de herramienta. Aún pueden usar llamadas de herramienta exitosas anteriores en sus respuestas.

## Cómo hacer una herramienta de función

### Verificar si la función es compatible

Para determinar si la función de llamada de herramienta de función es compatible, puedes llamar a `isToolCallingSupported` desde el objeto `SillyTavern.getContext()`. Esto verificará si la API actual admite llamadas de herramienta de función y si está habilitado en la configuración. Aquí hay un ejemplo de cómo verificar si la función es compatible:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### Registrar una función

Para registrar una herramienta de función, necesitas llamar a la función `registerFunctionTool` desde el objeto `SillyTavern.getContext()` y pasar los parámetros requeridos. Aquí hay un ejemplo de cómo registrar una herramienta de función:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### Anular el registro de una función

Para desactivar una herramienta de función, necesitas llamar a la función `unregisterFunctionTool` desde el objeto `SillyTavern.getContext()` y pasar el nombre de la herramienta de función a deshabilitar. Aquí hay un ejemplo de cómo anular el registro de una herramienta de función:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## Consejos y trucos

1. Las llamadas de herramienta exitosas se guardan como parte del historial visible y se mostrarán en la UI de chat, por lo que puedes inspeccionar los parámetros y resultados reales. Si esto no es deseable, establece el flag `stealth: true` al registrar una herramienta de función.
2. Si no deseas ver la llamada de herramienta en el historial de chat. Si deseas estilizar u ocultarlas con CSS personalizado, apunta a la clase `toolCall` en elementos `.mes`, es decir, `.mes.toolCall { display: none; }` o `.mes.toolCall { color: #999; }`.
