---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk é um sistema de clonagem de voz baseado em Coqui XTTS, F5-TTS, VITS, Piper e outros mecanismos de modelo TTS, projetado para produzir reprodução de voz de alta qualidade (seja clonagem de voz zero shot ou vozes integradas). No AllTalk V2, atualizações significativas aprimoram a funcionalidade e facilidade de uso, incluindo suporte para múltiplos mecanismos TTS, personalização expandida e otimizações de desempenho. Para uma lista abrangente de recursos, consulte a [Wiki do AllTalk aqui](https://github.com/erew123/alltalk_tts/wiki).

---

## 🟩 Recursos Principais no AllTalk V2
- **Suporte Multi-mecanismo**: Alterne facilmente entre Coqui XTTS, VITS, Piper, Parler, F5 e mecanismos personalizados.
- **Conversão de Voz (RVC)**: Pipeline aprimorado de clonagem de voz baseada em recuperação.
- **Configurações Personalizáveis**: Ajuste configurações por mecanismo e salve configurações de inicialização.
- **Funcionalidade de Narrador**: Especifique vozes separadas para narração e personagens.
- **Uso Standalone e Integrado**: Integração perfeita com SillyTavern.
- **Modos DeepSpeed e Low VRAM**: Otimização de desempenho para ambientes com recursos limitados.
- **Capturas de Tela**: Veja a interface do AllTalk V2 [aqui](https://github.com/erew123/alltalk_tts/discussions/237).

---

## 🟨 Opções de Configuração e Instalação

AllTalk oferece métodos de instalação standalone e integrados. A configuração mais rápida envolve usar uma das opções de instalação rápida fornecidas, com scripts automatizando a maior parte do processo.

- **Instalação Standalone**: Recomendado para a maioria dos usuários ([Guia Standalone](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Integração Text-generation-webui**: Para integração no Text-generation-webui ([Guia de Instalação TGWUI](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 Instalação Automatizada
**Este método é apenas para usuários Windows.**
Para novos usuários que desejam uma configuração rápida, a instalação automatizada usa o SillyTavern-Launcher.
Nota: Isso pressupõe que você já instalou o SillyTavern-Launcher. Se não o fez, visite https://github.com/SillyTavern/SillyTavern-Launcher e siga as instruções no arquivo readme.md para instalá-lo.
Uma vez que o SillyTavern-Launcher esteja instalado:
1. Execute Launcher.bat
2. Vá para: `Home > Toolbox > App Installer > Voice Generation`
3. Selecione a opção rotulada: **Install AllTalk V2**

#### 🟩 Instalação Manual
Para usuários avançados que requerem controle detalhado, siga o [Guia de Instalação Manual](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide) para uma configuração passo a passo no Windows, Linux ou Mac (não testado).

#### 🟩 Instalação Google Colab
Execute AllTalk em um ambiente de nuvem com a [Instalação Google Colab](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB) para usuários que preferem não instalar localmente.

---

## 🟨 Usando AllTalk dentro do SillyTavern

Uma vez que AllTalk esteja carregado, selecione-o dentro do SillyTavern na página TTS, garantindo selecionar a versão correta do servidor AllTalk nas configurações.

- **Gerenciamento de Configurações**: AllTalk pode habilitar ou desabilitar configurações específicas com base na sua configuração selecionada.
- **Sequência de Carregamento**: Se SillyTavern for carregado antes de AllTalk, recarregue a página de extensões TTS.
- **Otimização de Desempenho**: Habilite modos DeepSpeed e Low VRAM seletivamente para melhorar o desempenho com base nos recursos do sistema.
- **Função de narrador**: Detalhes da função Narrator podem ser encontrados na [Wiki AllTalk](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

Detalhes completos da Extensão AllTalk para SillyTavern serão atualizados na [página Wiki do AllTalk para SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

Usuários TGWUI que usam a extensão AllTalk para TGWUI precisam desabilitar `Enable TGWUI TTS` na interface de chat TGWUI, caso contrário você terá áudio TTS duplicado gerado.

---

## 🟨 Solução de Problemas

Se você tiver problemas que acredita serem específicos ao AllTalk dentro do SillyTavern, consulte a [página Wiki do AllTalk para SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) para as informações mais recentes.

---

### 🟪 Suporte, Assistência e Solicitações de Recursos

Para assistência adicional:
- Consulte a [Wiki](https://github.com/erew123/alltalk_tts/wiki) e documentação integrada.
- Participe de discussões no [Fórum de Discussão](https://github.com/erew123/alltalk_tts/discussions/245).
- Envie bugs ou solicitações de recursos através do [Rastreador de Problemas](https://github.com/erew123/alltalk_tts/issues).

---
