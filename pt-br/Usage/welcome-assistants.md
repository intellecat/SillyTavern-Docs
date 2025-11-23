---
tags: ['>=1.13.0']
icon: people
route: /usage/welcome-assistants/
---

# Assistentes da Página de Boas-Vindas

O SillyTavern apresenta uma Tela de Boas-Vindas que pode recebê-lo com um personagem "Assistente" designado. Esta tela aparece quando você inicia o SillyTavern sem um chat ativo ou depois de fechar sua última sessão de chat.

!!! Note
Se você não vê uma Tela de Boas-Vindas na inicialização do aplicativo, certifique-se de que a opção "Auto-Load Last Chat" esteja desabilitada na seção "Chat/Message Handling" do painel **<i class="fa-solid fa-user-cog"></i> User Settings**. Se esta opção estiver habilitada, o SillyTavern carregará automaticamente seu último chat em vez de mostrar a Tela de Boas-Vindas.
!!!

## A Tela de Boas-Vindas

Quando nenhum chat está ativo, a Tela de Boas-Vindas fornece vários elementos úteis:

* **Versão do SillyTavern:** Exibe o logotipo do aplicativo e a versão atual.
* **Links Rápidos:** Acesso fácil a:
  * **Docs:** Abre a documentação oficial do SillyTavern (você já está aqui!).
  * **GitHub:** Leva você ao repositório GitHub do SillyTavern (<https://github.com/SillyTavern/SillyTavern>).
  * **Discord:** Fornece um link para o servidor oficial do Discord do SillyTavern (<https://discord.gg/sillytavern>).
* **Botão Temporary Chat:** Permite que você inicie rapidamente uma nova sessão de chat temporária com o assistente neutro padrão, que não será salvo no seu histórico de chat a menos que você o salve explicitamente.
* **Seção Recent Chats:** Lista suas conversas recentes para acesso rápido. Você pode:
  * Mostrar ou ocultar esta seção.
  * Expandir a lista se mais de 3 chats estiverem disponíveis (até 15 chats recentes).

## Chat Temporário

!!! Note
Devido a uma limitação técnica, o recurso Temporary Chat não usará seu Assistente da Página de Boas-Vindas personalizado. Ele sempre iniciará um chat vazio sem prompts adicionais ou informações de personagem.
!!!

O botão Temporary Chat permite que você inicie rapidamente uma nova sessão de chat sem salvá-la no seu histórico de chat. Isso é útil para testes ou conversas casuais sem bagunçar seus chats salvos. Este chat será excluído assim que você o fechar ou mudar para outro chat.

* O botão **Save** permitirá que você exporte o chat temporário como um arquivo JSONL, que você pode importar depois.
* O botão **Load** permitirá que você restaure um arquivo de chat temporário salvo anteriormente.

## O que é um Assistente da Página de Boas-Vindas?

Um Assistente da Página de Boas-Vindas é um personagem que você escolhe para ser apresentado na Tela de Boas-Vindas. Isso permite uma saudação personalizada e uma maneira rápida de iniciar um chat com um personagem familiar desde o início.

### Definindo e Removendo um Assistente

Você pode escolher qualquer um dos seus personagens para atuar como seu Assistente da Página de Boas-Vindas.

**Para definir um assistente:**

1. Navegue até o painel **Character Management** (geralmente encontrado na barra lateral direita através do ícone <i class="fa-solid fa-address-card"></i>).
2. Encontre o personagem que deseja definir como seu assistente na lista.
3. Clique em "More..." e selecione **"Set / Unset as Welcome Page Assistant"** no menu suspenso.
4. Um pequeno ícone (<i class="fa-solid fa-user-graduate"></i>) aparecerá ao lado do nome do personagem, indicando que agora ele é seu Assistente da Página de Boas-Vindas ativo.

**Para remover um assistente:**

1. Vá para o painel Character Management.
2. Localize seu Assistente da Página de Boas-Vindas atual (ele terá o ícone <i class="fa-solid fa-user-graduate"></i>).
3. Clique em "More..." e selecione **"Set / Unset as Welcome Page Assistant"** novamente.
4. O personagem não será mais seu assistente, e o ícone <i class="fa-solid fa-user-graduate"></i> desaparecerá.
5. O SillyTavern reverterá para usar o Assistente Padrão (veja abaixo).

### Interagindo com o Assistente

Uma vez que a Tela de Boas-Vindas seja exibida com seu assistente escolhido, simplesmente digite sua mensagem na barra de entrada de chat na parte inferior da tela e pressione Enter ou clique no botão send. Isso iniciará uma nova sessão de chat com seu Assistente da Página de Boas-Vindas.

Para abrir um chat anterior com o assistente, use a seção Recent Chats ou encontre o chat no diálogo **Manage chat files** (acessível através do menu **<i class="fa-solid fa-bars"></i> Options**).

## Assistente Padrão

O SillyTavern criará automaticamente um personagem padrão chamado "Assistant" quando você interagir com a Tela de Boas-Vindas pela primeira vez. Este personagem serve como uma opção substituta se você não tiver definido um personagem específico como seu Assistente da Página de Boas-Vindas.

O assistente padrão não tem prompts específicos anexados a ele, e você é livre para personalizá-lo como quiser (por exemplo, renomear, adicionar uma imagem ou definir uma personalidade).

* Se você não tiver definido explicitamente um personagem como seu Assistente da Página de Boas-Vindas, este assistente padrão será usado.
* Se você remover seu assistente escolhido, o sistema reverterá para este assistente padrão.
* Se um personagem que você definiu como assistente for excluído, o sistema também reverterá para o assistente padrão.

**Nota:** Você não pode "remover" o assistente padrão do sistema da mesma maneira que remove um personagem que você escolheu. Para mudar do assistente padrão, você deve definir um dos seus outros personagens como o assistente.
