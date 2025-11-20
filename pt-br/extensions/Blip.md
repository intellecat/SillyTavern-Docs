---
route: /extensions/blip/
---

# Blip

Este guia irá orientá-lo na configuração e personalização da extensão blip para sua experiência SillyTavern. Esta extensão anima o texto de mensagens com velocidade variável e reproduz som junto com a animação. Você pode usar arquivo de áudio ou gerar o som.

## Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

- Certifique-se de estar usando a versão mais recente do SillyTavern.
- Instale a extensão "Blip" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).

## Configurações globais do Blip

1. **Blip user message**:
   - Habilite a caixa de seleção para reproduzir animação em mensagem do usuário.
   - Defina um perfil para o usuário ou um perfil padrão se quiser animação blip para o usuário.

2. **Blip only for certain text**:
   - Habilite a caixa de seleção para fazer blip apenas para texto dentro de aspas.
   - Habilite a caixa de seleção para ignorar tudo dentro de asteriscos.

3. **Automatic scroll down**:
   - Habilite a caixa de seleção para fazer o chat descer para seguir a animação do texto, desabilite-a se quiser rolar livremente durante a animação.

4. **Audio volume**
   - Silencie o áudio se apenas a animação do texto for desejada.
   - Você pode ajustar o volume global do áudio blip.

## Perfil de animação/voz do personagem

Você pode salvar um perfil para cada personagem:
   - incluindo o usuário e um perfil padrão opcional que será usado quando o personagem não tiver perfil.
   - Se apenas os personagens do chat atual forem mostrados na lista, clique na caixa de seleção para mostrar todos os seus personagens.

1. **Selecione o personagem para atribuir/atualizar perfil**:
   - Selecione um personagem, se ele tiver um perfil, ele será carregado.
   - Se ele não tiver um perfil ainda, os parâmetros atuais se tornarão suas configurações de perfil.
   - Qualquer perfil pode ser excluído usando o botão remove.
   - Use o botão refresh se seu personagem não aparecer na lista.

2. **Text animation settings**:
   - Defina a velocidade do texto: o atraso em milissegundos entre cada letra impressa.
   - Defina multiplicador de velocidade Min/max diferente de 1.0 para aleatoriedade da animação de velocidade.
   - Defina atraso de vírgula/frase superior a 0 para adicionar uma pausa quando caracteres especiais são impressos, pode adicionar mais vivacidade à animação. O áudio também é pausado neste caso.

3. **Audio parameters**:
   - Defina um multiplicador de volume que afetará apenas este perfil de voz se necessário.
   - Defina velocidade de áudio: o atraso entre cada som blip, independente da velocidade do texto.

4. **Blip origin: Generated sound**:
   - Use os controles deslizantes de frequência min/max para personalizar o som blip reproduzido.
   - Se min/max forem diferentes, um som aleatório nesta faixa é reproduzido cada vez.

5. **Blip origin: file**:
   - Escolha um arquivo na lista.
   - Você pode obter assets blip oficiais do ST no menu de extensão assets.
   - Ou colocar arquivo diretamente em: `\SillyTavern\data\<user-handle>\assets\blip`.
   - Habilite a caixa de seleção para forçar a esperar que todo o arquivo seja reproduzido antes de reproduzir novamente se necessário.

Obrigado por seguir este guia! Sua experiência SillyTavern agora está enriquecida com animação de texto e vozes blip.
