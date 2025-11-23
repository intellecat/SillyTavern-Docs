---
order: 50
icon: package
expanded: true
route: /installation/
---

# Instalación

Sigue la guía de instalación para tu plataforma:

* [Windows](/Installation/Windows.md)
* [Linux y Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Ramas

SillyTavern se está desarrollando utilizando un sistema de dos ramas para garantizar una experiencia fluida para todos los usuarios.

* `release` - 🌟 **Recomendado para la mayoría de usuarios.** Esta es la rama más estable y recomendada, actualizada solo cuando se lanzan versiones principales. Es adecuada para la mayoría de usuarios. Típicamente se actualiza una vez al mes.
* `staging` - ⚠️ **No recomendado para uso casual.** Esta rama tiene las características más recientes, pero tenga cuidado ya que puede romperse en cualquier momento. Solo para usuarios avanzados y entusiastas. Se actualiza varias veces al día.

## Modo Global / Independiente

Hay dos modos de ejecutar SillyTavern que difieren en cómo manejan las rutas de configuración y datos.

* **Modo Independiente** (predeterminado) - utiliza el archivo `config.yaml` y el directorio `data` en el directorio del servidor. Todos los datos estarán confinados a la ruta de instalación. Este es el modo recomendado para la mayoría de usuarios.
* **Modo Global** - utiliza rutas del sistema para la configuración y los datos. Esto es útil para instalar SillyTavern como un paquete o cuando desea compartir la misma configuración y datos entre múltiples instalaciones.

!!!info
Las instalaciones realizadas utilizando el [paquete npm oficial](https://www.npmjs.com/package/sillytavern) (p. ej. `npx sillytavern@latest`) se ejecutarán en modo global de forma predeterminada.
!!!

### Rutas de datos

Las rutas del **Modo Independiente** son relativas al directorio de instalación de SillyTavern:

* **Ruta de configuración**: `./config.yaml`
* **Raíz de datos**: `./data/`

Las rutas del **Modo Global** dependen del sistema operativo:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (o `$XDG_DATA_HOME/SillyTavern/config.yaml`) y `~/.local/share/SillyTavern/data/` (o `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` y `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` y `~/Library/Application Support/SillyTavern/data/`

### Cómo ejecutar en modo global

!!!warning
`dataRoot` y `configPath` no pueden ser reemplazados con [argumentos CLI](../Administration/config-yaml.md#command-line-arguments) o [config.yaml](../Administration/config-yaml.md) cuando se ejecuta en modo global.
!!!

1. Pasa el argumento `--global` al comando de inicio del servidor (p. ej. `node server.js --global`).
2. Pasa el argumento `--global` al script de inicio del shell (p. ej. `Start.bat --global` o `./start.sh --global`).
3. Usa el script `start:global` en el archivo `package.json` (p. ej. `npm run start:global`).
