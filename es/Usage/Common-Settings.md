---
order: 160
icon: sliders
route: /usage/common-settings/
---

# Configuración Común

Estos ajustes controlan el proceso de muestreo al generar texto usando un modelo de lenguaje. El significado de estos ajustes es universal para todos los backends soportados.

## Configuración de Contexto

### Respuesta (tokens)

El número máximo de tokens que la API generará para responder.

- Cuanto mayor sea la longitud de respuesta, más tiempo tardará en generar la respuesta.
- Si es compatible con la API, puede habilitar `Streaming` para mostrar la respuesta poco a poco mientras se genera.
- Cuando `Streaming` está desactivado, las respuestas se mostrarán todas a la vez cuando estén completas.

### Contexto (tokens)

El número máximo de tokens que SillyTavern enviará a la API como el prompt, menos la longitud de la respuesta.

- El contexto comprende información del personaje, prompts del sistema, historial de chat, etc.
- Una línea punteada entre mensajes denota el rango de contexto para el chat. Los mensajes por encima de esa línea no se envían a la IA.
- Para ver una composición del contexto después de generar el mensaje, haga clic en la opción de mensaje `Prompt Itemization` (expanda el menú `...` y haga clic en el icono de cuadrado rayado).

## Parámetros de Muestreador

### Temperatura

La temperatura controla la aleatoriedad en la selección de tokens:

- Baja temperatura (<1.0) conduce a texto más predecible, favoreciendo tokens de mayor probabilidad
- Alta temperatura (>1.0) aumenta la creatividad y diversidad en la salida al dar a tokens de menor probabilidad una mejor oportunidad.

Establezca en 1 para las probabilidades originales.

### Penalización por Repetición

Intenta frenar la repetición penalizando tokens basándose en la frecuencia con la que ocurren en el contexto.

Establezca el valor en 1 para desactivar su efecto.

#### Rango de Penalización por Repetición

Cuántos tokens desde el último token generado se considerarán para la penalización por repetición. Esto puede romper las respuestas si se establece demasiado alto, ya que palabras comunes como "the, a, and," etc. serán penalizadas más.

Establezca el valor en 0 para desactivar su efecto.

#### Pendiente de Penalización por Repetición

Si tanto este como `Repetition Penalty Range` están por encima de 0, la penalización por repetición tendrá un mayor efecto al final del prompt. Cuanto mayor sea el valor, más fuerte será el efecto.

Establezca el valor en 0 para desactivar su efecto.

### Top K

Top K establece una cantidad máxima de tokens principales que se pueden elegir. Por ejemplo, si Top K es 20, esto significa que solo se mantendrán los 20 tokens con mayor rango (independientemente de que sus probabilidades sean diversas o limitadas).

Establezca en 0 (o -1, dependiendo de su backend) para desactivar.

### Top P

Top P (también conocido como nucleus sampling) suma todos los tokens principales necesarios para sumar el porcentaje objetivo. Si los 2 tokens principales son ambos 25%, y Top P es 0.50, solo se consideran los 2 tokens principales.

Establezca el valor en 1 para desactivar su efecto.

### Typical P

Muestreo Típico P prioriza tokens basándose en su desviación de la entropía promedio del conjunto. Mantiene tokens cuya probabilidad acumulativa está cerca de un umbral predefinido (por ejemplo, 0.5), enfatizando los que tienen contenido de información promedio.

Establezca el valor en 1 para desactivar su efecto.

### Min P

Limita el grupo de tokens cortando tokens de baja probabilidad en relación con el token superior. Produce respuestas más coherentes pero también puede empeorar la repetición si se establece demasiado alto.

- Funciona mejor con valores bajos como `0.1-0.01`, pero se puede establecer más alto con una `Temperature` alta. Por ejemplo: `Temperature: 5, Min P: 0.5`

Establezca el valor en 0 para desactivar su efecto.

### Top A

Top A establece un umbral para la selección de tokens basándose en el cuadrado de la probabilidad del token más alto. Por ejemplo, si el valor de Top-A es 0.2 y la probabilidad del token superior es 50%, los tokens con probabilidades por debajo del 5% (0.2 * 0.5^2) se excluyen.

Establezca el valor en 0 para desactivar su efecto.

### Muestreo sin Cola

Muestreo sin Cola (TFS) busca una cola de tokens de baja probabilidad en la distribución, analizando la tasa de cambio en las probabilidades de tokens usando derivadas. Retiene tokens hasta un umbral (por ejemplo, 0.3) basándose en la segunda derivada normalizada. Cuanto más cerca de 0, más tokens descartados.

Establezca el valor en 1 para desactivar su efecto.

### Factor de Suavizado

Aumenta la probabilidad de tokens de alta probabilidad mientras disminuye la probabilidad de tokens de baja probabilidad usando una transformación cuadrática. Pretende producir respuestas más creativas independientemente de `Temperature`.

- Funciona mejor sin muestreadores de truncación como `Top K`, `Top P`, `Min P`, etc.

Establezca el valor en 0 para desactivar su efecto.

### Temperatura Dinámica

Escala la temperatura dinámicamente basándose en la probabilidad del token superior. Pretende producir salidas más creativas sin sacrificar la coherencia.

- Acepta un rango de temperatura de mínimo a máximo. Por ejemplo: `Minimum Temp: 0.75` y `Minimum Temp: 1.25`
- `Exponent` aplica una curva exponencial basándose en el token superior.

Desmarque para desactivar su efecto.

### Corte Epsilon

El corte Epsilon establece un piso de probabilidad por debajo del cual los tokens se excluyen del muestreo. En unidades de 1e-4; un valor razonable es 3.

Establezca en 0 para desactivar.

### Corte Eta

El corte Eta es el parámetro principal de la técnica especial de Muestreo Eta. En unidades de 1e-4; un valor razonable es 3. Consulte el artículo [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191) para más detalles.

Establezca en 0 para desactivar.

### Penalización DRY por Repetición

DRY penaliza tokens que extenderían el final de la entrada en una secuencia que ha ocurrido previamente en la entrada. Si desea permitir repetir ciertas secuencias textualmente (por ejemplo, nombres), puede agregarlas a la lista de separadores de secuencias. Consulte la Pull Request [aquí](https://github.com/oobabooga/text-generation-webui/pull/5677).

Establezca el multiplicador en 0 para desactivar.

### Excluir Opciones Principales (XTC)

El algoritmo de muestreo XTC elimina los tokens más probables de la consideración en lugar de podar los tokens menos probables. Elimina todo excepto el token menos probable que cumple con un umbral dado, con una probabilidad dada. Esto asegura que al menos una opción "viable" permanezca, manteniendo la coherencia. Consulte la Pull Request [aquí](https://github.com/oobabooga/text-generation-webui/pull/6335).

Establezca la probabilidad en 0 para desactivar.

### Mirostat

Mirostat iguala la perplejidad de salida con la de la entrada, evitando así la trampa de repetición (donde, a medida que la inferencia autorregresiva produce texto, la perplejidad de la salida tiende a cero) y la trampa de confusión (donde la perplejidad diverge). Para más detalles, consulte el artículo [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966).

El modo elige la versión de Mirostat.

- 0 = desactivar,
- 1 = Mirostat 1.0 (solo llama.cpp),
- 2 = Mirostat 2.0.

### Búsqueda de Haz

Un algoritmo voraz de fuerza bruta utilizado en el muestreo de LLM para encontrar la secuencia más probable de palabras o tokens. Expande múltiples secuencias candidatas a la vez, manteniendo un número fijo (ancho de haz) de secuencias principales en cada paso.

### Top nsigma

Un método de muestreo que filtra logits basándose en sus propiedades estadísticas. Mantiene tokens dentro de n desviaciones estándar del valor logit máximo, proporcionando una alternativa más simple al muestreo top-p/top-k mientras se mantiene la estabilidad del muestreo en diferentes temperaturas.
