---
order: 50
icon: package
expanded: true
route: /installation/
---

# Instalação

Siga o guia de instalação para sua plataforma:

* [Windows](/Installation/Windows.md)
* [Linux e Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Branches

SillyTavern está sendo desenvolvido usando um sistema de duas branches para garantir uma experiência tranquila para todos os usuários.

* `release` -🌟 **Recomendado para a maioria dos usuários.** Esta é a branch mais estável e recomendada, atualizada apenas quando lançamentos importantes são enviados. É adequada para a maioria dos usuários. Normalmente atualizada uma vez por mês.
* `staging` - ⚠️ **Não recomendado para uso casual.** Esta branch possui os recursos mais recentes, mas tenha cuidado, pois pode quebrar a qualquer momento. Apenas para usuários avançados e entusiastas. Atualizada várias vezes ao dia.

## Modo Global / Standalone

Existem dois modos de execução do SillyTavern que diferem em como lidam com os caminhos de configuração e dados.

* **Modo Standalone** (padrão) - usa o arquivo `config.yaml` e o diretório `data` no diretório do servidor. Todos os dados serão restritos ao caminho de instalação. Este é o modo recomendado para a maioria dos usuários.
* **Modo Global** - usa os caminhos de todo o sistema para configuração e dados. Isso é útil para instalar o SillyTavern como um pacote ou quando você deseja compartilhar a mesma configuração e dados em várias instalações.

!!!info
Instalações feitas usando o [pacote npm oficial](https://www.npmjs.com/package/sillytavern) (por exemplo, `npx sillytavern@latest`) serão executadas no modo global por padrão.
!!!

### Caminhos de dados

Os caminhos do **modo Standalone** são relativos ao diretório de instalação do SillyTavern:

* **Caminho de configuração**: `./config.yaml`
* **Raiz de dados**: `./data/`

Os caminhos do **modo Global** dependem do sistema operacional:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (ou `$XDG_DATA_HOME/SillyTavern/config.yaml`) e `~/.local/share/SillyTavern/data/` (ou `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` e `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` e `~/Library/Application Support/SillyTavern/data/`

### Como executar no modo global

!!!warning
`dataRoot` e `configPath` não podem ser substituídos com [argumentos CLI](../Administration/config-yaml.md#command-line-arguments) ou [config.yaml](../Administration/config-yaml.md) ao executar no modo global.
!!!

1. Passe o argumento `--global` para o comando de inicialização do servidor (por exemplo, `node server.js --global`).
2. Passe o argumento `--global` para o script de inicialização shell (por exemplo, `Start.bat --global` ou `./start.sh --global`).
3. Use o script `start:global` no arquivo `package.json` (por exemplo, `npm run start:global`).
