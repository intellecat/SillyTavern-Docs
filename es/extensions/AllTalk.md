---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk es un sistema de clonación de voz basado en Coqui XTTS, F5-TTS, VITS, Piper y otros motores de modelos TTS, diseñado para producir reproducción de voz de alta calidad (ya sea clonación de voz de cero disparos o voces integradas). En AllTalk V2, actualizaciones significativas mejoran la funcionalidad y facilidad de uso, incluyendo soporte para múltiples motores TTS, personalización expandida y optimizaciones de rendimiento. Para una lista completa de características, consulte el [Wiki de AllTalk aquí](https://github.com/erew123/alltalk_tts/wiki).

---

## 🟩 Características Clave en AllTalk V2
- **Soporte de Múltiples Motores**: Cambia fácilmente entre Coqui XTTS, VITS, Piper, Parler, F5 y motores personalizados.
- **Conversión de Voz (RVC)**: Canalización mejorada de clonación de voz basada en recuperación.
- **Configuración Personalizable**: Ajusta la configuración por motor y guarda configuraciones de inicio.
- **Funcionalidad de Narrador**: Especifica voces separadas para la narración y los personajes.
- **Uso Independiente e Integrado**: Integración sin problemas con SillyTavern.
- **Modos DeepSpeed y VRAM Bajo**: Optimización de rendimiento para entornos con recursos limitados.
- **Capturas de Pantalla**: Ver la interfaz de AllTalk V2 [aquí](https://github.com/erew123/alltalk_tts/discussions/237).

---

## 🟨 Opciones de Configuración e Instalación

AllTalk ofrece métodos de instalación tanto independientes como integrados. La configuración más rápida implica usar una de las opciones de instalación rápida proporcionadas, con scripts automatizando la mayoría del proceso.

- **Instalación Independiente**: Recomendado para la mayoría de usuarios ([Guía Independiente](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Integración Text-generation-webui**: Para integración en Text-generation-webui ([Guía de Instalación TGWUI](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 Instalación Automatizada
**Este método es solo para usuarios de Windows.**
Para usuarios nuevos que deseen una configuración rápida, la instalación automatizada utiliza SillyTavern-Launcher.
Nota: Esto asume que ya has instalado SillyTavern-Launcher. Si no lo has hecho, visita https://github.com/SillyTavern/SillyTavern-Launcher y sigue las instrucciones en el archivo readme.md para instalarlo.
Una vez que SillyTavern-Launcher está instalado:
1. Run Launcher.bat
2. Go to: `Home > Toolbox > App Installer > Voice Generation`
3. Select the option labeled: **Install AllTalk V2**

#### 🟩 Instalación Manual
Para usuarios avanzados que requieren control detallado, sigue la [Guía de Instalación Manual](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide) para una configuración paso a paso en Windows, Linux o Mac (no probado).

#### 🟩 Instalación Google Colab
Ejecuta AllTalk en un entorno en la nube con la [Instalación Google Colab](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB) para usuarios que prefieren no instalar localmente.

---

## 🟨 Usando AllTalk dentro de SillyTavern

Una vez que AllTalk esté cargado, selecciónalo dentro de SillyTavern en la página TTS, asegurándote de seleccionar la versión correcta del servidor AllTalk en la configuración.

- **Gestión de Configuración**: AllTalk puede habilitar o deshabilitar configuraciones específicas según tu configuración seleccionada.
- **Secuencia de Carga**: Si SillyTavern se carga antes de AllTalk, recarga la página de extensiones TTS.
- **Optimización de Rendimiento**: Habilita los modos DeepSpeed y VRAM Bajo selectivamente para mejorar el rendimiento según los recursos del sistema.
- **Función Narrador**: Los detalles de la función Narrador se pueden encontrar en el [Wiki de AllTalk](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

Los detalles completos de la Extensión SillyTavern AllTalk se actualizarán en la [página Wiki de AllTalk para SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

Los usuarios de TGWUI que usan la extensión AllTalk para TGWUI necesitan deshabilitar `Enable TGWUI TTS` en la interfaz de chat de TGWUI, de lo contrario tendrás audio TTS duplicado generado.

---

## 🟨 Solución de Problemas

Si experimentas problemas que crees son específicos de AllTalk dentro de SillyTavern, consulta la [página Wiki de AllTalk para SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) para obtener la información más reciente.

---

### 🟪 Soporte, Asistencia y Solicitudes de Características

Para más asistencia:
- Consulta el [Wiki](https://github.com/erew123/alltalk_tts/wiki) y la documentación integrada.
- Únete a las discusiones en el [Foro de Discusión](https://github.com/erew123/alltalk_tts/discussions/245).
- Envía errores o solicitudes de características a través del [Rastreador de Problemas](https://github.com/erew123/alltalk_tts/issues).

---
