---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# Guia de Migração 1.12.0

SillyTavern 1.12.0 (codinome atualização "Neo Server") inclui várias mudanças críticas que podem afetar a maneira como você usa o SillyTavern.

Este guia irá prepará-lo para a atualização e fornecer algumas orientações adicionais.

## Atualização de armazenamento de dados

1.12.0 muda a maneira como o SillyTavern lida com os dados do usuário.

Anteriormente, todos os dados persistentes eram armazenados junto com a parte frontend no diretório `/public`, o que criava confusão e pontos potenciais de falha, além de tornar a conteinerização e a instalação do aplicativo em todo o sistema bastante desafiadora.

### O que mudou?

Todas as informações persistentes de `/public` como configurações e chats (lista completa abaixo) foram movidas para um diretório separado com o caminho configurável, tornando-o portátil e independente do próprio servidor web. Quando necessário para fins de compatibilidade, por exemplo, para hospedar extensões, character cards de tamanho completo, uploads de imagens de usuário, etc., um redirecionamento inteligente foi configurado para hospedar automaticamente arquivos de usuário do diretório de dados.

### Definindo uma raiz de dados

Você pode fornecer um caminho absoluto ou relativo (ao diretório do repositório ST) para a raiz de dados por meio de `config.yaml` ou iniciando o servidor com o argumento de console `--dataRoot`.

> Exemplo YAML

```yaml
# -- CONFIGURAÇÃO DE DADOS --
# Diretório raiz para armazenamento de dados do usuário
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> Exemplo de Console

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OU
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

O caminho padrão da raiz de dados é `./data`, o que significa o diretório `data` no repositório do SillyTavern.

!!!info Nota
O caminho da raiz de dados deve ser um caminho **absoluto completo** ou um caminho **relativo completo**. Você _não pode_ usar atalhos de caminho como `~` ou `%APP_DATA%`, pois estes são resolvidos por um shell, não pelo sistema operacional.
!!!

### Migração

#### **IMPORTANTE!** Antes de começarmos

1. **Apenas se você quiser mover dataRoot do local padrão. Caso contrário, pule esta parte.** Defina a raiz de dados _antes_ de executar o servidor pela primeira vez após baixar uma atualização. Execute `npm install` para que o `config.yaml` seja populado com um novo valor, ou passe um argumento de console.
2. Todos os dados serão migrados para uma conta `default-user`. Veja mais sobre [Usuários](#users) abaixo.

#### Instalações sem container (bare metal)

Você não precisa fazer nada! Uma migração automática deve lidar com tudo para você quando você iniciar o servidor ST e ele detectar o formato de armazenamento antigo (verificando a existência do diretório `/public/characters`).

Ao mover quaisquer arquivos, um backup automático será criado no diretório `/backups/_migration/YYYY-MM-DD` (resolvido para a data atual), mas é sempre uma boa prática fazer um backup manual completo antes de executar a migração.

#### Instalações conteinerizadas (Docker)

Migrar os dados em volumes Docker é um pouco mais complicado, mas bastante direto. Embora o `docker-compose.yml` fornecido com o repositório tenha sido atualizado para refletir as mudanças, você pode precisar ajustar seus workflows/deployments personalizados.

**Passo 1.** Crie um novo volume e monte-o no caminho "/home/node/app/data" dentro do container. Não remova o volume `config`.

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**Passo 2.** Mova tudo, exceto o arquivo `config.yaml`, do volume `config` para o subdiretório `default-user` do volume `data`.

**Passo 3.** Reconstrua o container e inicie-o.

!!!info Nota
Links simbólicos entre o diretório `/public` e o volume `config` não são mais necessários e não estão incluídos no container Docker!
!!!

#### O que migrar?

Os seguintes arquivos e diretórios estão sujeitos à migração de dados. Assumindo a configuração padrão, os caminhos antes e depois são fornecidos na tabela abaixo.

| Antes                                  | Depois                               |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## Usuários

1.12.0 adiciona uma capacidade (completamente opcional) de criar uma configuração multi-usuário no mesmo servidor, permitindo que vários usuários usem suas próprias instâncias do SillyTavern totalmente isoladas, mesmo ao mesmo tempo. Contas de usuário também podem ser protegidas por senha para uma camada adicional de privacidade.

Por favor, consulte a documentação de [Usuários](/Administration/multi-user.md) para mais informações.
