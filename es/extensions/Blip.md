---
route: /extensions/blip/
---

# Blip

Esta guía te guiará a través de la configuración y personalización de la extensión blip para tu experiencia de SillyTavern. Esta extensión anima el texto de los mensajes con velocidad variable y reproduce sonido junto con la animación. Puedes usar un archivo de audio o generar el sonido.

## Requisitos previos

Antes de comenzar, asegúrate de haber cumplido los siguientes requisitos previos:

- Asegúrate de estar en la última versión de SillyTavern.
- Instala la extensión "Blip" desde el menú "Descargar extensiones y activos" en el panel de extensiones (icono de bloques apilados).

## Configuración global de Blip

1. **Mensaje de usuario Blip**:
   - Habilita la casilla de verificación para reproducir la animación en el mensaje del usuario.
   - Establece un perfil para el usuario o un perfil predeterminado si deseas animación blip para el usuario.

2. **Blip solo para cierto texto**:
   - Habilita la casilla de verificación para solo blip en texto dentro de comillas.
   - Habilita la casilla de verificación para ignorar todo dentro de asteriscos.

3. **Desplazamiento automático hacia abajo**:
   - Habilita la casilla de verificación para hacer que el chat se desplace hacia abajo siguiendo la animación de texto, desactívalo si quieres desplazarte libremente durante la animación.

4. **Volumen de audio**
   - Silencia el audio si solo se desea la animación del texto.
   - Puedes ajustar el volumen global del audio blip.

## Perfil de animación/voz de personaje

Puedes guardar un perfil para cada personaje:
   - incluyendo el usuario y un perfil predeterminado opcional que se utilizará cuando el personaje no tenga perfil.
   - Si solo se muestran los personajes del chat actual en la lista, haz clic en la casilla de verificación para mostrar todos tus personajes.

1. **Selecciona el personaje para asignar/actualizar perfil**:
   - Selecciona un personaje, si tiene un perfil se cargará.
   - Si aún no tiene perfil, los parámetros actuales se convertirán en la configuración de su perfil.
   - Cualquier perfil se puede eliminar usando el botón eliminar.
   - Usa el botón actualizar si tu personaje no aparece en la lista.

2. **Configuración de animación de texto**:
   - Establece la velocidad del texto: el retraso en milisegundos entre cada letra impresa.
   - Establece el multiplicador de velocidad Mín/máx diferente a 1.0 para aleatoriedad de la animación de velocidad.
   - Establece el retraso de coma/frase superior a 0 para agregar una pausa cuando se impriman caracteres especiales, puede agregar más vivacidad a la animación. El audio también se pausa en este caso.

3. **Parámetros de audio**:
   - Establece un multiplicador de volumen que solo afectará este perfil de voz si es necesario.
   - Establece la velocidad del audio: el retraso entre cada sonido blip, independiente de la velocidad del texto.

4. **Origen del Blip: Sonido generado**:
   - Utiliza el control deslizante de frecuencia mín/máx para personalizar el sonido blip reproducido.
   - Si mín/máx son diferentes, se reproduce un sonido aleatorio en este rango cada vez.

5. **Origen del Blip: archivo**:
   - Elige un archivo de la lista.
   - Puedes obtener activos blip oficiales de ST desde el menú de extensiones de activos.
   - O pon el archivo directamente en: `\SillyTavern\data\<user-handle>\assets\blip`.
   - Habilita la casilla de verificación para forzar a esperar a que todo el archivo se reproduzca antes de reproducirse nuevamente si es necesario.

¡Gracias por seguir esta guía! Tu experiencia de SillyTavern ahora está enriquecida con animación de texto y voces blip.
