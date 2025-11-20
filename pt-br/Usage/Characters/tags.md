---
order: 60
route: /usage/core-concepts/tags/
---

# Tags

Cards de personagens e grupos podem ter zero ou mais tags atribuídas. Elas são úteis para organizar coleções em rápido crescimento por temas, qualidade, proveniência ou o que você quiser.

## Marcação (Tagging)

Existem várias maneiras de adicionar ou remover tags de um card de personagem:

- Importar tags incorporadas durante a importação.
- Abrir um card no painel Character Management. De lá você poderá atribuir tags a um card de personagem.
- Marcação em massa.

Para fazer marcação em massa, clique no botão "Bulk edit characters" (ícone de lápis), selecione os cards que deseja marcar, clique com o botão direito em qualquer um deles e clique em "Tag" no menu contextual.

!!!info Nota
Por favor, note que grupos não podem ser marcados em massa.
!!!

A partir desta tela você poderá:

- Adicionar ou remover tags usando a caixa de combinação.
- Remover todas as tags dos cards selecionados ("All").
- Remover a interseção de tags entre todos os cards selecionados desses cards ("Mutual").
- Importar (criar localmente) todas as tags armazenadas no card do personagem, caso você o tenha importado ("Import All").
- Importar (criar localmente) tags armazenadas no card do personagem que também existem localmente com nomes correspondentes ("Import Existing").

## Gerenciamento

Para visualizar e gerenciar todas as tags existentes, abra o painel Character Management e clique no botão "Manage tags" (ícone de engrenagem).

Você pode fazer backup e restaurar todas as informações aqui (lista de tags, atribuições de tags a cards, cores, configurações de pasta, etc.) usando os botões no canto superior direito.

Você pode usar os botões de aderência à esquerda para reordenar as tags como elas aparecerão no filtro de tags no Character Management.

!!!warning Aviso
O arquivo JSON de backup de tags não é destinado a compartilhamento com outros, pois contém informações específicas apenas da sua instância, como nomes internos de entidades!
!!!

## Importando tags ao importar cards de personagem

Ao importar cards de personagem externos de imagens baixadas (ou do botão "Import content from external URL"), você será solicitado a opcionalmente importar as tags que ele contém. Elas não são necessárias para o funcionamento do card; tags são simplesmente organizacionais.

Tags incorporadas do card são armazenadas na seção "Creator's Metadata" do menu "Advanced Definitions" do editor de personagem. Se você deseja propor algumas tags para outros usuários que importariam esse personagem, preencha o campo "Tags to Embed" com uma lista de tags separadas por vírgulas.

!!!info Nota
Este pop-up aparecerá apenas se uma opção de User Settings "Import Card Tags" estiver definida como "Ask".
!!!

No pop-up "Import tags for CHARACTER NAME" que abre, você verá uma lista de tags Existing (que você já tinha localmente com um nome correspondente) e tags New (que você não tinha localmente).

Você pode:

- Cortar as listas conforme necessário e então apertar "Import" - as tags Existing restantes serão adicionadas ao card de personagem importado, e as tags New restantes serão criadas localmente e então adicionadas ao card.
- Ou simplesmente apertar "Import none" para ignorar as tags contidas no card do personagem e importar APENAS o card.
- Ou "Import All" como um atalho para importar todas as tags encontradas no card do personagem (NOTA: incluindo quaisquer que você cortou das listas acima; use o botão "Import" se você cortou).
- Ou "Import Existing" como um atalho para importar apenas as tags que existiam localmente com um nome correspondente.

## Filtrando cards de personagem

Depois de criar tags, você as verá em uma linha no painel Character Management. Você pode clicar nelas para alternar o estado de filtragem de tag; em ordem:

- Um clique mostrará cards marcados com esta tag.
- Outro clique para mostrar apenas cards NÃO marcados com esta tag.
- Outro clique para redefinir a filtragem por esta tag.

Você pode filtrar por qualquer número de tags ao mesmo tempo.

## Tags as Folders

!!!info Nota
Para usar esta funcionalidade, ela deve ser habilitada primeiro nas User Settings, sob a coluna UI Theme. O estado deste toggle também é salvo com o tema da UI.
!!!

Do botão "Manage tags" (ícone de engrenagem), cada entrada de tag tem um botão de alternância de múltiplos estados para alternar entre estes modos de tags-as-folder (chamado de "bogus folder" no código):

- um clique para transformar esta tag em uma "pasta aberta". Ela aparecerá como uma entrada virtual na lista de cards; clicar nela mostrará apenas cards com essa tag
- outro clique para transformar esta tag em uma "pasta fechada". Como acima, mas cards marcados com esta tag não aparecerão por padrão - você precisará clicar na pasta para vê-los.
- outro clique para redefinir o estado tag-as-folder para esta tag.
