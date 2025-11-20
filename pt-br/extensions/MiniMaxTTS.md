---
order: tts-minimax
route: /extensions/minimaxtts/
---

# MiniMax TTS

Esta página ensinará como usar adequadamente o provedor MiniMax TTS.

## Pré-requisitos

1. Conta MiniMax com acesso à API
2. API Key e Group ID válidos da MiniMax

## Obtendo Credenciais da API

### 1. Criar uma Conta MiniMax

1. Visite o [site da MiniMax (Internacional)](https://www.minimax.io/)
2. Clique em "Sign Up" ou "Login"
3. Complete o processo de registro da conta

!!!warning Diferenças Regionais
MiniMax possui versões chinesa e internacional separadas. Observe que:
- A versão chinesa não suporta recursos de clonagem de voz
- A versão chinesa suporta apenas o host de API `api.minimax.chat`
!!!

### 2. Obter API Key e Group ID

1. Faça login no [console MiniMax (Internacional)](https://www.minimax.io/platform/user-center/basic-information)
2. Você pode encontrar seu GroupId na página Basic Information
3. Vá para Settings → API Keys na barra lateral esquerda para criar e obter sua API Key

## Configuração no SillyTavern

### 1. Configuração Básica

1. Abra o SillyTavern
2. Navegue até "Extensions" → "TTS"
3. Selecione "MiniMax" como seu provedor TTS
4. Configure as seguintes configurações:
    - **API Key**: Sua chave API MiniMax
    - **Group ID**: Seu Group ID MiniMax
    - **API Host**: Escolha o servidor apropriado baseado em sua região:
        - `api.minimax.io` (Servidor internacional oficial)
        - `api.minimaxi.chat` (Outro host de servidor internacional)
        - `api.minimax.chat` (Servidor da China continental)

### 2. Seleção de Modelo

Os modelos disponíveis incluem:
- **Speech-02-HD**: Síntese de voz de alta qualidade (recomendado)
- **Speech-02-Turbo**: Síntese de voz rápida
- **Speech-01**: Modelo legado
- **Speech-01-240228**: Modelo legado (versão específica)

### 3. Parâmetros de Voz

Ajuste os seguintes parâmetros para personalizar a saída de voz:
- **Speed**: 0.5 - 2.0 (1.0 = velocidade normal)
- **Volume**: 0.1 - 2.0 (1.0 = volume normal)
- **Pitch**: 0.5 - 2.0 (1.0 = tom normal)
- **Audio Format**: MP3, WAV, FLAC

## Vozes Personalizadas

### 1. Obtendo Voice IDs

1. Acesse a [página MiniMax TTS (Internacional)](https://www.minimax.io/audio/text-to-speech)
2. Clique em "Voice" no lado direito para entrar na interface de Seleção de Voz
3. Encontre a voz que deseja usar
4. Clique no botão copiar ao lado do nome da voz para copiar o Voice ID

### 2. Adicionando Vozes Personalizadas

1. Nas configurações do MiniMax TTS, localize a seção "Custom Voice Management"
2. Preencha as seguintes informações:
    - **Voice Name**: Escolha qualquer nome para identificação
    - **Voice ID**: O voice ID obtido da plataforma MiniMax
    - **Language**: Selecione o idioma correspondente para a voz
3. Clique em "Add Custom Voice"

## Modelos Personalizados

### 1. Adicionando Modelos Personalizados

1. Na seção "Custom Model Management"
2. Preencha:
    - **Model ID**: Identificador do modelo
    - **Model Name**: Nome de exibição para o modelo
3. Clique em "Add Custom Model"

### 2. Obtendo Model IDs

1. Verifique a lista de modelos na [documentação oficial da MiniMax](https://www.minimax.io/platform/document/Model?key=684261f14c5738213294faa7)
2. Ou visualize modelos personalizados disponíveis no console
3. Copie o Model ID correspondente

## Solução de Problemas

### Problemas Comuns

1. **Falha na Autenticação da API**
    - Verifique se a API Key corresponde ao API Host correto
    - Confirme se o Group ID está correto
    - Verifique se sua conta tem saldo suficiente

2. **Falha na Geração de Voz**
    - Verifique se o Voice ID selecionado é válido
    - Certifique-se de que a voz é compatível com seu modelo selecionado

3. **Tempo Limite de Conexão**
    - Tente mudar para um API Host diferente
    - Verifique sua conexão de rede
    - Verifique as configurações do firewall

4. **Problemas de Qualidade de Áudio**
    - Tente usar um modelo diferente (Speech-02-HD para melhor qualidade)
    - Ajuste os parâmetros de voz (velocidade, tom, volume)
    - Verifique a compatibilidade do formato de áudio
