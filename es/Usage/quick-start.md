---
order: 190
icon: rocket
route: /usage/quick-start/
---

# Inicio Rápido

!!!light
No tengo idea. Solo dame la forma más fácil y rápida para comenzar a usar SillyTavern. -- *Anónimo*
!!!

Puedes comenzar con SillyTavern en solo unos minutos. Aquí hay dos formas fáciles de empezar:

* Puedes [usar AI Horde](#inicio-rápido-con-ai-horde) de forma gratuita. AI Horde es un servicio de IA impulsado por la comunidad que proporciona acceso a una variedad de modelos de IA.

* Si tienes una cuenta de OpenAI o deseas registrarte, puedes [usar OpenAI](#inicio-rápido-con-openai).

## Inicio rápido con AI Horde

1. Sigue la [Guía de Instalación](/Installation/index.md) para instalar e iniciar SillyTavern.

2. En la pantalla de incorporación de SillyTavern, ingresa un nombre para tu personaje. Este nombre se utilizará en el chat.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. Haz clic en el botón API Connections en la barra superior.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. Ingresa una clave de API para AI Horde. Puedes usar `0000000000` por ahora, u obtén una clave gratuita de [AI Horde](https://aihorde.net/).

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. Selecciona algunos modelos de IA para usar. Solo elige algunos de los primeros. Siempre puedes cambiarlos más tarde.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. Cierra la ventana de Conexiones de API. Ingresa un mensaje en el cuadro de chat en la parte inferior y presiona Entrar.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. Tu IA responderá en unos momentos. Puedes continuar [chateando](/Usage/Chatting/index.md) con él. ¡Éxito!

   ![This is an optional caption](/static/quick-start/6_success.png)

## Inicio rápido con OpenAI

### Instalar SillyTavern

Sigue la [Guía de Instalación](/Installation/index.md) para instalar e iniciar SillyTavern.

### Obtener acceso a OpenAI

1. Regístrate en OpenAI.
2. Ve a <https://platform.openai.com>
3. Haz clic en el icono de tu cuenta en la esquina superior derecha, luego Mostrar claves de API.
4. Haz clic en "Crear nueva clave secreta". Cópiala en algún lugar inmediatamente. **NO COMPARTAS ESTA CLAVE. QUIEN LA TENGA PUEDE USAR TU CUENTA PARA USAR GPT POR TU CUENTA.**

### Configurar SillyTavern para usar tu API

1. En la barra superior de SillyTavern, haz clic en API Connections.
2. En API, selecciona Chat Completion (OpenAI).
3. En Chat Completion Source, selecciona OpenAI.
4. Pega la clave de API que guardaste en el paso anterior.
5. Haz clic en el botón Connect. Confirma que diga Valid.
6. Por defecto, SillyTavern usará GPT-4 Turbo. Puedes elegir un modelo diferente, pero infórmate sobre los precios.

### Prueba tu configuración

1. En la barra superior de SillyTavern, haz clic en Character Management en el extremo derecho.
2. Selecciona un personaje existente como Seraphina.
3. En el cuadro de texto en la parte inferior, escribe algo a Seraphina, luego presiona Entrar o haz clic en el botón Enviar.

Si hiciste todo correctamente, después de unos segundos, Seraphina debería responder.
