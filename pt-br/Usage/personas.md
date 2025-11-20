---
order: 110
icon: smiley
route: /usage/core-concepts/personas/
templating: false
---

# Personas

## O que é uma Persona?

Uma persona no SillyTavern é a identidade que você usa para participar dos chats — essencialmente uma combinação do seu nome de exibição, avatar e texto descritivo opcional. As personas permitem que você troque facilmente de papéis ou "personagens" como os quais você fala, sem ter que atualizar manualmente seu nome de usuário/avatar cada vez.

!!!
**Nota:** Avatares/nomes de usuário legados que não estavam vinculados a uma persona foram removidos. Os dados existentes serão migrados para personas. Se nenhum nome foi especificado, a persona será nomeada "[Unnamed Persona]".
!!!

## Como Criar uma Persona?

1. Abra o painel **Persona Management** (botão <i class="fa-solid fa-face-smile"></i> no menu superior).
2. Crie uma persona em branco com o botão **Create** e dê um nome a ela.
3. Na lista de personas, selecione a persona recém-criada.
4. No lado direito, você pode preencher sua descrição e definir um avatar através do botão "Change Persona Image". Ambos são opcionais.
5. Agora sua persona está pronta para usar nos chats.

### Converter Personagem para Persona

Personas também podem ser criadas convertendo qualquer personagem existente. Simplesmente abra o personagem, selecione "More..." e clique em "Convert to Persona". Uma persona com o mesmo nome e descrição será criada. Outros campos do cartão de personagem como Scenario ou Personality não serão usados. O personagem não será excluído.

!!! Note
Como as macros `{{user}}` e `{{char}}` têm significados opostos quando usadas em descrições de Persona e Character, você será solicitado a trocá-las se a descrição convertida contiver alguma delas.
!!!

## Descrição da Persona

Cada persona pode armazenar uma descrição de texto personalizada — traços mentais e físicos, idade, ocupação ou quaisquer detalhes pessoais. Estas também podem incluir macros de template como `{{char}}` ou `{{user}}` (veja [Macros](/Usage/Characters/macros.md)).

Onde sua descrição de persona é injetada no prompt da IA depende da configuração **Position** no painel Persona Management:

- **None (disabled)**
- **In Story String / Prompt Manager** (o padrão)
- **Top of Author's Note** / **Bottom of Author's Note** (só será adicionada quando existir uma Author's Note)
- **In Chat @ Depth** (isso abrirá opções de configuração para definir profundidade e papel)

A posição é salva **por persona**.

## Título da Persona

O título é um campo de texto opcional que pode ser usado para armazenar informações adicionais sobre a persona e não é usado no prompt, mas exibido no painel Persona Management.

Para definir um título, clique no botão **<i class="fa-solid fa-pencil"></i> Rename Persona** no painel Persona Management e insira o título no campo "Persona Title", ou especifique-o durante a criação da persona. Definir um valor vazio quando o título já existe o removerá.

## Conexões de Persona / Bloqueio

As conexões de persona garantem que uma determinada persona seja automaticamente selecionada em certas situações. Se nenhuma persona estiver conectada, a persona atualmente escolhida permanecerá selecionada.

Existem três tipos de bloqueio:

1. **<i class="fa-solid fa-unlock"></i> Chat lock** – A persona está bloqueada para o chat atual.
2. **<i class="fa-solid fa-unlock"></i> Character lock** – A persona está bloqueada para um personagem específico.
3. **<i class="fa-solid fa-crown"></i> Default persona** – Uma persona que é usada sempre que nenhum outro bloqueio se aplica.

### 1. Bloquear para um Chat

Se uma persona estiver bloqueada para um chat, abrir esse chat no futuro mudará automaticamente sua persona ativa para a bloqueada.

- **Para bloquear**: Selecione a persona desejada, depois clique no botão **<i class="fa-solid fa-unlock"></i> Chat** na seção "Connections" (ou use `/persona-lock type=chat on`).
- **Para desbloquear**: Clique no botão novamente (ou use `/persona-lock type=chat off`).

### 2. Bloquear para um Personagem

Você também pode vincular uma persona a um personagem específico. Abrir qualquer chat com esse personagem selecionará automaticamente sua persona bloqueada.

- **Para bloquear**: Selecione a persona desejada, depois clique no botão **<i class="fa-solid fa-unlock"></i> Character** na seção "Connections" (ou use `/persona-lock type=character on`).
- **Para desbloquear**: Clique no botão novamente (ou use `/persona-lock type=character off`).

O painel Persona Management também mostra quais personagens estão vinculados a essa persona (exibidos como pequenos avatares). Clicar neles navega diretamente para o chat desse personagem.

#### Bloqueando Múltiplas Personas para o Mesmo Personagem

Se outra persona já estava vinculada com esse personagem, ela será automaticamente desvinculada por padrão.

Para ter múltiplas personas vinculadas ao mesmo tempo, a configuração global **Allow multiple persona connections per character** pode ser usada.
Se múltiplas personas estiverem vinculadas ao mesmo personagem, você verá um popup perguntando qual persona usar cada vez que abrir ou iniciar um novo chat com esse personagem (a menos que uma persona esteja vinculada ao chat).

### 3. Persona Padrão

Sua **persona padrão** é usada sempre que não houver outro bloqueio relevante. A persona padrão é reconhecível por uma borda amarela ao redor de seu avatar.

- **Para definir/remover padrão**: Selecione a persona desejada, depois clique no botão **<i class="fa-solid fa-crown"></i> Default** na seção "Connections" (ou use `/persona-lock type=default`).

Apenas uma persona pode ser escolhida como a persona padrão.

### Persona Temporária

Se alguma das três opções de conexão conectar uma persona ao personagem/chat atual, você ainda pode escolher usar uma persona diferente. Esta persona será marcada no painel de personas como "Temporary Persona". Qualquer recarga da janela do navegador ou mudança para um chat diferente e de volta irá redefini-la para a persona vinculada novamente.

Você pode manualmente *converter* uma Persona Temporária para ser persistentemente conectada vinculando-a ao chat.

## Configurações Globais de Persona

Todas as configurações em **Current Persona** são salvas por persona. Algumas configurações globais também existem, essas podem ser encontradas em **Global Persona Settings** no painel Persona Management.

1. **Show notifications on switching personas**
   - Habilita mensagens toast relacionadas a personas (por exemplo, "Persona Auto Selected", "Temporary Persona").

2. **Allow multiple persona connections per character**
   - Quando **habilitado**, você pode vincular múltiplas personas a um único personagem. Abrir o chat desse personagem solicitará qual persona usar. Se desabilitado, apenas uma persona pode ser conectada a um personagem por vez.

3. **Auto-lock a chosen persona to the chat**
   - Quando **habilitado**, sempre que você selecionar uma persona (manualmente ou por seleção automática) ou criar um novo chat, ela bloqueia essa persona para o chat.
   Isso combinado com "Allow multiple" fornece a opção de ter uma seleção de persona por personagem, mas mantê-la vinculada uma vez escolhida para um chat.

## Comandos Slash para Personas

### `/persona-lock type=<type?>`

- `chat` bloqueia a persona atual para seu chat ativo.
- `character` bloqueia a persona atual para o personagem em uso.
- `none` (ou nenhum argumento) desbloqueia/limpa o bloqueio de persona para o contexto atual.
- Se usado sem argumentos, retorna o estado atual de bloqueio (ou um erro se nenhum estiver definido).
- O estado de bloqueio pode ser escolhido via `on`, `off` ou `toggle`. O padrão é toggle.

### `/persona <name>`

- Troque rapidamente sua persona ativa por nome sem abrir o painel Persona Management.
- Exemplo: `/persona Blaze`.
- Usar `mode=temp` permite definir temporariamente seu nome da persona **atual**, mesmo que uma persona com o mesmo nome já possa existir (preservando seu avatar e descrição atuais).

### `/persona-sync`

- Reatribui todas as mensagens do usuário no chat ativo para a persona **atual** e seu nome.

> **Nota:** Os comandos mais antigos `/lock` e `/unlock` permanecem para compatibilidade com versões anteriores, mas podem ser removidos no futuro. Use `/persona-lock` em vez disso.

## Dicas Profissionais

1. **Trocar personas no meio do chat** não reatribui suas mensagens de usuário passadas para a nova persona; essas permanecem atribuídas à persona que você estava usando na época.
2. **Reatribuição em lote**: Se você precisar que todas as mensagens anteriores correspondam a uma nova persona, clique no botão **sync** ou use `/persona-sync`.
3. **Substituir imagens de persona** sem perder descrição ou bloqueios escolhendo sua persona e clicando no botão **<i class="fa-solid fa-images"></i> Change Persona Image**.
4. **Popups de vínculo de personagem**: Se múltiplas personas estiverem vinculadas ao mesmo personagem, você receberá um popup para escolher qual persona cada vez que abrir o chat. Esta é uma maneira prática de ter uma pequena seleção de personas para escolher para personagens específicos.
5. **Backups**: Você pode fazer backup de toda a sua lista de personas (nomes, conexões de personagens, descrições) com o botão **Backup** no Persona Management, e restaurá-la depois se necessário.

!!!tip Observações sobre Backup

- Imagens e conexões de chat não são salvas junto com as personas e não serão incluídas no backup.
- Esses backups não são projetados para serem compartilhados, pois contêm links internos.

!!!
