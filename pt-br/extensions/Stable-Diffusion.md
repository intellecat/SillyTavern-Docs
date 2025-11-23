---
route: /extensions/stable-diffusion/
templating: false
---

# Geração de Imagens

Use APIs de Stable Diffusion, FLUX ou DALL-E locais ou baseadas em nuvem para gerar imagens.

Gere automaticamente imagens como respostas às suas mensagens para imersão completa,
gere a partir do histórico do chat e informações do personagem no menu varinha ou comandos slash,
ou use o comando `/sd (qualquer_coisa_aqui)` na barra de entrada do chat para criar uma imagem com seu próprio prompt.

As configurações de geração mais comuns do Stable Diffusion são personalizáveis na interface do SillyTavern.

- Suporta [múltiplas fontes de geração de imagens](#fontes-suportadas), tanto locais quanto baseadas em nuvem
- Vários [modos de geração](#modos-de-geração) para personagens, cenas e prompts personalizados
- [Comandos slash](#como-gerar-uma-imagem) para geração fácil de imagens dentro dos chats
- [Modo interativo](#usar-modo-interativo) para acionar a geração de imagens com base em solicitações em linguagem natural
- Templates de prompt personalizáveis e [prefixos](#prefixo-de-prompt-comum) para estilo e qualidade consistentes
- [Prefixos de prompt específicos do personagem](#prefixo-de-prompt-específico-do-personagem) para imagens personalizadas de personagens
- [Presets de estilo](#estilos) para alternar rapidamente entre diferentes configurações de geração de imagens
- [Opções de visibilidade](#visibilidade-da-mensagem-de-chat) flexíveis para imagens geradas no chat
- [Integração avançada com ComfyUI](#configuração-do-comfyui) para workflows altamente personalizáveis
- Capacidade de [visualizar todas as imagens geradas](#visualizar-todas-as-imagens-geradas) em uma galeria de personagens
- Recurso de [deslizes de imagem](#deslizes-de-imagem) para regenerar imagens mantendo o mesmo prompt
- Opções para [editar prompts antes da geração](#editar-prompts-antes-da-geração) e [estender prompts de modo livre](#estender-prompts-de-modo-livre)
- Integração com [chamada de função](#usar-ferramenta-de-função) de IA para detecção automática de geração de imagens

## Fontes suportadas

| Fonte                                                                                            | Observações                                                                                         |
|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [AI.ML API](https://aimlapi.com/)                                                                 | Nuvem, pago                                                                                     |
| [Black Forest Labs](https://bfl.ai/)                                                              | Nuvem, pago                                                                                     |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)                                              | Local, código aberto (GPL3), gratuito, veja [Configuração do ComfyUI](#configuração-do-comfyui). |
| [Draw Things](https://drawthings.ai/)                                                             | Local, Mac/iOS, gratuito                                                                  |
| [Electron Hub](https://electronhub.ai/)                                                           | Nuvem, pago                                                                                     |
| [FAL.AI](https://fal.ai/)                                                                         | Nuvem, pago                                                                                     |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | Nuvem, pago. Série de modelos Imagen. AI Studio suporta apenas modelo Imagen 3.0 002.         |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index)                         | Nuvem, gratuito                                                                           |
| [NanoGPT](https://nano-gpt.com/)                                                                  | Nuvem, pago                                                                                     |
| [NovelAI Diffusion](https://novelai.net/)                                                         | Nuvem, requer assinatura ativa                                                          |
| [OpenAI](https://platform.openai.com/)                                                            | Nuvem, pago                                                                                     |
| [Pollinations](https://pollinations.ai/)                                                          | Nuvem, código aberto (MIT), gratuito                                                        |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic)                                   | Local, código aberto (AGPL3), gratuito                                                      |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)                           | Descontinuado, não recomendado                                                                     |
| [Stability AI](https://platform.stability.ai/)                                                    | Nuvem, pago                                                                                     |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | Local, código aberto (AGPL3), gratuito                                                      |
| [Stable Horde](https://stablehorde.net/)                                                          | Nuvem, código aberto (AGPL3), gratuito                                                      |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models)                        | Nuvem                                                                                           |
| [x.AI](https://x.ai/)                                                                             | Nuvem, pago                                                                                     |

## Modos de geração

| Item do menu varinha     | Argumento do comando slash | Descrição                                    | Observações                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | Um retrato de corpo inteiro do personagem atual. | -                                     |
| "Your Face"        | `face`                 | Um retrato em close do personagem atual.  | Força proporção de aspecto de retrato.       |
| "Me"               | `me`                   | Um retrato da persona do usuário.                | -                                     |
| "The Whole Story"  | `scene`                | Uma recapitulação visual dos eventos do chat.             | -                                     |
| "The Last Message" | `last`                 | Uma recapitulação visual da última mensagem do chat.       | -                                     |
| "Raw Last Message" | `raw_last`             | Última mensagem usada como prompt literalmente.        | -                                     |
| "Background"       | `background`           | Um plano de fundo de chat baseado no contexto da história.      | Força proporção de aspecto de paisagem ampla. |

## Como gerar uma imagem

1. Use o item "Image Generation" no menu de contexto de extensões (varinha).
2. Digite um comando slash `/sd (argumento)` com um argumento da tabela de Modos de geração. Qualquer outra coisa acionará um "modo livre" para fazer o SD gerar o que você solicitou. Exemplo: `/sd apple tree` geraria uma imagem de uma macieira.
3. Procure um ícone de pincel nas ações de contexto para mensagens de chat. Isso forçará o modo "Raw Message" para a mensagem selecionada.

Cada modo de geração além de raw message e modo livre acionará uma geração de prompt usando sua API de geração principal atualmente selecionada para converter o contexto do chat no prompt SD.
Você pode configurar o template de instrução para gerar prompts para cada modo de geração usando a gaveta de configurações "SD Prompt Templates" no painel de extensões.

### Dicas e truques para uso do comando `/sd`

#### Visualizar todas as imagens geradas

Para visualizar todas as imagens salvas para um personagem (incluindo outros chats), abra uma galeria no menu dropdown "More..." no painel de informações do personagem, ou use o comando slash `/show-gallery`.

#### Especificar um prompt negativo

Use um argumento nomeado `negative` antes do prompt para forçar um prompt negativo específico para esta geração.

```stscript
/sd negative="fries" cute tater farmer holding a tayto in a spud-field
```

#### Incluir um prefixo específico do personagem

Use uma macro especial `{{charPrefix}}` no modo de prompt livre para incluir prefixos de prompt positivos e negativos (se definidos) para o personagem atual.

```stscript
/sd {{charPrefix}}, riding a bike
```

#### Suprimir uma mensagem de chat

Você pode evitar postar uma imagem gerada no chat passando um argumento nomeado `quiet=true`. A imagem ainda será adicionada à galeria do personagem, e o comando produzirá uma URL relativa para a imagem que pode ser consumida por outros comandos.

O exemplo abaixo enviará a imagem gerada usando Markdown como persona de usuário.

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### Deslizes de imagem

Os deslizes de imagem permitem refazer a geração de imagem mantendo o mesmo prompt. Se uma seed fixa estiver definida, ela será randomizada para a próxima geração.

Para percorrer as imagens, passe o cursor do mouse (toque no celular) sobre uma imagem gerada para revelar botões de seta e contador de deslizes. Tocar na seta direita na última imagem gerará uma nova.

*'Swipes' aqui é apenas um nome, não tente o gesto real de deslizar, pois isso regenerará a mensagem em si, não a imagem anexada.*

## Opções

### Editar prompts antes da geração

Permite editar os prompts gerados automaticamente manualmente antes de enviá-los para a API do Stable Diffusion.

### Usar ferramenta de função

Usa [chamada de função](/extensions/Stable-Diffusion.md) para detectar automaticamente a intenção de gerar uma imagem.

**Requisitos:**

1. Deve ter geração de imagens configurada com uma fonte suportada.
2. Deve usar um modelo de API de Chat Completion suportado e ter chamada de ferramenta de função habilitada nas configurações de Resposta de IA.
3. A opção "Use function tool" deve estar habilitada nas configurações de Geração de Imagens.
4. O usuário deve expressar a intenção de gerar uma imagem na mensagem do chat, por exemplo, "Send me a picture of a cat".

!!!warning
O modo interativo não será acionado quando a ferramenta de função estiver habilitada.
!!!

### Usar modo interativo

Permite acionar uma geração de imagem em vez de texto como resposta a uma mensagem do usuário que segue o padrão especial:

1. Contém um dos seguintes verbos: send, mail, imagine, generate, make, create, draw, paint, render
2. Seguido por um dos seguintes substantivos (não mais que 10 caracteres de distância): pic, picture, image, drawing, painting, photo, photograph
3. Seguido por um assunto alvo da geração de imagens, que pode ser opcionalmente precedido por frases como "of a" ou "of this".

Exemplos de solicitações válidas e assuntos capturados:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

Alguns assuntos especiais acionam um modo de geração predefinido:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### Estender prompts de modo livre

Ao usar o modo interativo do comando slash, estender automaticamente as descrições de assunto de geração de modo livre solicitando sua API principal.

### Ajustar resoluções auto-ajustadas

Ajustar solicitações de geração de imagens com proporção de aspecto forçada (retratos, planos de fundo) para a resolução conhecida mais próxima, tentando preservar as contagens absolutas de pixels. Consulte o dropdown "Resolution" para a lista de opções possíveis.

**Recomendado para modelos SDXL**.

## Prefixo de prompt comum

!!!tip Dica Profissional
Use a macro `{prompt}` para especificar exatamente onde o prompt gerado será inserido.
!!!

Adicionado antes de cada prompt gerado ou de modo livre. Comumente usado para definir o estilo geral da imagem.

Exemplo: `best quality, anime lineart`.

## Prompt negativo

Características da imagem que você não quer que estejam presentes na saída.

Exemplo: `bad quality, watermark`.

## Prefixo de prompt específico do personagem

!!!tip Dica Profissional
Se suportado pela fonte de geração, você também pode usar LoRAs/embeddings aqui, por exemplo: `<lora:DonaldDuck:1>`.
!!!

Quaisquer características que descrevam o personagem atualmente selecionado. Será adicionado após um prefixo comum.

Exemplo: `female, green eyes, brown hair, pink shirt`.

Você também pode especificar um prefixo de prompt negativo para qualquer conteúdo indesejado. Ele será combinado com o prompt negativo geral.

Limitações:
1. Funciona apenas em chats 1-para-1. Não será usado em grupos.
2. Não será usado para gerações de planos de fundo e modo livre.

!!! Note
Para forçar a inclusão de um prefixo de personagem em um prompt de modo livre, use a macro `{{charPrefix}}` em qualquer lugar no prompt.
!!!

Se você quiser compartilhar os prefixos com outros, marque a caixa "Shareable". Isso os salvará com os dados do personagem, em vez de suas configurações locais.

## Estilos

Use isso para salvar e restaurar rapidamente seus presets favoritos de estilo/qualidade para usá-los depois ou ao alternar entre modelos. O seguinte está incluído no preset de Estilo:

1. Prefixo de Prompt Comum
2. Prompt Negativo

Você também pode alternar entre estilos usando o comando `/imagine-style` (ou `/sd-style` ou `/img-style`).

## Visibilidade da Mensagem de Chat

Imagens geradas inseridas no chat são ocultas nos prompts da API principal por padrão, mas isso pode ser substituído individualmente por iniciador de geração (ícone "Magic wand", comando slash, modo interativo). Isso pode ser usado para tornar a experiência mais imersiva permitindo que os personagens "reconheçam" as imagens. Modelos multimodais na API de Chat Completions também podem 'ver' as imagens se "Send inline images" estiver habilitado.

Uma mensagem de texto pode ser personalizada alterando o "Chat Message Template" em Image Prompt Templates. Todas as macros regulares podem ser usadas neste template, além de uma macro especial `{{prompt}}` para especificar onde o prompt da imagem será adicionado.

## Configuração do ComfyUI

[ComfyUI](https://github.com/comfyanonymous/ComfyUI) é uma opção rápida e muito flexível para geração de imagens.

Se você está familiarizado com ComfyUI, o tl;dr é: faça seu workflow no ComfyUI, baixe-o **em formato API**, e cole-o no SillyTavern ComfyUI Workflow Editor. ST enviará seu workflow para a API do ComfyUI e você receberá uma imagem em seu chat. Mas com grandes poderes vêm grandes responsabilidades, e a principal responsabilidade é inserir placeholders no JSON do seu workflow para que você possa alterar configurações do SillyTavern.

Se você não está familiarizado com ComfyUI, ainda pode usá-lo para gerar imagens no SillyTavern usando o workflow padrão. Depois, quando quiser grandes poderes, você pode aprender a usar o ComfyUI...

### Controles

Este painel permite configurar e gerenciar sua integração ComfyUI com SillyTavern.

Insira a URL do seu servidor ComfyUI no campo de entrada **ComfyUI URL**. O valor padrão é `http://127.0.0.1:8188`.
Se você estiver usando [SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI), a porta padrão para o
[servidor ComfyUI gerenciado](https://github.com/mcmonkeyprojects/SwarmUI/blob/master/src/BuiltinExtensions/ComfyUIBackend/README.md) é `7821`,
20 portas acima da porta padrão para SwarmUI.

Após inserir a URL, escolha <i class="fa-solid fa-check"></i> **Connect** para validar e estabelecer uma conexão. O servidor ComfyUI deve estar acessível da máquina host do SillyTavern.

### Gerenciamento de Workflow

Selecione um workflow ComfyUI no menu dropdown. Dois workflows padrão são fornecidos:

- Default_Comfy_Workflow.json: Um workflow básico de texto para imagem que suporta as configurações de geração de imagens mais comuns.
- Char_Avatar_Comfy_Workflow.json: Um workflow de exemplo de imagem para imagem que usa o avatar do personagem, além do prompt, para gerar uma imagem.

Use os seguintes botões para gerenciar seus workflows:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor** para visualizar e modificar o workflow selecionado.
- <i class="fa-solid fa-plus"></i> **Create new workflow** para criar um novo workflow com um nome personalizado.
- <i class="fa-solid fa-trash-can"></i> **Delete workflow** para remover o workflow selecionado.

### Editor de Workflow

O ComfyUI Workflow Editor permite visualizar e modificar workflows ComfyUI para uso com SillyTavern.

O componente principal do editor é uma grande área de texto onde você pode inserir ou editar seu workflow ComfyUI em formato JSON.

Para adicionar um workflow ComfyUI ao editor, siga estas etapas:

1. Habilite 'Dev Mode' nas configurações do ComfyUI.
2. Use a opção 'Save (API Format)' no ComfyUI para baixar os dados JSON.
3. Crie um novo workflow no SillyTavern e abra o editor.
4. Cole os dados JSON baixados na área de texto.
5. Substitua valores específicos por placeholders conforme necessário para seu caso de uso.

!!!tip Dicas
Você pode adicionar o arquivo JSON em formato API diretamente ao diretório `data/default-user/user/workflows` na sua instalação do SillyTavern. Isso economizará as etapas 3 e 4.

Mantenha o arquivo JSON original. Se você precisar abrir o workflow novamente no ComfyUI para fazer alterações, é muito mais conveniente editar o arquivo original do que aquele com todos os placeholders.
!!!

### Placeholders

O editor fornece uma lista de placeholders predefinidos que podem ser usados em seu JSON de workflow. Esses placeholders são substituídos por valores dinâmicos quando o workflow é executado no SillyTavern.

Placeholders marcados com ✅ estão presentes em seu JSON de workflow. Placeholders marcados com ❌ não estão presentes em seu JSON de workflow. Você pode adicionar esses placeholders ao seu JSON de workflow conforme necessário. Você não precisa adicionar todos os placeholders, apenas os que seu workflow usa e você deseja substituir dinamicamente.

#### Prompts

Os placeholders `%prompt%` e `%negative_prompt%` são usados para inserir os prompts de geração de imagem no workflow. Eles contêm os prompts finais gerados pelo SillyTavern, incluindo o prompt gerado para seu modo `/sd` escolhido, o prefixo de prompt comum, prompt negativo e prefixo de prompt específico do personagem.

Por exemplo, você pode ter testado seu workflow com um prompt como "forest elf" no ComfyUI. Para usar este workflow no SillyTavern, você pode substituir o prompt "forest elf" pelo placeholder `%prompt%`:

+++ JSON com placeholder
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "%prompt%"
    }
}
```
+++ JSON original
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "forest elf"
    }
}
```
+++

Observe que o placeholder está entre aspas duplas. Isso é importante para o formato JSON e é exigido pelo sistema de substituição de placeholder do SillyTavern. Mesmo para números, você deve usar aspas duplas no JSON do template.

Às vezes o prompt (ou outro valor) não aparece onde você pode esperar. ComfyUI removerá nós da versão API do workflow se não forem necessários para o workflow funcionar no modo API.

Por exemplo, este workflow usa um [nó de carregador de tags LoRA](https://github.com/badjeff/comfyui_lora_tag_loader) com uma primitiva de prompt para que o workflow seja mais claro no modo UI:

![Prompt primitive and LoRA loader](/static/extensions/sd-comfy-prompt-primitive.png)

O nó primitivo de prompt será removido da versão API do workflow, então você insere o placeholder no nó LoraTagLoader. Encontre o texto "apple tree" no workflow e substitua-o pelo placeholder `%prompt%`:

+++ JSON com placeholder
```json
{
    "inputs": {
      "text": "%prompt%",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++ JSON original
```json
{
    "inputs": {
      "text": "apple tree",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++

Em alguns casos, você pode precisar fazer várias substituições no JSON do workflow, mesmo que o prompt apareça apenas uma vez na UI.

#### Modelo

O placeholder `%model%` inserirá o valor do modelo selecionado nas configurações de geração de imagens.

Um exemplo do workflow de texto para imagem padrão:

+++ JSON com placeholder
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "%model%"
    }
}
```
+++ JSON original
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "sd15.safetensors"
    }
}
```
+++

Para carregar UNets quantizados em GGUF, use um nó [UNet Loader (GGUF)](https://github.com/city96/ComfyUI-GGUF) em seu workflow,
escolha um modelo `GGUF` no dropdown de modelo do SillyTavern e use o placeholder `%model%` nas configurações do nó assim:

+++ JSON com placeholder
```json
{
    "inputs": {
      "unet_name": "%model%"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++ JSON original
```json
{
    "inputs": {
      "unet_name": "flux1-dev-Q4_0.gguf"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++

!!!info Se você tiver tipos de modelo além dos checkpoints SD usuais no ComfyUI
Checkpoints de Stable Diffusion, UNets SD e UNets quantizados em GGUF aparecem todos no dropdown Model.
Modelos de um tipo não funcionarão com workflows/nós de carregador esperando outro tipo.
Se você escolher um tipo de modelo incompatível no ST, ComfyUI relatará um problema com o nó de carregador.
!!!

#### Imagens de avatar

Use os placeholders `%user_avatar%` e `%char_avatar%` para incluir os avatares de usuário e personagem no workflow. Esses placeholders são substituídos pelos dados PNG dos avatares quando o workflow é executado. Os dados da imagem são codificados em formato base64, então você deve decodificá-los em seu workflow. Uma escolha popular para esta tarefa é o nó [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes).

Neste exemplo, o avatar do personagem é carregado com um nó `Load Image (Base64)`. Ele também usa um nó Image Resize para redimensionar a imagem para qualquer tamanho especificado nas configurações de geração de imagens:

![Load image from base64 string and resize](/static/extensions/sd-comfy-load-b64.png)

Insira os placeholders `%char_avatar%`, `%width%` e `%height%` no JSON para os nós Load Image (Base64) e Image Resize:

```json
{
    "97": {
        "inputs": {
            "image": "%char_avatar%"
        },
        "class_type": "ETN_LoadImageBase64",
        "_meta": {"title": "Load Image (Base64)"}
    },
    "98": {
        "inputs": {
            "mode": "resize",
            "resize_width": "%width%",
            "resize_height": "%height%",
            "image": ["97", 0]
        },
        "class_type": "Image Resize",
        "_meta": {"title": "Resize image"}
    }
}
```

Para obter uma string de imagem codificada em base64 para testar seu workflow no ComfyUI, use qualquer ferramenta online que converta imagens em strings base64.
Aqui está uma string de exemplo que você pode usar para testes iniciais: [sd-comfy-base64-test-string.txt](/static/extensions/sd-comfy-base64-test-string.txt).

#### Outros placeholders

A maioria dos outros placeholders usa os valores dos controles correspondentes nas configurações de geração de imagens, ou os valores que você especifica com o comando `/sd`:

- `%vae%`, mas a maioria dos modelos SD inclui um VAE, então os workflows padrão não usam este placeholder. Use-o com workflows personalizados para carregar um VAE junto com um UNet, substituir o VAE padrão, etc.
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`: para o workflow de exemplo de imagem para imagem, varie a quantidade de denoise entre cerca de 0.5 (alterações quase imperceptíveis na imagem de origem) e 1.0 (uma imagem completamente diferente como se nenhuma imagem de origem fosse usada). Não usado pelo workflow padrão de texto para imagem porque não faz sentido usar um valor diferente de 1.0 para texto para imagem.
- `%clip_skip%`: não usado pelos workflows padrão, mas disponível para workflows personalizados.

O placeholder `%seed%` inserirá o valor da seed do controle se você tiver especificado um. Se você definir a seed como `-1`, SillyTavern gerará uma nova seed aleatória para cada imagem em `%seed%`.

#### Placeholders personalizados

Você pode adicionar placeholders personalizados ao seu workflow:

1. Procure a seção "Custom" abaixo dos placeholders predefinidos.
2. Clique no botão "+" para adicionar um novo placeholder personalizado.
3. Insira um nome para o placeholder no campo `find`.
4. Insira o valor que você deseja que substitua o placeholder no campo `replace`.

Placeholders personalizados aparecerão em uma lista separada abaixo dos predefinidos.

Por exemplo, você poderia substituir o prefixo "SillyTavern" para nomes de arquivo de imagem salvos no workflow padrão por um placeholder personalizado. Adicione um novo placeholder personalizado com `find` definido como `filename_prefix` e `replace` definido como `ServiceTesnor`. Insira o novo placeholder `%filename_prefix%` em seu JSON de workflow. Agora você pode alterar o prefixo do nome do arquivo de SillyTavern para ServiceTesnor alterando o valor do placeholder personalizado.

+++ JSON com placeholder
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "%filename_prefix%",
        "images": ["8", 0]
    }
}
```
+++ JSON original
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "SillyTavern",
        "images": ["8", 0]
    }
}
```
+++

### Truques do Comfy

Leia todas as informações gerais nesta página para se familiarizar com as opções de geração de imagens. Opções como estilos alteráveis e prefixos de prompt comuns, quando combinados com a flexibilidade total dos workflows ComfyUI, permitem criar uma grande variedade de configurações de geração de imagens.

#### Carregando LoRAs

Use um nó de carregador de tags LoRA (como [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader)) para carregar quaisquer LoRAs especificados no prompt.
Agora você pode adicionar quantos LoRAs quiser ao seu prompt com tags como `<lora:CroissantStyle:0.8>`, e eles serão carregados em seu workflow.
Isso também fará a "dica profissional" de usar LoRAs em [prefixos de prompt específicos do personagem](#prefixo-de-prompt-específico-do-personagem) funcionar com ComfyUI.

#### Definindo valores de workflow a partir de estilos ou comandos slash

Você pode usar macros em valores de placeholder personalizados. Como exemplo prático,
digamos que você às vezes queira gerar imagens sem plano de fundo, e gostaria que isso fosse alternável com um
comando slash ou estilo de imagem. Veja como você poderia fazer:

1. Faça um workflow ComfyUI que remove o plano de fundo da imagem, ou não, dependendo do valor de uma entrada
2. Use um placeholder personalizado para definir o valor dessa entrada, mas use `{{getvar::remove_background}}` como o valor replace
3. Agora você pode definir o valor de `remove_background` com `/setvar key=remove_background true` ou `/setvar key=remove_background false` antes de gerar uma imagem
4. O workflow usará o valor que você definiu para determinar se deve remover o plano de fundo
5. Faça um estilo de imagem "No background" com prefixo de prompt comum `{{setvar::remove_background::true}}`
6. Use o controle de estilo ou `/imagine-style No background` para definir o valor de `remove_background` como `true` antes de gerar uma imagem
