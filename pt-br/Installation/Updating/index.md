---
label: Updating
icon: repo-pull
order: -1
expanded: false
route: /installation/updating/
---

# Como Atualizar o SillyTavern

Encontre seu sistema operacional abaixo e siga as instruções para atualizar o ST.

!!! Para instruções de instalação, consulte a página de [Instalação](/Installation/index.md).

Este guia assume que você já instalou e executou o SillyTavern pelo menos uma vez.
!!!

----

## Linux/Termux ou MacOS

Você definitivamente instalou via git, então apenas faça 'git pull' dentro do diretório SillyTavern.

- `cd SillyTavern` para entrar na pasta correta.
- `git pull` para obter a atualização.
- `./start.sh` ou `bash start.sh` para iniciar o ST.

----

## Windows

>Primeiro tente usar o `UpdateAndStart.bat` que está localizado na pasta base de instalação do seu SillyTavern.

Se isso falhar, volte aqui e continue lendo.

### Método 1 - GIT

Sempre recomendamos que os usuários instalem usando 'git'. Aqui está o porquê:

Quando você instalou via `git clone`, tudo que você precisa fazer para atualizar é digitar `git pull` [em uma linha de comando na pasta ST](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder).
Alternativamente, se o prompt de comando der problemas (e você tiver o GitHub Desktop instalado), você pode usar o menu `Repository` e selecionar `Pull`.

As atualizações são aplicadas automaticamente e com segurança.

#### "Ajuda! Eu originalmente instalei via Zip e agora quero converter para instalação Git"

Você escolheu um caminho sábio.

Como sua instalação foi feita via Zip, você precisará fazer uma nova instalação usando git.

Felizmente temos [instruções](/Installation/Windows.md) sobre como fazer isso.

Uma vez que você usou git para instalar um NOVO SillyTavern em uma pasta DIFERENTE, volte a esta página e prossiga para o **Passo 4** das instruções de 'Atualização via Zip' abaixo.

### Método 2 - ZIP

Se você insiste em instalar via zip, aqui está o processo tedioso para fazer a atualização:

1. Baixe o novo zip da release.
2. Descompacte-o em uma pasta FORA da sua instalação atual do ST.
3. Faça o procedimento usual de configuração para seu SO para instalar os requisitos do NodeJS.

4. Copie os seguintes arquivos/pastas conforme necessário(*) da sua antiga instalação do ST:

    (*) 'Conforme necessário' = "Se você fez qualquer conteúdo personalizado relacionado a essas pastas".

    #### Atualizando >=1.12.0

    Copie o diretório `/data` e o arquivo `config.yaml` de uma instalação para outra. Se você tiver extensões em todo o servidor (instaladas para "Todos os usuários") que deseja preservar, também copie o diretório `/public/scripts/extensions/third-party`.

    #### Atualizando de <1.12.0 para >1.12.0

    1.12.0 inclui um procedimento de migração automatizado. Os passos abaixo são necessários *apenas* se a migração foi interrompida ou deu erro.

5. Execute a instalação do servidor atualizado pelo menos uma vez para criar o diretório `/data/default-user`.
6. Transfira os arquivos do antigo `/public` para o novo `/data/default-user` conforme necessário.

    Nenhuma das pastas é obrigatória, então copie apenas o que você precisa.

    **NOTA: NÃO COPIE A PASTA /PUBLIC/ INTEIRA**

    Fazer isso pode quebrar a nova instalação e impedir que novos recursos estejam presentes.

    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- este está na pasta base, não em /public/
    ```

7. Uma vez que essas pastas/arquivos estejam copiados, cole-os na pasta /data/default-user (com secrets.json indo para a raiz da pasta) da nova instalação.
8. Inicie o SillyTavern novamente com o método apropriado para seu SO, e reze para ter acertado.
9. Se tudo aparecer, você pode deletar com segurança a pasta antiga do ST.

### Problemas Comuns de Atualização

#### "There are unresolved conflicts in the working directory."

Isso significa que você modificou arquivos padrão que foram alterados no repositório remoto (como presets de configuração).

Para corrigir isso, execute isso no terminal. Use com cautela, pois pode ser destrutivo. Certifique-se de ter um backup se necessário.

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### Mudanças de arquivo impedem git pull

- Se você alterar arquivos de sistema do SillyTavern, `git pull` pode não funcionar.
- Às vezes uma atualização pode exigir que alteremos um arquivo importante, o que pode causar o mesmo problema.
- Geralmente são arquivos de preset padrão ou `package-lock.json`.
- Neste caso você pode tentar mover o arquivo para uma pasta diferente (ou deletar o arquivo) e então fazer `git pull`.
- Outra solução é usar `git pull --rebase --autostash`

#### Error: Cannot find module "***" when starting the server

- Isso significa que o SillyTavern adicionou um novo requisito de pacote npm.
- Execute `npm install` no diretório SillyTavern para corrigir isso. Os scripts Start.bat e start.sh fornecidos farão isso automaticamente.
- Não ajudou? Remova a pasta node_modules

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```

## Docker

1. Abra uma janela de terminal e navegue até seu diretório docker `cd SillyTavern/docker`
2. Delete seu container com `docker compose down`
3. Delete a imagem docker do SillyTavern do cache `docker rmi ghcr.io/sillytavern/sillytavern:latest` (Substitua `sillytavern:latest` por `sillytavern:staging` se você estiver visando a branch staging.)
4. Reconstrua o container com `sudo docker compose up -d`

Se tudo correr bem, o docker deve começar a baixar novamente a imagem, e você estará funcionando em breve. Se você enfrentar algum problema, consulte a próxima seção deste guia.

### Problemas Comuns de Atualização
#### Eu uso Docker e todos os meus dados desapareceram após a atualização!

Você deve seguir o [Guia de Migração para containers Docker](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)
 para atualizar os mapeamentos de volume para o novo modelo de dados introduzido em 1.12.0

#### Permission denied when running docker commands

Este é um problema do Linux, e implica que suas permissões não estão configuradas corretamente. Existem duas maneiras de contornar isso:

1. **O método fácil**: Se você tem acesso sudo em seu usuário, simplesmente prefixe comandos com `sudo` (por exemplo: `sudo docker compose down`)
2. **O método adequado**: Corrija suas permissões. Isso varia dependendo da versão do Linux que você usa. Há muitos guias online para ajudá-lo a corrigir este problema.
