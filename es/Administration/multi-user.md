---
title: Modo multi-usuario
icon: people
order: -10
route: /administration/multi-user/
---

El modo multi-usuario permite que varias personas usen un servidor de SillyTavern. Cada usuario tiene sus propias configuraciones, extensiones y datos. Las cuentas de usuario también pueden estar protegidas con contraseña.

!!!warning
Las contraseñas de usuario proporcionan privacidad básica entre usuarios de una configuración multi-usuario. No son una característica de seguridad y no deben considerarse como tal. Todos los datos de usuario (incluido el historial de chat, claves API y otra información sensible) se almacenan en texto plano en el servidor. Pueden ser vistos y modificados por cualquier persona con acceso al sistema de archivos del servidor. **No uses SillyTavern en un servidor público o con usuarios no confiables.**
!!!

## Configuración

Para habilitar y usar el modo multi-usuario, edita el archivo `config.yaml`:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. Cuando la configuración de cuenta de usuario está deshabilitada, se utiliza una cuenta de administrador de respaldo `default-user` para almacenar los datos del usuario.
2. Cuando la configuración de inicio de sesión discreto está deshabilitada, se muestra una lista de usuarios activos en la pantalla de inicio de sesión. Si está habilitada, el usuario debe ingresar su identificador manualmente.

!!!info
No puedes _eliminar_ la cuenta `default-user` de la lista de usuarios porque se utiliza para servir los datos del usuario en caso de que `enableUserAccounts` esté configurado como `false`. Pero puedes _deshabilitarla_ para ocultarla de la lista y no permitir inicios de sesión.
!!!

## Identificadores de usuario

Un identificador es el identificador único de un usuario. Solo puede consistir en letras minúsculas, números y guiones.

La ruta al directorio de datos del usuario asume usar el siguiente patrón: `%DATA_ROOT%/%USER_HANDLE%`.

Ejemplos de identificadores de usuario válidos:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## Roles

- **Admin** - puede gestionar (crear, eliminar, modificar) otros usuarios. Puede instalar extensiones para todos los usuarios.
- **User** - no puede gestionar otros usuarios. Puede instalar extensiones solo para sí mismo.

Excepto por tener acceso al panel de administración, ambos roles de usuario son funcionalmente idénticos y pueden usar toda la gama de características de SillyTavern sin restricciones. Una implementación de permisos de usuario está por definirse.

Todas las cuentas de usuario se crean primero como usuarios regulares, y luego podrían ser promovidas a administradores si es necesario.

### Pantalla de inicio de sesión

Allí puedes seleccionar una cuenta de usuario para usar. Tiene dos estilos, dependiendo del valor de configuración `enableDiscreetLogin`.

La pantalla de inicio de sesión se omite y no se muestra cuando tienes solo un usuario activo y no está protegido con contraseña.

### Perfil de usuario

Puedes acceder a un menú de autogestión de cuenta usando el botón "Account" bajo el panel "User settings" en la barra de menú superior.

1. Nombre para mostrar - usado en la pantalla de inicio de sesión, puede cambiarse. No se correlaciona con las personas y no es visible para las APIs de IA - aún puedes usar tantas personas como quieras.
2. Imagen de perfil - usada en la pantalla de inicio de sesión. Puedes usar una imagen personalizada, la imagen de persona predeterminada (si está configurada), o la última persona usada de lo contrario.
3. Contraseña - un ícono de candado refleja el estado de protección de la cuenta (candado abierto = sin contraseña). Una contraseña puede establecerse, cambiarse o eliminarse usando el botón "Change Password".
4. Instantáneas de configuración - accede y revisa las copias de seguridad de tu archivo `settings.json`, con la capacidad de crear o restaurar instantáneas.
5. Descargar copia de seguridad - descarga un archivo de tu carpeta de datos de usuario.
6. Restablecer configuración - restablece la configuración predeterminada de fábrica, dejando otros datos (personaje, chats) intactos.

## Recuperación de contraseña

1. Una contraseña puede recuperarse desde la pantalla de inicio de sesión. Necesitas acceso a la consola del servidor para obtener un código de recuperación de un solo uso (que consiste en 4 dígitos).
2. Alternativamente, puedes usar un script de utilidad en el servidor de SillyTavern para restablecer una contraseña proporcionando el identificador de usuario.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## Andamiaje de contenido

Para agregar contenido personalizado para usuarios, puedes usar la característica de andamiaje de contenido. Esta característica te permite definir un conjunto de archivos que se copiarán al directorio de datos de cada usuario cuando el servidor se inicie.

Debes crear un archivo `index.json` en el directorio `/default/scaffold` para que esta característica funcione. La sintaxis es la misma que para el contenido predeterminado. Todas las rutas de archivo deben ser relativas al directorio `/default/scaffold`, y puedes organizar archivos usando subdirectorios.

Los archivos andamiados se copian antes que los archivos predeterminados, lo que significa que sobrescribirán cualquier archivo predeterminado (presets/configuración/etc.) que tenga el mismo nombre de archivo.

!!!tip
Cada directorio de datos de usuario tiene un archivo `content.log` que lista todos los archivos copiados desde los directorios scaffold y default. Elimina este archivo para forzar al servidor a sincronizar el contenido nuevamente en el próximo reinicio.
!!!

### Tipos de contenido reconocidos

| Tipo                          | Valor                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Tarjeta de personaje          | `'character'`        |
| Sprites de personaje          | `'sprites'`          |
| Imagen de fondo               | `'background'`       |
| Archivo World Info            | `'world'`            |
| Avatar de persona             | `'avatar'`           |
| Tema de UI                    | `'theme'`            |
| Flujo de trabajo ComfyUI      | `'workflow'`         |
| Preset de KoboldAI Classic    | `'kobold_preset'`    |
| Preset de Chat Completion     | `'openai_preset'`    |
| Preset de NovelAI             | `'novel_preset'`     |
| Preset de Text Completion     | `'textgen_preset'`   |
| Plantilla de Instruct Mode    | `'instruct'`         |
| Plantilla de Context Formatting | `'context'`        |
| Preset de MovingUI            | `'moving_ui'`        |
| Conjunto de Quick Replies     | `'quick_replies'`    |
| Plantilla de System Prompt    | `'sysprompt'`        |
| Plantilla de Reasoning Formatting | `'reasoning'`    |

### Ejemplo (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
