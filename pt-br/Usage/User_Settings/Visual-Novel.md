---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /usage/user-settings/visual-novel/
---

# Visual Novel (VN) Mode

O Visual Novel Mode é um layout de tela especial no SillyTavern que permite conversar com personagens usando sprites (ou a imagem do cartão de personagem) que se assemelha a uma visual novel como Doki Doki Literature Club, The Fruits of Grisaia, Fate: Stay/night e outros famosos jogos VN.

## Alternando Visual Novel Mode

### Habilitando Visual Novel Mode

O Visual Novel Mode vem integrado ao SillyTavern e pode ser alternado indo em *User Settings* (ícone User Settings) e marcando **Visual Novel Mode** abaixo de *No Text Shadows*.

![User Settings](/static/vn/vn-mode-toggle.png)

### Desabilitando Visual Novel Mode

Desabilitar o Visual Novel Mode são os mesmos passos de habilitá-lo. Desmarque Visual Novel Mode e você deve voltar à tela de chat normal.

!!!warning Sobre VN Mode com Extensões VN
Algumas extensões (como a Prome VN Extension) alternarão 'Visual Novel Mode' se você usar seus próprios modos VN respectivos. Habilitar/Desabilitar VN Mode no menu *User Settings* também afetará essas extensões.
!!!

## A Interface do Visual Novel

![VN Display](/static/vn/vn-display.png)

No Visual Novel Mode, a interface é ligeiramente alterada para acomodar sprites de personagens (ou a imagem do cartão de personagem) que são mostrados no centro. Em um chat em grupo com múltiplos personagens, no entanto, os sprites de personagens se espalharão, acomodando uns aos outros como mostrado abaixo.

![Group VN Display](/static/vn/group-vn-display.png)

### VN Mode com MovingUI

!!!info
Para alternar MovingUI, vá em *User Settings* e marque **MovingUI**. Note que este recurso **apenas** funciona em Desktops.
!!!

Se **MovingUI** estiver habilitado em *User Settings*, os sprites (ou imagem do cartão de personagem) podem ser movidos se você desejar movê-los ou colocá-los em uma área mais específica na tela.

!!!warning Sobre Tamanhos de Sprites
Se o tamanho dos sprites do seu personagem for relativamente grande, será um desafio tentar mover certos sprites com MovingUI, pois o botão para arrastar sprites pode estar coberto embaixo de um sprite existente. Você provavelmente terá que movê-los um pouco mais do que o normal, especialmente se houver mais personagens na tela para melhor posicionamento.
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## Obtendo Sprites de Personagens

Obter sprites de personagens pode ser feito navegando na internet por sprites existentes, por exemplo, de um personagem existente de uma Visual Novel ou um jogo que usa um recurso de Visual Novel como DDLC ou CounterSide. Se o personagem que você deseja sprites não vier com sprites já, você tem várias opções restantes.

1. Procure no post do personagem por qualquer pacote ZIP de sprites ou link para um pacote de sprites.
    !!!info
    Alguns criadores de bots podem lançar seus bots com um pacote de sprites (seja dentro do mesmo post ou em um canal de sprites). Procure nesses posts se alguém já não fez sprites do personagem que você quer.
    !!!
2. Crie os seus próprios usando LoRAs e Stable Diffusion.
    !!!warning
    Gerar sprites do zero é demorado (especialmente se não existem LoRAs para seu personagem e/ou para o modelo Stable Diffusion que você quer usar) e exigirá hardware decente para gerá-los, mais ainda se você planeja fazer 28 expressões de sprite do que 6 e se estiver usando SDXL e/ou fazendo upscaling de sprites para uma resolução mais alta.
    !!!
3. Use a imagem do cartão de personagem. Pode não ser como um sprite, mas pelo menos você terá algo para olhar na tela. No entanto, múltiplos cartões de personagem não podem ser usados no modo VN.
    !!! Imagens de Cartão de Personagem com a Prome Visual Novel Extension
    Com a Prome Visual Novel Extension 1.0.6+, há um recurso chamado `Emulate Character Card as Sprite` que permite ter um chat em grupo com personagens com e sem sprites usando o cartão de personagem como sprite no chat.

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## Extensões VN

### Prome Visual Novel Extension

A Prome Visual Novel Extension é uma extensão de terceiros endossada de Bronya Rand e Prometheus que aprimora a experiência de visual novel no SillyTavern ainda mais com recursos como Letterbox Mode que torna a interface de visual novel mais "cinemática", Focus Mode com Darken Character Sprites, Traditional VN Mode onde apenas a última mensagem no chat aparece no chat e mais planejado para vir!

|                              Letterbox Mode                              |                          Traditional VN Mode                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Hide Sheld (Message Box)                  |                      Focus Mode (w/ Darken Sprites)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

Para instalar a Prome Visual Novel Extension, você pode instalar indo em `Download Extensions & Assets` e encontrando *Prome Visual Novel Extension*, ou seguir as instruções de instalação na página do Github da [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage). Ajustar as configurações da Prome pode ser encontrado em *Extensions* -> **Prome (Visual Novel Extension)** ou através do menu 🪄 (Wand).
