---
# icon: container
label: Docker
route: /installation/docker/
---

# Instalación de Docker

!!!
Estas instrucciones asumen que ha instalado Docker, puede acceder a su línea de comandos para la instalación de contenedores y está familiarizado con su funcionamiento general.
!!!

## Usando el GitHub Container Registry

Usar una imagen precompilada es la forma más rápida y fácil de comenzar con SillyTavern en Docker. Puede extraer la última imagen del GitHub Container Registry.

### Docker Compose (recomendado)

Descargue el archivo `docker-compose.yml` del [GitHub Repository](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) y ejecute el siguiente comando en el directorio donde se encuentra el archivo. Esto extraerá la última imagen de versión del GitHub Container Registry e iniciará el contenedor, creando automáticamente los volúmenes necesarios.

```sh
docker compose up
```

Puede editar el archivo y aplicar personalizaciones adicionales según sus necesidades:

- El puerto predeterminado es 8000. Puede cambiarlo modificando la sección `ports`.
- Cambie la etiqueta `image` a `staging` si desea utilizar la rama de desarrollo en lugar de la versión estable.
- Si desea ajustar la configuración del servidor utilizando variables de entorno, consulte la página [Environment Variables](/Administration/config-yaml.md#environment-variables).

### Docker CLI (avanzado)

Necesitará dos mapeos de directorios obligatorios y un mapeo de puerto para permitir que SillyTavern funcione. En el comando, reemplace sus selecciones en los siguientes lugares:

#### Container Variables

##### Volume Mappings

- `CONFIG_PATH` - El directorio donde se almacenarán los archivos de configuración de SillyTavern en su máquina host
- `DATA_PATH` - El directorio donde se almacenarán los datos de usuario de SillyTavern (incluidos los personajes) en su máquina host
- `PLUGINS_PATH` - (opcional) El directorio donde se almacenarán los complementos de servidor de SillyTavern en su máquina host
- `EXTENSIONS_PATH` - (opcional) El directorio donde se almacenarán las extensiones de interfaz de usuario global en su máquina host

##### Port Mappings

- `PUBLIC_PORT` - El puerto para exponer el tráfico. Esto es obligatorio, ya que accederá a la instancia desde fuera de su contenedor de máquina virtual. NO exponga esto a Internet sin implementar un servicio separado para seguridad.

##### Additional Settings

- `SILLYTAVERN_VERSION` - En la [GitHub Packages page](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) verá la lista de versiones de imágenes etiquetadas. La etiqueta de imagen "latest" lo mantendrá actualizado con la versión actual. También puede utilizar "staging" que apunta a la imagen nocturna de la rama respectiva.

#### Running the container

1. Abra su línea de comandos
2. Ejecute el siguiente comando en una carpeta donde desee almacenar los archivos de configuración y datos:

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
Por defecto, el contenedor se ejecutará en primer plano. Si desea ejecutarlo en segundo plano, agregue la bandera `-d` al comando `docker run`.
!!!

## Compilando la Imagen de Docker

!!!info
La siguiente sección asume que instaló SillyTavern en una carpeta sin privilegios de root (sin administrador). Si instaló SillyTavern en una carpeta con privilegios de root, es posible que tenga que ejecutar algunos de estos comandos con derechos de administrador [`sudo`, `doas`, Command Prompt (Administrator)].
!!!

Si desea compilar la imagen de Docker usted mismo, puede hacerlo siguiendo estos pasos. Esto es útil si desea personalizar la imagen o utilizarla con fines de desarrollo.

### Linux

1. Instale Docker siguiendo la guía de instalación de Docker [aquí](https://docs.docker.com/engine/install/).
   !!!danger
   **No** instale Docker Desktop.
   !!!
2. Siga los pasos en **Manage Docker as a non-root user** en la [Post-Installation Guide](https://docs.docker.com/engine/install/linux-postinstall/) de Docker.
3. Instale [Git](https://git-scm.com/download/linux) usando su administrador de paquetes.

    - Debian (Ubuntu/Pop! OS/etc.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/etc.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), etc.
        ```sh
        sudo dnf install git
        ```

4. Clone el repositorio de SillyTavern.

    - Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Ejecute `docker compose` ejecutando el siguiente comando dentro de la carpeta de Docker.

    ```sh
    docker compose up -d
    ```

6. Abra un navegador nuevo y vaya a [http://localhost:8000](http://localhost:8000). Debería ver que SillyTavern se carga en unos momentos.

### Windows

!!!warning Regarding Docker on Windows
Usar Docker en Windows es **_muy_** complicado. No solo necesita activar _Windows Subsystem for Linux_ dentro de _Turn Windows features on or off_, sino también configurar su sistema para Virtualización (Intel VT-d/AMD SVM) que difiere de un fabricante de PC a otro (o de un fabricante de placa base a otro). A veces, esta opción no está presente en algunos sistemas.

Se sugiere altamente que instale SillyTavern siguiendo nuestra guía [Windows](/Installation/Windows.md). Esta sección es una idea _aproximada_ de cómo se puede hacer en Windows.
!!!

1.  Instale Docker Desktop siguiendo la guía de instalación de Docker [aquí](https://docs.docker.com/desktop/setup/install/windows-install/).
2.  Instale [Git for Windows](https://git-scm.com/download/win).
3.  Clone el repositorio de SillyTavern.

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Ejecute `docker compose` ejecutando el siguiente comando dentro de la carpeta de Docker.

    ```sh
    docker compose up -d
    ```

5.  Abra un navegador nuevo y vaya a [http://localhost:8000](http://localhost:8000). Debería ver que SillyTavern se carga en unos momentos.

### macOS

!!!
Aunque macOS es similar a Linux, no tiene el Docker Engine. Tendrá que instalar Docker Desktop de manera similar a Windows.
También necesitará instalar [Homebrew](https://brew.sh/) para poder instalar Git en su Mac. Esta sección es una idea _aproximada_ de cómo se puede hacer en macOS.
!!!

1.  Instale Docker Desktop siguiendo la guía de instalación de Docker [aquí](https://docs.docker.com/desktop/setup/install/mac-install/).
2.  Instale `git` usando Homebrew.

    ```sh
    brew install git
    ```

3.  Clone el repositorio de SillyTavern.

    -   Release (Stable Branch)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Development Branch)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Ejecute `docker compose` ejecutando el siguiente comando dentro de la carpeta de Docker.

    ```sh
    docker compose up -d
    ```

5.  Abra un navegador nuevo y vaya a [http://localhost:8000](http://localhost:8000). Debería ver que SillyTavern se carga en unos momentos.

## Configurando SillyTavern

El archivo de configuración de SillyTavern (config.yaml) se ubicará dentro de la carpeta `config`. Configurar el archivo config debería ser igual a configurarlo sin Docker, sin embargo, necesitará ejecutar `nano` o un editor de código con derechos de administrador para guardar sus cambios.

!!!warning
¡No olvide reiniciar el contenedor de Docker para SillyTavern para aplicar sus cambios! Asegúrese de ejecutar este comando dentro de la carpeta `docker`.

```sh
docker compose restart sillytavern
```

!!!

## Locating User Data

La carpeta de datos de SillyTavern se ubicará dentro de la carpeta `data`. Hacer una copia de seguridad de sus archivos debería ser fácil, sin embargo, restaurar o agregar contenido en ella puede requerir que lo haga con derechos de administrador.

## Running Server Plugins

Ejecutar complementos como [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) o [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) dentro de Docker no es diferente de ejecutarlo en su sistema sin Docker, sin embargo, necesitaremos hacer una ligera modificación en el script de Docker Compose para hacerlo.

!!! Note
Si ya ve una carpeta _plugins_ dentro de la carpeta `docker`, puede omitir los pasos 1-2.
!!!

1. Usando `nano` o un editor de código, abra _docker-compose.yml_ y agregue la siguiente línea debajo de `volumes`.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. Cree una nueva carpeta dentro de la carpeta `docker` llamada _plugins_.
3. Siga las instrucciones de su complemento para instalar el complemento.
4. Usando `nano` o un editor de código con derechos de administrador, abra _config.yaml_ (dentro de la carpeta `config`) y habilite `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. Reinicie el contenedor de Docker.

    ```sh
    docker compose restart sillytavern
    ```

## Common issues with Docker

### SELinux Permission Issues with Mounted Volumes

Las distribuciones de Linux con SELinux habilitado (como RHEL, CentOS, Fedora, etc.) pueden impedir que los contenedores de Docker accedan a volúmenes montados debido a políticas de seguridad. Esto puede resultar en errores de permiso denegado cuando el contenedor intenta leer o escribir en los directorios montados.

Se pueden agregar dos sufijos `:z` o `:Z` al montaje de volumen. Estos sufijos le dicen a Docker que reetiquete objetos de archivo en los volúmenes compartidos.

- La opción `z` se utiliza cuando el contenido del volumen se compartirá entre contenedores.
- La opción `Z` se utiliza cuando el contenido del volumen solo debe ser utilizado por el contenedor actual.

Example:

```yaml
# docker-compose.yml
volumes:
  ## Shared volume
  - ./config:/home/node/app/config:z
  ## Private volume
  - ./data:/home/node/app/data:Z
```

### Forbidden by Whitelist

!!!
Las IP de la puerta de enlace de Docker deben ser agregadas a la lista blanca automáticamente si el valor de configuración [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) está establecido en `true`.

Si aún no puede acceder a SillyTavern, siga las instrucciones a continuación para actualizar la lista blanca manualmente.
!!!

1. Ejecute el siguiente comando de Docker para obtener la IP de su contenedor de Docker de SillyTavern.

    ```sh
    docker network inspect docker_default
    ```

    Debería recibir algún tipo de salida similar a la siguiente.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    Copie la IP que ve en _Gateway_ ya que esto será importante.

2. Ejecutando un editor de texto de su elección con derechos de administrador, vaya a `config` y abra `config.yaml`.

    Dentro de su editor, desplácese hacia la sección `whitelist`. Debería ver algo similar a lo siguiente.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Agregue una nueva línea debajo de _127.0.0.1_ y ponga la IP que copió de Docker. Debería verse algo similar a lo siguiente después.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Guarde el archivo y salga del editor de texto.

    !!!info
    Tenga en cuenta que si configuró la red de Docker como un puente, también puede agregar direcciones IP externas a la lista blanca como de costumbre.
    !!!

3. Reinicie el Contenedor de Docker para aplicar la nueva configuración.

    ```sh
    docker compose restart sillytavern
    ```
