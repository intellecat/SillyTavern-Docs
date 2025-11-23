---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# Guía de Migración 1.12.0

SillyTavern 1.12.0 (con el nombre en clave "Neo Server" update) incluye varios cambios críticos que pueden afectar la forma en que utilizas SillyTavern.

Esta guía te preparará para la actualización y te proporcionará más orientación.

## Actualización de almacenamiento de datos

1.12.0 cambia la forma en que SillyTavern maneja los datos del usuario.

Anteriormente, todos los datos persistentes se almacenaban juntos con la parte frontend en el directorio `/public`, lo que creaba confusión y posibles puntos de fallo, además de hacer que la containerización e instalación de la aplicación en todo el sistema fuera bastante desafiante.

### ¿Qué ha cambiado?

Toda la información persistente de `/public` como configuración y chats (lista completa abajo) se movió a un directorio separado con ruta configurable, haciéndolo portátil e independiente del servidor web en sí. Cuando sea necesario para propósitos de compatibilidad, por ejemplo, para alojar extensiones, tarjetas de personaje a tamaño completo, cargas de imágenes de usuario, etc., se ha configurado una redirección inteligente para alojar automáticamente archivos de usuario desde el directorio de datos.

### Establecer una raíz de datos

Puedes proporcionar una ruta absoluta o relativa (al directorio del repositorio de ST) a la raíz de datos ya sea mediante `config.yaml` o iniciando el servidor con el argumento de consola `--dataRoot`.

> Ejemplo de YAML

```yaml
# -- DATA CONFIGURATION --
# Root directory for user data storage
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> Ejemplo de consola

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OR
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

La ruta de raíz de datos predeterminada es `./data`, lo que significa el directorio `data` en el repositorio de SillyTavern.

!!!info Nota
La ruta de la raíz de datos debe ser una ruta **absoluta completa** o una ruta **relativa completa**. _No puedes_ usar atajos de ruta como `~` o `%APP_DATA%`, ya que estos se resuelven por un shell, no por el sistema operativo.
!!!

### Migración

#### **¡IMPORTANTE!** Antes de comenzar

1. **Solo si quieres mover dataRoot desde la ubicación predeterminada. De lo contrario, omite esta parte.** Establece la raíz de datos _antes_ de ejecutar por primera vez el servidor después de extraer una actualización. Ejecuta `npm install` para que `config.yaml` se complete con un nuevo valor, o pasa un argumento de consola.
2. Todos los datos se migrarán a una cuenta `default-user`. Ver más en [Usuarios](#usuarios) abajo.

#### Instalaciones sin contenedor (metal desnudo)

No tienes que hacer nada. Una migración automática debería manejar todo por ti cuando inicies el servidor ST y detecte el formato de almacenamiento antiguo (verificando la existencia del directorio `/public/characters`).

Al mover archivos, se creará una copia de seguridad automática en el directorio `/backups/_migration/YYYY-MM-DD` (resuelto a la fecha actual), pero siempre es una buena práctica hacer una copia de seguridad manual completa antes de ejecutar la migración.

#### Instalaciones containerizadas (Docker)

Migrar los datos en volúmenes de Docker es un poco más complicado pero bastante directo. Aunque el `docker-compose.yml` proporcionado con el repositorio se actualizó para reflejar los cambios, es posible que necesites ajustar tus flujos de trabajo/implementaciones personalizadas.

**Paso 1.** Crea un nuevo volumen y móntalo en la ruta "/home/node/app/data" dentro del contenedor. No elimines el volumen `config`.

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**Paso 2.** Mueve todo excepto el archivo `config.yaml` del volumen `config` al subdirectorio `default-user` del volumen `data`.

**Paso 3.** Reconstruye el contenedor e inicia.

!!!info Nota
Los enlaces simbólicos entre el directorio `/public` y el volumen `config` ya no son necesarios y no se incorporan al contenedor de Docker.
!!!

#### ¿Qué migrar?

Los siguientes archivos y directorios están sujetos a la migración de datos. Asumiendo la configuración predeterminada, las rutas antes y después se proporcionan en la tabla de abajo.

| Antes                                  | Después                              |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## Usuarios

1.12.0 añade una habilidad (completamente opcional) de crear una configuración multi-usuario en el mismo servidor, permitiendo que múltiples usuarios usen sus propias instancias de SillyTavern completamente aisladas incluso al mismo tiempo. Las cuentas de usuario también pueden estar protegidas con contraseña para una capa adicional de privacidad.

Consulta la documentación de [Usuarios](/Administration/multi-user.md) para más información.
