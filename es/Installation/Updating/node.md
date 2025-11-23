---
order: -50
route: /installation/updating/node/
---

# Cómo actualizar Node.js

Es importante mantener su Node.js runtime actualizado por razones de seguridad y rendimiento. A continuación se encuentran los pasos para actualizar Node.js según su sistema operativo.

Recomendamos usar la versión Long Term Support (LTS) más reciente, que puede encontrar en el [sitio web oficial de Node.js](https://nodejs.org/en/about/previous-releases).

## Cómo comprobar su versión actual de Node.js

1. Abra su terminal o símbolo del sistema.
2. Escriba el siguiente comando y presione Intro:

```bash
node -v
```

## nvm (Node Version Manager) - Multiplataforma

Si está utilizando `nvm`:

1. Abra su terminal.
2. Escriba el siguiente comando:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - Instalación Regular

1. Vaya a la [página de descargas](https://nodejs.org/en/download/) de Node.js.
2. Descargue el instalador de Windows para la versión LTS.
3. Ejecute el instalador y siga las indicaciones para completar la instalación.

## Windows - SillyTavern Launcher

Si ha instalado usando el SillyTavern Launcher:

1. Abra el SillyTavern Launcher.
2. Navegue a `Toolbox / App Installer / Core Utilities / Install Node.js`.

**O:**

Hágalo manualmente usando winget en PowerShell:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Abra la aplicación Termux.
2. Escriba los siguientes comandos:

```bash
pkg update
pkg upgrade nodejs-lts
```

No olvide aceptar los mensajes que puedan aparecer durante el proceso de actualización presionando `Y` en el teclado virtual.

## macOS - Instalación Regular

1. Vaya a la [página de descargas](https://nodejs.org/en/download/) de Node.js.
2. Descargue el instalador de macOS para la versión LTS.
3. Ejecute el archivo `.pkg` y siga las indicaciones para completar la instalación.

## macOS - Homebrew

Si tiene Homebrew instalado, puede actualizar Node.js con los siguientes comandos:

```bash
brew update
brew upgrade node
```

## Linux - Administrador de Paquetes

El método para actualizar Node.js en Linux depende de su distribución.

Pero como la versión de Node.js en los repositorios oficiales puede no ser la más reciente, recomendamos usar [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) o el [repositorio NodeSource](https://github.com/nodesource/distributions).

## Docker

No se requiere ninguna acción. La imagen Docker prediseñada que proporcionamos se compila con la versión más reciente de Node.js.
