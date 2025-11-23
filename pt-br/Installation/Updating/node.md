---
order: -50
route: /installation/updating/node/
---

# Como atualizar o Node.js

É importante manter seu runtime Node.js atualizado por razões de segurança e desempenho. Abaixo estão os passos para atualizar o Node.js dependendo do seu sistema operacional.

Recomendamos usar a versão Long Term Support (LTS) mais recente, que você pode encontrar no [site oficial do Node.js](https://nodejs.org/en/about/previous-releases).

## Como verificar sua versão atual do Node.js

1. Abra seu terminal ou prompt de comando.
2. Digite o seguinte comando e pressione Enter:

```bash
node -v
```

## nvm (Node Version Manager) - Multiplataforma

Se você está usando `nvm`:

1. Abra seu terminal.
2. Digite o seguinte comando:

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

## Windows - Instalação Regular

1. Vá para a [página de download](https://nodejs.org/en/download/) do Node.js.
2. Baixe o Windows Installer para a versão LTS.
3. Execute o instalador e siga as instruções para concluir a instalação.

## Windows - SillyTavern Launcher

Se você instalou usando o SillyTavern Launcher:

1. Abra o SillyTavern Launcher.
2. Navegue até `Toolbox / App Installer / Core Utilities / Install Node.js`.

**OU:**

Faça isso manualmente usando winget no PowerShell:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Abra o aplicativo Termux.
2. Digite os seguintes comandos:

```bash
pkg update
pkg upgrade nodejs-lts
```

Não esqueça de aceitar quaisquer prompts que possam aparecer durante o processo de atualização pressionando `Y` no teclado virtual.

## macOS - Instalação Regular

1. Vá para a [página de download](https://nodejs.org/en/download/) do Node.js.
2. Baixe o macOS Installer para a versão LTS.
3. Execute o arquivo `.pkg` e siga as instruções para concluir a instalação.

## macOS - Homebrew

Se você tem o Homebrew instalado, pode atualizar o Node.js com os seguintes comandos:

```bash
brew update
brew upgrade node
```

## Linux - Gerenciador de Pacotes

O método para atualizar o Node.js no Linux depende da sua distribuição.

Mas como a versão do Node.js nos repositórios oficiais pode não ser a mais recente, recomendamos usar o [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) ou o [repositório NodeSource](https://github.com/nodesource/distributions).

## Docker

Nenhuma ação necessária. A imagem Docker pré-construída que fornecemos é compilada com a versão atualizada do Node.js.
