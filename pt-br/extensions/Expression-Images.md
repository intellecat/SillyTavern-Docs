---
route: /extensions/expression-images/
---

# Expressões de Personagem

## O que é isso?

Imagens de expressão são imagens (também conhecidas como 'sprites') do seu personagem de IA que são mostradas ao lado (ou atrás) da janela de chat.

Imagens de expressão podem mudar automaticamente com base em uma classificação, ajustando-se ao sentimento expresso na resposta mais recente do chat da IA.

## Adicionando Imagens de Expressão de Personagem

1. Abra o Painel Extensions e expanda a seção 'Character Expressions'. Se você tiver o chat do personagem aberto, verá uma grade de placeholders de imagem.
![Expression Drawer](/static/extensions/expression-drawer.png)
2. Clique no botão 'Upload image' no canto superior esquerdo de cada imagem na grade e selecione a imagem que deseja aplicar a essa emoção. Isso salvará a imagem com o nome de arquivo correto dentro da pasta `/data/<user-handle>/characters/(character_name_here)/`.
3. Repita isso para todas as expressões às quais deseja atribuir uma imagem.

### Importando um arquivo ZIP de imagens de Expressão

Usando o botão '<i class="fa-solid fa-file-zipper"></i> Upload sprite pack (ZIP)', você pode importar um arquivo zip que contém uma coleção de imagens de expressão, e essas imagens serão automaticamente adicionadas à pasta correta para seu **personagem atualmente selecionado**. O arquivo ZIP deve conter todas as imagens em uma estrutura plana (sem subpastas) e arquivos corretamente nomeados. Importar um zip não renomeará automaticamente nenhuma imagem para fazer com que correspondam às emoções.

## Alterar Expressões Manualmente

1. Clique em qualquer uma das imagens de expressão carregadas (sprites) para exibi-las perto da interface de chat (com modo UI padrão) ou no centro da tela (em modo Visual Novel).
2. Use o comando slash `/expression-set (name)` ou Quick Reply correspondente para definir o sprite sem abrir o menu de extensões.

## Alterar Expressões Automaticamente

Para definir expressões automaticamente quando o personagem responde, você tem múltiplas opções.
As expressões mudam por mensagem ou em intervalos regulares quando o streaming de mensagens está habilitado.

### Como funciona o módulo classify?

O módulo `classify` usa um pequeno modelo de 'análise de sentimento' que roda junto com o servidor SillyTavern. Este modelo pega a nova saída da IA e detecta que tipo de sentimento, ou emoção, o texto está expressando. Embora múltiplos sentimentos possam ser expressos em uma única mensagem, o modelo escolhe apenas o mais provável e retorna isso ao SillyTavern. A extensão frontend então exibe a imagem associada a esse sentimento.

### Instruções de Configuração (Local)

1. Abra o painel de extensões e expanda o menu de extensão "Character Expressions".
2. Selecione "Local" no dropdown de fonte de classificação.
3. Isso iniciará um download único do modelo de classificação do HuggingFace Hub (cerca de ~100 Mb).
4. Gere qualquer mensagem para verificar se a classificação funciona e o sprite aparece. Você também pode verificar o console do servidor para logs de debug.

A classificação local tem como padrão 28 rótulos de imagem possíveis: [Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

Para usar o modelo de classificação de 6 opções, altere o valor da variável `extensions.models.classification` no arquivo `config.yaml` para: [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

### Instruções de Configuração (com LLM)

1. Conecte-se a qualquer uma das APIs suportadas e adequadamente configuradas via **<i class="fa-solid fa-plug"></i> API Connections**.
2. Importe as imagens de expressão da mesma forma mencionada acima.
3. Selecione "Main API" no dropdown de fonte de classificação.
4. Opcionalmente, configure o prompt de instrução de classificação.
5. Gere qualquer mensagem para verificar se a classificação funciona e o sprite aparece. Você também pode verificar o console do servidor para logs de debug.

#### Estratégias de Construção de Prompt

A fonte Main LLM permite escolher como o prompt de classificação será construído:

* **Limited Context**: Apenas a última mensagem e um prompt de instrução do sistema são enviados.
* **Full Context**: Todo o histórico do chat, incluindo o card do personagem, são enviados.

### Instruções de Configuração (WebLLM)

1. Instale a [extensão oficial WebLLM](https://github.com/SillyTavern/Extension-WebLLM).
2. Importe as imagens de expressão da mesma forma mencionada acima.
3. Selecione "WebLLM" no dropdown de fonte de classificação.
4. Opcionalmente, configure o prompt de instrução de classificação.
5. Gere qualquer mensagem para verificar se a classificação funciona e o sprite aparece. Você também pode verificar o console do servidor para logs de debug.

### Instruções de Configuração (com Extras)

> [!WARNING]
> Extras está descontinuado e pode ser removido em atualizações futuras.

1. Tenha Extras instalado e rodando com o módulo `classify` habilitado: `python server.py --enable-modules=classify`
2. Importe as imagens de expressão da mesma forma mencionada acima.
3. Selecione "Extras" no dropdown de fonte de classificação.
4. A imagem de expressão apropriada será exibida automaticamente sempre que a IA enviar uma resposta.

A API Extras usa um modelo de classificação com 6 opções por padrão: [nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

Também há um modelo com 28 opções: [joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

Para usar este modelo você precisa alterar sua linha de comando Extras para incluir o seguinte argumento (com um espaço antes e depois): `--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

## Expressões Personalizadas

Como obter mais opções de expressão do que as fornecidas por padrão? Você pode configurar **Expressões Personalizadas** nas configurações da extensão. Você pode atribuir qualquer nome às Expressões Personalizadas. Elas aparecerão na lista de imagens de expressão e podem receber imagens como outras expressões. Elas terão um indicador mostrando que são personalizadas.

> [!TIP]
> Tanto Local quanto Extras suportam apenas uma lista limitada de expressões.
>
> Se você quiser que Expressões Personalizadas sejam exibidas, você precisa treinar um modelo de classificação com rótulos suportados (fora do escopo deste guia), ou pode usar LLM ou WebLLM como fonte de classificação, que ambos usarão automaticamente todas as expressões existentes - tanto as padrão quanto quaisquer personalizadas.

## Quais formatos de imagem são suportados para Expressões?

Qualquer formato de imagem é permitido, incluindo webp e gifs animados.

O formato mais comum é um arquivo PNG com fundo transparente.

## Usando Expressões Padrão

Se você não tiver imagens de expressão para todas as expressões de um personagem, ou nenhuma imagem, há múltiplas opções sobre o que exibir por padrão.
Todas essas podem ser selecionadas via dropdown em 'Default / Fallback Expression'.

1. **Escolha uma Expressão de Fallback**: Se uma expressão for escolhida onde você não tem uma imagem, a expressão de fallback é mostrada em vez disso. Simplesmente selecione uma das expressões disponíveis no dropdown.
2. **[No Fallback]**: Quando nenhuma imagem existe, não mostrar nada.
3. **[Default emojis]**: Você pode usar as expressões padrão integradas que estão incluídas no SillyTavern. Estas são imagens simples no estilo emoji.

## Usando Múltiplas Imagens por Expressão

É possível adicionar múltiplas imagens por expressão para permitir mais variedade nas expressões exibidas.
Para habilitar isso, simplesmente alterne **Allow multiple sprites per expression**.
Agora você pode fazer upload de mais de uma imagem, e quaisquer imagens adicionais serão exibidas com um pequeno marcador.

Imagens individuais podem ser escolhidas manualmente selecionando-as com um clique, ou via `/expression-set type=sprite`, que listará imagens de sprite disponíveis, em vez de expressões.

Sempre que uma expressão com múltiplas imagens for automaticamente escolhida, uma das imagens existentes será selecionada aleatoriamente.
Se você quiser forçar uma nova imagem dessa expressão a ser escolhida quando a mesma expressão for usada várias vezes, você pode habilitar **Re-roll if same sprite is used again**.

### Convenção de Nomenclatura para Múltiplas Imagens por Expressão

No caso de múltiplas imagens por expressões, os arquivos precisam ser nomeados de uma maneira específica.
Os arquivos precisam começar com o nome das expressões, e depois seguidos por um sufixo, seja separado por um ponto ou um traço. Exemplos: `joy.png`, `joy-1.png`, `joy.expressive.png`
Os nomes de arquivo devem seguir este formato tanto para uploads diretos quanto para importações ZIP.

## Substituição de Pasta de Sprite

> [!NOTE]
> Nomes de exibição (não nomes de arquivo de card de personagem) ditam qual conjunto de imagens é usado

Se você tiver mais de um personagem com o mesmo nome de exibição, ambos usarão o mesmo conjunto de imagens de expressão.

Se você quiser um conjunto de imagens diferente a ser usado para cada versão do personagem com o mesmo nome, você pode usar a substituição de pasta de sprites.
Substituições de pasta também podem ser usadas para definir conjuntos de sprites diferentes (roupas, etc.) do mesmo personagem.

### Como definir uma substituição

1. Crie uma pasta em `/data/<user-handle>/characters` com qualquer nome e coloque imagens lá, por exemplo `/data/<user-handle>/characters/Boris`.
2. Abra o chat com o personagem cujos sprites você gostaria de substituir.
3. Insira o nome da pasta de substituição no input "Sprite Folder Override" e clique em "Submit".
4. A lista de Sprites recarregará e o indicador "Sprite set" deve mostrar a pasta de substituição.
5. Alternativamente, você pode usar o comando slash `/costume` para alcançar o mesmo resultado: `/costume Boris`.
6. Ao preceder o nome da pasta de substituição com uma barra invertida, ele resolverá para uma subpasta na pasta de sprites do personagem atual, por exemplo `/costume \tracksuit` para o personagem chamado Boris resolverá para a pasta `/data/<user-handle>/characters/Boris/tracksuit`.
