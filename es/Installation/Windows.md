---
order: 10
label: Windows
route: /installation/windows/
---
# Instalación de Windows

!!!advertencia
NO INSTALE EN NINGUNA CARPETA CONTROLADA POR WINDOWS (Program Files, System32, etc).

NO EJECUTE START.BAT CON PERMISOS DE ADMINISTRADOR

LA INSTALACIÓN EN WINDOWS 7 ES IMPOSIBLE YA QUE NO PUEDE EJECUTAR NODEJS 18.16
!!!

## Instalación a través de Git

1. Instale [NodeJS](https://nodejs.org/en) (se recomienda la versión LTS más reciente)
2. Instale [Git for Windows](https://gitforwindows.org/)
3. Abra el Explorador de Windows (`Win+E`)
4. Navegue a o cree una carpeta que no esté controlada ni supervisada por Windows. (ej: C:\MySpecialFolder\)
5. Abra el Símbolo del sistema dentro de esa carpeta haciendo clic en la "Barra de direcciones" en la parte superior, escribiendo `cmd` y presionando Intro.
6. Una vez que aparezca la caja negra (Símbolo del sistema), escriba UNO de lo siguiente en ella y presione Intro:

   - para la rama Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - para la rama Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. Una vez que todo esté clonado, haga doble clic en `Start.bat` para que NodeJS instale sus requisitos.
8. Luego, el servidor se iniciará y SillyTavern aparecerá en su navegador.

## Instalación a través de SillyTavern Launcher

1. En su teclado: presione **`WINDOWS + R`** para abrir el cuadro de diálogo Ejecutar. Luego, ejecute el siguiente comando para instalar git:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. En su teclado: presione **`WINDOWS + E`** para abrir el Explorador de archivos, luego navegue a la carpeta donde desea instalar el lanzador. Una vez en la carpeta deseada, escriba `cmd` en la barra de direcciones y presione Intro. Luego, ejecute el siguiente comando:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## Instalación a través de GitHub Desktop
(Esto permite el uso de git **solamente** en GitHub Desktop, si también desea usar `git` en la línea de comandos, también necesita instalar [Git for Windows](https://gitforwindows.org/))

1. Instale [NodeJS](https://nodejs.org/en) (se recomienda la versión LTS más reciente)
2. Instale [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. Después de instalar GitHub Desktop, haga clic en `Clone a repository from the internet....` (Nota: **NO necesita** crear una cuenta de GitHub para este paso)

    ![image](/static/windows-1.png)

4. En el menú, haga clic en la pestaña URL, ingrese esta URL `https://github.com/SillyTavern/SillyTavern` y haga clic en Clonar. Puede cambiar la ruta Local para cambiar dónde se descargará SillyTavern.

    ![image](/static/windows-2.png)

5. Para abrir SillyTavern, use el Explorador de Windows para navegar a la carpeta donde clonó el repositorio. De forma predeterminada, el repositorio se clonará aquí: `C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`

6. Haga doble clic en el archivo `start.bat`. (Nota: la parte `.bat` del nombre del archivo podría estar oculta por su SO, en ese caso, se verá como un archivo llamado "`Start`". Esto es lo que hace doble clic para ejecutar SillyTavern)

    ![image](/static/windows-3.png)

7. Después de hacer doble clic, debería abrirse una gran ventana de consola de comandos negra y SillyTavern comenzará a instalar lo que necesita para operar.

8. Después del proceso de instalación, si todo funciona, la ventana de la consola de comandos debería verse así y una pestaña de SillyTavern debería estar abierta en su navegador:

    ![image](/static/windows-4.png)

9. ¡Conéctese a cualquiera de las [API compatibles](/Usage/API_Connections/index.md) y comience a chatear!
