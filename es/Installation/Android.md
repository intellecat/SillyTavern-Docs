---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Instalación de Android (Termux)

SillyTavern puede ejecutarse de forma nativa en dispositivos Android usando Termux.

## Instalación de Termux

!!!tip
Evita instalar Termux desde Google Play Store, esa versión ya no se mantiene.
En su lugar, usa F-Droid (recomendado) o las versiones de GitHub para obtener la versión más reciente.
!!!

1. Descarga Termux desde [F-Droid](https://f-droid.org/en/packages/com.termux/) o [versiones de GitHub](https://github.com/termux/termux-app/releases).
2. Instala el archivo APK descargado.
3. Abre Termux y ejecuta tu primer comando:

   ```bash
   termux-change-repo
   ```

4. Selecciona "Mirror group" y elige los servidores más cercanos. Puedes tocar la pantalla o usar gestos de deslizamiento con [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en).
5. Actualiza Termux:

   ```bash
   pkg update && pkg upgrade
   ```

## Instalación de Dependencias

Instala los paquetes requeridos:

```bash
pkg install git nodejs-lts nano
```

!!!warning
Si estás ejecutando Android de 32 bits, consulta la sección [Errores Comunes](#errores-comunes) a continuación para obtener pasos adicionales.
!!!

## Instalación de SillyTavern

Clona el repositorio de SillyTavern ([Cómo elegir una rama](/Installation/index.md#branches)):

- **Rama Release:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Rama Staging:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## Ejecutar SillyTavern

Para ejecutar SillyTavern, navega al directorio clonado y ejecuta el script de inicio:

```bash
cd ~/SillyTavern
bash start.sh
```

Para actualizar SillyTavern, navega al directorio de SillyTavern y ejecuta:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

Consulta la sección [Crear Alias](#opcional-crear-alias) a continuación para crear atajos que simplifiquen este proceso.

## Errores Comunes

### Unsupported platform: android arm LEtime-web

Android de 32 bits requiere una dependencia externa que no se puede instalar con npm.

Usa el siguiente comando para instalarla:

```bash
pkg install esbuild
```

Luego procede con los pasos de instalación anteriores.

### Optimización de Rendimiento

!!!info
Para obtener consejos generales sobre cómo mejorar el rendimiento, consulta la sección [FAQ](/Usage/faq.md#performance-tips) respectiva.
!!!

Debido a las limitaciones de hardware en dispositivos Android, es posible que desees ajustar los siguientes parámetros de [config.yaml](/Administration/config-yaml.md) de SillyTavern para mejorar el uso de memoria, almacenamiento y CPU:

```yaml
performance:
  # Avoid loading all character data until needed
  lazyLoadCharacters: true
  # Disable disk caching to reduce storage usage
  useDiskCache: false
backups:
  chat:
    # Optional: Disable automatic chat backups to save storage
    enabled: false
```

!!!tip
Usa el editor de texto `nano` incluido con Termux para editar el archivo `config.yaml`: `nano ~/SillyTavern/config.yaml`
!!!

## Opcional: Crear Alias

Puedes crear atajos para comandos comunes para facilitar tu flujo de trabajo.

1. Abre un editor para modificar tu archivo `.bashrc`:

   ```bash
   nano ~/.bashrc
   ```

2. Agrega las siguientes líneas para crear alias:

   ```bash
   # Update Termux packages
   alias pkgup="pkg update && pkg upgrade"
   #Start SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # Update SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. Guarda el archivo y sal del editor (en nano, presiona `CTRL + X`, luego `Y`, luego `Enter`).

4. Para aplicar los cambios, ejecuta:

   ```bash
   source ~/.bashrc
   ```

Ahora puedes usar los siguientes comandos:

- `st` para iniciar SillyTavern
- `stup` para actualizar SillyTavern
- `pkgup` para actualizar paquetes de Termux

## Lecturas Adicionales

!!!info
Los tutoriales enlazados a continuación no son mantenidos por el equipo de SillyTavern.
!!!

- SillyTavern en guía de Termux por ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- Acceso a archivos de Termux con Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Evitar que el proceso Termux entre en reposo profundo: <https://wiki.termux.com/wiki/Termux-wake-lock>
