---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# Razonamiento

En los modelos de lenguaje, el razonamiento (también conocido como pensamiento del modelo) se refiere a una técnica de cadena de pensamiento (CoT) que refleja la resolución de problemas humana a través del análisis paso a paso. SillyTavern proporciona varias características que hacen que el uso de modelos de razonamiento sea más eficiente y consistente en todos los backends soportados.

## Problemas comunes

1. Al usar modelos de razonamiento, el proceso de razonamiento interno del modelo consume parte de tu asignación de tokens de respuesta, incluso si este razonamiento no se muestra en la salida final (por ejemplo, o3-mini o Gemini Thinking). Si notas que tus respuestas vuelven incompletas o vacías, deberías intentar ajustar la configuración de Longitud Máxima de Respuesta que se encuentra en el panel **<i class="fa-solid fa-sliders"></i> Configuración de Respuesta de IA**. Para modelos de razonamiento, es típico usar límites de tokens significativamente más altos - en cualquier lugar de 1024 a 4096 tokens - en comparación con modelos conversacionales estándar.

## Configuración

!!!
La mayoría de las configuraciones relacionadas con razonamiento se pueden configurar en la sección "Razonamiento" del panel **<i class="fa-solid fa-font"></i> Formato Avanzado**.
!!!

Los bloques de razonamiento aparecen en el chat como secciones de mensaje colapsables. Se pueden agregar manualmente, automáticamente por el backend, o a través del análisis de respuesta (ver abajo).

De forma predeterminada, los bloques de razonamiento están colapsados para ahorrar espacio. Haz clic en un bloque para expandirlo y ver su contenido. Puedes configurar bloques para expandirse automáticamente habilitando **Auto-Expandir** en la configuración de razonamiento.

Cuando un bloque de razonamiento está expandido, puedes copiar o editar su contenido usando los botones **<i class="fa-solid fa-copy"></i> Copiar** y **<i class="fa-solid fa-pencil"></i> Editar**.

Algunos modelos soportan razonamiento, pero no enviarán sus pensamientos de vuelta. Es posible mostrar el bloque de razonamiento con tiempo de razonamiento para aquellos alternando la configuración **Mostrar Oculto**.

## Agregando Razonamiento

### Manualmente

Agrega un bloque de razonamiento a cualquier mensaje a través del menú **<i class="fa-solid fa-pencil"></i> Editar Mensaje**. Haz clic en **<i class="fa-solid fa-lightbulb"></i>** mientras editas para agregar una sección de razonamiento. Las extensiones de terceros también pueden agregar razonamiento escribiendo en el campo `extra.reasoning` del objeto del mensaje antes de agregarlo al chat.

### Con un Comando

Usa el comando STscript `/reasoning-set` para agregar razonamiento a un mensaje. El comando toma `at` (ID del mensaje, por defecto el último mensaje) y texto de razonamiento como argumentos.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### Por Backend

Si tu backend LLM elegido y el modelo soportan salida de razonamiento, habilitar "Solicitar razonamiento del modelo" en el panel **<i class="fa-solid fa-sliders"></i> Configuración de Respuesta de IA** agregará un bloque de razonamiento que contiene el proceso de pensamiento del modelo.

Fuentes soportadas:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

"Solicitar razonamiento del modelo" no determina si un modelo hace razonamiento. Claude y Google (2.5 Flash) permiten que el modo de pensamiento sea alternado; ver [Esfuerzo de Razonamiento](#reasoning-effort).

### Por Análisis

Habilita "Auto-Analizar" en el panel **<i class="fa-solid fa-font"></i> Formato Avanzado** para analizar automáticamente el razonamiento de la salida del modelo.

La respuesta debe contener una sección de razonamiento envuelta en secuencias de Prefijo y Sufijo configuradas. Las secuencias proporcionadas de forma predeterminada corresponden al formato de razonamiento DeepSeek R1.

Ejemplo con prefijo `<think>` y sufijo `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Indicaciones con Razonamiento

De forma predeterminada, el contenido de bloques de razonamiento reconocidos no se envía de vuelta al modelo. Para incluir razonamiento en indicaciones, habilita "Agregar a Indicaciones" en el panel **<i class="fa-solid fa-font"></i> Formato Avanzado**. El contenido de razonamiento será envuelto en secuencias de Prefijo y Sufijo configuradas y separado por un Separador del contexto principal. La configuración numérica de Máximas Adiciones controla cuántos bloques de razonamiento pueden incluirse, contando desde el final de la indicación.

!!!
La mayoría de proveedores de modelos no recomiendan enviar CoT de vuelta al modelo en conversaciones multi-turno.
!!!

### Continuando desde Razonamiento

Un caso especial cuando el razonamiento puede ser enviado de vuelta al modelo sin tener el toggle "Agregar a Indicaciones" habilitado es cuando la generación es continuada (por ejemplo, presionando "Continuar" del menú **<i class="fa-solid fa-bars"></i> Opciones**), pero el mensaje siendo continuado contiene solo el razonamiento sin contenido real. Esto da al modelo una oportunidad de terminar un razonamiento incompleto e iniciar la generación del contenido principal. La indicación será enviada de la siguiente manera:

```txt
<think>
Incomplete reasoning...
```

## Scripts de Expresión Regular

Los scripts de expresión regular de la [extensión Regex](/extensions/Regex.md) pueden ser aplicados al contenido de bloques de razonamiento. Marca "Razonamiento" en la sección "Afecta" del editor de scripts para apuntar a bloques de razonamiento específicamente.

Las diferentes opciones de efemeridad afectan los bloques de razonamiento de las siguientes maneras:

1. Sin efemeridad: el contenido de razonamiento se cambia permanentemente.
2. Ejecutar al editar: el script de expresión regular será re-evaluado cuando el bloque de razonamiento sea editado.
3. Alterar visualización del chat: la expresión regular se aplica al texto de visualización del bloque de razonamiento, no al contenido subyacente.
4. Alterar indicaciones salientes: la expresión regular se aplica solo a bloques de razonamiento antes de que sean enviados al modelo.

## Esfuerzo de Razonamiento

Esfuerzo de Razonamiento es una configuración de Completitud de Chat en el panel **<i class="fa-solid fa-sliders"></i> Configuración de Respuesta de IA** que influye en cuántos tokens pueden potencialmente ser usados en razonamiento. El efecto de cada opción depende de la fuente conectada. Para las fuentes de abajo, Auto simplemente significa que el parámetro relevante no está incluido en la solicitud.

| Opción  | Claude (≤ 21333 if no streaming) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) |
| ------- | -------------------------------- | -------------------- | -------------------------------- | -------------------- | -------------------- |
| Modelos  | Opus 4, Sonnet 4/3.7             | o4-mini, o3\*, o1\*  | applicable models                | grok-3-mini          | sonar-deep-research  |
| Auto    | not specified, **no thinking**   | not specified        | not specified, effect depends    | not specified        | not specified        |
| Mínimo | budgets 1024 tokens              | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Bajo     | 15% of max response, min 1024    | "low"                | "low", or 20% of max response    | "low"                | "low"                |
| Medio  | 25% of max response, min 1024    | "medium"             | "medium", or 50% of max response | "low"                | "medium"             |
| Alto    | 50% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |
| Máximo | 95% of max response, min 1024    | "high"               | "high", or 80% of max response   | "high"               | "high"               |

- Para Claude, el presupuesto está limitado a 21333 si el streaming está deshabilitado. Si el presupuesto calculado sería menos de 1024, entonces la respuesta máxima se cambia a 2048.
- Para OpenRouter, Perplexity e AI/ML API, solo se envía una palabra clave de estilo OpenAI.

Google AI Studio y Vertex AI son de la siguiente manera:

| Modelo          | Auto (dynamic thinking) | Mínimo            | Bajo                          | Medio     | Alto       | Máximo               |
| -------------- | ----------------------- | ------------------ | ---------------------------- | ---------- | ---------- | --------------------- |
| 2.5 Pro        | thinkingBudget = -1     | 128                | 15% of max response, min 128 | 25% of max | 50% of max | lower of max or 32768 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **no thinking** | 15% of max response          | 25% of max | 50% of max | lower of max or 24576 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **no thinking** | 15% of max response, min 512 | 25% of max | 50% of max | lower of max or 24576 |

- Para Gemini 2.5 Pro y 2.5 Flash/Lite, el presupuesto está limitado a 32768 o 24576 tokens respectivamente, independientemente de la configuración de streaming.
