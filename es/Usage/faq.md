---
order: 180
icon: question
route: /usage/faq/
---

# Preguntas Frecuentes

## Explicar qué trata SillyTavern

Los modelos de lenguaje de IA moderna como ChatGPT se han vuelto tan poderosos que algunos de ellos ahora pueden simular convincentemente un personaje que creas, con el que puedas chatear, escribir ficción, etc. Por ejemplo, puedes decirle a la IA que pretenda ser un instructor de Go llamado Jubei de la antigua Japón, y actuará y responderá en consecuencia. Puedes tener una larga conversación con Jubei, ir al pub juntos, decidir pelearte con samuráis, lo que imagines, y la IA se sumará y escribirá/reaccionará alrededor de este contenido, actuando como tu contrincante y máster de calabozos. Tu imaginación es el límite. Puedes decirle a la IA que pretenda ser Wonder Woman. También puedes especificar un escenario ("Wonder Woman y yo estamos robando un banco"), un estilo de escritura ("Wonder Woman habla en ebónica"), o cualquier otra cosa que se te ocurra.

SillyTavern es una aplicación para facilitar estos usos:

* Es una interfaz de usuario que maneja la comunicación con modelos de lenguaje de IA.
* Te permite crear nuevas tarjetas de personaje (indicaciones) y cambiar entre ellas fácilmente.
* Te permite importar personajes creados por otras personas.
* Mantendrá tu historial de chat con un personaje, permitiéndote reanudar en cualquier momento, iniciar un nuevo chat, revisar chats antiguos, etc.
* En segundo plano, realiza lo necesario para preparar el indicador de IA para ti. Específicamente, enviará un indicador del sistema (instrucciones para la IA) que prepara a la IA para seguir ciertas reglas a fin de mejorar la precisión de las respuestas.

## Dame un resumen de mis opciones de modelo de IA

SillyTavern puede interactuar con dos tipos de IA:

1. [Servicios web](/Usage/API_Connections/openai.md) (Basado en la nube, generalmente de pago, propietario, cerrado)
2. [Alojado localmente](/Usage/API_Connections/self-hosted.md) (local, gratuito, código abierto)

### Servicios web de IA pagos

Los modelos web pagos son cajas negras. Pagas a una empresa para usar su servicio de IA. Pones tu información de cuenta en SillyTavern y se conectará a tu proveedor para usar la IA en tu nombre.

Ventajas:

* Muy fácil de empezar.
* Escritura de IA de la más alta calidad.

Desventajas:

* Cuestan dinero de usar.
* Todo se registra en su servidor. Preocupaciones sobre la privacidad.
* A menudo están censurados y se negarán a chatear contigo sobre ciertos temas.

### IA autohospedada

Los modelos autohospedados son modelos gratuitos que puedes ejecutar en tu PC pero requieren una PC potente y más trabajo para configurar.

Ventajas:

* Una vez configurados, pueden usarse de forma gratuita incluso sin acceso a Internet.
* Total privacidad. Todo lo que escribas se queda en tu propio PC.
* Hay una amplia variedad de modelos. Como tecnología impulsada por la comunidad, puedes encontrar modelos que se ajusten a ciertas tareas o comportamientos que deseas.

Desventajas:

* No son tan capaces como los modelos <abbr title="State of the art">SOTA</abbr> (es decir, escriben diálogos peor, son menos creativos, etc).
* Ejecutar modelos locales requiere una GPU con al menos 6GB de VRAM.

Si estás interesado en usar estos, consulta la guía dedicada aquí: [Cómo usar un modelo autohospedado](/Usage/API_Connections/self-hosted.md).

## ¿Puedo usar SillyTavern en mi teléfono o tableta?

Los iPhones e iPads no pueden ejecutar toda la aplicación SillyTavern, pero como es solo una interfaz web, puedes ejecutarla en otra computadora en tu Wi-Fi doméstico y luego acceder a ella en tu navegador móvil. Consulta [Conexiones remotas](/Administration/remote-connections.md) para más información.

Para usuarios de Android, además de lo anterior, puedes ejecutar SillyTavern completamente en tu teléfono, sin necesidad de una PC, usando la aplicación Termux. Consulta [Instalación (Android)](/Installation/Android.md). (NOTA: Las instalaciones de Termux no son oficialmente compatibles, y no podemos garantizar que funcione.)

## Intenté importar una tarjeta de personaje PNG pero obtuve un error que dice que es inválida. ¿Por qué?

Dos posibilidades:

1. La tarjeta no tenía las definiciones incrustadas dentro de ella y era solo un archivo de imagen normal. Algunos programas o administradores de archivos quitarán las definiciones incrustadas de la tarjeta cuando la guardes. Asegúrate de usar el archivo PNG sin procesar tal como fue publicado por la persona que lo compartió.
2. El archivo PNG era en realidad un archivo WEBP con un nombre de archivo `.png`. Puedes intentar renombrar la tarjeta a `.webp` antes de importarla, o buscar una versión PNG adecuada de la imagen.

## ¿Cómo puedo crear mi propio personaje de IA?

1. Haz clic en el botón Gestión de personajes
2. Haz clic en Crear nuevo personaje
3. Bajo Nombre del personaje, dale un nombre, como Amanda
4. Opcionalmente, haz clic en el botón Seleccionar avatar para elegir un retrato de imagen para este personaje
5. Bajo Descripción, describe el personaje e incluye cualquier información que consideres relevante para el chat. Por ejemplo: ```Amanda es una estudiante viajando durante su año sabático. Mide 6 pies de alto y es jugadora de voleibol. Tiene una figura atlética. Tiene cabello largo marrón. Ama el período de la Inglaterra victoriana y ve y lee novelas relacionadas con ese período.```
Por ejemplo, si quieres que Amanda sea amigable, entonces agregarías: ```Amanda es extremadamente alegre y extrovertida.```
6. Bajo Primer mensaje, escribe el saludo del personaje cuando comiences un nuevo chat. Por ejemplo: ```*Amanda te saluda con la mano* ¡Oye! ¿También eres mochilero?```
7. Haz clic en el botón Crear personaje

Ahora tienes un personaje básico con el que puedas chatear. Selecciona a Amanda de la lista de personajes y comenzará un nuevo chat.

Ten en cuenta que puedes usar la Descripción y/o Primer mensaje para crear un escenario más específico, y/o incluirte a ti mismo en la descripción. Por ejemplo:

```txt
Descripción:
Amanda es una estudiante viajando durante su año sabático. Mide 6 pies de alto y es jugadora de voleibol. Tiene una figura atlética. Tiene cabello largo marrón. Ama el período de la Inglaterra victoriana y ve y lee novelas relacionadas con ese período. Ha estado guardando un secreto que pesa mucho en su alma. Está esperando a la persona adecuada para desahogarse, pero esto puede llevar a un juego del gato y el ratón contra una poderosa sociedad secreta. Recientemente llegó a Calcuta.

Eres Rajesh Nahasmapetilon, una estrella mundial de voleibol indio. Estás paseando por Calcuta. Amanda te ve y grita de emoción.

Primer mensaje:
*Amanda corre hacia ti, radiante.* ¡Rajesh! ¡No puedo creerlo! Soy un gran fan. Tengo tu póster en mi dormitorio.
```

Cualquier información relevante que incluyas puede ser utilizada. Cuán bien se utiliza depende del nivel de poder del modelo de IA.

NOTA: puedes volver atrás y editar cualquiera de esta información una vez que se crea el personaje, excepto el nombre.

## ¿Dónde se almacenan mis claves de API? ¿Por qué no puedo verlas?

SillyTavern guarda tus claves de API en un archivo `secrets.json` en el directorio de datos del usuario (la ruta predeterminada es `/data/default-user/secrets.json`).

Por defecto, las claves de API no serán visibles desde la interfaz después de que las hayas guardado y actualizado la página.

Para habilitar la visualización de tus claves:

1. Establece el valor de `allowKeysExposure` en `true` en el archivo `config.yaml`.
2. Reinicia el servidor de SillyTavern.
3. Haz clic en el enlace "Ver claves de API ocultas" en la esquina inferior derecha del Panel de conexión de API.

## Consejos de rendimiento

### ¿Por qué la interfaz de usuario es tan lenta/entrecortada?

* Intenta habilitar el modo Sin efecto de desenfoque (UI rápida) en el panel de configuración del usuario.
* Habilita Movimiento reducido en la configuración del tema de la interfaz de usuario para eliminar las animaciones cosméticas.
* Asegúrate de que tu navegador esté usando Aceleración de hardware.
* Si usas transmisión de respuestas, establece los FPS de transmisión en un valor más bajo (se recomiendan 10-15 FPS).

### Estoy experimentando retraso en la entrada. ¿Qué puedo hacer?

La degradación del rendimiento, particularmente el retraso en la entrada, se atribuye más comúnmente a extensiones del navegador. Las extensiones problemáticas conocidas incluyen:

* Gestor de contraseñas iCloud
* Traducción de DeepL
* Herramientas de corrección gramatical basadas en IA
* Varias extensiones de bloqueo de anuncios

Si experimentas problemas de rendimiento y no puedes identificar la causa, o sospechas un problema con SillyTavern en sí, por favor:

1. [Registra un perfil de rendimiento](https://developer.chrome.com/docs/devtools/performance/reference)
2. Exporta el perfil como un archivo JSON
3. Envíalo al equipo de desarrollo para análisis

Recomendamos primero probar con todas las extensiones del navegador y extensiones SillyTavern de terceros deshabilitadas para aislar la fuente de la degradación del rendimiento.

### Cuando importo muchos personajes, la aplicación se vuelve lenta. ¿Por qué?

Desafortunadamente, SillyTavern no fue diseñado para manejar bibliotecas de personajes enormes. Cuantos más tengas, más tiempo tardará en cargar la lista de personajes. Los datos evidentes sugieren que la degradación del rendimiento comienza a volverse notoria cuando tienes más de 1000 personajes.

Sin embargo, hay algunas cosas que puedes hacer para mitigar el problema:

**1. Usa carga perezosa.**

Habilita la carga perezosa de caracteres estableciendo el valor `performance.lazyLoadCharacters` en true en el archivo `config.yaml`. Después del próximo reinicio del servidor, la lista de caracteres solo cargará los datos completos de los caracteres con los que interactúes. Ten en cuenta que algunas extensiones de terceros pueden no funcionar correctamente con esta configuración habilitada si no se actualizaron para compatible (contacta al desarrollador de la extensión para más información).

**2. Usa caché de memoria.**

Aumenta la capacidad de caché de memoria si tienes algo de RAM disponible. Esto permitirá que el servidor mantenga más caracteres en la memoria, reduciendo el tiempo que tarda en cargarlos. Puedes hacer esto ajustando el valor de `performance.memoryCacheCapacity` a un número más alto en el archivo `config.yaml`. El valor predeterminado es `100mb`. Regla general aproximada: aumenta el valor en 100mb por cada 3000 caracteres que tengas.

**Limitaciones:**

1. La búsqueda avanzada (difusa) de caracteres no funcionará con la carga perezosa habilitada. Solo se buscarán nombres de caracteres.
2. La caché de memoria está deshabilitada en dispositivos Android debido a la cantidad limitada de memoria disponible.

## ¿Cómo hacer que la IA escriba más?

A veces, la IA solo responderá con una sola oración cuando desearías que sea más verbosa.
Esto suele ser un problema con modelos ejecutados localmente.

Si simplemente quieres que el bot continúe escribiendo desde donde lo dejó al final de su respuesta más reciente, puedes enviar un mensaje de usuario vacío escribiendo nada en la barra de entrada y haciendo clic en Enviar. Esto forzará al bot a continuar la historia.

Estrategias para solucionar esto:

* Aumenta el valor de la configuración `Longitud de respuesta`
* Diseña un buen `Primer mensaje` para el personaje, que lo muestre hablando de una manera prolija. Los modelos de IA pueden mejorar mucho cuando se les da orientación sobre el estilo de escritura que esperas.
* Añade una frase en el cuadro de descripción del personaje como "le gusta hablar mucho" o "hablador muy verboso"
* Haz lo mismo con tu `Nota del autor` o `Indicación de instrucción posterior al historial`
* Como último recurso, puedes intentar activar `Auto-continuar` (en el panel de configuración del usuario), pero hará que las respuestas salgan más lentamente porque está haciendo que la IA produzca pequeñas respuestas una tras otra, y luego las combine en una respuesta grande. También puede ser incompatible con algunas opciones de API.

## ¿Cómo hacer que la IA escriba menos?

Esto es principalmente solo un problema para modelos como ChatGPT o Claude. Las mismas estrategias pueden aplicarse pero en sentido inverso.

* Disminuye el valor de la configuración `Longitud de respuesta`
* Dale al personaje una frase como "de pocas palabras" o "no habla mucho" en su descripción.
* Dale al personaje un primer mensaje breve para establecer el tono y la expectativa para el chat.
* Asegúrate de que `Auto-continuar` esté desactivado.

## ¿Cómo hacer que la IA deje de escribir las acciones de mi personaje e impulse la trama por su cuenta?

Esto debe manejarse en la `Nota del autor` con una combinación de frases como:

* \{\{char\}\}'s responses shall only be passive and reactive to \{\{user\}\}'s actions.
* Your next response shall be solely from the POV of \{\{char\}\}.
* You are never allowed to dictate actions or speech for \{\{user\}\}
