---
route: /extensions/dynamic-audio/
---

# Dynamic Audio

Este guia irá orientá-lo na configuração e personalização de assets de áudio dinâmico para sua experiência SillyTavern.

## Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

- Certifique-se de estar usando a versão mais recente do SillyTavern.
- Instale a extensão "Dynamic Audio" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).

## Configuração de Dynamic Audio (Navegador)

1. **Conecte-se ao Repositório de Assets**:
   - Inicie o SillyTavern e navegue até **Extensions** > **Assets**.
   - Clique no botão "Connect" para estabelecer uma conexão com o repositório oficial de assets.
   - Baixe os assets de áudio desejados, como música de fundo (BGM) ou sons ambientes, que correspondam aos planos de fundo que você pretende usar.

2. **Habilite a Extensão Dynamic Audio**:
   - No SillyTavern, vá para **Extensions** > **Dynamic Audio**.
   - Habilite a extensão, tire o mudo e ajuste o volume de BGM e sons ambientes para sua preferência.
   - Quando o bgm terminar, outro tocará aleatoriamente, clique no botão de loop para manter o bgm atual tocando
   - Clique no botão de roll para escolher outro bgm aleatoriamente

3. **BGM baseado em Expressão**:
   - Habilite o switch de BGM de expressão se quiser que o bgm siga a expressão do personagem (requer bgm na pasta do personagem veja abaixo).
   - Ajuste o temporizador de cooldown (em segundos) entre atualizações de BGM. Aumente-o se achar que o BGM muda muito frequentemente em chats em grupo ou ao usar BGM específico do personagem com detecção de emoções.

## Importando Música para Personagens

Para configurar música personalizada para as emoções de seus personagens, siga estas etapas:

1. **Navegue até a Pasta do Personagem**:
   - Vá para a pasta de personagens, por exemplo, `\SillyTavern\data\<user-handle>\characters\Seraphina`.

2. **Crie a Pasta BGM**:
   - Dentro da pasta do personagem, crie uma subpasta chamada `bgm`.

3. **Importe Música de Emoções**:
   - Dentro da pasta `bgm`, importe os arquivos de música para cada emoção. Extensões de áudio suportadas incluem `.mp3`, `.ogg` e `.wav`.
   - Convenção de nomenclatura: `[emotion]_[number].mp3`, por exemplo, `anger_0.mp3`, `joy_0.mp3`.

4. **Múltiplas Faixas para Emoções**:
   - Você pode importar múltiplas faixas para a mesma emoção incrementando o número, por exemplo, `neutral_1.mp3`, `neutral_2.mp3`.

5. **Seleção de Música Padrão**:
   - Quando nenhuma emoção é detectada, uma faixa neutral aleatória tocará como padrão. Emoções são detectadas de forma semelhante à atualização de sprites; consulte a [documentação de expression images](/extensions/Expression-Images.md) para detalhes.

## Alterando a Música BGM Padrão

Se um personagem não tiver BGM personalizado em sua pasta, uma faixa padrão tocará. Veja como você pode alterá-la:

1. **Navegue até a Pasta BGM**:
   - Vá para a seguinte pasta: `\SillyTavern\data\<user-handle>\assets\bgm`.

2. **Substituir/Adicionar Música**:
   - Substitua ou adicione arquivos de música (`.mp3`, `.ogg`, `.wav`) a esta pasta.
   - Estes são os assets de áudio oficiais baixados usando a extensão assets.
   - Uma dessas faixas tocará aleatoriamente quando nenhum BGM específico do personagem for encontrado (chat solo ou em grupo).

## Alterando Sons Ambientes

Sons ambientes adicionam profundidade às suas cenas. Veja como você pode personalizá-los:

1. **Navegue até a Pasta Ambient**:
   - Vá para a seguinte pasta: `\SillyTavern\data\<user-handle>\assets\ambient`.

2. **Convenção de Nomenclatura de Arquivos**:
   - Nomes de arquivos de áudio ambiente correspondem aos nomes de arquivos de imagem de fundo, substituindo espaços por traços.
   - Exemplo: `"bedroom-clean.mp3"` corresponde ao plano de fundo "bedroom clean.jpg".
   - Se o botão de bloqueio estiver desbloqueado, o arquivo de áudio correspondente ao plano de fundo tocará. Ativar o bloqueio manterá o som ambiente atual tocando.

3. **Ambients Personalizados**:
   - Você pode adicionar seus próprios sons ambientes para planos de fundo personalizados ou existentes seguindo o mesmo padrão de nomenclatura.

Obrigado por seguir este guia! Sua experiência SillyTavern agora está enriquecida com áudio dinâmico.
