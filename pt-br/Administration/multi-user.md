---
title: Multi-user mode
icon: people
order: -10
route: /administration/multi-user/
---

O modo multiusuário permite que várias pessoas usem um servidor SillyTavern. Cada usuário tem suas próprias configurações, extensões e dados. As contas de usuário também podem ser protegidas por senha.

!!!warning
As senhas de usuário fornecem privacidade básica entre usuários de uma configuração multiusuário. Elas não são um recurso de segurança e não devem ser consideradas como tal. Todos os dados do usuário (incluindo histórico de chat, chaves de API e outras informações sensíveis) são armazenados em texto simples no servidor. Podem ser visualizados e modificados por qualquer pessoa com acesso ao sistema de arquivos do servidor. **Não use o SillyTavern em um servidor público ou com usuários não confiáveis.**
!!!

## Configuração

Para habilitar e usar o modo multiusuário, edite o arquivo `config.yaml`:

```yaml
# Enable multi-user mode
enableUserAccounts: true
# Enable discreet login mode: hides user list on the login screen
enableDiscreetLogin: true
```

1. Quando a configuração de conta de usuário está desabilitada, uma conta de administrador de fallback `default-user` é utilizada para armazenar os dados do usuário.
2. Quando a configuração de login discreto está desabilitada, uma lista de usuários ativos é exibida na tela de login. Se habilitada, um usuário deve inserir seu identificador manualmente.

!!!info
Você não pode _deletar_ a conta `default-user` da lista de usuários porque ela é usada para servir os dados do usuário caso `enableUserAccounts` esteja definido como `false`. Mas você pode _desabilitá-la_ para ocultá-la da lista e desautorizar logins.
!!!

## Identificadores de usuário

Um identificador (handle) é o identificador único de um usuário. Ele pode consistir apenas de letras minúsculas, números e traços.

Um caminho para o diretório de dados do usuário assume o uso do seguinte padrão: `%DATA_ROOT%/%USER_HANDLE%`.

Exemplos de identificadores de usuário válidos:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## Funções

- **Admin** - pode gerenciar (criar, deletar, modificar) outros usuários. Pode instalar extensões para todos os usuários.
- **User** - não pode gerenciar outros usuários. Pode instalar extensões apenas para si mesmo.

Exceto por ter acesso ao painel de administração, ambas as funções de usuário são funcionalmente idênticas e podem usar toda a gama de recursos do SillyTavern sem quaisquer restrições. Uma implementação de permissões de usuário está por vir (TBD).

Todas as contas de usuário são criadas como usuários regulares primeiro, e então podem ser promovidas a administradores se necessário.

### Tela de login

Lá você pode selecionar uma conta de usuário para usar. Possui dois estilos, dependendo do valor de configuração `enableDiscreetLogin`.

A tela de login é ignorada e não exibida quando você tem apenas um usuário ativo e ele não está protegido por senha.

### Perfil do usuário

Você pode acessar um menu de autogerenciamento de conta usando o botão "Account" no painel "User settings" na barra de menu superior.

1. Display name - usado na tela de login, pode ser alterado. Não se correlaciona com personas e não é visível para as APIs de IA - você ainda pode usar quantas personas quiser.
2. Profile picture - usado na tela de login. Você pode usar uma imagem personalizada, a imagem padrão da persona (se definida) ou a última persona usada caso contrário.
3. Password - um ícone de cadeado reflete o status de proteção da conta (cadeado aberto = sem senha). Uma senha pode ser definida, alterada ou removida usando o botão "Change Password".
4. Settings Snapshots - acesse e revise os backups do seu arquivo `settings.json`, com a capacidade de criar ou restaurar snapshots.
5. Download Backup - baixe um arquivo da sua pasta de dados do usuário.
6. Reset Settings - redefinir as configurações padrão de fábrica, deixando outros dados (personagem, chats) intactos.

## Recuperação de senha

1. Uma senha pode ser recuperada da tela de login. Você precisa de acesso ao console do servidor para obter um código de recuperação único (consistindo de 4 dígitos).
2. Alternativamente, você pode usar um script utilitário no servidor SillyTavern para redefinir uma senha fornecendo o identificador do usuário.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## Estrutura de conteúdo

Para adicionar conteúdo personalizado para usuários, você pode usar o recurso de estrutura de conteúdo. Este recurso permite que você defina um conjunto de arquivos que serão copiados para o diretório de dados de cada usuário quando o servidor for iniciado.

Você deve criar um arquivo `index.json` no diretório `/default/scaffold` para que este recurso funcione. A sintaxe é a mesma do conteúdo padrão. Todos os caminhos de arquivo devem ser relativos ao diretório `/default/scaffold`, e você pode organizar arquivos usando subdiretórios.

Os arquivos estruturados são copiados antes dos arquivos padrão, o que significa que eles substituirão quaisquer arquivos padrão (presets/configurações/etc.) que tenham o mesmo nome de arquivo.

!!!tip
Cada diretório de dados do usuário tem um arquivo `content.log` que lista todos os arquivos copiados dos diretórios scaffold e default. Remova este arquivo para forçar o servidor a sincronizar o conteúdo novamente na próxima reinicialização.
!!!

### Tipos de conteúdo reconhecidos

| Type                          | Value                |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Character card                | `'character'`        |
| Character sprites             | `'sprites'`          |
| Background image              | `'background'`       |
| World Info file               | `'world'`            |
| Persona avatar                | `'avatar'`           |
| UI theme                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic preset       | `'kobold_preset'`    |
| Chat Completion preset        | `'openai_preset'`    |
| NovelAI preset                | `'novel_preset'`     |
| Text Completion preset        | `'textgen_preset'`   |
| Instruct Mode template        | `'instruct'`         |
| Context Formatting template   | `'context'`          |
| MovingUI preset               | `'moving_ui'`        |
| Quick Replies set             | `'quick_replies'`    |
| System Prompt template        | `'sysprompt'`        |
| Reasoning Formatting template | `'reasoning'`        |

### Exemplo (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
