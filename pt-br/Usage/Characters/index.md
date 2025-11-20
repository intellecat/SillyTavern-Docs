---
order: 100
icon: person-fill
route: /usage/characters/
---

# Personagens

Personagens são as identidades de IA que você pode criar e gerenciar para moldar o papel da IA na conversa. Cada
personagem tem um nome, personalidade e histórico de conversa. Você pode criar quantos personagens quiser e
alternar entre eles a qualquer momento.

Os personagens podem ser usados em chats solo, ou adicione vários personagens a um chat em grupo para
deixá-los interagir uns com os outros.

## Painel de Gerenciamento de Personagens

Abra o painel <i class="fa-solid fa-address-card"></i> **Characters** na barra de navegação para acessar a lista
de personagens. Clique em um personagem ou grupo para conversar com eles ou editá-los, ou
escolha <i class="fa-solid fa-user-plus"></i> **Create New Character** para adicionar um novo personagem.

### Controles do Painel

* <i class="fa-solid fa-lock"></i> **Pin Panel**: Mantém o painel aberto durante a interação
* <i class="fa-solid fa-list-ul"></i> **Character List**: Retorna à visualização da lista de personagens
* **HotSwap Bar**: Acesso rápido aos personagens favoritos

### Lista de Personagens

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: Adiciona um novo personagem
* <i class="fa-solid fa-file-import"></i> **Import Character**: Carrega personagem de arquivo
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: Importa de URL
* <i class="fa-solid fa-users-gear"></i> **Create Group**: Inicia um novo chat em grupo

#### Pesquisar e ordenar

* **Search Bar**: Filtra personagens por nome ou atributos
* **Sort Dropdown**: Múltiplas opções de ordenação:
    - Alfabética (A-Z, Z-A)
    - Cronológica (Newest, Oldest)
    - Baseada em uso (Recent, Most/Least chats)
    - Baseada em tamanho (Most/Least tokens)
    - Especial (Favorites, Random)

#### Filtrar personagens por tipo ou tag

* <i class="fa-solid fa-star"></i> **Favorites Filter**: Mostra personagens favoritos
* <i class="fa-solid fa-users"></i> **Groups Filter**: Mostra apenas chats em grupo
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: Organiza por hierarquia de tags
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [Configuração de tags](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: Visualiza todas as tags disponíveis
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: Redefine todos os filtros

### Painel de Criação/Edição de Personagem

* **Avatar Image**: Faz upload e visualiza a foto de perfil do personagem
* **Token Count**: [Uso de tokens](characterdesign.md#character-tokens) para o personagem
* <i class="fa-solid fa-ranking-star"></i> **Stats**: Histórico de chat e estatísticas de uso
* [Gerenciamento de tags](/Usage/Characters/Tags.md)

#### Ações Rápidas

- <i class="fa-solid fa-star"></i> Alternar favorito
- <i class="fa-solid fa-book"></i> Definições avançadas
- <i class="fa-solid fa-globe"></i> Lore do personagem
- <i class="fa-solid fa-passport"></i> Chat lore: vincular o chat a um [World Info](/Usage/worldinfo.md)
- <i class="fa-solid fa-file-export"></i> Exportar personagem
- <i class="fa-solid fa-clone"></i> Duplicar
- <i class="fa-solid fa-skull"></i> Excluir

#### Opções Estendidas

* Vinculação de World Info
* Importação de lore do card
* Substituição de cenário
* Conversão de persona
* Renomear personagem
* Vinculação de fonte
* Substituir/Atualizar
* Importação de tags
* Visualização de galeria

#### Campos de Conteúdo

* **[Character Description](characterdesign.md#character-description)**: Resumo breve do personagem
* **[First Message](characterdesign.md#first-message)**: Saudação ou prompt inicial ao iniciar um novo chat
* **Alternative greetings**: Define várias primeiras mensagens entre as quais você pode deslizar ao iniciar um chat

### Painel de Definições Avançadas

Clique no botão <i class="fa-solid fa-book"></i> **Advanced Definitions** para acessar as configurações estendidas do personagem.

#### Prompt Overrides (Chat Completion/Instruct Mode)

* **Main Prompt**: Substitui o [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) padrão, pode usar
  o placeholder \{\{original\}\} para incluir o prompt original
* **Post-History Instructions**: Substitui as
  [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) padrão

#### Creator's Metadata

Informações não relacionadas ao prompt sobre o personagem:

- Nome/contato do criador
- Versão do personagem
- Notas do criador
- Lista de tags incorporadas

#### Character Personality

* **[Personality Summary](characterdesign.md#personality-summary)**: Visão geral breve dos traços do personagem
* **[Scenario](characterdesign.md#scenario)**: Contexto e circunstâncias do diálogo
* **Character's Note**: Mensagem personalizada com profundidade selecionável e função de mensagem (veja também
  [Author's Note](/Usage/Characters/Author's-Note.md))
* **Talkativeness** (Group Chats): Controle deslizante para Shy → Normal → Chatty
* **Example Messages**: Exemplos do estilo de escrita do personagem

### Gerenciamento de Chat em Grupo

Se este é um chat em grupo, você pode gerenciar os membros do grupo e as configurações deste painel.

Veja [Group Chats](/Usage/Characters/groupchats.md) para mais detalhes.
