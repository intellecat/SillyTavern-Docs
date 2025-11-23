---
route: /usage/api-connections/horde/
---

# AI Horde

## Descargo de responsabilidad

- AI Horde es un clúster de GPU distribuido y colaborativo ejecutado completamente por voluntarios.
- Por defecto, tus datos se envían de forma anónima y las respuestas no pueden ser vistas por la persona que ejecuta el Worker de Horde.
- Sin embargo, como es un programa de código abierto, Workers maliciosos podrían modificar el código para:
  - registrar tu actividad (solicitudes de entrada, respuestas de IA).
  - producir respuestas malas u ofensivas.

!!!warning
Al usar Horde **nunca envíes** información personal como nombres, direcciones de correo electrónico, etc.
!!!

Al activar la casilla "Trusted Workers Only" (Solo Workers de confianza) limitarás la selección de workers disponibles a solo aquellos que han estado alojando en Horde durante un tiempo y generalmente se consideran confiables. Pero aun así podrían ver las solicitudes, por ejemplo, alojando usando software no contabilizado.

Para ayudar a reducir este problema, SillyTavern ha incorporado la siguiente función:

- Cuando un Worker de Horde genera una respuesta de chat, SillyTavern registra el ID del Worker y el modelo que estaban usando.
- Esta información se puede ver pasando el cursor del ratón sobre el elemento del chat (ver imagen a continuación).
- Si crees que recibiste una respuesta maliciosa, puedes pasar esta información al administrador de Horde en el [Discord de AI Horde](https://discord.gg/3DxrhksKzn) para revisión y posible acción disciplinaria contra ese Worker.

![Horde Worker Info Popup](/static/horde-worker.png)

## Configuración

- SillyTavern puede conectarse con Horde sin configuración adicional requerida.
- Selecciona 'AI Horde' del Selector de API desplegable en el Panel API de ST.
- Selecciona uno o más Modelos ('cerebros de IA' para los personajes) del Selector de Modelo en la parte inferior del panel.
- Selecciona un personaje y comienza a chatear.

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
Por defecto, tu instancia de SillyTavern se conecta a la cuenta de invitado de baja prioridad de Horde.
Esto significa que puede que tengas que esperar mucho tiempo para una respuesta.
Para reducir los tiempos de espera, sigue los consejos a continuación.
!!!

## Consejos

- [Registra una cuenta en el sitio web de Horde](https://aihorde.net/register) y luego añade tu clave de Horde en la caja de clave API de Horde de SillyTavern.
- [Configura un Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme) para proporcionar tu GPU para otros.
  - Dejar que otros usen tu GPU te gana ['Kudos', una especie de moneda solo de Horde](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos).
  - Cuantos más kudos tenga tu cuenta, más rápido obtendrás respuestas de chat de otros Workers de Horde.
  - Kudos también se puede usar para crear imágenes de IA en [Stable Horde](https://stablehorde.net).
    - SillyTavern admite la generación de imágenes de Stable Horde sin necesidad de configuración adicional.
- Si tu GPU no es lo suficientemente potente para ejecutar una IA, o no tienes una computadora, aún puedes [participar en la comunidad de Horde para ganar Kudos de varias formas](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos).
