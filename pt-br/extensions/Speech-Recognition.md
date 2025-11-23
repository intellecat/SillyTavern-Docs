---
route: /extensions/speech-recognition/
---

# Reconhecimento de Voz

Este guia irá orientá-lo na configuração do reconhecimento de voz para transcrever sua voz em texto dentro do SillyTavern.

## Pré-requisitos

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos:

- Certifique-se de estar usando a versão mais recente do SillyTavern.
- Instale a extensão "Speech Recognition" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).
- Tenha o binário ffmpeg instalado. Consulte [Configuração RVC](RVC.md#rvc-setup) para mais detalhes.

## Configuração de Reconhecimento de Voz (Navegador)

1. **Configure o SillyTavern**:
   - Inicie o SillyTavern e vá para **Extensions** > **Speech Recognition**.
   - Selecione "Browser" nas opções dropdown.
   - Se seu navegador não suportar reconhecimento de voz, um popup de erro aparecerá.

2. **Selecione o Modo de Mensagem**:
   - Escolha o "Message Mode" que deseja:
     - **Append**: Sua mensagem será anexada à área de texto da mensagem do usuário atual.
     - **Replace**: Sua mensagem substituirá a mensagem do usuário atual na área de texto.
     - **Auto send**: Sua mensagem será enviada automaticamente assim que o fim da fala for detectado.

3. **Habilite o Mapeamento de Mensagens** *(Opcional)*:
   - Configure o mapeamento de frases para atalhos vocais.
   - Por exemplo, ao adicionar "command delete = /del2", o comando "/del2" substituirá sua mensagem de voz quando "command delete" for detectado.
   - Útil quando combinado com o modo de envio automático para controle de voz completo. Habilite isso marcando "Enable messages mapping".

4. **Selecione o Idioma**:
   - Escolha o idioma que deseja falar (Nota: nem todos os navegadores suportam todos os idiomas).

5. **Gravação**:
   - Para começar a gravar, clique no botão de microfone à direita da área de mensagem ao lado do botão enviar. Clique novamente para parar a gravação. A gravação pode parar automaticamente se nenhuma voz for detectada.

## Configuração de Reconhecimento de Voz (Whisper/Vosk)

1. **Habilite o Provedor**:
   - Habilite o provedor de reconhecimento de voz desejado no servidor extras usando o seguinte comando:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     ou
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - Você também pode usar um modelo personalizado adicionando a opção `--stt-vosk-model-path` ou `--stt-whisper-model-path` com o caminho para o modelo.

2. **Configure o SillyTavern**:
   - Inicie o SillyTavern e vá para **Extensions** > **Speech Recognition**.
   - Selecione "Vosk" ou "Whisper" nas opções dropdown (whisper é mais preciso).
   - As configurações são semelhantes à configuração do provedor "Browser" (exceto idioma) veja acima.

## Configuração de Reconhecimento de Voz (Streaming)

1. **Habilite o Provedor**:
   - Habilite o módulo de reconhecimento de voz em streaming no Sillytavern-extras com o seguinte comando:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **Configure o SillyTavern**:
   - (Opcional) Especifique um modelo Whisper personalizado como na configuração Whisper acima.
   - (Opcional mas recomendado) Configure palavras-gatilho no SillyTavern. Apenas mensagens começando com essas palavras-gatilho serão enviadas ao SillyTavern como mensagens reais. Isso evita que fala aleatória ou ruído seja transcrito. Habilite isso com a caixa de seleção. As palavras-gatilho podem ser incluídas/excluídas da mensagem real usando uma caixa de seleção.
   - Outras configurações são semelhantes a outros provedores.

Agora você está pronto para transcrever sua voz em texto usando reconhecimento de voz no SillyTavern.
