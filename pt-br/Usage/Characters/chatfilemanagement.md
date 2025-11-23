---
order: 80
route: /usage/core-concepts/chatfilemanagement/
---

# Gerenciamento de Arquivos de Chat

Esta página descreve as maneiras de gerenciar seus arquivos de chat de IA.

!!!info Nota
Algumas dessas opções estão disponíveis no diálogo "Manage chat files" que abre no menu de opções inferior esquerdo.
!!!

## Chats Solo vs Chats em Grupo

A maneira mais simples de usar um card de personagem é um chat Solo; basta clicar no card deles e começar a conversar.

Depois de ter alguns cards de personagem, você também pode usar o botão "Create New Chat Group" para criar um [chat em grupo](/Usage/Characters/groupchats.md) incluindo vários personagens que então interagirão entre si e com você.

## Importação de chat

**Importar chats do Character.AI para o SillyTavern.**

Para importar chats e bots do Character.AI, use a extensão de navegador CAI Tools: [https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools).

Outros programas e ferramentas dos quais você pode importar chats incluem:

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## Exportar como .jsonl

Ao clicar em "Manage chat files", cada entrada na lista de arquivos de chat terá um botão para exportá-la em um formato que pode ser reimportado como está. Use isso para compartilhar ou migrar chats incluindo todos os seus metadados (mas excluindo imagens e anexos de arquivo).

Se você se preocupa com privacidade, certifique-se de inspecionar o arquivo JSONL exportado e remover tudo o que você não quer compartilhar.

## Exportar como .txt

Você também pode exportar uma versão simplificada somente texto com o botão "Download chat as plain text document". Não pode ser reimportado novamente, pois perde metadados importantes!

## Checkpoints

"Checkpoints" são clones do chat atual, no sentido de que copiam todas as mensagens do chat dado até certo ponto, e armazenam um link para a fonte (pelo nome do arquivo de chat).

Do botão de três pontos à direita de cada mensagem de chat, você tem duas maneiras de criar checkpoints:

* "Create Branch" clonará o chat atual até aquela mensagem e alternará para ele
* "Create Checkpoint" clonará o chat atual até aquela mensagem, pedirá um nome e o criará mas NÃO alternará para ele

Você pode pensar neles aproximadamente como "abrir link em nova aba" e "abrir link em nova aba em segundo plano" em um navegador.

Você pode voltar ao pai de um checkpoint entrando no botão de menu hambúrguer à esquerda da caixa de texto de mensagem, depois clicando em "Back to parent chat".

## Renomear Chat

Por padrão, os arquivos de chat recebem um nome com a data e hora em que foram iniciados.

Você pode alterar isso clicando no ícone de lápis e digitando um novo nome.

Observe que isso quebrará links para esse chat de checkpoints (já que eles são vinculados pelo nome do arquivo de chat).
