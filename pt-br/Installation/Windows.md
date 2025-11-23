---
order: 10
label: Windows
route: /installation/windows/
---
# Instalação no Windows

!!!warning
NÃO INSTALE EM NENHUMA PASTA CONTROLADA PELO WINDOWS (Program Files, System32, etc).

NÃO EXECUTE START.BAT COM PERMISSÕES DE ADMINISTRADOR

A INSTALAÇÃO NO WINDOWS 7 É IMPOSSÍVEL, POIS ELE NÃO PODE EXECUTAR NODEJS 18.16
!!!

## Instalando via Git

1. Instale o [NodeJS](https://nodejs.org/en) (versão LTS mais recente é recomendada)
2. Instale o [Git for Windows](https://gitforwindows.org/)
3. Abra o Windows Explorer (`Win+E`)
4. Navegue ou crie uma pasta que não seja controlada ou monitorada pelo Windows. (ex: C:\MinhasPastas\)
5. Abra um Prompt de Comando dentro dessa pasta clicando na 'Barra de Endereços' no topo, digitando `cmd` e pressionando Enter.
6. Quando a caixa preta (Prompt de Comando) aparecer, digite UM dos seguintes comandos e pressione Enter:

   - para a Branch Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - para a Branch Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. Quando tudo estiver clonado, clique duas vezes em `Start.bat` para que o NodeJS instale seus requisitos.
8. O servidor então iniciará e o SillyTavern abrirá no seu navegador.

## Instalando via SillyTavern Launcher

1.  No seu teclado: pressione **`WINDOWS + R`** para abrir a caixa de diálogo Executar. Em seguida, execute o seguinte comando para instalar o git:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. No seu teclado: pressione **`WINDOWS + E`** para abrir o Explorador de Arquivos, depois navegue até a pasta onde deseja instalar o launcher. Uma vez na pasta desejada, digite `cmd` na barra de endereços e pressione enter. Em seguida, execute o seguinte comando:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## Instalando via GitHub Desktop
(Isso permite o uso do git **apenas** no GitHub Desktop, se você quiser usar `git` na linha de comando também, você também precisa instalar o [Git for Windows](https://gitforwindows.org/))

1. Instale o [NodeJS](https://nodejs.org/en) (versão LTS mais recente é recomendada)
2. Instale o [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. Após instalar o GitHub Desktop, clique em `Clone a repository from the internet....` (Nota: Você **NÃO precisa** criar uma conta GitHub para este passo)

    ![image](/static/windows-1.png)

4. No menu, clique na aba URL, insira esta URL `https://github.com/SillyTavern/SillyTavern` e clique em Clone. Você pode alterar o caminho Local para mudar onde o SillyTavern será baixado.

    ![image](/static/windows-2.png)

5. Para abrir o SillyTavern, use o Windows Explorer para navegar até a pasta onde você clonou o repositório. Por padrão, o repositório será clonado aqui: `C:\Users\[Seu Nome de Usuário do Windows]\Documents\GitHub\SillyTavern`

6. Clique duas vezes no arquivo `start.bat`. (Nota: a parte `.bat` do nome do arquivo pode estar oculta pelo seu sistema operacional, neste caso, parecerá um arquivo chamado "`Start`". É nele que você clica duas vezes para executar o SillyTavern)

    ![image](/static/windows-3.png)

7. Após clicar duas vezes, uma grande janela de console de comando preta deve abrir e o SillyTavern começará a instalar o que precisa para operar.

8. Após o processo de instalação, se tudo estiver funcionando, a janela do console de comando deve parecer assim e uma aba do SillyTavern deve estar aberta no seu navegador:

    ![image](/static/windows-4.png)

9. Conecte-se a qualquer uma das [APIs suportadas](/Usage/API_Connections/index.md) e comece a conversar!
