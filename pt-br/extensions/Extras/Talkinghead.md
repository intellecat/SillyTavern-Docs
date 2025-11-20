---
route: /extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning
**O SUPORTE PARA TALKINGHEAD FOI REMOVIDO NO SILLYTAVERN 1.12.13. ESTA PÁGINA É MANTIDA PARA FINS HISTÓRICOS.**
!!!

### O que é isso?

Uma implementação do Talking Head Anime 3 Demo para AITuber. Possui os seguintes recursos:

- Gera ações de movimento Live 2D-like aleatórias a partir de uma única imagem estática.
- Sincronização labial com a saída de som de qualquer saída TTS.

Esta extensão contém os programas demo originais para o projeto Talking Head(?) Anime from a Single Image 3: Now the Body Too. Como o nome implica, o projeto permite animar personagens de anime, e você só precisa de uma única imagem desse personagem para fazer isso. Existem dois programas demo:

O manual_poser permite manipular a expressão facial de um personagem, rotação da cabeça, rotação do corpo e expansão do peito devido à respiração através de uma interface gráfica do usuário, para que você possa salvá-las como expressões padrão, ou seja, Happy, sad, joy, etc.
ifacialmocap_puppeteer permite transferir seu movimento facial para um personagem de anime.

### Requisitos de Hardware

Você pode usar os modos CPU ou GPU (CPU é padrão). No entanto, no modo CPU espere cerca de 1 FPS, e no modo GPU em uma RTX3060 estou obtendo cerca de 9-10 FPS.

O ifacialmocap_puppeteer requer um dispositivo iOS capaz de calcular parâmetros de blend shape a partir de um feed de vídeo. Isso significa que o dispositivo deve ser capaz de executar iOS 11.0 ou superior e deve ter uma câmera frontal TrueDepth. (Veja esta página para mais informações.) Em outras palavras, se você tiver o iPhone X ou algo melhor, deve estar tudo certo.

### Como usar

Você deve iniciar extras com os seguintes módulos para que o talkinghead funcione: `classify` e `talkinghead`!
classify é necessário para o manuseio do arquivo talkinghead.png. Além disso, você também pode usar `--talkinghead-gpu` para carregar os modelos de blend na memória da GPU e tornar as animações 10x mais rápidas. É altamente recomendado usar aceleração GPU! Por padrão, uma vez que o programa inicia, ele carregará uma imagem padrão SillyTavern-extras\talkinghead\tha3\images\lambda_00.png. Você pode verificar se está funcionando acessando http://localhost:5100/api/talkinghead/result_feed ou `YOUR EXT URL:PORT/api/talkinghead/result_feed`.

- Uma vez que o servidor tenha iniciado, vá para a aba Extension API e conecte. Em seguida, simplesmente selecione um card de personagem para carregar. (`--enable-modules=classify,talkinghead --talkinghead-gpu` ao iniciar server.py)

- Agora selecione Character Expressions, se você marcar a caixa de tipo de imagem talkinghead, o script substituirá sua expressão de personagem atual pelo resultado de `YOUR EXT URL:PORT/api/talkinghead/result_feed` desmarcar a caixa DEVERIA retornar a imagem de volta à expressão original, no entanto, às vezes você tem que enviar uma nova mensagem para o chat para "recarregar" a imagem.

- Se você não tiver um arquivo talkinghead.png no diretório do personagem, ele simplesmente mostrará a imagem padrão ou o último card de personagem que tinha um arquivo talkinghead.png. A imagem fonte de animação é alterada quando o card do personagem é alterado.

- Agora abra as expressões de personagem, role para baixo até a imagem talkinghead e faça upload de um arquivo de imagem que atenda aos requisitos na seção abaixo chamada "Restrições nas Imagens de Entrada".

- Em seguida, marque e desmarque a caixa talkinghead para recarregar o personagem. Se a imagem estiver engraçada, provavelmente é porque não é transparente / não tem camada alfa. Caso contrário, siga as instruções e template abaixo.

### Restrições nas Imagens de Entrada
Para que o sistema funcione bem, a imagem de entrada deve obedecer às seguintes restrições:

Deve ter resolução de 512 x 512. (Se o programa receber uma imagem de entrada de qualquer outro tamanho, ele redimensionará a imagem para esta resolução e também fará a saída nesta resolução.)
Deve ter um canal alfa.
Deve conter apenas um personagem humanoide.
O personagem deve estar em pé e voltado para frente.
As mãos do personagem devem estar abaixo e longe da cabeça.
A cabeça do personagem deve estar aproximadamente contida na caixa de 128 x 128 no meio da metade superior da imagem.
Os canais alfa de todos os pixels que não pertencem ao personagem (ou seja, pixels de fundo) devem ser 0.

![Input Constraints](/static/input_spec.png)

### SEÇÃO AVANÇADA

### Ambiente Python

Além do recurso base (app.py), tanto manual_poser quanto ifacialmocap_puppeteer estão disponíveis como aplicativos desktop. Para executá-los, você precisa configurar um ambiente para executar programas escritos na linguagem Python. O ambiente precisa ter os seguintes pacotes de software:

* Python >= 3.8
* PyTorch >= 1.11.0 com suporte CUDA
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

Uma maneira de fazer isso é instalar o Anaconda e executar os seguintes comandos em seu shell:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### Modelos de Blend Adicionais

Há apenas um modelo (mais leve) incluído, se você quiser os modelos de blend adicionais você precisa baixar os arquivos de modelo de https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 e descompactá-lo na pasta SillyTavern-extras\talkinghead\tha3\models. No final, a pasta de dados deve parecer assim:

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

Os arquivos de modelo são distribuídos com a Licença Creative Commons Attribution 4.0 International, o que significa que você pode usá-los para fins comerciais. No entanto, Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>, é o criador.

### Executando o Aplicativo Desktop manual_poser
Abra um shell. Mude seu diretório de trabalho para o diretório raiz do repositório. Em seguida, execute:

> python tha3/app/manual_poser.py
Note que antes de executar o comando acima, você pode ter que ativar o ambiente Python que contém os pacotes necessários.

> conda activate extras
se você ainda não ativou o ambiente.
