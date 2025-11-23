---
route: /extensions/rvc/
---

# Conversão de Voz Baseada em Recuperação (RVC)

Este guia irá orientá-lo no uso do RVC, uma técnica que permite transferir características de voz de um clipe de áudio para outro, permitindo que vozes falem em tons e estilos diferentes.

Já curtiu aqueles famosos vídeos "Presidents Play X"? Eles foram criados usando RVC. Com a extensão RVC, você pode fazer seus personagens do SillyTavern falarem com qualquer voz que desejar, seja anime, filme ou até sua própria voz única.

RVC NÃO é TTS: é mais como fala-para-fala. Ele recebe um clipe de áudio como entrada. Em segundo plano, o que o RVC faz é trabalhar em conjunto com a extensão TTS do SillyTavern: ele aguarda o TTS gerar um arquivo de áudio (que o TTS faria independentemente de você usar RVC ou não), então o RVC fará uma segunda passagem que pega o arquivo de áudio TTS e o transforma na voz clonada da sua configuração RVC.

## Configuração do RVC

O RVC do SillyTavern suporta várias fontes de API que executam conversão de áudio:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (descontinuado)

### Pré-requisitos comuns

Antes de começar, certifique-se de ter atendido aos seguintes pré-requisitos.

#### ffmpeg

Certifique-se de ter o binário `ffmpeg` em sua variável de ambiente PATH. Esta ferramenta é usada para converter áudio de entrada.

**Windows**:

* Use o Toolbox no script SillyTavern Launcher para instalar o ffmpeg automaticamente: <https://github.com/SillyTavern/SillyTavern-Launcher>
* Ou baixe a compilação aqui: <https://www.gyan.dev/ffmpeg/builds/>
* Como modificar a variável PATH: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* Para testar se você fez as coisas corretamente, abra um prompt de comando e execute ```ffmpeg```. Deve imprimir a versão do ffmpeg e informações.

**Linux**:

Instale o ffmpeg usando seu gerenciador de pacotes.

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

Instale o ffmpeg usando [Homebrew](https://brew.sh/):

```shell
brew install ffmpeg
```

#### Certifique-se de que o TTS está habilitado e funciona

RVC depende do TTS, você precisa habilitar uma extensão TTS. Seu TTS precisa estar funcionando corretamente e narrando seus chats antes de tentar adicionar RVC à mistura!

Observe que:

* O mecanismo TTS do sistema não suporta conversão de voz.
* TTS em streaming aguardará o fim do fluxo de áudio antes da conversão.

#### Instale a extensão

Instale a extensão "RVC" no menu "Download Extensions & Assets" no painel Extensions (ícone de blocos empilhados).

#### Habilite o RVC no SillyTavern

No SillyTavern, navegue até **Extensions** > **RVC** e habilite-o.

#### Escolha a fonte

Nas configurações da extensão, escolha uma fonte RVC para usar. Em seguida, prossiga para as instruções de instalação específicas da fonte.

### Configuração do rvc-python

#### 1. Instale o pacote

Siga as instruções de instalação da página do GitHub: [Instalação rvc-python](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation). É recomendado seguir as instruções de instalação CUDA se você tiver uma GPU Nvidia.

Se estiver tendo problemas ao instalar no Windows (por exemplo, a etapa de compilação do fairseq falha), certifique-se de que o seguinte software esteja instalado no seu PC:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. Prepare os modelos

Crie um diretório para armazenar modelos RVC. Por padrão, é chamado `rvc_models` e é obtido do seu diretório atual ao iniciar o servidor. Cada modelo é uma subpasta (seu nome será visível na UI) que deve conter arquivos `.pth` (obrigatório) e `.index` (opcional).

Leia mais: [Gerenciamento de Modelos rvc-python](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. Inicie o servidor API

Inicie o servidor API executando o seguinte comando:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**Argumentos:**

* `5050` - define uma porta de escuta para o servidor. Altere se quiser hospedar em uma porta diferente.
* `models_path` - define um caminho para modelos. Remova se quiser usar o diretório padrão `rvc_models`.
* `-l` - define o servidor para escutar em todas as interfaces de rede. Remova para escutar apenas no localhost.

#### 4. Conecte-se ao servidor

* Nas configurações da extensão RVC, defina uma **rvc-python API URL** apropriada. Por padrão, será `http://localhost:5050`.
* Marque a caixa **Use CUDA** se você instalou o rvc-python para suportar aceleração CUDA.
* Pressione "Refresh" para carregar uma lista de vozes disponíveis.

#### 5. Configure um mapa de voz

**O mapa de voz define configurações de conversão de voz para cada personagem ou persona de usuário.**

* Para configurar um mapa de voz, escolha o nome do seu personagem ou persona no dropdown "Character", depois escolha uma "Voice" RVC e clique em Apply.
* Opcionalmente, você também pode configurar outras configurações relacionadas, como correção de pitch ou filtragem.
* Se você fez tudo corretamente, a área de debug do Voice Map mostrará algo como 'Betty:MyVoice(rvpme)'.

### Configuração do SillyTavern Extras

#### 1. Prepare os Arquivos de Modelo RVC

* Em um navegador de arquivos, navegue até: `\SillyTavern-extras\data\models\rvc`.
* Crie uma subpasta como 'Betty' e coloque os arquivos `.pth` e `.index` nela. (Dica: você pode baixar arquivos de voz de https://voice-models.com, certifique-se de que o nome da voz diga que é RVPME.)

#### 2. Instale os Requisitos

Instale os requisitos necessários usando o comando:

```shell
pip install -r requirements-rvc.txt
```

#### 3. Execute o SillyTavern-extras com RVC habilitado

Inicie o SillyTavern-extras com o módulo RVC habilitado. Esta invocação de exemplo assume que você usou Edge TTS, que vem pré-instalado com SillyTavern-extras:

```shell
python server.py --enable-modules=rvc,edge-tts
```

Opcionalmente, você pode desejar executar RVC na sua GPU se tiver uma compatível, adicionando ```--cuda``` ao comando de inicialização. Com base em um teste rápido, o uso de VRAM foi de 3,4GB para narrar 50 tokens (~36 palavras) e 7,6GB para 200 tokens (~150 palavras).

#### 4. Configure o Mapeamento de Voz

Crie um mapa de voz para RVC. Defina seu Character para o nome do personagem desejado do SillyTavern e defina Voice para a pasta RVC que você criou na etapa 1, depois clique em Apply. Se você fez as coisas corretamente, o Voice Map mostrará algo como 'Betty:MyVoice(rvpme)'.

#### 5. Selecione a Extração de Pitch

* Escolha "rmvpe" como método de extração de pitch.
* Se você tiver problemas com "rmvpe", tente outros métodos (por exemplo, "harvest" ou "torchcrepe").

#### 6. (Opcional) Configure o RVC para salvar suas gerações em arquivo

Se para fins de teste ou solução de problemas você deseja salvar o áudio RVC gerado, adicione ```--rvc-save-file``` ao seu comando de inicialização. Isso salvará a última geração em `SillyTavern-extras/data/tmp/rvc_output.wav`:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### Voz Dinâmica Baseada em Expressão

##### 1. Configure Modelos RVC

Na sua pasta de modelo RVC, tenha arquivos `.pth` e `.index` separados para cada expressão classificada (por exemplo, anger, fear, joy, love, sadness, surprise).

##### 2. Habilite Módulos

Habilite os módulos RVC e classify:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. Use o Módulo RVC

A configuração restante é semelhante ao uso do módulo RVC sozinho (conforme explicado acima).

## Treine Seu Próprio Modelo RVC

### Usando RVC Easy Menu por Deffcolony (Apenas Windows)

Instale e inicie automaticamente o Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

#### 1. Clone o Repositório

Clone o repositório no local desejado:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. Inicie o RVC-Launcher.bat

* Abra o arquivo `RVC-Launcher.bat`.
* Escolha a opção 1 para instalar o RVC.

#### 3. Complete a Instalação

Quando solicitado, instale pacotes e dependências necessários.

#### 4. Abra o WebUI para Treinamento de Voz

Após a instalação, escolha a opção 2 para abrir o WebUI para treinamento de voz.

### Mangio-RVC: Treinando um Modelo de Voz

***Preparação do Dataset***:

**1. Prepare o Áudio**:

* Coloque o áudio que deseja treinar na pasta `datasets`.
* Certifique-se de que o áudio esteja livre de ruído de fundo – apenas voz bruta é necessária.
* Áudio mais longo resulta em melhor qualidade de saída.

***Treinamento no WebUI***:

**1. Acesse a Aba de Treinamento**:

* Clique na aba de treinamento no WebUI.

**2. Configure o Experimento**:

* Insira um nome de experimento (por exemplo, `my-epic-voice-model`).
* Defina a versão como v2.

**3. Processe Dados e Extraia Recursos**:

* Clique em "Process data" e "Feature extraction".
* Defina "Save frequency" como 50.

**4. Parâmetros de Treinamento**:

* Defina "Total training epochs" como 300.
* Clique em "Train feature index" e "Train model".
