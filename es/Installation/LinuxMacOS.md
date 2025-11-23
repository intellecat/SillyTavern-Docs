---
label: MacOS y Linux
order: 5
route: /installation/linuxmacos/
---

# Instalación en Linux/MacOS

## Instalación Manual con Git

Para MacOS / Linux, todo esto se hará en una Terminal.

1. Instala git y NodeJS (el método para hacer esto variará según tu sistema operativo)
2. Clona el repositorio

   - para Rama de Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - para Rama de Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern` para navegar a la carpeta de instalación.
4. Ejecuta el script `start.sh` con uno de estos comandos:

- `./start.sh`
- `bash start.sh`

## Lanzador de SillyTavern

### Para usuarios de Linux
1. Abre tu terminal favorita e instala git
2. Descarga el Lanzador de SillyTavern con: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. Navega a SillyTavern-Launcher con: `cd SillyTavern-Launcher`
4. Inicia el lanzador de instalación con: `chmod +x install.sh && ./install.sh` y elige qué deseas instalar
5. Después de la instalación, inicia el lanzador con: `chmod +x launcher.sh && ./launcher.sh`

### Para usuarios de Mac
1. Abre una terminal e instala Homebrew con: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Luego instala git con: `brew install git`
3. Descarga el Lanzador de SillyTavern con: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. Navega a SillyTavern-Launcher con: `cd SillyTavern-Launcher`
5. Inicia el lanzador de instalación con: `chmod +x install.sh && ./install.sh` y elige qué deseas instalar
6. Después de la instalación, inicia el lanzador con: `chmod +x launcher.sh && ./launcher.sh`
