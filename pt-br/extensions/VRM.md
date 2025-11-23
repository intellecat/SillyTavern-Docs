---
route: /extensions/vrm/
---

# VRM

Este guia irá orientá-lo no processo de configuração e personalização da extensão VRM para sua experiência no SillyTavern. Esta extensão permite que você use modelos animados VRM para seu personagem, fornecendo um elemento dinâmico e interativo ao seu personagem virtual.

## Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

1. **Seleção de Branch**: Certifique-se de estar usando o branch da versão mais recente do SillyTavern para acessar os recursos e atualizações mais recentes.

2. **Instalação da Extensão**: Instale a extensão "VRM" do menu "Download Extensions & Assets" no painel Extensions (representado pelo ícone de blocos empilhados).

3. **Posicionamento da Pasta de Modelos**: Coloque seus arquivos de modelo VRM (.vrm) no diretório `/data/<user-handle>/assets/vrm/model` e seus arquivos de animação no diretório `/data/<user-handle>/assets/vrm/animation`. Os formatos de arquivo de animação atualmente suportados são .fbx e .bvh que são compatíveis com modelos VRM. Isso inclui qualquer animação que você possa obter do Mixamo (https://www.mixamo.com/) e qualquer animação que você possa exportar de ferramentas como XR Animator (https://github.com/ButzYung/SystemAnimatorOnline).

## Configurações da Extensão

A extensão VRM oferece várias configurações para personalizar o comportamento do seu modelo animado. Aqui estão as configurações principais:

![Configurações globais da UI](/static/extensions/vrm-global.png)

### Configurações Globais

1. **Enabled**:
   - Habilite esta caixa de seleção para ativar a extensão, permitindo que seu modelo VRM interaja dentro do SillyTavern.
   - Você pode desabilitar a extensão se quiser usar apenas sprites normais.

2. **Look at camera**:
   - Habilite esta caixa de seleção para fazer os olhos do modelo VRM olharem para a câmera.

3. **Blink**:
   - Habilite esta caixa de seleção para fazer os olhos do modelo VRM piscarem em intervalos aleatórios. As expressões do modelo devem definir adequadamente a propriedade de peso de piscar, caso contrário, o modelo pode piscar com os olhos fechados, por exemplo. Se isso acontecer:
    - corrija o modelo se você tiver o arquivo .vroid
    - não use essa expressão facial incorreta
    - desabilite completamente o piscar com esta caixa de seleção

4. **TTS Lip sync**
    - Habilite esta caixa de seleção para que o movimento da boca do VRM siga o som do seu TTS quando for reproduzido. Funciona apenas com TTS cujo som é reproduzido pelo próprio Sillytavern, como XTTS (não no modo streaming). Se desabilitado, a boca será animada de acordo com o comprimento do texto da mensagem quando uma nova mensagem de personagem for recebida.

5. **Auto-send Interaction**:
   - Habilite esta caixa de seleção para acionar automaticamente interações de personagem quando você clicar em áreas com mensagens mapeadas (consulte a seção de áreas de acerto para detalhes).

### Configurações de Performance

1. **Body hitboxes**
    - Habilite esta caixa de seleção para ativar a detecção de clique em várias partes do modelo VRM. Dependendo do modelo, as seguintes áreas podem ser detectadas: cabeça/peito/mãos/virilha/bunda/pernas/pés. As localizações das hitboxes são computadas a cada quadro e seguem a animação do corpo, desabilitar esta opção pode melhorar a performance.

2. **Use model cache**
    - Habilite esta caixa de seleção para manter modelos VRM na memória ao trocar modelos, permite voltar ao modelo anterior mais rápido. Útil se você usar modelos diferentes para o mesmo personagem para mudar roupa ou forma, por exemplo. Pode afetar a performance.

3. **Use animation cache**
    - Habilite esta caixa de seleção para manter todas as animações reproduzidas durante a sessão na memória. Todas as animações atribuídas a um modelo também serão carregadas na primeira vez que o modelo aparecer. Aumentará o tempo de carregamento do modelo na primeira vez, mas tornará todas as trocas de animação instantâneas. Pode afetar a performance.

### Configurações de Debug

1. **Show grid**
    - Habilite esta caixa de seleção para visualizar a grade 3d, caixa de arrastar do modelo e hitboxes do corpo.

2. **Reload button**
    - Clique neste botão para recarregar a cena 3d, limpar o cache e todos os modelos VRM. Use-o se ocorrer algum bug ou se o cache começar a afetar a performance.

### Configurações de Cena

![Configurações de cena da UI](/static/extensions/vrm-scene.png)

1. **Light Color**
    - Defina a cor da luz na cena 3d. Clique no botão reset para defini-la de volta para a cor branca padrão. Dependendo do seu navegador, você pode usar um seletor de cores, por exemplo, você pode selecionar a cor da sua imagem de fundo para adicionar mais imersão.

2. **Light intensity**
    - Defina a intensidade da luz em porcentagem usando o controle deslizante. Clique no botão reset para defini-la de volta para o valor padrão de 100%. Modelos VRM podem reagir de forma diferente à luz dependendo dos shaders incorporados no modelo, brinque com o valor e veja como fica.

![Configurações de modelo da UI](/static/extensions/vrm-model.png)

## Seleção de Personagem

Essas configurações permitem que você gerencie personagens e atribua modelos VRM a eles.

1. **Botão Refresh**:
   - Clique no botão refresh para atualizar a lista de personagens no chat atual.

2. **Select Character**:
   - Use a lista suspensa para escolher um personagem para atribuir um modelo VRM.

3. **Botão Remove**:
   - Clique neste botão para excluir o modelo atribuído a um personagem.

## Seleção de Modelo

1. **Botão Refresh**:
   - Clique no botão refresh se seu modelo VRM não aparecer na lista.

2. **Select Model**:
   - Escolha um modelo da lista para atribuí-lo ao personagem selecionado.
   - O modelo deve estar localizado no diretório `/data/<user-handle>/assets/vrm/model`.

3. **Botão Reset**
    - Clique neste botão para redefinir as configurações do modelo para seu padrão. Se você tiver arquivos de animação que correspondem ao valor padrão, eles serão mapeados automaticamente. Veja o mapeamento de nomenclatura no final deste README.

## Configurações de Modelo

1. **Model Scale**:
   - Use o controle deslizante para ajustar o tamanho do modelo, tornando-o maior ou menor.

2. **Model Center X/Y Offset**:
   - Use esses controles deslizantes para alterar a posição horizontal/vertical do modelo em relação ao centro da janela.

3. **Model X/Y Rotation**
    - Use esses controles deslizantes para alterar a rotação horizontal/vertical do modelo em relação aos quadris do modelo.

### Observações
    - As configurações são salvas por modelo, não por personagem, e carregam em chats diferentes.
    - Se você quiser usar o mesmo modelo para dois personagens diferentes com configurações diferentes, faça uma cópia do arquivo .vrm.
    - Você também pode arrastar o modelo com o mouse, e essas configurações serão atualizadas e salvas. Clique com o botão esquerdo e segure para arrastar um modelo pela tela. Clique com o botão do meio do mouse e segure para girar o modelo ou use shift+clique esquerdo. Use a roda do mouse com o cursor no modelo para ampliá-lo ou reduzi-lo ou use ctrl+clique esquerdo.
    - Use essas configurações de UI para trazer seu modelo de volta à tela se você de alguma forma o colocou fora da vista. Além disso, marque a caixa de seleção "Show frame" para ver claramente onde você pode clicar para arrastar o modelo.

![Configurações de hitboxes da UI](/static/extensions/vrm-hitboxes.png)

## Mapeamento de Hitboxes

    - Dependendo da definição dos ossos do modelo, algumas áreas de hitboxes podem ser geradas, elas serão listadas nesta parte da UI, e você pode atribuir uma expressão/animação/mensagem a cada uma delas que será acionada quando você clicar na área.

![Configurações de classificação da UI](/static/extensions/vrm-classify.png)

## Mapeamento de Expressões Classificadas

1. **Requisitos**
    - Requer o uso da extensão classify expression; caso contrário, voltará à animação padrão.

2. **Mapeamento**
    - Para cada emoção detectada pela extensão classify, você pode atribuir uma expressão/movimento/mensagem. A mensagem pode conter comandos.

## Comandos

1. **/vrmlightcolor**
    - define a cor da luz
    - argumentos: color
    - exemplo: "/vrmlightcolor white" ou "/vrmlightcolor purple".
2. **/vrmlightintensity**
    - define a intensidade da luz em porcentagem
    - argumentos: intensity
    - exemplo: "/vrmlightintensity 0" ou "/vrmlightintensity 100
3. **/vrmmodel**
    - atribui o modelo vrm ao personagem
    - argumentos: character, model
    - exemplo: "/vrmmodel Seraphina.vrm" em chat solo ou "/vrmmodel character=Seraphina model=Seraphina.vrm" em chat em grupo
4. **/vrmexpression**
    - muda a expressão do modelo
    - argumentos: character, expression
    - exemplo: "/vrmexpression happy" em chat solo ou "/vrmexpression character=Seraphina expression=happy" em chat em grupo

5. **/vrmmotion**
    - muda a animação do modelo
    - argumentos: character, motion, loop, random
    - "/vrmmotion idle" ou "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## Mapeamento padrão de animações
Se seus arquivos de animação forem nomeados da seguinte maneira, eles serão mapeados automaticamente ao redefinir as configurações de um modelo. Por exemplo, os arquivos nomeados "assets/vrm/animation/neutral.bvh" e "assets/vrm/animation/neutral1.fbx" serão automaticamente mapeados como um grupo para animação classificada padrão e neutra. O mesmo vale para as hitboxes.

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

Obrigado por seguir este guia! Sua experiência no SillyTavern agora está enriquecida com modelos 3D animados e interativos.

## Observações
    - Os modelos VRM carregados por esta extensão são os arquivos .vrm, não os arquivos .vroid.
    - Os arquivos de animação devem ser compatíveis com VRM, você pode usar uma ferramenta como XR animation (https://github.com/ButzYung/SystemAnimatorOnline) para converter arquivos de animação fbx/bvh.
    - Você pode criar grupos de animação tendo arquivos com o mesmo nome terminando com números diferentes, por exemplo: "idle1.bvh", "idle2.bhv", "idle3.bvh" serão considerados como um grupo "idle" e quando selecionado em um mapeamento, um aleatório será reproduzido quando acionado, pode ser usado para adicionar variedade às animações.
    - Você pode obter animações curadas deste repositório: https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral tem alguns vídeos tutoriais sobre como usar a extensão e o repositório de animações: https://www.youtube.com/@nitralai
