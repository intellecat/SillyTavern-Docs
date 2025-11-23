---
order: 60
route: /usage/prompts/cfg/
---

# CFG

Página escrita por: kingbri

Contribuidores: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## ¿Qué es?

CFG, o Classifier Free Guidance, es un método que se usa para ayudar a hacer que partes de un prompt sean menos o más prominentes.

### APIs de Backend Soportadas

Actualmente, los backends soportados son textgen WebUI de oobabooga, NovelAI y TabbyAPI.
NovelAI tenía su propia [documentación para CFG](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html).

ADVERTENCIA: CFG aumenta el uso de VRAM debido a la ingesta de más de 1 prompt. Si la memoria de tu GPU se agota mientras generas un prompt con CFG activado, considera reducir tu tamaño de contexto, usar un modelo con menos parámetros o desactivar CFG por completo.

---

## Configuración

Acceder a la configuración de CFG es lo mismo que acceder a la nota del Autor:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

Y así se ve el panel de CFG:

![CFGchatpanelpng](/static/cfg-panel.png)

Hay cuatro menús desplegables en el panel de CFG:

- CFG de Chat

  - Limita la escala de CFG y los prompts solo a este chat
- CFG de Personaje

  - Limita la escala de CFG y los prompts al personaje especificado
- CFG Global

  - Anula globalmente la escala de CFG y los prompts (¡también anula el preset del modelo!)
- CFG Configuración Avanzada (anteriormente llamada CFG Prompt Cascading)

  - Un lugar para combinar prompts de los 3 menús desplegables anteriores y establecer la profundidad de inserción.

NOTA: Si la escala de orientación se establece en 1, no se enviará nada ya que es cuando CFG está en estado "apagado".

#### Chats de Grupo

En chats de grupo, el panel de escala de CFG se ve así:

![CFGpanelgcpng](/static/cfg-groups.png)

El cambio principal es que el CFG de personaje se elimina y una casilla de verificación llamada `Use Character CFG Scales` está presente en el menú desplegable de CFG de chat. Esto permite usar la escala de orientación del personaje actual en lugar de la escala de CFG de chat establecida.

La utilidad principal de esta función es alterar la escala basándose en las necesidades individuales de cada personaje.

Además, marcar la casilla `Character Negatives` en el cascading de prompts añadirá los prompts negativos de personaje independientes junto con los de chat (si están habilitados).

---

## Conceptos

### ¿No está esto en Stable Diffusion?

Sí y no. CFG con LLMs funciona de manera diferente a la que uno podría estar acostumbrado en Stable Diffusion. CFG basado en LLM funciona en el principio de "prompt mixing". La fórmula de CFG toma un prompt positivo y uno negativo, luego mezcla las *diferencias* entre ellos. Desde allí, se envía un prompt combinado y se genera una respuesta.

Aquí hay una ilustración para ayudar a visualizar este concepto. El rojo representa el prompt negativo, el azul representa el prompt neutral y el púrpura representa el resultado mezclado que se interpreta. Todo el espacio en blanco es el mismo en los 3 prompts, por lo que no se utilizan para la mezcla de CFG.

![stcfgdiagrampng](/static/cfg-diagram.png)

Si deseas saber más sobre CFG y LLMs, el documento original de Vermifuge está ubicado aquí. Te sugiero que lo leas/escuches:

- Documento - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- Versión de audio - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### ¿Necesito prompts de CFG?

¡No! Los prompts de CFG son completamente opcionales. Solo ajustar la escala de orientación por encima de `1` también ayudará a producir un efecto en las respuestas, lo que puede acentuar chats e interacción de personajes.

### ¿Qué hace un buen prompt de CFG?

Entonces, establecimos que los prompts de CFG no son lo mismo que las etiquetas negativas e incrustaciones de Stable Diffusion. ¿Cómo hacemos un prompt?

Advertencia: Esto supone que has creado un personaje usando PLists y Ali:Chat. Si no lo has hecho, siéntete libre de experimentar con varias técnicas de prompting.

Digamos que tengo un personaje llamado "John". John se supone que debe sentirse feliz y emocionado todo el tiempo según sus diálogos de ejemplo. Sin embargo, al chatear con John, a veces está triste y deprimido.

Para eliminar esto, ¡CFG viene al rescate! Solo haz el prompt negativo `[John's feelings: sad, depressed]` para ayudar a eliminar las partes de tristeza. Opcionalmente puedes hacer el prompt positivo `[John's feelings: happy, joyful]` para resaltar aún más las partes felices de John.

### Prompts Positivos

Ya cubrí esto en la sección anterior, pero me gustaría tocarlo un poco más. Los prompts positivos se utilizan para acentuar aún más partes de un personaje. Usemos a John nuevamente como nuestro ejemplo. Al hacerlo más feliz con un prompt positivo de `[John's feelings: happy, joyful]`, John debería empezar a producir diálogos con un sentimiento más feliz que si el prompt positivo no estuviera incluido.

### Pero...

Estas son solo **directrices generales** basadas en la experiencia con un formato de personaje específico. Hay muchas otras formas de crear prompts que deberías experimentar. ¡Siéntete libre de compartir tus ideas con otros usuarios!

### Escala de Orientación

Aquí hay una regla general. Una escala de orientación de `1` significa que CFG está deshabilitado. De hecho, SillyTavern no enviará nada a tu backend si la escala de orientación es 1. Una escala de orientación `>1` proporcionará los resultados mostrados en las otras secciones en varios grados.

Sin embargo, una escala de orientación de `<1` dará el efecto *opuesto* ya que el prompt negativo se utiliza como el prompt principal aquí.

Usemos el ejemplo con John nuevamente. El prompt negativo es `[John's feelings: sad, depressed]` y el prompt positivo es `[John's feelings: happy, joyful]` con una escala de orientación de `0.8`.

Esto a su vez acentuará más el prompt *negativo* y verás a John empezar a actuar más triste que lo normal en lugar de más feliz.

TL;DR; Usa una escala de orientación de `1.5` y trabaja hacia arriba y hacia abajo desde allí basándote en tus salidas.

### Cascading de Prompts

Los negativos y positivos pueden ser cascadeados entre tipos de CFG (siendo los tipos per-chat, per-character y overrides globales). Ver el encabezado de Configuración para más información.

### Profundidad de Inserción

Sigue la regla básica: Cuanto más bajo esté algo ubicado en el prompt, más influyente es en la respuesta. Para chatear, recomiendo usar la profundidad predeterminada de `1` ya que es muy flexible con otros componentes de SillyTavern.

Sin embargo, si deseas experimentar, una profundidad de inserción de `0` está disponible. Sin embargo, estos pueden alterar drásticamente cómo se verá tu respuesta y NO se recomienda usar cascading de prompts aquí!
