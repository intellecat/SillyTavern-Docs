---
route: /extensions/regex/
---

# Regex

## ¿Qué es?

La extensión Regex permite al usuario detectar automáticamente patrones específicos en una cadena de texto (llamados 'secuencias') y aplicar manipulaciones (reemplazos) a ellos. Puede ser una herramienta poderosa cuando se usa en conjunto con otras características de SillyTavern como [Quick Replies o STscript](/For_Contributors/st-script.md), o simplemente una forma de eliminar ciertas palabras del chat.

## Enlaces útiles

**Este documento no explicará el proceso de escribir una secuencia RegEx en profundidad. Hay muchos recursos en línea para ayudarte con eso.**

- [https://regexr.com](https://regexr.com)

- [https://regex101.com](https://regex101.com)

- [https://extendsclass.com/regex-tester.html](https://extendsclass.com/regex-tester.html)

- [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression)

## Requisitos previos

Regex es una extensión integrada de SillyTavern, por lo que no se requiere configuración adicional.

Puedes encontrar su configuración en el panel **<i class="fa-solid fa-cubes"></i> Extensions**.

## Casos de uso comunes

RegEx se utiliza frecuentemente para aplicar una función de buscar-reemplazar en ciertas palabras del chat, para agregar estilos markdown a ciertas palabras o tipos de oraciones, o para devolver un valor booleano a un STscript.

## Lista de scripts

![RegEx Extension Script List](/static/extensions/regex-listview.png)

- Los botones en la parte superior se utilizan para crear un nuevo script.
  - Los scripts 'Global' se aplicarán a todos los personajes y se guardarán en `settings.json`.
  - Los scripts 'Scoped' solo se aplicarán al personaje activo actualmente y se guardarán en los datos de la tarjeta del personaje.
- 'Import' te permite importar scripts RegEx que fueron exportados desde otra instancia de SillyTavern.

Debajo de esto hay una lista de tus scripts con algunos botones de acción.

- Los tiradores de arrastre (tres barras horizontales a la izquierda del nombre del script) te permiten arrastrar/soltar los scripts en cualquier orden que desees.
- El interruptor principal de encendido/apagado se puede alternar rápidamente para habilitar o deshabilitar el script sin cambiar nada más. Los scripts deshabilitados se muestran con estilo de ~~tachado~~. **Si un script está deshabilitado aquí, no se puede activar mediante Quick Reply o STscript.**
- El botón 'Edit' (lápiz) abrirá el editor de scripts RegEx.
- 'Move to scoped' (flecha hacia abajo) convertirá un script global a un script scoped y lo aplicará al personaje actual. A la inversa (flecha hacia arriba), convertiría un script scoped a global.
- 'Export' hará que tu navegador descargue un archivo `.json` exportado del Script, que luego puede ser compartido e importado a otra instancia de SillyTavern.
- 'Delete' (bote de basura) elimina el script.

## Editor de RegEx

![RegEx Editor](/static/extensions/regex-editor.png)

- **Test Mode** : Esto abrirá una vista de comparación en la parte superior del editor. Escribe algo de texto en la casilla 'Input' y los resultados de tu script RegEx se mostrarán en la casilla Output. Es una herramienta de depuración valiosa ya que actualizará la casilla Output en tiempo real conforme realizas cambios en la configuración del script.

- **Name** : La etiqueta para el script mostrada en la lista de scripts de la extensión. **Esto también se utiliza para dirigirse al script cuando se activa mediante comando slash o STscript.**

- **Find Regex** : Esta es la Expresión Regular que se utiliza para detectar tu patrón de texto objetivo. Esta es generalmente la parte más compleja de cualquier script RegEx y es el lugar más fácil para cometer errores. Consulta los enlaces en la parte superior de la página para obtener información sobre cómo escribir una secuencia RegEx. Esta casilla puede resolver los valores de [macros comunes de SillyTavern](/Usage/Characters/macros.md) (tales como \{\{user\}\}, \{\{char\}\}, etc) si 'Macros in Find Regex' está configurado para hacerlo (ver abajo).

- **Replace With**: Esto es lo que reemplazará la secuencia coincidente. En un ejemplo muy simple, si tu 'Find Regex' es `apple` y tu 'Replace With' es `orange`, la primera aparición de 'apple' sería automáticamente cambiada a 'orange' en cualquier texto donde se aplique el script.

  - Agregar la macro específica de la extensión \{\{match\}\} en esta casilla insertará la secuencia de texto completamente coincidente. Esto se utiliza comúnmente para aplicar estilos a palabras específicas. Volviendo al ejemplo anterior, si \*\*\{\{match\}\}\*\* se pusiera en la casilla 'Replace With' en su lugar, todas las apariciones de la palabra 'apple' serían reemplazadas con `**apple**`, lo que aplicaría el estilo markdown en negrita a ella.

  - Variables como $1, $2, $3 etc se pueden usar para insertar lo que se llaman 'Capture Groups'. Estos son subcadenas ubicadas en la secuencia de texto coincidente con la secuencia 'Find Regex'. **Tenga en cuenta que el uso de estas variables requiere que la expresión coincidente contenga conjuntos de paréntesis para definir qué parte de la cadena coincidente cuenta como un grupo capturado.** Consulta los enlaces en la parte superior para obtener referencia sobre cómo configurar Capture Groups.

- **Trim Out** : el texto colocado en esta casilla se eliminará de la secuencia de texto coincidente antes de que se aplique el proceso 'Replace With'. Por ejemplo, si nuestra coincidencia fue 'apple' y la casilla Trim Out contiene 'le', entonces las letras 'le' serían eliminadas primero antes de que se aplique el proceso 'Replace With'. Dado que nuestra casilla 'Replace With' contiene \*\*\{\{match\}\}\*\* resultaría en `**app**` siendo colocado como el reemplazo para 'apple' (primero 'le' se elimina, y el texto coincidente restante se le da el estilo markdown en negrita). Se pueden aplicar múltiples recortes añadiendo una nueva línea entre cada cadena que desees eliminar.

- **Affects** : Esta lista de casillas de verificación define las fuentes de texto a las cuales se aplicará el script RegEx.
  - 'User Input': el script se ejecutará contra el contenido de la entrada mecanografiada del usuario después de que presione Send.
  - 'AI Response': el script se ejecutará contra el contenido de la respuesta del AI después de que se reciba.
  - 'Slash Commands': el script se ejecutará contra los valores insertados en prompt/chat por comandos slash.
  - 'World Info': el script se ejecutará contra el contenido de las entradas World Info conforme se inyectan en el prompt. **Requiere que 'Alter Outgoing Prompt' esté marcado (o ambas casillas de efemeralidad estén desmarcadas).**
  - 'Reasoning': el script se ejecutará contra el contenido del objeto 'reasoning' devuelto por APIs de Chat Completion como Gemini o Deepseek. Si 'Alter Outgoing Prompt' está marcado bajo Ephemerality, el script también será aplicado a cualquier bloque reasoning que se agregue al prompt en turnos de chat posteriores.
  - **Si todo aquí está desmarcado el script nunca se activará durante el chateo normal, pero aún puede ser activado mediante comando slash o STscript.**

- **Other Options** :
  - 'Disabled' previene que el script se ejecute. Esto se utiliza como una anulación para prevenir que el script se ejecute cuando simplemente no deseas cambiar ninguna de la configuración del script y/o no deseas deshabilitarlo completamente mediante el interruptor en la lista de scripts (ya que hacerlo impediría que comandos slash lo activaran).
  - 'Run on Edit' hace que el script también se ejecute después de que un mensaje de chat haya sido editado. Si esto está desmarcado, el contenido de los mensajes de chat editados no activará el script.

- **Macros in Find Regex** : Selecciona si deseas reemplazar o no macros (tales como \{\{user\}\}, \{\{char\}\}, etc) que estén presentes en la secuencia de la casilla Find Regex.
  - 'Don't Substitute' hará que cualquier macro de SillyTavern sea ignorado de modo que el script RegEx los trate literalmente cuando busque.
  - 'Raw' enviará el valor de la macro textualmente. Esto podría alterar la forma en que tu script RegEx busca en el texto si el valor de la macro contiene ciertos caracteres especiales.
  - 'Escaped' agregará una barra de escape RegEx `\` antes de cada carácter para asegurar que no alteren accidentalmente la secuencia RegEx general. Esto puede ser útil si tienes ciertos caracteres especiales en los valores de la macro.

### Configuración de profundidad

La configuración de profundidad mínima/máxima proporciona control preciso sobre qué mensajes en el historial de chat se verán afectados por tu patrón regex:

- **Min Depth**: Solo afecta los mensajes que están al menos N niveles de profundidad en el historial de chat
  - 0 = último mensaje
  - 1 = penúltimo mensaje
  - etc.
  - Cuando está en blanco (establecido a 'Unlimited'), o -1, también afectará el mensaje para continuar en la acción Continue

- **Max Depth**: Solo afecta los mensajes no más profundos que N niveles en el historial de chat
  - Debe ser mayor que Min Depth para que el regex se aplique
  - Los prompts del sistema y los prompts de utilidad no se ven afectados por estas configuraciones

Por ejemplo, establecer Min Depth a 0 y Max Depth a 2 solo aplicaría tu regex a los tres mensajes más recientes en el chat.

### Banderas

Por defecto, el patrón Find Regex es sensible a mayúsculas/minúsculas y se aplica solo a la primera coincidencia. Para ajustar este comportamiento, así como otras banderas RegEx, puedes agregarlas como sigue:

```txt
/yourpattern/flags
```

Ejemplo: `/yourpattern/gi` coincidirá con todas las instancias de 'yourpattern' en el texto, independientemente de las mayúsculas/minúsculas.

Algunas de las banderas más comunes son:

- `i` : case-insensitive
- `g` : global (applies to all matches, not just the first)
- `s` : dotAll (treats the input as a single line, so `.` will match newlines)
- `m` : multi-line (treats the input as multiple lines, so `^` and `$` match the start/end of each line, not just the whole string)
- `u` : unicode (treats the input as unicode, so `\d`, `\w`, etc. will match unicode characters)

Para más información sobre banderas RegEx, consulta la siguiente página de MDN: [Advanced searching with flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags)

### Efemeralidad

Por defecto (cuando ninguna casilla aquí está marcada), un script RegEx editará directamente los valores de texto almacenados dentro del archivo JSONL del chat. Esto garantiza que tanto el prompt saliente como la visualización del chat siempre contendrán los mismos valores. Sin embargo, estos cambios al archivo de chat son irreversibles.

Si no deseas que esto suceda, puedes habilitar una de las casillas aquí para limitar los efectos del script RegEx solo a la visualización o al prompt saliente.

Si solo una de las casillas está marcada, no se realizarán cambios al archivo de chat, pero **solo el elemento marcado** será cambiado. Esto significa que estarás viendo una cosa, pero el LLM estará viendo otra. Úsalo cuidadosamente.

Si ambos están seleccionados, el script funcionará normalmente en todos los aspectos EXCEPTO que no escribirá ningún cambio en el archivo de chat.

## Uso avanzado

Aunque RegEx se utiliza comúnmente como una herramienta simple de Buscar/Reemplazar, también se puede usar de formas más complejas.

Por ejemplo, la casilla 'Replace With' podría incluir un conjunto de reglas CSS y HTML para agregar un elemento HTML con estilo específico a tu chat siempre que se encuentre una cierta palabra. Esto requerirá que la casilla `Show <tags> in responses` esté desmarcada en el panel User Settings.

El script también se puede configurar para nunca activarse durante el uso normal, sino que podría ser activado mediante comando slash como parte de una verificación lógica dentro de un STscript. La casilla 'Replace With' incluiría un valor único que el script reconoce para indicar si una verificación lógica es verdadera o falsa. Esto expande la utilidad de RegEx a toda la capacidad de todos los comandos slash, permitiendo niveles verdaderamente ilimitados de control y automatización basados en el contenido del chat.
