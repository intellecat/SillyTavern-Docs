---
order: tts
route: /extensions/tts/
---

# TTS

O SillyTavern tem uma ampla gama de opções de TTS (text-to-speech) que são usadas para ter uma voz narrando partes do seu chat. Esta página explica a configuração e o uso.

## Configurando TTS

### Seleção de Provedor TTS

Usado para selecionar qual serviço de TTS você deseja usar. Algumas das opções são gratuitas, algumas requerem uma assinatura paga e algumas são executadas localmente no seu PC.

Opções disponíveis (a lista pode mudar ao longo do tempo):

- **AllTalk** - gratuito, instalação local de código aberto, oferece uma variedade de motores TTS. Veja a página [AllTalk](./AllTalk.md) para instruções de configuração.
- **Azure TTS** - mesmas vozes do Microsoft Edge. Requer uma conta Azure e uma assinatura paga.
- **Coqui-TTS** (descontinuado) - gratuito, requer API Extras para executar. Modelos Text2Speech de alta performance (Tacotron, Tacotron2, Glow-TTS, SpeedySpeech) assim como Bark.
- **Edge** - gratuito, executa via Azure. Ao executar com "Plugin" selecionado como provedor, você também precisa instalar [este plugin do servidor](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin). A outra opção requer API Extras (descontinuada) para executar.
- **Electron Hub** - reutiliza sua chave de API [Electron Hub](https://electronhub.ai/) para acessar vozes na nuvem (GPT-4o Mini TTS, vozes neurais da Microsoft, etc.) com controles por modelo.
- **ElevenLabs** - assinatura paga necessária. Obtenha uma chave de API de [ElevenLabs](https://elevenlabs.io/).
- **Google Translate** - uma voz gratuita fornecida pelo Google, uma por idioma, a qualidade pode variar amplamente.
- **Google Gemini TTS** - requer uma chave de API do [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) ou [AI Studio](/Usage/API_Connections/google.md#google-ai-studio), usa modelos [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts).
- **Kokoro** - gratuito, usa [kokoro.js](https://www.npmjs.com/package/kokoro-js) para executar o modelo localmente no seu navegador. No entanto, [alguns navegadores](https://caniuse.com/webgpu) podem não suportar WebGPU para a opção de dispositivo.
- **MiniMax** - requer uma chave de API de [MiniMax](https://www.minimax.io/). Veja a página [MiniMax TTS](./MiniMaxTTS.md) para instruções de configuração.
- **Novel** - requer uma assinatura paga da NovelAI, gerado pelo motor TTS da NovelAI
- **OpenAI** - chave de API paga necessária, usa modelos TTS da OpenAI.
- **Pollinations** - acesso gratuito aos modelos TTS da OpenAI, mas com limite de taxa. [Website](https://pollinations.ai/).
- **Silero** - gratuito, executa no seu PC, a qualidade pode variar amplamente. Requer uma instalação de [servidor de API dedicado](https://github.com/ouoertheo/silero-api-server) ou API Extras (descontinuada).
- **System** - usa o motor TTS do seu SO, se houver um. A qualidade pode variar amplamente dependendo do SO.
- **XTTS** - gratuito, requer uma instalação de servidor de API dedicado. Veja a página [XTTS](./XTTS.md) para instruções de configuração.

### Caixas de seleção

- **Enabled** - liga/desliga a reprodução de TTS
- **Auto Generation** - permite que o TTS comece a tocar automaticamente quando uma nova mensagem entra no chat
- **Only narrate "quotes"** - Limita a reprodução de TTS para incluir apenas texto dentro de `"aspas"`. Isso irá `*incluir "aspas" dentro de linhas de asterisco*` (nome da variável interna = `narrate_quoted_only`)
- **Ignore \*text, even "quotes", inside asterisks\*** - TTS não reproduzirá nenhum texto dentro de `*asteriscos*`, mesmo "aspas" (nome da variável interna = `narrate_dialogues_only`)
- *ter ambas as caixas de seleção "only narrate quotes" e "ignore asterisks" marcadas resultará no TTS apenas lendo "aspas" que não estão em asteriscos, e ignorando todo o resto.*
- **Narrate only the translated text** - isso fará com que o TTS narre apenas o texto traduzido.

Dado o texto de exemplo: `*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
Aqui está uma tabela mostrando como o texto será modificado com base nos estados booleanos de **Ignore \*text, even "quotes", inside asterisks\*** e **Only narrate "quotes"**:

| **Ignore \*text, even "quotes", inside asterisks\*** | **Only narrate "quotes"** | **Output**                                                                |
|:------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------|
| Disabled                                              | Disabled                  | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Disabled                                              | Enabled                   | "nya"... "Good evening, senpai"                                           |
| Enabled                                               | Disabled                  | "Good evening, senpai", she says.                                         |
| Enabled                                               | Enabled                   | "Good evening, senpai"                                                    |

### Controles deslizantes

Estes mudarão dependendo da API que você selecionar.

### Botões

- **Apply** - isso deve ser clicado após definir uma API TTS e após editar o mapa de voz.
- **Refresh** - recarrega a lista de vozes da API TTS selecionada.
- **Available voices** - carrega um popup com todas as vozes disponíveis para sua API selecionada, e permite que você as visualize com diálogos de amostra.

## Usando TTS

1. Clique na caixa de seleção "Enable", ou nada acontecerá.
2. Clique na caixa de seleção "Auto-generation" se você quiser que o TTS inicie automaticamente toda vez que uma nova mensagem chegar no chat.
3. Opcionalmente, clique no ícone de megafone dentro do canto superior direito de qualquer mensagem para reprodução sob demanda.
4. Clique no botão "Stop" no canto inferior direito (encontrado dentro do menu de varinha) para parar qualquer reprodução.

### Mapa de Voz

Você deve fornecer um mapa de voz para o TTS usar, caso contrário, ele não saberá quais vozes devem ser usadas para cada personagem. Para configurar o mapa de voz, primeiro abra um chat com um personagem ao qual você gostaria de atribuir uma voz e/ou selecione uma persona de usuário para atribuir uma voz, depois selecione uma voz listada por um provedor de TTS no dropdown. Se você não vir uma lista de vozes e/ou personagens, certifique-se de que seu provedor de TTS está configurado corretamente e clique em "Refresh". Alguns provedores (como compatíveis com OpenAI ou NovelAI) requerem que você preencha a lista de vozes manualmente.
