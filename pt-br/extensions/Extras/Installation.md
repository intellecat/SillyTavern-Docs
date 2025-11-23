---
icon: gear
label: Local Installation
route: /extensions/extras/installation/
---

# Instalação do Extras

Esta página contém instruções para instalar o SillyTavern Extras em seu dispositivo local.

!!! Discontinued
O projeto Extras foi descontinuado em abril de 2024 e não receberá novas atualizações ou módulos. A grande maioria dos módulos está disponível nativamente no aplicativo principal SillyTavern. Você ainda pode instalá-lo e usá-lo, mas não espere obter suporte imediato se enfrentar algum problema.
!!!

A instalação local do Extras pode ser difícil ou impossível no seu sistema operacional (especialmente Termux).

## Use o [Colab Oficial do Extras](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* Simples de configurar
* Gratuito para usar
* Não requer créditos de GPU Colab (use as opções `use_cpu`)
* Veja a [Página do Guia Colab](/extensions/Extras/Installation.md#running-extras-in-colab) para detalhes.

### Executando Extras no Colab

* Abra o [Colab Oficial do Extras](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* Selecione as opções "Extra" desejadas
* selecione `use_cpu` para executar Extras sem requerer crédito de GPU
  * isso tornará o Stable Diffusion mais lento, mas tudo mais rodará normalmente
* Não obrigatório, mas recomendado: selecione a opção `secure` para gerar a chave API para proteger sua instância compartilhada.
* Clique no botão Start à esquerda (parece um botão de 'play' triangular)
* Aguarde o carregamento de tudo
* Procure o link `trycloudflare.com` na parte inferior da saída. Ignore o link localhost, ele não funcionará (nós tentamos!).
* Começará com o texto `Running on`
* Copie o link da URL da API listado abaixo dessa linha. (**NÃO copie a URL 'localhost', use a outra**)
* Inicie SillyTavern com suporte a extensões: (defina `enableExtensions` como `true` em seu `config.yaml` se necessário)
* Navegue até o menu Extensions do SillyTavern (clique no ícone 'blocos empilhados' no topo da página).
* Cole a URL da API na caixa no topo. (**NÃO na caixa API Key**)
* Se você NÃO habilitou a opção `secure`, certifique-se de que a caixa API Key esteja completamente vazia ao usar o colab oficial.
* Se você habilitou a opção `secure`, cole a chave API gerada na caixa API Key.
* A chave API aparecerá na saída do console do colab, por exemplo: `Your API key is fee2f3f559`
* Clique em "Connect"

---

## Métodos de Instalação Local

### MiniConda (recomendado)

Este método é recomendado porque o Conda cria um 'ambiente virtual' para os pacotes de requisitos do Extras viverem, então eles não afetam sua configuração Python em todo o sistema.

1. Instale [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(Importante!) Leia [como usar Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. Instale [git](https://git-scm.com/downloads)

    _(Chads que instalaram SillyTavern com git para começar podem pular esta etapa!)_

    Depois de ter ambos instalados...

    Digite/cole os comandos abaixo `UM POR VEZ` NA `JANELA DE PROMPT DE COMANDO CONDA` e pressione `Enter` após cada um.

3. Crie um novo ambiente Conda (vamos chamá-lo de `extras`):

    `conda create -n extras`

4. Ative o novo ambiente

    `conda activate extras` (você deve ver `(extras)` aparecer no lado esquerdo do seu prompt de comando)

5. Instale os pacotes de sistema necessários (isso levará algum tempo)

    `conda install python=3.11 git`

6. Clone o repositório GitHub do Extras

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. Navegue até seu repositório Extras clonado

    `cd SillyTavern-extras`

8. Instale os requisitos do Extras usando **um** dos seguintes comandos (levará tempo, novamente):

   * `pip install -r requirements.txt` - para recursos básicos
   * `pip install -r requirements-rvc.txt` - para clonagem de voz em tempo real
   * `pip install -r requirements-coqui.txt` - para Coqui TTS (não recomendado)

    Veja a página [Problemas Comuns](/extensions/Extras/Installation.md#extras-install-common-problems) se você receber erros nesta etapa!

9. Veja abaixo 'Executando Extras Após Instalação'

---

### Instalação em Todo o Sistema

Isso é mais fácil, mas afetará sua instalação Python em todo o sistema.

Isso pode causar conflitos se você trabalhar com muitos programas Python que têm requisitos diferentes.

Se esta é sua primeira vez tocando em algo relacionado a Python, isso não deve ser um problema.

1. Instale Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. Instale git: <https://git-scm.com/downloads>
3. Abra uma janela de prompt de comando e vá para uma pasta na qual você tem permissões de acesso completas.
4. Clone o repositório: `git clone https://github.com/SillyTavern/SillyTavern-extras`, pressione Enter.
5. Após o clone ter terminado, digite `cd SillyTavern-extras`, pressione Enter.
6. Digite `python -m pip install -r requirements.txt`
7. Veja abaixo 'Executando Extras Após Instalação'

---

## Executando Extras Após Instalação

### Confirme que as extensões estão habilitadas

1. Abra o arquivo chamado `config.yaml` em um editor de texto. O arquivo está localizado na pasta de instalação base do ST.
2. Procure a linha que diz `enableExtensions`.
3. Certifique-se de que essa linha tem `true`, e não `false`.

### Decida qual módulo usar

(Isso só precisa ser feito uma vez)

* Extras é sempre iniciado com uma linha de comando Python.
* `python server.py` é o mínimo, mas não habilita nenhum módulo útil.
* para habilitar módulos você deve usar o modificador `--enable-modules=`, com uma lista separada por vírgulas de nomes de módulos

Exemplo: `python server.py --enable-modules=caption,summarize,classify`

Isso habilitaria Image Captioning, Chat Summary e atualização em tempo real de Character Expressions.

Abaixo está uma tabela que descreve cada módulo.

| Name         | Description                                                         |
|--------------|---------------------------------------------------------------------|
| `caption`    | Image captioning                                                    |
| `summarize`  | Text summarization                                                  |
| `classify`   | Text sentiment classification                                       |
| `sd`         | Stable Diffusion image generation                                   |
| `silero-tts` | [Silero TTS server](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS client](https://github.com/rany2/edge-tts)      |
| `chromadb`   | Vector storage server                                               |
| `coqui-tts`  | Coqui TTS                                                           |
| `rvc`        | Real-time voice cloning                                             |

* Decida quais módulos você quer adicionar à sua linha de comando Python.
* Eles serão usados na próxima etapa.

**NOTA: NÃO deve haver `espaços de forma alguma na lista de módulos do seu comando Python!`**

### Iniciar Servidor Extras

Ainda em sua janela de prompt de comando dentro da pasta de instalação Extras...

1. Certifique-se de que seu ambiente conda esteja ativo (se você usou o método de instalação Conda)
2. Digite `activate extras` se o ambiente não estiver ativo.
3. Digite `python server.py --enable-modules=SUA,LISTA,DE,MÓDULOS,SELECIONADOS,AQUI`
4. O servidor extras carregará.
5. Após um tempo, ele mostrará uma URL no final. Para instalações locais, o padrão é `http://localhost:5100`.
6. Copie a URL da API.

### Conectar ST ao servidor Extras

1. Inicie seu servidor SillyTavern e visualize a interface SillyTavern em seu navegador.
2. Abra o painel Extensions (via ícone 'Blocos Empilhados' no topo da página)
3. Cole a URL da API na caixa de entrada.
4. Clique em `Connect`.

Para executar Extras novamente, simplesmente ative o ambiente e execute esses comandos em um prompt de comando.

`conda activate extras`, Pressione Enter.
`python server.py`, Pressione Enter.

Certifique-se de usar as opções adicionais para server.py (veja abaixo) que sua configuração requer.

## Fazer um Arquivo .bat para Inicialização Fácil

Isso é Opcional e só se aplica ao Windows, mas algo semelhante deve ser possível no MacOS.

1. Visualize sua Área de Trabalho do Windows
2. Clique com o botão direito, selecione `New`, e depois clique em `Text Document`
3. Um novo arquivo aparecerá em sua Área de Trabalho, pedindo um nome.
4. Nomeie o arquivo `STExtras.txt`
5. Abra o arquivo recém-criado em um editor de texto.
6. Cole o seguinte código nele:

    ```
    cd C:\_seu_\_caminho_\_completo_\_da_\_pasta_\_Extras_\
    call conda activate extras
    python server.py --enable-modules=SUA,LISTA,DE,MÓDULOS,SELECIONADOS,AQUI,SEM,ESPAÇOS
    call conda deactivate
    pause
    ```

7. Substitua o caminho de pasta placeholder pelo caminho real da sua pasta de instalação Extras.
8. Substitua a linha de comando python pela sua linha de comando real
9. Salve o arquivo com um novo nome `STExtras.bat` (Use `File` >> `Save As` na maioria dos editores de texto)

Agora você pode simplesmente dar um duplo clique neste arquivo .bat para iniciar facilmente o Extras.

Se você quiser alterar a lista de módulos (ou quaisquer outros modificadores de linha de comando para o servidor extras), simplesmente edite o comando python dentro do arquivo .bat.

## Problemas Comuns de Instalação do Extras

Esta seção lista perguntas e problemas comuns encontrados ao instalar o SillyTavern Extras.

### Error: Could not import the 'talkinghead' module on Linux

Requer a instalação de um pacote adicional porque não é instalado automaticamente devido à incompatibilidade com Colab. Execute isso depois de instalar outros requisitos:

`pip install wxpython`

### Servidor Extras não consegue conectar ao Stable Diffusion Web UI do AUTOMATIC1111

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Certifique-se de que webui-user.bat que você usa para iniciar Stable Diffusion contém a opção de linha de comando --api na variável COMMANDLINE_ARGS.**

Encontre e substitua essa linha em seu "webui-user.bat": `set COMMANDLINE_ARGS=--api`

![How it should look](/static/extensions/sd-user.png)

Se o modo API estiver desabilitado para SD Web UI, o servidor Extras não poderá fazer uma conexão e você não poderá gerar imagens!

#### Ainda não funciona?

Certifique-se de que você inicia tudo na ordem adequada, esperando que cada programa termine de carregar antes de prosseguir para a próxima etapa:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

O servidor extras não pode reconectar à API do Stable Diffusion se foi carregado depois.

### erro de construção de wheel hnswlib ao instalar ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Antes de instalar o módulo ChromaDB você deve primeiro fazer `uma das seguintes`:

* Instalar ferramentas de build Visual C++: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Instalar o pacote `hnswlib` com conda: `conda install -c conda-forge hnswlib`

---

### Erro ao instalar requisitos Python no Mac

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac não suporta CUDA, então pacotes torch devem ser instalados sem suporte CUDA.

Instale os requisitos usando o arquivo `requirements-silicon.txt` em vez disso.

---

### Módulos Faltando?

* Você deve especificar uma lista de nomes de módulos em sua linha de comando Python, com o modificador `--enable-modules`.
* Veja a seção [Modules](/extensions/Extras/Installation.md#decide-which-module-to-use).

---

### Para que serve a caixa API Key?

* A caixa API Key no painel Extensions do SillyTavern é usada apenas quando você tem:
  * criado um arquivo de texto chamado `api_key.txt` em sua pasta de instalação Extras, que contém sua 'senha' Extras escolhida.
  * iniciado extras com o argumento de linha de comando `--secure`.
* Isso torna a API Extras 'protegida por senha', então apenas usuários que tenham essa chave em sua caixa API Key podem acessá-la.
* Isso é principalmente útil para pessoas que querem fazer sua própria implantação pública do Extras (colab, etc).
* Usuários executando Extras em seu próprio PC para uso pessoal não devem digitar nada na caixa API Key.

### E quanto a mobile/Android/Termux? 🤔

* Há algumas pessoas na comunidade tendo sucesso executando Extras em seus telefones via Ubuntu no Termux.
* No entanto, Extras não foi feito com suporte móvel em mente.
* Nenhum suporte será fornecido para pessoas executando Extras em seus dispositivos Android.
* Direcione todas as suas perguntas ao criador do guia linkado abaixo.

#### ❗ Isso NÃO TEM SUPORTE

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
