---
# icon: container
label: Docker
route: /installation/docker/
---

# Instalação via Docker

!!!
Estas instruções assumem que você instalou o Docker, consegue acessar sua linha de comando para a instalação de containers e está familiarizado com sua operação geral.
!!!

## Usando o GitHub Container Registry

Usar uma imagem pré-construída é a maneira mais rápida e fácil de começar com o SillyTavern no Docker. Você pode obter a imagem mais recente do GitHub Container Registry.

### Docker Compose (recomendado)

Baixe o arquivo `docker-compose.yml` do [Repositório GitHub](https://github.com/SillyTavern/SillyTavern/blob/release/docker/docker-compose.yml) e execute o seguinte comando no diretório onde o arquivo está localizado. Isso obterá a imagem de lançamento mais recente do GitHub Container Registry e iniciará o container, criando automaticamente os volumes necessários.

```sh
docker compose up
```

Você pode editar o arquivo e aplicar personalização adicional para atender suas necessidades:

- A porta padrão é 8000. Você pode alterá-la modificando a seção `ports`.
- Altere a tag `image` para `staging` se quiser usar a branch de desenvolvimento em vez da release estável.
- Se quiser ajustar a configuração do servidor usando variáveis de ambiente, consulte a página [Variáveis de Ambiente](/Administration/config-yaml.md#environment-variables).

### Docker CLI (avançado)

Você precisará de dois mapeamentos de diretório obrigatórios e um mapeamento de porta para permitir que o SillyTavern funcione. No comando, substitua suas seleções nos seguintes lugares:

#### Variáveis do Container

##### Mapeamentos de Volume

- `CONFIG_PATH` - O diretório onde os arquivos de configuração do SillyTavern serão armazenados em sua máquina host
- `DATA_PATH` - O diretório onde os dados do usuário do SillyTavern (incluindo personagens) serão armazenados em sua máquina host
- `PLUGINS_PATH` - (opcional) O diretório onde os plugins do servidor SillyTavern serão armazenados em sua máquina host
- `EXTENSIONS_PATH` - (opcional) O diretório onde as extensões globais da UI serão armazenadas em sua máquina host

##### Mapeamentos de Porta

- `PUBLIC_PORT` - A porta para expor o tráfego. Isso é obrigatório, pois você estará acessando a instância de fora de seu container de máquina virtual. NÃO exponha isso à internet sem implementar um serviço separado para segurança.

##### Configurações Adicionais

- `SILLYTAVERN_VERSION` - Na [página do GitHub Packages](https://github.com/SillyTavern/SillyTavern/pkgs/container/sillytavern) você verá a lista de versões de imagem marcadas. A tag de imagem "latest" manterá você atualizado com a release atual. Você também pode utilizar "staging" que aponta para a imagem noturna da respectiva branch.

#### Executando o container

1. Abra sua Linha de Comando
2. Execute o seguinte comando em uma pasta onde você deseja armazenar os arquivos de configuração e dados:

```bash
SILLYTAVERN_VERSION="latest"
PUBLIC_PORT="8000"
CONFIG_PATH="./config"
DATA_PATH="./data"
PLUGINS_PATH="./plugins"
EXTENSIONS_PATH="./extensions"

docker run \
  --name="sillytavern" \
  -p "$PUBLIC_PORT:8000/tcp" \
  -v "$CONFIG_PATH:/home/node/app/config:rw" \
  -v "$DATA_PATH:/home/node/app/data:rw" \
  -v "$EXTENSIONS_PATH:/home/node/app/public/scripts/extensions/third-party:rw" \
  -v "$PLUGINS_PATH:/home/node/app/plugins:rw" \
  ghcr.io/sillytavern/sillytavern:"$SILLYTAVERN_VERSION"
```

!!!tip
Por padrão, o container será executado em primeiro plano. Se você quiser executá-lo em segundo plano, adicione a flag `-d` ao comando `docker run`.
!!!

## Construindo a Imagem Docker

!!!info
A seção a seguir assume que você instalou o SillyTavern em uma pasta não-root (não-admin). Se você instalou o SillyTavern em uma pasta root, pode ter que executar alguns desses comandos com direitos de administrador [`sudo`, `doas`, Command Prompt (Administrator)].
!!!

Se você quiser construir a imagem Docker você mesmo, pode fazê-lo seguindo estes passos. Isso é útil se você quiser personalizar a imagem ou usá-la para fins de desenvolvimento.

### Linux

1. Instale o Docker seguindo o guia de instalação do Docker [aqui](https://docs.docker.com/engine/install/).
   !!!danger
   **Não** instale o Docker Desktop.
   !!!
2. Siga os passos em **Manage Docker as a non-root user** no [Guia de Pós-Instalação](https://docs.docker.com/engine/install/linux-postinstall/) do Docker.
3. Instale o [Git](https://git-scm.com/download/linux) usando seu gerenciador de pacotes.

    - Debian (Ubuntu/Pop! OS/etc.)

        ```sh
        sudo apt install git
        ```

    - Arch Linux (Manjaro/EndeavourOS/etc.)

        ```sh
        sudo pacman -S git
        ```

    - Fedora, Red Hat Enterprise Linux (RHEL), etc.
        ```sh
        sudo dnf install git
        ```

4. Clone o repositório SillyTavern.

    - Release (Branch Estável)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    - Staging (Branch de Desenvolvimento)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

5. Execute `docker compose` executando o seguinte comando dentro da pasta Docker.

    ```sh
    docker compose up -d
    ```

6. Abra um novo navegador e vá para [http://localhost:8000](http://localhost:8000). Você deve ver o SillyTavern carregar em alguns momentos.

### Windows

!!!warning Sobre Docker no Windows
Usar Docker no Windows é **_realmente_** complicado. Você não só precisa ativar o _Windows Subsystem for Linux_ dentro de _Turn Windows features on or off_, mas também configurar seu sistema para Virtualização (Intel VT-d/AMD SVM) que difere de fabricante de PC para fabricante de PC (ou fabricante de placa-mãe). Às vezes, esta opção não está presente em alguns sistemas.

É altamente sugerido que você instale o SillyTavern seguindo nosso guia de [Windows](/Installation/Windows.md). Esta seção é uma ideia _aproximada_ de como isso pode ser feito no Windows.
!!!

1.  Instale o Docker Desktop seguindo o guia de instalação do Docker [aqui](https://docs.docker.com/desktop/setup/install/windows-install/).
2.  Instale o [Git for Windows](https://git-scm.com/download/win).
3.  Clone o repositório SillyTavern.

    -   Release (Branch Estável)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Branch de Desenvolvimento)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Execute `docker compose` executando o seguinte comando dentro da pasta Docker.

    ```sh
    docker compose up -d
    ```

5.  Abra um novo navegador e vá para [http://localhost:8000](http://localhost:8000). Você deve ver o SillyTavern carregar em alguns momentos.

### macOS

!!!
Embora o macOS seja similar ao Linux, ele não possui o Docker Engine. Você terá que instalar o Docker Desktop de forma similar ao Windows.
Você também precisará instalar o [Homebrew](https://brew.sh/) para poder instalar o Git em seu Mac. Esta seção é uma ideia _aproximada_ de como isso pode ser feito no macOS.
!!!

1.  Instale o Docker Desktop seguindo o guia de instalação do Docker [aqui](https://docs.docker.com/desktop/setup/install/mac-install/).
2.  Instale o `git` usando o Homebrew.

    ```sh
    brew install git
    ```

3.  Clone o repositório SillyTavern.

    -   Release (Branch Estável)

        ```sh
        git clone https://github.com/SillyTavern/SillyTavern && cd SillyTavern/docker
        ```

    -   Staging (Branch de Desenvolvimento)
        ```sh
        git clone https://github.com/SillyTavern/SillyTavern -b staging && cd SillyTavern/docker
        ```

4.  Execute `docker compose` executando o seguinte comando dentro da pasta Docker.

    ```sh
    docker compose up -d
    ```

5.  Abra um novo navegador e vá para [http://localhost:8000](http://localhost:8000). Você deve ver o SillyTavern carregar em alguns momentos.

## Configurando o SillyTavern

O arquivo de configuração do SillyTavern (config.yaml) estará localizado dentro da pasta `config`. Configurar o arquivo de configuração não deve ser diferente de configurá-lo sem Docker, no entanto você precisará executar o `nano` ou um editor de código com direitos de administrador para salvar suas alterações.

!!!warning
Não esqueça de reiniciar o container Docker para o SillyTavern para aplicar suas alterações! Certifique-se de executar este comando dentro da pasta `docker`.

```sh
docker compose restart sillytavern
```

!!!

## Localizando Dados do Usuário

A pasta de dados do SillyTavern estará dentro da pasta `data`. Fazer backup de seus arquivos deve ser fácil de fazer, no entanto, restaurar ou adicionar conteúdo a ela pode exigir que você faça isso com direitos de administrador.

## Executando Plugins do Servidor

Executar plugins como [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS) ou [SillyTavern-Fandom-Scraper](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) dentro do Docker não é diferente de executá-lo em seu sistema sem Docker, no entanto precisaremos fazer uma pequena modificação no script Docker Compose para fazer isso.

!!! Note
Se você já vê uma pasta _plugins_ dentro da pasta `docker`, você pode pular os Passos 1-2.
!!!

1. Usando `nano` ou um editor de código, abra _docker-compose.yml_ e adicione a seguinte linha abaixo de `volumes`.

    ```sh
        volumes:
            - "./config:/home/node/app/config"
            - "./data:/home/node/app/data"
            - "./plugins:/home/node/app/plugins"
    ```

2. Crie uma nova pasta dentro da pasta `docker` chamada _plugins_.
3. Siga as instruções do seu plugin sobre como instalar o plugin.
4. Usando `nano` ou um editor de código com direitos de administrador, abra _config.yaml_ (dentro da pasta `config`) e habilite `enableServerPlugins`

    ```sh
    enableServerPlugins: true
    ```

5. Reinicie o container Docker.

    ```sh
    docker compose restart sillytavern
    ```

## Problemas comuns com Docker

### Problemas de Permissão SELinux com Volumes Montados

Distribuições Linux com SELinux habilitado (como RHEL, CentOS, Fedora, etc.) podem impedir que containers Docker acessem volumes montados devido a políticas de segurança. Isso pode resultar em erros de permissão negada quando o container tenta ler ou escrever nos diretórios montados.

Dois sufixos `:z` ou `:Z` podem ser adicionados ao mount de volume. Esses sufixos informam ao Docker para re-rotular objetos de arquivo nos volumes compartilhados.

- A opção `z` é usada quando o conteúdo do volume será compartilhado entre containers.
- A opção `Z` é usada quando o conteúdo do volume deve ser usado apenas pelo container atual.

Exemplo:

```yaml
# docker-compose.yml
volumes:
  ## Volume compartilhado
  - ./config:/home/node/app/config:z
  ## Volume privado
  - ./data:/home/node/app/data:Z
```

### Forbidden by Whitelist

!!!
IPs de gateway do Docker devem ser colocados na whitelist automaticamente se o valor de configuração [whitelistDockerHosts](/Administration/config-yaml.md#ip-whitelisting) estiver definido como `true`.

Se você ainda não conseguir acessar o SillyTavern, siga as instruções abaixo para atualizar a whitelist manualmente.
!!!

1. Execute o seguinte comando Docker para obter o IP do seu container Docker do SillyTavern.

    ```sh
    docker network inspect docker_default
    ```

    Você deve receber uma saída similar à seguinte abaixo.

    ```json
    [
        {
            "Name": "docker_default",
            "IPAM": {
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
                    }
                ]
            }
        }
    ]
    ```

    Copie o IP que você vê em _Gateway_ pois isso será importante.

2. Executando um editor de texto de sua escolha com direitos de administrador, vá para `config` e abra `config.yaml`.

    Dentro do seu editor, vá até a seção `whitelist`. Você deve ver algo similar ao seguinte abaixo.

    ```yaml
    whitelist:
        - 127.0.0.1
    ```

    Adicione uma nova linha abaixo de _127.0.0.1_ e coloque o IP que você copiou do Docker. Deve parecer algo similar ao seguinte depois.

    ```yaml
    whitelist:
        - 127.0.0.1
        - 172.18.0.1
    ```

    Salve o arquivo e saia do editor de texto.

    !!!info
    Note que se você configurou a rede Docker como bridge, você também pode adicionar endereços IP externos à whitelist normalmente.
    !!!

3. Reinicie o Container Docker para aplicar a nova configuração.

    ```sh
    docker compose restart sillytavern
    ```
