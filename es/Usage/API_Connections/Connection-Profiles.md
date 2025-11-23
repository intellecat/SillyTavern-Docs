---
icon: paperclip
route: /usage/core-concepts/connection-profiles/
order: 100
---

# Perfiles de Conexión

Guarde Perfiles de Conexión para cambiar rápidamente entre diferentes APIs, modelos y plantillas de formato. Esto es útil cuando usa activamente múltiples conexiones API o necesita cambiar entre diferentes configuraciones sin navegar por los menús.

## Accediendo a Perfiles de Conexión

Esta función está habilitada por defecto a partir de SillyTavern 1.12.6 o posterior como extensión integrada, y está disponible en el menú de Conexiones API. Si desea *deshabilitarla*, abra el panel de Extensiones, haga clic en "Manager extensions", localice Connection Profiles en la lista, desmarque la casilla "Enabled", y luego haga clic en "Close".

## ¿Qué se Guarda?

Los Perfiles de Conexión almacenan las siguientes selecciones.

### Común

* [Tipo de API, modelo y URL del servidor](/Usage/API_Connections/index.md)
* [Clave Secreta](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Preajuste de Configuración](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (puede estar explícitamente vacío)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (puede estar explícitamente vacío)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### APIs de Finalización de Texto

* [System Prompt and its state](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Instruct Mode state and template](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### APIs de Finalización de Chat

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Preajuste de proxy

## Administrando Perfiles de Conexión

!!!info Nota
Los Perfiles solo guardan la selección en campos de lista desplegable, sin saber nada sobre la configuración subyacente. Esto significa que perderá los cambios no guardados al cambiar a un perfil diferente. Para evitar esto, asegúrese de actualizar todos los preajustes y plantillas si no desea perder cambios temporales.
!!!

* Para guardar un perfil, establezca toda la configuración requerida y haga clic en el botón "Create". Luego revise la configuración y proporcione un nombre para el perfil. **Un nombre debe ser único.**
* Para ver información detallada sobre un perfil elegido, haga clic en el botón "Information". Haga clic nuevamente para ocultar los detalles.
* La configuración del Perfil de Conexión se guarda en `settings.json` sin alterar el archivo de perfil guardado asociado hasta que presione el botón "Update". Esto significa que si configura un perfil, pero luego cambia a uno diferente sin actualizar, perderá todos sus cambios anteriores.
* Para restaurar las selecciones cambiadas desde un perfil guardado, haga clic en el botón "Reload".
* Para eliminar un perfil, haga clic en el botón "Delete" y confirme la eliminación. **Esta acción es irreversible.**

## Comandos de Barra Oblicua

Los Perfiles de Conexión se pueden administrar usando los siguientes comandos de barra oblicua.

1. `/profile [name]` - cambiar a un perfil si se proporciona el argumento, u obtener el nombre del perfil actual si no.
2. `/profile-create [name]` - guarda la configuración actual como un nuevo perfil con el nombre proporcionado.
3. `/profile-list` - devuelve una matriz serializada en JSON de nombres de perfil disponibles.
4. `/profile-get [name]` - obtiene los detalles del perfil con el nombre proporcionado como un objeto serializado en JSON.
5. `/profile-update` - actualiza el perfil seleccionado con la configuración actual.
