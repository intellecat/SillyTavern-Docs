---
order: 120
icon: gear
route: /usage/user-settings/
---

# User Settings


:::callout
**[UI Customization](uicustomization.md)**

Altere o tema, aparência e sensação da interface de chat para adequar às suas preferências.
:::



:::callout
**[Visual Novel mode](Visual-Novel.md)**

Converse com personagens com sprites, como em visual novels como Doki Doki Literature Club e outros famosos jogos VN.
:::


## General Settings

Essas são as configurações principais que afetam sua experiência geral no SillyTavern.

### UI Language

A interface do usuário do SillyTavern está disponível em vários idiomas. O seletor de idioma fornece estas opções:
* **Default**: Usa o idioma do seu sistema se disponível
* **English**: Força a interface em inglês independentemente das configurações do sistema
* Outros idiomas disponíveis através do dropdown

Nota: Esta configuração afeta apenas o texto da interface do usuário. Para tradução de conversas com IA, use a extensão [Chat Translation](../../extensions/Translation.md).

### Software Version

Sua versão atual do SillyTavern é exibida no canto superior direito. Esta informação é essencial para:
* Solucionar problemas
* Garantir compatibilidade com extensões
* Determinar se atualizações estão disponíveis

Para atualizar o SillyTavern para a versão mais recente, consulte a documentação de [Atualização](/Installation/Updating).

### Account Management

Controle sua conta de usuário do SillyTavern, faça backup de suas configurações e dados de usuário, e gerencie funções e permissões de usuário no [modo multiusuário](/Administration/multi-user.md).

#### <i class="fa-fw fa-solid fa-user-shield"></i> Account

No diálogo Account, você pode visualizar e editar informações do seu perfil, alterar sua senha e gerenciar configurações da conta.

**Informações do Perfil**

* Nome de exibição (editável via ícone de lápis)
* Avatar do usuário (também pode ser alterado usando [Personas](/Usage/personas.md))
* Identificador da conta
* Função do usuário
* Data de criação da conta
* Status da senha (ícone de cadeado/desbloqueado indica proteção)

**Ações da Conta**

* **Settings Snapshots**: Criar, gerenciar e restaurar backups de suas configurações de usuário
* **Download Backup**: Exportar um backup completo de todos os seus dados de usuário
* **Change Password**: Atualizar suas credenciais de segurança da conta

**Danger Zone**

Operações críticas da conta que devem ser usadas com cautela:
* **Reset Settings**: Restaurar todas as configurações para os padrões de fábrica
* **Reset Everything**: Limpeza completa da conta e redefinição de fábrica

#### <i class="fa-fw fa-solid fa-user-tie"></i> Admin Panel

!!! Aplica-se a: [modo multiusuário](/Administration/multi-user.md)

Recursos de múltiplas contas requerem que `enableUserAccounts` seja definido como true em config.yaml.
!!!

Selecione **Manage Users** para visualizar e gerenciar contas de usuário existentes.

##### User Profile

- Gerenciamento de avatar personalizado (upload/remover)
- Nome de exibição e identificador
- Informações de função e status
- Data de criação da conta
- Status de proteção por senha

##### Account Controls

- <i class="fa-fw fa-solid fa-pencil"></i> Editar nome de exibição
- <i class="fa-fw fa-solid fa-check"></i> Habilitar conta
- <i class="fa-fw fa-solid fa-ban"></i> Desabilitar conta
- <i class="fa-fw fa-solid fa-arrow-up"></i> Promover para admin
- <i class="fa-fw fa-solid fa-arrow-down"></i> Rebaixar para usuário regular

##### Management Actions

- <i class="fa-fw fa-solid fa-download"></i> Baixar backup de dados do usuário
- <i class="fa-fw fa-solid fa-key"></i> Alterar senha do usuário
- <i class="fa-fw fa-solid fa-trash"></i> Excluir conta

##### New User

Selecione **New User** para criar uma nova conta de usuário.

* Display Name* (ex: "John Snow")
* User Handle* (apenas letras minúsculas, números e traços)
* Password (opcional)
* Password Confirmation

Criar um novo usuário gera automaticamente uma subpasta no diretório /data/ usando o identificador do usuário como nome da pasta.

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> Logout

!!! Aplica-se a: [modo multiusuário](/Administration/multi-user.md)
!!!

Sair da sua sessão atual.

### Settings Search

Uma barra de pesquisa conveniente que ajuda você a encontrar rapidamente configurações específicas:
* Digite qualquer palavra-chave para filtrar e destacar configurações em qualquer lugar em User Settings
* Pesquisa através de nomes e descrições de configurações
* Ajuda a navegar configurações complexas de forma mais eficiente

## UI Theme

Altere a aparência da interface de chat para adequar às suas preferências.

Para mais informações sobre as configurações nesta seção de <i class="fa-fw fa-solid fa-user-gear" title="User Settings icon"></i> **User Settings**, veja [UI Customization](uicustomization.md#ui-theme).

## Character Handling

* **Char List Subheader**: Escolha quais informações adicionais exibir sob os nomes dos personagens na lista de [<i class="fa-fw fa-solid fa-address-card" title="Characters icon"></i> Characters](/Usage/Characters/characterdesign.md):
    - Character Version
    - Created by
* **Import Card Tags**: Controla como as tags são tratadas ao importar cartões de personagem:
    - Ask - Mostrar diálogo para cada importação
    - None - Não importar nenhuma tag
    - All - Importar todas as tags
    - Existing - Importar apenas tags que já existem
* **Advanced Character Search**: Quando habilitado, usa correspondência difusa e pesquisa todos os campos de dados de personagem, não apenas nomes.
* **Prefer Char. Prompt**: Se habilitado, usa a substituição de System Prompt do cartão de personagem quando disponível.
* **Prefer Char. Instructions**: Se habilitado, usa a substituição de Post-History Instructions do cartão de personagem quando disponível.
* **Never resize avatars**: Impede recorte/redimensionamento de imagens de personagem importadas. Quando desabilitado, as imagens são redimensionadas para 512x768.
* **Show avatar filenames**: Exibe os nomes de arquivo reais dos avatares de personagem na lista de personagens.
* **Spoiler Free Mode**: Oculta definições de personagem atrás de um botão de spoiler no painel do editor.

## Miscellaneous

* **Reload Chat**: Recarrega e redesenha o chat atual.
* **[Debug Menu](#debug-menu)**: Acessa opções de depuração.
* **Smooth Streaming**: Suaviza a geração por streaming mostrando o texto letra por letra. Inclui controle deslizante de velocidade.
* **Stream Fade-In**: Aplica um efeito de fade-in ao texto por streaming. Pode ser usado com ou sem Smooth Streaming.
* **[Message Sound](uicustomization.md#message-sound)**: Reproduz um som quando a geração de mensagem é concluída.
    - **Background Sound Only**: Reproduz sons apenas quando a aba do navegador está sem foco.
* **Relaxed API URLs**: Reduz requisitos de formatação para URLs de API.
* **Lorebook Import Dialog**: Mostra diálogo de importação para World Info/Lorebook ao importar personagens com lore incorporado.
* **Auto-select Input Text**: Seleciona automaticamente texto em certos campos de entrada ao clicar.
* **Markdown Hotkeys**: Habilita atalhos de teclado para formatação markdown.
* **Restore User Input**: Preserva entrada do usuário não salva quando a página é atualizada.
* **MovingUI**: Permite reposicionar elementos da UI arrastando (apenas PC).
    - <i class="fa-solid fa-recycle" title="Reset icon"></i> Botão **Reset** para restaurar posições padrão
    - Sistema de preset para salvar/carregar layouts de UI

## Chat/Message Handling

### Message Display Settings

Controla como as mensagens são carregadas e exibidas na interface de chat. Essas configurações afetam a experiência geral de chat e desempenho.
* **# Messages to Load**: Número de mensagens do histórico de chat a carregar antes da paginação (0 = Todas)
* **Streaming FPS**: Velocidade de atualização do texto por streaming (5-100 FPS)
* **Example Messages Behavior**:
    - Gradual push-out
    - Always include examples
    - Never include examples

### Input & Response Controls

Configurações que determinam como as mensagens são enviadas e como a IA continua suas respostas.
* **Enter to Send**: Escolha entre Disabled, Automatic (PC) ou Enabled
* **"Send" to Continue**: Usar botão Send para continuar respostas da IA
* **Quick "Continue" button**: Mostrar botão para estender a última mensagem da IA
* **Quick "Impersonate" button**: Mostrar botão para personificação de personagem de mensagem única
* **Swipes**: Mostrar botões de seta para respostas alternativas da IA (PC e mobile)
* **Gestures**: Habilitar gestos de deslizar para geração (Apenas Mobile)

### Auto-Management

Recursos automatizados que ajudam a gerenciar o fluxo e conteúdo do chat.
* **Auto-load Last Chat**: Carregar automaticamente o chat mais recente ao iniciar
* **Auto-scroll Chat**: Rolar automaticamente para mensagens mais recentes
* **Auto-save Message Edits**: Salvar edições de mensagem sem confirmação
* **Confirm message deletion**: Solicitar antes de excluir mensagens
* **Auto-fix Markdown**: Corrigir automaticamente formatação markdown

#### Auto-swipe

Rejeita e regenera automaticamente mensagens da IA com base em critérios configuráveis.
* **Enable Auto-swipe**: Alternância principal para a função de auto-swipe
* **Minimum generated message length**: Aciona um auto-swipe se a mensagem for mais curta que este valor
* **Blacklisted words**: Lista de palavras que podem acionar auto-swipe, separadas por vírgulas
* **Blacklisted word count to swipe**: Número mínimo de palavras da lista negra que devem ser detectadas para acionar um auto-swipe

#### Auto-Continue

Continua automaticamente uma resposta se o modelo parou antes de atingir um certo comprimento.

Isso permite que sua IA escreva uma resposta longa em várias partes, para que você possa ter uma configuração de [comprimento de resposta](/Usage/Common-Settings.md#response-tokens) curta enquanto ainda recebe respostas longas.

Não fará a IA escrever mais do que ela teria escrito de outra forma. Pedir à IA para continuar uma mensagem que ela considera "terminada" geralmente não funciona. Veja [Como fazer a IA escrever mais?](/Usage/faq.md#how-to-make-the-ai-write-more) para outras ideias.

* **Enable Auto-continue**: Alternância principal para continuação automática
* **Allow for Chat Completion APIs**: Habilita funcionalidade de auto-continue para endpoints de API de Chat Completion
* **Target length (tokens)**: O comprimento de mensagem desejado em tokens - acionará continue se a mensagem for mais curta que este valor (0-1024)

### Message Formatting & Display

Controla como as mensagens são formatadas e qual conteúdo é exibido.
* **Forbid External Media**: Bloquear mídia incorporada de domínios externos
* **Show {\{char}}: in responses**: Manter prefixo de nome do personagem nas respostas se gerado
* **Show {\{user}}: in responses**: Manter prefixo de nome do usuário nas respostas se gerado
* **Show tags in responses**: Permitir que (algumas) tags HTML nas respostas sejam exibidas como HTML
* **Relax message trim in Groups**: Permitir que a IA fale por outros personagens em chats em grupo, em vez de parar a geração de resposta
* **Show group chat queue**: Exibir ordem de resposta na lista de personagens para chats em grupo
* **Pin greeting message styles**: Sempre renderizar tags de estilo de saudações, mesmo se a mensagem estiver descarregada devido ao carregamento preguiçoso.

### Prompt Inspection and Debugging

* **Log prompts to console**: Exibir prompts no console do navegador
* **Request token probabilities**: Solicitar probabilidades de token para respostas da IA da API. Quando disponíveis, estas podem ser visualizadas em <i class="fa-solid fa-bars" title="Burger Menu icon"></i> [Token Probabilities](../../Usage/Chatting/index.md#token-probabilities-panel).

### AutoComplete

- Auto-hide details
- Matching style (Starts with/Includes/Fuzzy)
- Visual style (Theme/Dark/Light)
- Opções de seleção por teclado
- Escala de fonte
- Controles de largura

## STscript Settings

Opções de configuração para o [parser STscript](/For_Contributors/st-script.md#parser-flags).

### STRICT_ESCAPING

* Pipes não precisam ser escapados em valores entre aspas.
* Uma barra invertida na frente de um símbolo pode ser escapada para fornecer a barra invertida literal seguida pelo símbolo funcional.

Veja [Strict Escaping](/For_Contributors/st-script.md#strict-escaping) para mais informações.

### REPLACE_GETVAR

Ajuda a evitar substituições duplas quando os valores das variáveis contêm texto que poderia ser interpretado como macros.

Veja [Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros) para mais informações.

## Clean-Up Menu

O menu Clean-Up fornece uma ferramenta de manutenção de dados que ajuda você a identificar e remover arquivos desnecessários da sua instalação do SillyTavern. Este recurso ajuda a manter seu diretório de dados organizado e pode liberar espaço significativo em disco.

!!! warning "Aviso Importante"
A ferramenta Clean-up excluirá arquivos permanentemente. **Esta ação não pode ser desfeita!**

Uploads manuais para os diretórios `/data/user/files/` e `/data/user/images/` serão excluídos se não estiverem associados a mensagens de chat ou entradas do Data Bank.

Se não tiver certeza, faça um backup de seus dados antes de usar o menu Clean-up.
!!!

### Como Usar o Clean-Up

1. Clique no botão **Clean-Up** na seção **Miscellaneous**
2. Clique em **Scan** para analisar sua instalação. Isso pode levar algum tempo dependendo do tamanho do seu diretório de dados
3. Revise as categorias de arquivos encontrados
4. Use **View** para visualizar o conteúdo dos arquivos antes da exclusão
5. Use **Download** para salvar arquivos antes da exclusão
6. Exclua arquivos individuais ou categorias inteiras conforme necessário

### Categorias de Clean-Up

A ferramenta Clean-Up verifica arquivos soltos nas seguintes categorias:

#### Files

* **O que encontra**: Arquivos que não estão associados a mensagens de chat ou entradas do Data Bank
* **Localização**: `/data/<user-handle>/user/files/`
* **Risco**: ⚠️ **EXCLUIRÁ UPLOADS MANUAIS** que não estão referenciados em chats
* **Quando limpar**: Seguro excluir se você não precisar de arquivos não referenciados

#### Images

* **O que encontra**: Imagens que não estão associadas a mensagens de chat
* **Localização**: `/data/<user-handle>/user/images/`
* **Risco**: ⚠️ **EXCLUIRÁ UPLOADS MANUAIS** que não estão referenciados em chats
* **Quando limpar**: Seguro excluir se você não precisar de imagens não referenciadas

#### Chats

* **O que encontra**: Arquivos de chat associados a personagens excluídos
* **Localização**: `data/<user-handle>/chats/`
* **Risco**: ⚠️ **Chats órfãos serão permanentemente perdidos**
* **Quando limpar**: Seguro excluir se você intencionalmente excluiu personagens e não precisa mais de seus históricos de chat

#### Group Chats

* **O que encontra**: Arquivos de chat associados a grupos excluídos
* **Localização**: `data/<user-handle>/group chats/`
* **Risco**: ⚠️ **Chats de grupo órfãos serão permanentemente perdidos**
* **Quando limpar**: Seguro excluir se você intencionalmente excluiu grupos e não precisa mais de seus históricos de chat

#### Avatar Thumbnails

* **O que encontra**: Miniaturas para avatares de personagens ausentes ou excluídos
* **Localização**: `data/<user-handle>/thumbnails/avatar`
* **Risco**: ✅ **Seguro excluir** - miniaturas são regeneradas automaticamente quando necessário
* **Quando limpar**: Sempre seguro limpar, ajuda a liberar espaço

#### Background Thumbnails

* **O que encontra**: Miniaturas para fundos ausentes ou excluídos
* **Localização**: `data/<user-handle>/thumbnails/bg`
* **Risco**: ✅ **Seguro excluir** - miniaturas são regeneradas automaticamente quando necessário
* **Quando limpar**: Sempre seguro limpar, ajuda a liberar espaço

#### Chat Backups

* **O que encontra**: Backups de chat gerados automaticamente
* **Localização**: `data/<user-handle>/backups/chat_*`
* **Risco**: ⚠️ **Arquivos de backup serão permanentemente perdidos**
* **Quando limpar**: Considere manter backups recentes, mas os mais antigos podem ser excluídos com segurança

#### Settings Backups

* **O que encontra**: Backups de configurações gerados automaticamente
* **Localização**: `data/<user-handle>/backups/settings_*`
* **Risco**: ⚠️ **Arquivos de backup de configurações serão permanentemente perdidos**
* **Quando limpar**: Considere manter backups recentes, mas os mais antigos podem ser excluídos com segurança

## Debug menu

!!!warning Essas funções são destinadas apenas a usuários avançados.

Não as use a menos que você entenda completamente suas consequências.
!!!

O Debug Menu fornece funcionalidade para solução de problemas, manutenção e propósitos de desenvolvimento. Essas funções devem ser usadas com cautela, pois podem impactar significativamente sua instalação do SillyTavern.

Como extensões podem adicionar funções de debug, as opções disponíveis variarão dependendo das extensões que você instalou.

### Translation & Locale Functions
* **Get missing translations**: Analisa o locale atual (ou todos os locales se inglês estiver selecionado) para traduções faltantes e exibe resultados no console do navegador
* **Apply locale**: Força uma atualização das configurações de idioma atuais reaplicando o locale selecionado
### Cache & Storage Management
* **Clear WebSearch cache**: Remove todos os resultados de busca armazenados do cache local
* **Purge all vector indices**: Remove completamente todos os vetores armazenados em todas as fontes
* **Reset token cache**: Limpa contagens de tokens armazenadas, forçando re-tokenização completa de todos os chats
* **Delete itemized prompts**: Remove todos os prompts itemizados do armazenamento local
### Data & Statistics
* **Refresh Stat File**: Reconstrói o arquivo de estatísticas usando dados de chat existentes
* **Backfill token counters**: Recalcula contagens de tokens para todas as mensagens no chat atual
    - Útil ao alternar entre modelos com tokenizers diferentes
    - Aciona recarga de chat após conclusão
    - Apenas alterações visuais, não modifica conteúdo do chat
### API & Extension Testing
* **Change Mancer base URL**: Modifica a URL base para o servidor API Mancer
* **Test WebSearch extension**: Realiza uma busca de teste usando configurações atuais
* **Send a generation request**: Testa geração de texto usando a API atualmente selecionada
### System & Debug Tools
* **Force onboarding**: Reinicia o processo de integração
* **Toggle event tracing**: Habilita/desabilita rastreamento de eventos para depuração
* **Copy ST setup**: [Trabalho em Progresso] Copia dados de configuração do sistema para área de transferência para relatórios de bugs

Cada função pode ser executada usando o botão "Execute" abaixo de sua descrição. Considere fazer backup de seus dados antes de usar essas ferramentas, pois algumas operações não podem ser desfeitas.
