---
route: /extensions/websearch/
---

# Búsqueda Web

Agrega resultados de búsqueda web a los mensajes del LLM.

!!! Nota
Algunas fuentes de [Chat Completion](/Usage/API_Connections/openai.md) proporcionan funcionalidad integrada de búsqueda web. En este caso, esta extensión será en gran medida redundante. Verifica el panel **<i class="fa-solid fa-sliders"></i> Configuración de Respuesta de IA** para el botón "Habilitar búsqueda web". Por ejemplo, esto está disponible para backends Claude, Google AI Studio / Vertex AI, xAI, y OpenRouter.
!!!

## Fuentes disponibles

### Complemento Selenium

Requiere que un complemento de servidor oficial esté instalado y habilitado.

Consulta [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) para más detalles.

Admite motores de búsqueda Google y DuckDuckGo.

### API de Extras

Requiere un módulo `websearch` y un navegador web Chrome/Firefox instalado en la máquina host.

Admite motores de búsqueda Google y DuckDuckGo.

### SerpApi

Requiere una clave de API.

Obtén la clave aquí: <https://serpapi.com/dashboard>

### SearXNG

Requiere una URL de instancia SearXNG (privada o pública). Utiliza formato HTML para resultados de búsqueda.

Cadena de preferencias SearXNG: obtenida de SearXNG - preferencias - COOKIES - Copiar hash de preferencias

Más información: <https://docs.searxng.org/>

### Tavily AI

Requiere una clave de API.

Obtén la clave aquí: <https://app.tavily.com/>

### KoboldCpp

La URL de KoboldCpp debe proporcionarse en la configuración de la API de Finalización de Texto. La versión de KoboldCpp debe ser >= 1.81.1 y el módulo WebSearch debe estar habilitado al iniciar: habilita Red => Habilitar WebSearch en el iniciador GUI o añade `--websearch` a la línea de comandos.

Ver: <https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

Requiere una clave de API.

Obtén la clave aquí: <https://serper.dev/>

## Cómo usar

1. Asegúrate de usar la última versión de SillyTavern.
2. Instala la extensión a través del menú "Descargar Extensiones y Activos" en SillyTavern.
3. Abre la configuración de la extensión "Búsqueda Web", establece tu clave de API o conéctate a Extras, y habilita la extensión.
4. Los resultados de búsqueda web se añadirán al mensaje de forma orgánica mientras chateas. **Solo los mensajes del usuario activan la búsqueda.**
5. Para incluir resultados de búsqueda de forma más orgánica, envuelve las consultas de búsqueda con comillas simples: ```Cuéntame sobre la `película reciente de Ryan Gosling`.``` producirá una consulta de búsqueda `película reciente de Ryan Gosling`.
6. Opcionalmente, configura los ajustes a tu gusto.

## Configuración

### General

1. Habilitado - alterna la extensión encendida y apagada.
2. Fuentes = establece la fuente de resultados de búsqueda.
3. Duración de la Caché - cuánto tiempo (en segundos) los resultados de búsqueda se cachean para tu mensaje. Por defecto = una semana.

### Configuración de Mensaje

1. Presupuesto de Mensaje - establece la capacidad máxima del texto insertado (en caracteres de texto, NO fichas). Regla general: 1 ficha ~ 3-4 caracteres, ajusta según los límites de contexto de tu modelo. Por defecto = 1500 caracteres.
2. Plantilla de Inserción - cómo el resultado se inserta en el mensaje. Admite la macro habitual + macro especial: \{\{query\}\} para la consulta de búsqueda y \{\{text\}\} para resultados de búsqueda.
3. Posición de Inyección - dónde va el resultado en el mensaje. Las mismas opciones que para la Nota del Autor: como inyección en chat o antes/después del mensaje del sistema.

### Activación de Búsqueda

1. Usar herramienta de función - utiliza [llamada de función](/For_Contributors/Function-Calling.md) para activar búsqueda o raspar páginas web. Debe usar una API de Chat Completion compatible y estar habilitada en los ajustes de Respuesta de IA. **Deshabilita todos los otros métodos de activación cuando está activa.**
2. Usar Comillas Simples - habilita la activación de búsqueda usando palabras encerradas en comillas simples.
3. Usar Frases Activadoras - habilita la activación de búsqueda usando frases activadoras.
4. Expresiones regulares - proporciona una expresión regular de sabor JS para coincidir con el mensaje del usuario. Si la regex coincide, se activará la búsqueda con la consulta dada. La consulta de búsqueda admite `{{macros}}` y sintaxis $1 para hacer referencia al grupo coincidente. Ejemplo: la expresión regular `/what is happening in (.*)/i` para la consulta de búsqueda `news in $1` coincidirá con un mensaje que contenga `what is happening in New York` y activará la búsqueda con la consulta `news in New York`.
5. Frases Activadoras - añade frases que activarán la búsqueda, una por una. Puede estar en cualquier lugar del mensaje, y la consulta comienza desde la palabra activadora y abarca un total de "Palabras Máximas". Para excluir un mensaje específico del procesamiento, debe comenzar con un punto, por ejemplo, `.What do you think?`. Prioridad de activadores: primero por orden en el cuadro de texto, luego el primero en el mensaje del usuario.
6. Palabras Máximas - cuántas palabras se incluyen en la consulta de búsqueda (incluyendo la frase activadora). Google tiene un límite de aproximadamente 32 palabras por mensaje. Por defecto = 10 palabras.

### Raspado de Página

1. Visitar Enlaces - el texto se extraerá de las páginas de resultados de búsqueda visitadas y se guardará en un archivo adjunto.
2. Recuento de Visitas - cuántos enlaces se visitarán y analizarán para obtener texto.
3. Lista Negra de Dominio de Visita - dominios del sitio a excluir de visitación. Uno por línea.
4. Encabezado de Archivo - plantilla de encabezado de archivo, insertada al inicio del archivo de texto, tiene una macro adicional \{\{query\}\}.
5. Encabezado de Bloque - plantilla de bloque de enlace, insertada con el contenido analizado de cada enlace. Usa la macro \{\{link\}\} para URL de página y \{\{text\}\} para contenido de página.
6. Guardar Destino - dónde guardar los resultados del raspado. Opciones posibles: archivos adjuntos de mensaje de activación, o archivos adjuntos de chat de Banco de Datos, o solo imágenes (si la fuente lo admite).
7. Incluir Imágenes - adjuntar imágenes relevantes al chat. Requiere una fuente que admita imágenes (ver abajo).

## Más información

Los resultados de búsqueda de la consulta más reciente permanecerán incluidos en el mensaje hasta que se encuentre la siguiente consulta válida.
Si deseas hacer preguntas adicionales sin activar accidentalmente la búsqueda, comienza tu mensaje con un punto.

!!!info
La herramienta de función de Búsqueda Web siempre anula otros activadores si está habilitada y disponible.
!!!

Prioridad de activadores (si múltiples están habilitados):

1. Comillas simples.
2. Expresiones regulares.
3. Frases activadoras.

Para descartar todas las consultas anteriores del procesamiento, comienza el mensaje del usuario con un signo de exclamación, por ejemplo, un mensaje de usuario `!Now let's talk about...` descartará este y cada mensaje por encima de él.

Esta extensión también proporciona un comando de barra `/websearch` para usar en STscript. Más información aquí: [Referencia del Lenguaje STscript](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – realiza una consulta de búsqueda web. Usa argumentos nombrados para especificar qué devolver - fragmentos de página (predeterminado: on), páginas parseadas completas (predeterminado: off) o ambas.

Ejemplo: /websearch links=off snippets=on cómo hacer un sándwich
```

### ¿Qué puede incluirse en el resultado de búsqueda?

**Tesauro:**

- Cuadro de respuesta: Respuesta directa a la pregunta.
- Gráfico de conocimiento: Conocimiento enciclopédico sobre el tema.
- Fragmentos de página: Extractos relevantes de las páginas web.
- Preguntas relevantes: Preguntas y respuestas a temas similares.
- Imágenes: Imágenes relevantes.

#### SerpApi

1. Cuadro de respuesta.
2. Gráfico de conocimiento.
3. Fragmentos de página (máx 10).
4. Preguntas relevantes (máx 10).
5. Imágenes (máx 10).

#### Complemento Selenium e API de Extras

1. Google - cuadro de respuesta, gráfico de conocimiento, fragmentos de página.
2. DuckDuckGo - fragmentos de página.

**Complemento Selenium** puede proporcionar adicionalmente imágenes.

#### SearXNG

1. Cuadro de información.
2. Fragmentos de página.
3. Imágenes.

#### Tavily AI

1. Respuesta.
2. Contenidos de página.
3. Imágenes (hasta 5).

#### KoboldCpp

1. Títulos de página.
2. Fragmentos de página.

#### Serper

1. Cuadro de respuesta.
2. Gráfico de conocimiento.
3. Fragmentos de página.
4. Preguntas relevantes.
5. Imágenes.
