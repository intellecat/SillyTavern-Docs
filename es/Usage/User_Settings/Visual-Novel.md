---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /usage/user-settings/visual-novel/
---

# Modo Visual Novel (VN)

El Modo Visual Novel es un diseño de pantalla especial en SillyTavern que te permite chatear con personajes con sprites (o su imagen de tarjeta de personaje) que se asemejan a una novela visual como Doki Doki Literature Club, The Fruits of Grisaia, Fate: Stay/night y otros juegos VN famosos.

## Activar/Desactivar Modo Visual Novel

### Activar Modo Visual Novel

El Modo Visual Novel viene integrado en SillyTavern y se puede activar yendo a *Configuración de Usuario* (icono de Configuración de Usuario) y marcando **Modo Visual Novel** debajo de *Sin Sombras de Texto*.

![User Settings](/static/vn/vn-mode-toggle.png)

### Desactivar Modo Visual Novel

Desactivar el Modo Visual Novel es los mismos pasos que activarlo. Desmarca el Modo Visual Novel y deberías volver a la pantalla de chat normal.

!!!advertencia Respecto al Modo VN con Extensiones VN
Algunas extensiones (como la Extensión Prome VN) activarán el 'Modo Visual Novel' si usas sus respectivos modos VN. Activar/Desactivar el Modo VN desde el menú *Configuración de Usuario* también afectará a estas extensiones.
!!!

## Interfaz de Usuario Visual Novel

![VN Display](/static/vn/vn-display.png)

En el Modo Visual Novel, la interfaz se modifica ligeramente para acomodar los sprites de personajes (o la imagen de la tarjeta de personaje) que se muestran en el centro. En un chat grupal con múltiples personajes, sin embargo, los sprites de personajes se distribuyen, acomodándose entre sí como se muestra a continuación.

![Group VN Display](/static/vn/group-vn-display.png)

### Modo VN con MovingUI

!!!información
Para activar/desactivar MovingUI, ve a *Configuración de Usuario* y marca **MovingUI**. Ten en cuenta que esta función **solo** funciona en Computadoras de Escritorio.
!!!

Si **MovingUI** está habilitado en *Configuración de Usuario*, los sprites (o la imagen de la tarjeta de personaje) se pueden mover si deseas moverlos o colocarlos en un área más específica de la pantalla.

!!!advertencia Respecto a los Tamaños de los Sprites
Si el tamaño de tus sprites de personaje es relativamente grande, será un desafío intentar mover ciertos sprites con MovingUI, ya que el botón para arrastrar sprites podría estar cubierto debajo de un sprite existente. Probablemente tendrás que moverlos un poco más de lo normal, especialmente si hay más personajes en la pantalla para un mejor posicionamiento.
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## Cómo Obtener Sprites de Personajes

Los sprites de personajes se pueden obtener navegando por internet para encontrar sprites existentes, por ejemplo, de un personaje existente de una Visual Novel o de un juego que usa la función Visual Novel como DDLC o CounterSide. Si el personaje del que deseas sprites aún no viene con sprites, tienes varias opciones restantes.

1. Busca en el post del personaje cualquier paquete ZIP de sprites o enlace a un paquete de sprites.
    !!!información
    Algunos creadores de bots pueden lanzar sus bots con un paquete de sprites (ya sea en el mismo post o en un canal de sprites). Busca en esos posts si alguien aún no ha hecho sprites del personaje que deseas.
    !!!
2. Crea los tuyos propios usando LoRAs y Stable Diffusion.
    !!!advertencia
    Generar sprites desde cero consume mucho tiempo (especialmente si no existen LoRAs para tu personaje y/o para el modelo Stable Diffusion que deseas usar) y requerirá un hardware decente para generarlos, más aún si planeas hacer 28 expresiones de sprite en lugar de 6 y si estás usando SDXL y/o mejorando la resolución de los sprites.
    !!!
3. Usa la imagen de la tarjeta de personaje. Podría no ser como un sprite, pero al menos tienes algo que mirar en pantalla. Sin embargo, no se pueden usar múltiples tarjetas de personaje en el modo VN.
    !!! Imágenes de Tarjeta de Personaje con la Extensión Prome Visual Novel
    Con la Extensión Prome Visual Novel 1.0.6+, existe una función llamada `Emulate Character Card as Sprite` que te permite tener un chat grupal con personajes con y sin sprites usando su tarjeta de personaje como sprite en el chat.

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## Extensiones VN

### Extensión Prome Visual Novel

La Extensión Prome Visual Novel es una extensión de terceros aprobada de Bronya Rand y Prometheus que mejora aún más la experiencia de novela visual en SillyTavern con características como Modo Letterbox que hace que la interfaz de novela visual sea más "cinematográfica", Modo Enfoque con Sprites de Personaje Oscurecidos, Modo VN Tradicional donde solo el último mensaje en el chat aparece en el chat y más por venir.

|                              Modo Letterbox                              |                          Modo VN Tradicional                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Ocultar Sheld (Cuadro de Mensaje)                  |                      Modo Enfoque (con Sprites Oscurecidos)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

Para instalar la Extensión Prome Visual Novel, puedes instalarla yendo a `Descargar Extensiones y Activos` y encontrando *Extensión Prome Visual Novel*, o seguir las instrucciones de instalación en la página de [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage) Github. Ajustar la configuración de Prome se puede encontrar en *Extensiones* -> **Prome (Extensión Visual Novel)** o a través del menú 🪄 (Varita).
