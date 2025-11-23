---
label: MacOS & Linux
order: 5
route: /installation/linuxmacos/
---

# Instalação em Linux/MacOS

## Instalação Manual via Git

Para MacOS / Linux, todos esses passos serão feitos em um Terminal.

1. Instale git e nodeJS (o método para fazer isso varia dependendo do seu sistema operacional)
2. Clone o repositório

   - para a Branch Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - para a Branch Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern` para navegar até a pasta de instalação.
4. Execute o script `start.sh` com um destes comandos:

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### Para usuários Linux
1. Abra seu terminal favorito e instale o git
2. Baixe o SillyTavern Launcher com: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. Navegue até o SillyTavern-Launcher com: `cd SillyTavern-Launcher`
4. Inicie o launcher de instalação com: `chmod +x install.sh && ./install.sh` e escolha o que você quer instalar
5. Após a instalação, inicie o launcher com: `chmod +x launcher.sh && ./launcher.sh`

### Para usuários Mac
1. Abra um terminal e instale o brew com: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Então instale o git com: `brew install git`
3. Baixe o SillyTavern Launcher com: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. Navegue até o SillyTavern-Launcher com: `cd SillyTavern-Launcher`
5. Inicie o launcher de instalação com: `chmod +x install.sh && ./install.sh` e escolha o que você quer instalar
6. Após a instalação, inicie o launcher com: `chmod +x launcher.sh && ./launcher.sh`
