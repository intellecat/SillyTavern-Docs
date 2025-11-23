---
route: /extensions/live2d/
---

# Live2D

Este guia irá orientá-lo no processo de configuração e personalização da extensão Live2D para sua experiência SillyTavern. Esta extensão permite que você use modelos animados Live2D para seu personagem, fornecendo um elemento dinâmico e interativo ao seu personagem virtual.

## Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

1. **Seleção de Branch**: Certifique-se de estar usando a versão mais recente do SillyTavern para acessar os recursos e atualizações mais recentes.

2. **Instalação da Extensão**: Instale a extensão "Live2D" no menu "Download Extensions & Assets" no painel Extensions (representado pelo ícone de blocos empilhados).

3. **Posicionamento da Pasta do Modelo**: Coloque suas pastas de modelo Live2D no diretório `/data/<user-handle>/assets/live2d`. Uma pasta de assets `live2d` adequadamente organizada pode parecer com isto:

    ![Asset folder example](/static/extensions/live2d-folder.png)

    - Uma pasta de modelo Live2D deve incluir todos os componentes necessários para o modelo Live2D, como expressões, movimentos, texturas, sons e arquivos de configurações. Notavelmente, o arquivo `***.model.json` deve estar na raiz da pasta do modelo Live2D para que o modelo seja detectado pela extensão. Neste exemplo, a pasta do modelo live2d `shizuku` pode parecer com isto:

    ![Live2d model folder example](/static/extensions/live2d-model.png)

    - Nota: Modelos também podem ser colocados em pastas específicas do personagem, como `/data/<user-handle>/characters/Shizuku/live2d/`. No entanto, modelos em pastas de personagens só estarão acessíveis para esse personagem específico.

## Configurações da Extensão

A extensão Live2D oferece várias configurações para personalizar o comportamento do seu modelo animado. Aqui estão as configurações principais:

![UI global settings](/static/extensions/live2d-global.png)

### Configurações Globais

1. **Enabled**:
   - Habilite esta caixa de seleção para ativar a extensão, permitindo que seu modelo Live2D interaja dentro do SillyTavern.
   - Você pode desabilitar a extensão se quiser usar apenas sprites normais.
   - Você pode desabilitar a extensão quando quiser mover sprites normais em um chat em grupo e habilitá-la novamente quando estiver pronto para usar modelos Live2D.

2. **Follow Cursor**:
   - Habilite esta caixa de seleção para fazer o modelo Live2D seguir seu cursor, desde que o modelo suporte este recurso.

3. **Auto-send Interaction**:
   - Habilite esta caixa de seleção para acionar automaticamente interações de personagem quando você clicar em áreas com mensagens mapeadas (consulte a seção de áreas de hit para detalhes).


## Configurações de Debug

Essas configurações ajudam você a controlar o comportamento e visibilidade do seu modelo Live2D para fins de debug.

1. **Reset Model Before Animation**:
   - Habilite esta caixa de seleção para recarregar o modelo antes de qualquer animação. Isso força a animação a começar e permite que você faça spam de cliques se necessário. Alguns modelos podem requerer isso para garantir que as animações comecem de um estado compatível.

2. **Show Model Frames**:
   - Habilite esta caixa de seleção para exibir o quadro do modelo, facilitando a identificação de onde clicar para arrastar o modelo. Também mostra a área de hit, se disponível. Passar o mouse sobre uma área de hit mostrará seu nome.

3. **Reload button**
    - Clique neste botão para recarregar todos os modelos live2d. Use-o caso algo apresente falha.

## Seleção de Personagem

Essas configurações permitem gerenciar personagens e atribuir modelos Live2D a eles.

1. **Refresh Button**:
   - Clique no botão refresh para atualizar a lista de personagens no chat atual.

2. **Select Character**:
   - Use a lista dropdown para escolher um personagem para atribuir um modelo Live2D.

3. **Remove Button**:
   - Clique neste botão para excluir todos os modelos atribuídos para um personagem. Um prompt de confirmação aparecerá para confirmar a exclusão.

## Seleção de Modelo

![UI model list](/static/extensions/live2d-list.png)

1. **Refresh Button**:
   - Clique no botão refresh se seu modelo Live2D não aparecer na lista.

2. **Select Model**:
   - Escolha um modelo da lista para atribuí-lo ao personagem selecionado.
   - O modelo pode estar localizado na pasta asset ou na pasta do personagem atual.
   - A lista exibe o nome da pasta do modelo, sua origem (asset ou personagem) e o nome do arquivo de configuração do modelo detectado.
   - Note que algumas pastas de modelo podem conter versões diferentes do mesmo modelo. Você pode tentar diferentes arquivos de modelo para ver qual funciona melhor.
   - Selecionar none usará sprites normais se houver algum
   - Configurações são salvas por personagem e modelo

## Configurações do Modelo

![UI model settings](/static/extensions/live2d-settings.png)

1. **Model Scale**:
   - Use o controle deslizante para ajustar o tamanho do modelo, tornando-o maior ou menor.

2. **Model Center X Offset**:
   - Use o controle deslizante para alterar a posição horizontal do modelo em relação ao centro da janela.

3. **Model Center Y Offset**:
   - Use o controle deslizante para ajustar a posição vertical do modelo em relação ao centro da janela.


### Observações
- As configurações são salvas e persistem em diferentes chats.
- Você também pode arrastar o modelo com o mouse, e essas configurações serão atualizadas e salvas.
- Use essas configurações de UI para trazer seu modelo de volta à tela se você o colocou fora de vista de alguma forma. Além disso, marque a caixa "Show frame" para ver claramente onde você pode clicar para arrastar o modelo.

## Model Talk

![UI model talk](/static/extensions/live2d-talk.png)

1. **Param mouth open Y id**
    - Selecione da lista o ID do parâmetro correspondente ao valor Y da boca do modelo. Nem todos os modelos têm um, e os nomes podem variar de modelo para modelo. Geralmente algo como "PARAM_MOUTH_OPEN_Y" ou "ParamMouthOpenY". Verifique o modelo ao selecionar um elemento da lista; ele tentará executar a animação de fala. Se a boca se mover, você acertou!

2. **Mouth movement speed**
    - Ajuste o controle deslizante para alterar a velocidade de movimento da animação da boca.

3. **Time per character**
    - Defina a duração de tempo de cada caractere. A duração da animação de fala será este tempo multiplicado pelo número de caracteres da mensagem.

### Observações
- Esta animação da boca não funciona em todos os modelos e todas as animações. Mesmo que seu modelo tenha animações onde a boca se move, não significa que a animação da boca possa ser controlada por esta extensão. Se nada aparecer na lista de parâmetros, seu modelo provavelmente foi feito com uma versão muito antiga do Live2D para acessar os parâmetros adequadamente.

## Animações do Modelo

![UI model animations](/static/extensions/live2d-animations.png)

1. **Starter animation**
    - Selecione uma expressão e movimento das listas que tocarão ao iniciar um chat com o personagem. Você também pode adicionar um atraso durante o qual o modelo ficará invisível se precisar esconder o personagem por algum tempo para alcançar um efeito perfeito.

2. **Default animation**
    - Selecione uma expressão e movimento da lista que tocarão quando o personagem enviar uma mensagem. Use uma animação de fallback ao usar a extensão de expressão classify.

### Observações
- Animações tocarão quando você selecionar uma nas listas.
- Use o botão replay para repetir a animação selecionada.
- Alguns modelos têm expressões definidas como movimentos.
- Se nada aparecer nas listas, é provável que o arquivo de configuração do seu modelo não tenha expressões/movimentos definidos.

## Mapeamento de áreas de hit

![UI model mapping](/static/extensions/live2d-mapping.png)

1. **Default click animation**
    - Selecione uma expressão e movimento da lista que tocarão quando você clicar no modelo. Você também pode definir uma mensagem que será enviada como mensagem de usuário.

2. **Hit areas**
    - Se o modelo tiver áreas de hit, elas serão listadas, e você pode atribuir uma animação/mensagem a cada uma delas.


### Observações
- Alguns modelos não têm áreas de hit, mas o clique padrão é detectado para todos.
- O clique padrão será acionado se você clicar em uma área de hit sem nada mapeado ou se você clicar fora de qualquer área de hit.
- Áreas de hit têm prioridade definida no modelo; por exemplo, "mouth" está dentro de "head". Se não se comportar adequadamente, pode ser devido ao arquivo do modelo.
- Para alguns modelos, as animações precisam terminar antes de iniciar outra. Use a caixa de seleção de debug se quiser forçar a atualização e fazer spam de animações.

## Mapeamento de Expressões Classificadas

![UI model classify](/static/extensions/live2d-classify.png)

1. **Requirements**
    - Requer o uso da extensão classify expression; caso contrário, voltará à animação padrão.

2. **Mapping**
    - Para cada emoção detectada pela extensão classify, você pode atribuir uma animação de expressão/movimento.

### Observações
- Se a animação anterior não terminou quando uma nova mensagem é recebida, é possível que a nova animação não toque. Este comportamento depende do modelo Live2D. Use a caixa de seleção de debug se quiser forçar a animação a tocar.

Obrigado por seguir este guia! Sua experiência SillyTavern agora está enriquecida com modelos Live2D animados e interativos.
