---
route: /extensions/captioning/
templating: false
---

# Image Captioning

Image Captioning permite ao SillyTavern gerar automaticamente descrições de texto para imagens usadas em chats.

Use Image Captioning quando quiser que seu personagem de IA "veja" e responda ao conteúdo visual em suas conversas.

- Crie legendas para imagens que você carrega ou cola em mensagens
- Adicione contexto a imagens existentes no histórico de chat
- Use várias fontes para geração, incluindo modelos locais, APIs na nuvem e redes colaborativas

Existem opções que não requerem configuração, dinheiro ou GPU. Também existem opções que requerem alguns ou todos esses itens. Escolha a que se adequa às suas necessidades e recursos.

A extensão de image captioning é integrada ao SillyTavern e não precisa ser instalada separadamente.

## Início rápido

1. Configurar:
    - Abra o painel **Image Captioning** no painel **<i class="fa-solid fa-cubes"></i> Extensions**
    - Escolha uma fonte de captioning (provavelmente "Local" ou "Multimodal")
    - Para "Multimodal", certifique-se de ter configurado a conexão na aba **<i class="fa-solid fa-plug"></i> API Connections**
2. Gerar uma legenda:
    - Escolha "**Generate Caption**" no menu popup **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions**
    - Selecione um arquivo de imagem quando solicitado
    - Aguarde a legenda ser gerada
3. Revisar e enviar:
    - A imagem legendada será inserida em sua mensagem
    - Veja a legenda usando a dica da imagem
    - Clique em **<i class="fa-solid fa-paper-plane"></i> Send** para ver o que seu personagem pensa da imagem!

## Controles do painel

### Seleção de Fonte

Escolha a fonte para image captioning. Opções suportadas:

| Fonte                            | Descrição                                                                                                                                                                                                    |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **Nuvem**: OpenAI, Anthropic, Google, MistralAI e outros. <br>**Local**: Ollama, llama.cpp, KoboldCpp, Text Generation WebUI e vLLM. <br>Suporta prompts personalizados para que você possa fazer perguntas às suas imagens. |
| [Local](#local-source)           | Usa [transformers.js](https://huggingface.co/docs/transformers.js/en/index) rodando localmente dentro do seu servidor SillyTavern. Configuração zero!                                                       |
| Horde                            | Usa a rede [AI Horde](https://aihorde.net/), uma rede distribuída colaborativa de modelos de geração de imagens. Nada para baixar, configurar ou pagar. Tempos de resposta variáveis.                       |
| Extras                           | O projeto Extras foi descontinuado em abril de 2024 e não é mantido ou suportado.                                                                                                                           |

### Configuração de Caption
- **Caption Prompt**: Insira um prompt personalizado para captioning. O prompt padrão é "What's in this image?"
- **Ask every time**: Alterne para solicitar um prompt personalizado para cada legenda de imagem

### Message Template
- **Message Template**: Personalize o template de mensagem de legenda. Use a macro `{{caption}}` para inserir a legenda gerada. O template padrão é `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### Auto-captioning
- **Automatically caption images**: Alterne para habilitar legendagem automática de imagens coladas ou anexadas a mensagens
- **Edit captions before saving**: Alterne para permitir editar legendas antes de serem salvas

## Legendando imagens

Todas as maneiras de legendar imagens no SillyTavern:

* Escolha "**Generate Caption**" no menu popup **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** e selecione um arquivo de imagem quando solicitado
* Clique no ícone <i class="fa-solid fa-envelope-open-text"></i> **Caption** no topo de uma imagem já em uma mensagem
* Cole uma imagem diretamente na entrada de chat com [auto-captioning](#auto-captioning) habilitado
* Anexe um arquivo de imagem a uma mensagem usando o botão <i class="fa-solid fa-paperclip"></i> **Embed File or Image** nas ações de uma mensagem
* Envie uma mensagem com uma imagem incorporada
* Use o [comando slash](#slash-command-caption) `/caption`

## Auto-Captioning
O recurso de auto-captioning permite gerar automaticamente legendas para imagens conforme elas são adicionadas ao chat, sem acionar manualmente o processo de legendagem cada vez.

Para habilitar, marque a caixa de seleção "Automatically caption images" no painel Image Captioning. Você também pode optar por editar legendas antes de serem salvas marcando a caixa "Edit captions before saving".

Uma vez habilitado, o auto-captioning será acionado nos seguintes cenários:

- Quando uma imagem é colada diretamente na entrada de chat.
- Quando um arquivo de imagem é anexado a uma mensagem.
- Quando uma mensagem com uma imagem incorporada é enviada.

O sistema usará sua fonte de captioning selecionada (Local, Extras, Horde ou Multimodal) e as configurações definidas para gerar uma legenda para a imagem.

### Editando legendas antes de salvar (Modo Refinar)

Se você habilitou a opção "Edit captions before saving":
1. Depois que uma imagem é adicionada, um popup aparecerá com a legenda gerada.
2. Você pode revisar e editar a legenda conforme necessário.
3. Clique em "OK" para aplicar a legenda, ou "Cancel" para descartar a legenda sem salvar.

### Envio de caption
A legenda gerada (e opcionalmente editada) será automaticamente inserida no prompt usando o Message Template que você configurou. Por padrão, será enviada neste formato:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```


## Comando Slash: /caption
A extensão fornece um comando slash `/caption` para usar na caixa de chat ou em scripts.

### Uso

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (opcional): Um prompt personalizado para o modelo de captioning. Suportado apenas por fontes multimodais.
- `quiet=true|false`: Se definido como true, suprime o envio de uma mensagem legendada para o chat. O padrão é false.
- `mesId=number`: Especifica um ID de mensagem para legendar uma imagem de uma mensagem existente em vez de carregar uma nova.

Se nenhum `mesId` for fornecido, o comando solicitará que você carregue uma imagem. Quando `quiet` é false (padrão), uma nova mensagem com a imagem legendada será enviada ao chat. A legenda gerada pode ser usada como entrada para outros comandos.

### Exemplos
Legendar uma nova imagem com as configurações padrão:

```
/caption
```

Legendar uma nova imagem com um prompt personalizado:

```
/caption Describe the main colours and shapes in this image
```

Legendar uma imagem da mensagem #5 sem enviar uma nova mensagem:

```
/caption mesId=5 quiet=true
```

Legendar uma imagem da mensagem #10 com um prompt personalizado e então [gerar uma nova imagem](/extensions/Stable-Diffusion.md) baseada na legenda:

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine
```

## Fonte Local

Você pode alterar o modelo em [config.yaml](/Administration/config-yaml.md#extensions-configuration). A chave é chamada `extensions.models.captioning`. Insira o ID do modelo Hugging Face que você deseja usar. O padrão é `Xenova/vit-gpt2-image-captioning`.

Você pode usar qualquer modelo que suporte image captioning (`VisionEncoderDecoderModel` ou pipeline "image-to-text"). O modelo precisa ser compatível com a biblioteca transformers.js. Ou seja, precisa ter pesos ONNX. Procure por modelos com as tags `ONNX` e `image-to-text`, ou que tenham uma pasta chamada `onnx` cheia de arquivos `.onnx`.

## Fonte Multimodal

### Configuração geral

- **Model**: Escolha o modelo para image captioning. As opções variam com base na API selecionada.
- **Allow reverse proxy**: Alterne para permitir usar um proxy reverso se definido e válido (OpenAI, Anthropic, Google, Mistral, xAI)

As chaves de API e URLs de endpoint para fontes de captioning são gerenciadas no painel [API Connections](/Usage/API_Connections/index.md). Configure a conexão em API Connections primeiro, depois selecione-a como sua fonte de captions em Captioning.

!!!warning Configure primeiro no painel API Connections
Mais uma vez: configure a chave de API/endereço/porta em **<i class="fa-solid fa-plug"></i> API Connections** e use a conexão em Captioning.

Você ainda pode usar Claude para chats e Google AI Studio para image captioning, ou o que quiser. Apenas configure *ambos* primeiro na aba 'API Connections'. Então mude sua fonte de Chat Completion para Claude e sua fonte de Captioning para Google AI Studio.
!!!

Para a maioria dos backends locais, você precisará definir algumas opções no backend do modelo em vez de no SillyTavern. Se seu backend só pode executar um modelo por vez e não suporta troca automática, você tem várias opções para usar modelos diferentes para chat e captioning:

1. **Endpoints secundários:** Use o recurso de endpoint secundário (veja a seção [Endpoints secundários](#secondary-endpoints) abaixo) para se conectar a um servidor de API diferente para captioning
2. **Múltiplos tipos de conexão:** Conecte-se ao seu backend usando ambos os modos Text Completion e Chat Completion em API Connections - isso lhe dá duas conexões separadas para o mesmo tipo de backend

### Fontes

Para usar uma dessas fontes de caption, selecione Multimodal no dropdown de Fonte.

* "Eu quero a melhor legendagem possível e não me importo em pagar por isso": Anthropic
* "Eu não quero pagar nada ou executar nada": camada gratuita do Google AI Studio
* "Eu quero legendar imagens localmente e que simplesmente funcione": Ollama
* "Eu quero manter o sonho da IA local viva": [KoboldCpp](#koboldcpp)
* "Eu quero reclamar quando não funcionar": ~~Extras~~

| Provedor de API                   | Descrição                                                                                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AI/ML API                         | Nuvem, pago, vários modelos GPT, Claude e Gemini com recursos de visão                                                                                                             |
| Claude                            | Nuvem, pago, todos os modelos Claude com recursos de visão                                                                                                                          |
| Cohere                            | Nuvem, pago, Aya Vision 8B / 32B                                                                                                                                                    |
| Custom (OpenAI-compatible)        | Para APIs personalizadas compatíveis com OpenAI, usa o modelo atualmente configurado na aba API Connections                                                                         |
| Electron Hub                      | Nuvem, pago, vários modelos com recursos de visão                                                                                                                                   |
| Google AI Studio                  | Nuvem, camada gratuita depois pago, Gemini Flash/Pro                                                                                                                                |
| Google Vertex AI                  | Nuvem, camada gratuita, Gemini Flash/Pro                                                                                                                                            |
| Groq                              | Nuvem, llama-4 scout/maverick                                                                                                                                                       |
| KoboldCpp                         | Local, deve configurar modelo no KoboldCpp                                                                                                                                          |
| llama.cpp                         | Local, deve configurar modelo no llama.cpp                                                                                                                                          |
| MistralAI                         | Nuvem, pago, pixtral-large, pixtral-12B, magistral, mistral-large, etc.                                                                                                            |
| Moonshot AI                       | Nuvem, pago, moonshot-vision                                                                                                                                                        |
| NanoGPT                           | Nuvem, pago, vários modelos GPT/Claude/Google com recursos de visão                                                                                                                |
| Ollama                            | Local, pode alternar entre modelos disponíveis e baixar [modelos de visão adicionais](https://ollama.com/search?c=vision) dentro do Captioning após configurar em API Connections |
| OpenAI                            | Nuvem, pago, GPT-4 Vision, 4-turbo, 4o, 4o-mini                                                                                                                                     |
| OpenRouter                        | Nuvem, pago (talvez opções gratuitas), muitos modelos, escolha dos disponíveis dentro do Captioning após configurar em API connections                                             |
| Pollinations                      | Nuvem, gratuito                                                                                                                                                                     |
| Text Generation WebUI (oobabooga) | Local, deve configurar modelo no ooba                                                                                                                                               |
| vLLM                              | Local                                                                                                                                                                               |
| xAI (Grok)                        | Nuvem, pago, grok-vision                                                                                                                                                            |

### Endpoints secundários

Por padrão, a fonte Multimodal usa o endpoint primário configurado na aba API Connections.
Você também pode configurar um endpoint secundário especificamente para captioning multimodal.

- Abra o painel **Image Captioning** no painel **<i class="fa-solid fa-cubes"></i> Extensions**.
- Selecione "Multimodal" como a fonte de captioning e um provedor de API preferido.
- Insira uma URL válida para o endpoint secundário no campo "Secondary captioning endpoint URL".
- Marque a caixa "Use secondary URL" para habilitar o endpoint secundário.

!!!tip
Não adicione `/v1` ou `/chat/completions` ao final da URL. A extensão cuidará disso automaticamente.
!!!

Isso é suportado apenas pelas seguintes APIs:

- KoboldCpp
- llama.cpp
- Ollama
- Text Generation WebUI (oobabooga)
- vLLM

### Guias específicos de fonte

#### KoboldCpp

Para informações gerais sobre instalar e usar o [KoboldCpp](https://github.com/LostRuins/koboldcpp), veja a [documentação do KoboldCpp](https://github.com/LostRuins/koboldcpp/wiki).

Para usar o KoboldCpp para captioning multimodal:

* obtenha um modelo capaz de multimodal, treinado para processar prompts de texto e imagem ao mesmo tempo.
* também obtenha as projeções multimodais para o modelo. Esses pesos permitem que o modelo entenda como as partes de texto e imagem da entrada se relacionam entre si.
* carregue o modelo e as projeções na GUI de lançamento do KoboldCpp ou interface de linha de comando.

O modelo multimodal local original e clássico é o LLaVA. Arquivos no formato GGUF para o modelo e projeções estão disponíveis em [Mozilla/llava-v1.5-7b-llamafile](https://huggingface.co/Mozilla/llava-v1.5-7b-llamafile). Para carregá-los da linha de comando, defina o modelo e as projeções com as flags `--model` e `--mmproj`. Por exemplo:

```shell
./koboldcpp \
--model="models/llava-v1.5-7b-Q4_K.gguf" \
--mmproj="models/ llava-v1.5-7b-mmproj-Q4_0.gguf" \
... other flags ...
```

Alguns ajustes finos do LLaVA que você pode tentar: [xtuner/llava-llama-3-8b-v1_1-gguf](https://huggingface.co/xtuner/llava-llama-3-8b-v1_1-gguf), [xtuner/llava-phi-3-mini-gguf](https://huggingface.co/xtuner/llava-phi-3-mini-gguf).

Você pode usar projeções multimodais para o modelo base que seu ajuste fino específico foi construído. Projeções para alguns modelos base comuns estão disponíveis em [koboldcpp/mmproj](https://huggingface.co/koboldcpp/mmproj/tree/main).
