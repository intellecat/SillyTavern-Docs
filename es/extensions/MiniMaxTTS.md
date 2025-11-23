---
order: tts-minimax
route: /extensions/minimaxtts/
---

# MiniMax TTS

Esta página te enseñará cómo usar correctamente el proveedor MiniMax TTS.

## Requisitos previos

1. Cuenta de MiniMax con acceso a API
2. API Key y Group ID válidos de MiniMax

## Obtención de credenciales de API

### 1. Crear una cuenta de MiniMax

1. Visita el [sitio web de MiniMax (Internacional)](https://www.minimax.io/)
2. Haz clic en "Registrarse" o "Iniciar sesión"
3. Completa el proceso de registro de cuenta

!!!warning Diferencias Regionales
MiniMax tiene versiones separadas en chino e internacional. Tenga en cuenta:
- La versión en chino no admite funciones de clonación de voz
- La versión en chino solo admite el host de API `api.minimax.chat`
!!!

### 2. Obtener API Key y Group ID

1. Inicia sesión en la [consola de MiniMax (Internacional)](https://www.minimax.io/platform/user-center/basic-information)
2. Puedes encontrar tu GroupId en la página de Información Básica
3. Ve a Configuración → API Keys en la barra lateral izquierda para crear y obtener tu API Key

## Configuración en SillyTavern

### 1. Configuración básica

1. Abre SillyTavern
2. Navega a "Extensiones" → "TTS"
3. Selecciona "MiniMax" como tu proveedor de TTS
4. Configura los siguientes ajustes:
    - **API Key**: Tu API key de MiniMax
    - **Group ID**: Tu Group ID de MiniMax
    - **API Host**: Elige el servidor apropiado según tu región:
        - `api.minimax.io` (Servidor internacional oficial)
        - `api.minimaxi.chat` (Otro host de servidor internacional)
        - `api.minimax.chat` (Servidor de China continental)

### 2. Selección de modelo

Los modelos disponibles incluyen:
- **Speech-02-HD**: Síntesis de voz de alta calidad (recomendado)
- **Speech-02-Turbo**: Síntesis de voz rápida
- **Speech-01**: Modelo heredado
- **Speech-01-240228**: Modelo heredado (versión específica)

### 3. Parámetros de voz

Ajusta los siguientes parámetros para personalizar la salida de voz:
- **Speed**: 0.5 - 2.0 (1.0 = velocidad normal)
- **Volume**: 0.1 - 2.0 (1.0 = volumen normal)
- **Pitch**: 0.5 - 2.0 (1.0 = tono normal)
- **Audio Format**: MP3, WAV, FLAC

## Voces personalizadas

### 1. Obtención de Voice IDs

1. Accede a la [página TTS de MiniMax (Internacional)](https://www.minimax.io/audio/text-to-speech)
2. Haz clic en "Voz" en el lado derecho para entrar en la interfaz de selección de voz
3. Encuentra la voz que deseas usar
4. Haz clic en el botón de copiar junto al nombre de la voz para copiar el Voice ID

### 2. Agregación de voces personalizadas

1. En la configuración de MiniMax TTS, ubica la sección "Gestión de voces personalizadas"
2. Completa la siguiente información:
    - **Voice Name**: Elige cualquier nombre para identificación
    - **Voice ID**: El Voice ID obtenido de la plataforma MiniMax
    - **Language**: Selecciona el idioma correspondiente para la voz
3. Haz clic en "Agregar voz personalizada"

## Modelos personalizados

### 1. Agregación de modelos personalizados

1. En la sección "Gestión de modelos personalizados"
2. Completa:
    - **Model ID**: Identificador del modelo
    - **Model Name**: Nombre de visualización para el modelo
3. Haz clic en "Agregar modelo personalizado"

### 2. Obtención de Model IDs

1. Consulta la lista de modelos en la [documentación oficial de MiniMax](https://www.minimax.io/platform/document/Model?key=684261f14c5738213294faa7)
2. O visualiza modelos personalizados disponibles en la consola
3. Copia el Model ID correspondiente

## Solución de problemas

### Problemas comunes

1. **Autenticación de API fallida**
    - Verifica que la API Key corresponda al API Host correcto
    - Confirma que el Group ID es correcto
    - Verifica si tu cuenta tiene saldo suficiente

2. **Generación de voz fallida**
    - Verifica que el Voice ID seleccionado sea válido
    - Asegúrate de que la voz sea compatible con tu modelo seleccionado

3. **Tiempo de conexión agotado**
    - Intenta cambiar a un API Host diferente
    - Verifica tu conexión de red
    - Verifica la configuración del firewall

4. **Problemas de calidad de audio**
    - Intenta usar un modelo diferente (Speech-02-HD para la mejor calidad)
    - Ajusta los parámetros de voz (velocidad, tono, volumen)
    - Verifica la compatibilidad del formato de audio
