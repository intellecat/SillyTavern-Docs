---
order: 20
route: /usage/core-concepts/uicustomization/
---

# Personalización de la Interfaz de Usuario

## Tema de la Interfaz de Usuario

### Gestión de Temas

Los archivos de tema te permiten guardar, compartir y reutilizar tus personalizaciones de la interfaz. Puedes mantener múltiples temas para diferentes estados de ánimo u propósitos, e intercambiar entre ellos al instante.

* Importar/Exportar archivos de tema
* Eliminar temas existentes
* Guardar cambios en el tema actual
* Guardar como nuevo tema

Todos los ajustes en esta sección se guardan en el tema actual. Si cambias de tema, los ajustes se reemplazarán por los del nuevo tema.

### Configuración de Pantalla

Estas opciones de visualización afectan cómo se presentan los personajes y los mensajes en la interfaz de chat.

#### Estilo de Avatar

Elige entre Circle, Square, Rectangle o Rounded Square. Este ajuste se aplica tanto a avatares de usuario como de IA.

#### Estilo de Chat

| Estilo       | Descripción                                                                                                                                                    | [Comando de Barra](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| **Flat**     | Estilo limpio y continuo de "registro de chat", un lienzo plano para que tus interacciones con la IA cobren vida.                                                                 | `/flat`<br>`/default`                                      |
| **Bubbles**  | Estilo de "mensajería instantánea" con burbujas distintas para cada mensaje, esquinas redondeadas delightful y un efecto 3D sutil.                                          | `/bubble`<br>`/bubbles`                                    |
| **Document** | Apariencia compacta, similar a un documento, con un diseño enfocado en texto. Oculta avatares, marcas de tiempo y botones de control de mensaje para mensajes anteriores. | `/single`<br>`/story`                                      |

### Notificaciones

Establece una posición donde aparecerán las notificaciones emergentes (mensajes de notificación) en la pantalla.

* Superior Izquierda
* Superior Centro (predeterminado)
* Superior Derecha
* Inferior Izquierda
* Inferior Centro
* Inferior Derecha

### Colores del Tema

Personaliza el esquema de color de cada elemento de la interfaz para crear tu tema perfecto. Los colores se pueden seleccionar usando un selector de color e incluyen opciones de transparencia donde sea aplicable.

* Texto Principal
* Texto en Cursiva
* Texto Subrayado
* Texto de Cita
* Sombra de Texto
* Fondo del Chat
* Fondo de la Interfaz
* Borde de la Interfaz
* Mensaje del Usuario
* Mensaje de IA

### Configuración de Diseño y Visual

Ajusta la presentación visual de la interfaz con estos deslizadores.

* **Chat Width**: Ajusta el ancho de la ventana de chat (25-100% de la pantalla)
* **Font Scale**: Personaliza el tamaño del texto (0,5-1,5x)
* **Blur Strength**: Controla el desenfoque del panel de interfaz (0-30)
* **Shadow Width**: Ajusta la intensidad de la sombra de texto (0-5)

### Alternancias de Tema

Estos interruptores controlan varias características y comportamientos de la interfaz. Algunas opciones pueden mejorar el rendimiento en dispositivos de gama baja, mientras que otras agregan información útil o funcionalidad a la interfaz de chat.

* **Reduced Motion**: Deshabilita animaciones y transiciones
* **No Blur Effect**: Elimina el desenfoque de fondo para mejor rendimiento
* **No Text Shadows**: Deshabilita efectos de sombra de texto
* **[Visual Novel mode](Visual-Novel.md)**: Chat compacto con sprite de fondo
* **Expand Message Actions**: Siempre mostrar menú de contexto de mensaje completo
* **Zen Sliders**: Controles de parámetros simplificados
* **Mad Lab Mode**: Rangos de parámetros sin restricciones
* **Message Timer**: Mostrar tiempo de generación de respuesta de IA
* **Chat Timestamps**: Mostrar marcas de tiempo de los mensajes
* **Model Icons**: Mostrar iconos de modelo de IA para los mensajes
* **Message IDs**: Mostrar números de mensaje secuenciales
* **Hide Chat Avatars**: Eliminar avatares del chat
* **Message Token Count**: Mostrar conteos de tokens por mensaje
* **Compact Input Area**: Entrada de una sola fila (Solo en dispositivos móviles)
* **Swipe # for All Messages**: Mostrar números de deslizamiento en todos los mensajes
* **Characters Hotswap**: Botones de selección rápida para personajes favoritos
* **Avatar Hover Magnification**: Efecto de zoom al pasar el ratón sobre el avatar
* **Tags as Folders**: Organizar personajes usando etiquetas como carpetas
* **Click to Edit**: Haz clic en los mensajes para abrir rápidamente un editor de mensajes

### CSS Personalizado

Te permite aplicar estilos CSS personalizados para personalizar aún más la apariencia de la interfaz de chat.

Usa <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** para expandir la ventana del editor para una mejor visibilidad y edición.

Si cambias de tema, tu CSS personalizado será reemplazado por el CSS personalizado del nuevo tema. Asegúrate de guardar tu CSS personalizado en un tema si deseas mantenerlo al cambiar de tema.

Si usas mucho CSS personalizado, o quieres usar el mismo CSS personalizado con varios temas, la extensión no oficial [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets) puede ayudarte a administrar y organizar tu CSS personalizado.

---

## Sonido de Mensaje

Para reproducir tu propio sonido personalizado al recibir un nuevo mensaje del bot, reemplaza el siguiente archivo MP3 en tu carpeta SillyTavern:

`public/sounds/message.mp3`

Se reproduce al 80% del volumen.

Si la opción "[Background Sound Only](index.md#miscellaneous)" está habilitada, el sonido se reproduce solo si la ventana de SillyTavern está **desenfocada**.

## Representación de Fórmulas

Para habilitar la representación de fórmulas matemáticas, usa la [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX). Para obtener la extensión, necesitas instalarla a través del menú "Download Extensions & Assets" en SillyTavern.

Escribe tus fórmulas en bloques de código con identificadores de lenguaje `latex` o `asciimath` para LaTeX y AsciiMath respectivamente. La extensión utiliza [KaTeX](https://katex.org/) para la representación.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Aviso de Deprecación
La sintaxis heredada `$` y `$$` ya no es compatible. Utiliza los siguientes scripts de regex para hacer un polyfill de la sintaxis anterior:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
