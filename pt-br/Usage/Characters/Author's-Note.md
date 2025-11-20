---
order: 50
route: /usage/core-concepts/authors-note/
---

# Author's Note

## O que é?

Author's Note é uma ferramenta poderosa para personalizar respostas da IA que insere uma seção de texto no prompt em qualquer posição e em qualquer frequência que você desejar.

## Uso

O Author's Note pode ser encontrado no menu Options no lado esquerdo da barra de entrada de chat.

| Options Menu                          | Author's Note Panel                    |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## Configurando Author's Notes

### Author's Note específica do Chat

A caixa no topo do painel Author's Note contém o Author's Note para seu chat atual.

**O conteúdo desta caixa não é automaticamente transferido para nenhum novo chat.**

### Opções de posicionamento

#### After Scenario

Isso coloca o Author's Note no topo do contexto após a seção 'Scenario' da Definição do Personagem. Se nenhum cenário for especificado, será colocado após a última porção da Definição do Personagem e antes das mensagens de exemplo.

#### In-chat

Isso coloca o Author's Note no histórico do chat na profundidade especificada.

Depth 0 = colocado no final do histórico do chat.

Depth 4 = colocado antes das 3 mensagens mais recentes do histórico do chat, tornando-se a 4ª entidade no histórico do chat.

_Quanto mais próximo o Author's Note estiver da parte inferior do prompt, mais impacto ele tem na próxima resposta da IA._

### Insertion Frequency

Esta é a frequência com que você deseja que o Author's Note seja incluído no chat.

Frequency 0 = Author's Note nunca será inserido.

Frequency 1 = Author's Note será inserido com cada prompt de entrada do usuário.

Frequency 4 = Author's Note será inserido em cada 4º prompt de entrada do usuário.

### Default Author's Note

A caixa na parte inferior do painel contém o Default Author's Note que será aplicado a cada novo chat.

## Casos de Uso Comuns

### Lembrar a IA da formatação de resposta

O Author's Note pode ser usado para especificar como a IA deve escrever suas respostas.

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### Reforçando Instruções

- [Remember the instructions you were given at the beginning of this chat.]

### Como World Info temporário, Character Bias, ou Instruct para modelos não-Instruct

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
