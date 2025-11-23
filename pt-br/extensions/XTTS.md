---
order: tts-xtts
route: /extensions/xtts/
---

# XTTS com clonagem de voz

Saudações! Então, você ficou impressionado com aqueles posts do Reddit mostrando o quão longe a tecnologia chegou para o text-to-speech de IA?

Sentindo-se animado para dar à sua waifu/husbando robótico um novo e brilhante modulador de voz?

Não tema, esta impressionante tecnologia inovadora já está disponível no seu SillyTavern local, você só precisa de um simples...

## Pré-requisitos

1. Versão mais recente do SillyTavern.
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) instalado.
3. (Windows) [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) instalado.
4. Arquivos WAV com clipes de voz para clonar (~10 segundos por arquivo). Requisitos do arquivo: PCM, Mono, 22050Hz, 16-bit (converta via Audacity).
5. Crie uma pasta com subpastas "speakers" e "output". Coloque os arquivos WAV em "speakers".

Exemplo de estrutura de pastas:
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## Instalação

[daswer123](https://github.com/daswer123) criou um servidor de API que executa o modelo XTTSv2 no seu computador e se conecta à extensão TTS do SillyTavern.

É completamente independente da API Extras e usaria um ambiente separado.

**Muito importante:** Não instale os seguintes requisitos no seu ambiente Extras ou Python do sistema.
Isso quebrará seus outros pacotes, fará downgrades desnecessários, etc.

A seguinte instrução é fornecida usando Miniconda, mas você também pode fazer isso com venv (não coberto aqui).
Abra o prompt de comando do Anaconda e siga as instruções linha por linha.

### Colocando o servidor em funcionamento

1. Navegue até a pasta que você criou no passo 4 dos pré-requisitos.
    ```
    cd C:\xtts
    ```
2. Crie um novo ambiente conda. A partir de agora, vamos chamá-lo de `xtts`.
    ```
    conda create -n xtts
    ```
3. Ative o ambiente recém-criado.
    ```
    conda activate xtts
    ```
4. Instale Python 3.10 no seu ambiente. Confirme com "y" quando solicitado.
    ```
    conda install python=3.10
    ```
5. Instale o servidor XTTS com seus requisitos.
    ```
    pip install xtts-api-server pydub
    ```
6. Instale PyTorch. Isso pode levar algum tempo. A seguinte linha instala PyTorch com suporte de aceleração GPU (CUDA).
Se você quiser usar apenas inferência de CPU, remova a última parte que começa com `--index-url`.
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. Inicie o servidor XTTS no host e porta padrão: <http://localhost:8020>
    ```
    python -m xtts_api_server
    ```
8. Durante sua primeira inicialização, o modelo será baixado (cerca de ~2 GB).
Não se esqueça de ler o aviso legal da Coqui AI com muito cuidado. Haha, estou brincando, apenas aperte "y" novamente.

### Conectando ao SillyTavern

1. Abra o painel de extensões, expanda o menu TTS e escolha "XTTSv2" na lista de provedores.
2. Escolha seu idioma de text-to-speech no dropdown Language (ficarei triste se não for polonês).
3. Verifique se o endpoint do provedor aponta para <http://localhost:8020> e "Available voices" mostra uma lista de suas amostras de voz.
4. Escolha qualquer personagem e defina um mapeamento entre a amostra de voz e o personagem.
Se a lista de personagens estiver vazia, clique em "Reload" algumas vezes.
5. Configure o restante das configurações de TTS de acordo com suas preferências.

### Está tudo pronto agora!

Clique no ícone de megafone no menu de ações de contexto para qualquer mensagem e ouça a bela voz clonada emanando
de seus alto-falantes. A geração leva algum tempo e não é em tempo real mesmo em GPUs RTX de última geração.

### Streaming?

É possível usar streaming HTTP com a versão mais recente do servidor XTTS para obter os pedaços de áudio gerado assim que estiver disponível!

#### Isso não funciona com RVC!

O áudio ainda será gerado (assumindo que você está usando a versão mais recente da extensão RVC) e convertido, *mas não transmitido em streaming* pois o RVC requer ter o arquivo de áudio completo antes de iniciar a conversão. Streaming com RVC ainda está sendo investigado...

#### Como obter suporte de streaming?

1. Atualize o SillyTavern para a versão mais recente.
2. Atualize o servidor XTTS para a versão mais recente.

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. Inicie e conecte o XTTS ao ST como de costume.
4. Habilite a configuração "Streaming" da extensão XTTS no SillyTavern.

### Áudio entrecortado?

Tente aumentar a configuração "chunk size".

Para referência: com um chunk size de 200, a RTX 3090 pode produzir áudio ininterrupto ao custo de uma latência de áudio ligeiramente aumentada.

### Como reiniciar o servidor TTS?

Apenas faça os passos 1, 3 e 7 da instrução de instalação.

### Android??

Improvável, não pode executar aplicativos que requerem PyTorch sem alguma magia negra arcana para a qual não fornecemos suporte. Você pode tentar por sua conta e risco, mas nenhum suporte será fornecido se você enfrentar problemas.

Sua melhor solução é hospedar a API TTS no seu PC pela rede local, apenas não se esqueça de especificar o host e a porta para escutar - veja [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md).
