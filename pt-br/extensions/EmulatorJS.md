---
route: /extensions/emulatorjs/
templating: false
---

# EmulatorJS

Esta extensão permite que você jogue jogos de console retrô diretamente do chat do SillyTavern.

## Instalação

**Pré-requisitos:**

- Versão de lançamento mais recente do SillyTavern.
- Arquivos ROM baixados da internet. Você pode encontrá-los [em qualquer lugar](https://archive.org/details/ni-romsets).

**Como instalar:**

1. Instale usando o downloader de extensões do SillyTavern.
2. Ou use este link: `https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## Uso

- Abra o menu de extensão "EmulatorJS".
- Clique em "Add ROM file". ROMs são salvos no armazenamento do seu navegador e não são armazenados em um servidor.
- Selecione o arquivo do jogo para adicionar. Insira o nome e o core (se não foi detectado automaticamente). Se o core requer um arquivo BIOS, adicione-o também.
- Clique no botão "Play" na lista ou inicie via menu varinha.
- Você pode personalizar controles e outras configurações no quadro do emulador após iniciar o jogo.
- Use as funções save/load state se precisar fazer uma pausa.

Confira a documentação do EmulatorJS para ver a lista de cores disponíveis e seus requisitos: [Cores](https://emulatorjs.org/docs4devs/cores).

## Modo de comentários

Com o poder de modelos multimodais, seus bots de IA podem ver seu gameplay e fornecer comentários espirituosos e característicos.

### Requisitos

1. Um navegador que suporte [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility). Testado no Chrome desktop. Firefox requer habilitá-lo com config. Safari não funcionará.
2. API de Chat Completion com modo de inlining de imagem é recomendado. Verifique a documentação da API para ver se o modelo escolhido suporta prompts multimodais.
3. Se o inlining de imagem estiver desabilitado, certifique-se de que a extensão [Image Captioning](./captioning.md#multimodal-source) esteja habilitada, depois selecione a fonte de captioning "Multimodal".

### Como habilitar comentários

1. Certifique-se de definir o intervalo de fornecimento de comentários nas configurações da extensão EmulatorJS. Esta configuração define com que frequência o personagem é consultado para comentários usando uma imagem do seu gameplay atual. Um valor de 0 indica que nenhum comentário é fornecido.
2. Selecione um chat de personagem e inicie o jogo. Para melhor desempenho, certifique-se de que o arquivo ROM esteja devidamente nomeado para que a IA possa ter mais contexto de fundo.
3. Comece a jogar como normalmente faria. O modelo de visão será consultado periodicamente para escrever um comentário baseado na última captura de tela que ele "vê".

### Configurações

1. Caption template - um prompt usado para descrever a captura de tela do jogo. Macros adicionais `{{game}}` e `{{core}}` são suportadas.
2. Comment template - um prompt usado para escrever um comentário baseado na legenda gerada. Macros adicionais `{{game}}`, `{{core}}`, `{{caption}}` são suportadas. Para modo de inlining de imagem, `{{caption}}` é substituído por `see included image`.
3. Force captions - forçará o uso de captioning multimodal mesmo se o inlining de imagem for suportado e habilitado.

### Por que não estou vendo comentários?

Comentários são pausados temporariamente (etapa de intervalo ignorada) se:

1. O emulador está pausado (com botão de pausa, não no jogo).
2. A janela do navegador está fora de foco.
3. A área de entrada do usuário não está vazia. Isso é para deixá-lo digitar sua resposta em paz.
4. Outra geração de resposta está em andamento.
5. A voz TTS está sendo lida em voz alta. O comentário é adiado (máximo de 30 segundos) até que termine, mas não é ignorado.
6. Um card de personagem ou grupo está atualmente aberto. O modo de comentário é desabilitado ao iniciar o jogo de uma tela de boas-vindas.

Outros problemas comuns:

1. Certifique-se de ter definido um intervalo de comentários antes de iniciar o jogo.
2. Certifique-se de ter definido uma chave de API multimodal e não há erros no console do servidor ST.

Ainda não funciona? Envie-nos os logs do console de debug do seu navegador (pressione F12).

## Créditos

- Motor EmulatorJS (GPLv3): <https://github.com/EmulatorJS/EmulatorJS>
