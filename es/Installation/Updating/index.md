---
label: Actualizando
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---

# Cómo Actualizar SillyTavern

Encuentra tu SO a continuación y sigue las instrucciones para actualizar ST.

!!! Para instrucciones de instalación, consulta la página [Instalación](/Installation/index.md).

Esta guía asume que ya has instalado y ejecutado SillyTavern al menos una vez.
!!!

----

## Linux/Termux o MacOS

Definitivamente instalaste mediante git, así que simplemente 'git pull' dentro del directorio de SillyTavern.

- `cd SillyTavern` para entrar en la carpeta correcta.
- `git pull` para obtener la actualización.
- `./start.sh` o `bash start.sh` para iniciar ST.

----

## Windows

>Primero intenta usar `UpdateAndStart.bat` que se encuentra en tu carpeta base de instalación de SillyTavern.

Si eso falla, vuelve aquí y continúa leyendo.

### Método 1 - GIT

Siempre recomendamos a los usuarios instalar usando 'git'. Aquí está el por qué:

Cuando hayas instalado mediante `git clone`, todo lo que tienes que hacer para actualizar es escribir `git pull` [en una línea de comando en la carpeta ST](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder).
Alternativamente, si la línea de comandos te causa problemas (y tienes GitHub Desktop instalado), puedes usar el menú `Repository` y seleccionar `Pull`.

Las actualizaciones se aplican automática y seguramente.

#### "Ayuda Originalmente instalé mediante Zip y ahora quiero convertir a instalación Git"

Has elegido un camino sabio.

Como tu instalación se realizó mediante Zip, necesitarás hacer una nueva instalación usando git.

Afortunadamente tenemos [instrucciones](/Installation/Windows.md) sobre cómo hacerlo.

Una vez que hayas usado git para instalar un NUEVO SillyTavern en una carpeta DIFERENTE, vuelve a esta página y continúa con el **Paso 4** de las instrucciones 'Actualización de Zip' a continuación.

### Método 2 - ZIP

Si insistes en instalar mediante un zip, aquí está el tedioso proceso para hacer la actualización:

1. Descarga el nuevo zip de lanzamiento.
2. Descomprimelo en una carpeta FUERA de tu instalación actual de ST.
3. Realiza el procedimiento de configuración habitual para tu SO para instalar los requisitos de NodeJS.

4. Copia los siguientes archivos/carpetas según sea necesario(*) de tu vieja instalación de ST:

    (*) 'Según sea necesario' = "Si realizaste algún contenido personalizado relacionado con esas carpetas".

    #### Actualizando >=1.12.0

    Copia el directorio `/data` y el archivo `config.yaml` de una instalación a otra. Si tienes extensiones de todo el servidor (instaladas para "Todos los usuarios") que deseas preservar, también copia el directorio `/public/scripts/extensions/third-party`.

    #### Actualizando de <1.12.0 a >1.12.0

    1.12.0 incluye un procedimiento de migración automatizado. Los pasos a continuación son necesarios *solo* si la migración fue interrumpida o hubo un error.

5. Ejecuta la instalación del servidor actualizado al menos una vez para crear el directorio `/data/default-user`.
6. Transfiere los archivos de `/public` anterior a `/data/default-user` nuevo según sea necesario.

    Ninguna de las carpetas es obligatoria, así que solo copia lo que necesitas.

    **NOTA: NO COPIES LA CARPETA /PUBLIC/ COMPLETA**

    Hacerlo podría romper la nueva instalación e impedir que nuevas características estén presentes.

    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- este está en la carpeta base, no en /public/
    ```

7. Una vez que esas carpetas/archivos sean copiados, pégalos en la carpeta /data/default-user (con secrets.json yendo a la raíz de la carpeta) de la nueva instalación.
8. Inicia SillyTavern una vez más con el método apropiado para tu SO, y reza para que lo hayas hecho correctamente.
9. Si todo aparece, puedes eliminar de forma segura la carpeta antigua de ST.

### Problemas Comunes de Actualización

#### "Hay conflictos no resueltos en el directorio de trabajo."

Esto significa que has modificado archivos predeterminados que han sido cambiados en el repositorio remoto (como la configuración de presets).

Para arreglar esto, ejecuta esto en la terminal. Úsalo con cuidado, ya que puede ser destructivo. Asegúrate de tener una copia de seguridad si es necesario.

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### Los cambios de archivos previenen git pull

- Si cambias archivos del sistema de SillyTavern, `git pull` puede no funcionar.
- A veces, una actualización puede requerirns cambiar un archivo importante, lo que puede causar el mismo problema.
- Generalmente son archivos de presets predeterminados o `package-lock.json`.
- En este caso puedes intentar mover el archivo a una carpeta diferente (o eliminar el archivo) y luego hacer `git pull`.
- Otra solución es usar `git pull --rebase --autostash`

#### Error: No se puede encontrar el módulo "***" al iniciar el servidor

- Esto significa que SillyTavern agregó un nuevo requisito de paquete npm.
- Ejecuta `npm install` en el directorio de SillyTavern para arreglar esto. Los scripts proporcionados Start.bat y start.sh lo harán automáticamente.
- ¿No funcionó? Elimina la carpeta node_modules

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```

## Docker

1. Abre una ventana de terminal y navega a tu directorio docker `cd SillyTavern/docker`
2. Elimina tu contenedor con `docker compose down`
3. Elimina la imagen de Docker de SillyTavern del caché `docker rmi ghcr.io/sillytavern/sillytavern:latest` (Reemplaza `sillytavern:latest` con `sillytavern:staging` si estás apuntando a la rama de staging.)
4. Reconstruye el contenedor con `sudo docker compose up -d`

Si todo va bien, Docker debe comenzar a descargar nuevamente la imagen y estarás en funcionamiento en breve. Si encuentras algún problema, consulta la siguiente sección de esta guía.

### Problemas Comunes de Actualización
#### ¡Uso Docker y todos mis datos desaparecieron después de la actualización!

Debes seguir la [Guía de migración para contenedores Docker](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)
 para actualizar las asignaciones de volumen para el nuevo modelo de datos introducido en 1.12.0

#### Permiso denegado al ejecutar comandos de Docker

Este es un problema de Linux e implica que tus permisos no están configurados correctamente. Hay dos formas de solucionar esto:

1. **El método fácil**: Si tienes acceso sudo en tu usuario, simplemente prefija los comandos con `sudo` (por ejemplo: `sudo docker compose down`)
2. **El método correcto**: Arregla tus permisos. Esto varía dependiendo de la versión de Linux que uses. Hay muchas guías en línea para ayudarte a arreglar este problema.
