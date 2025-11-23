---
icon: cpu
route: /administration/config-yaml/
---

# Arquivo de Configuração

!!!warning Aviso

Esta documentação pode estar obsoleta, incompleta ou incorreta. Por favor, consulte o [config.yaml padrão](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) na sua instalação para a lista mais atualizada de configurações.

**AVISO: NÃO EDITE O CONFIG PADRÃO DIRETAMENTE. ISSO NÃO TERÁ NENHUM EFEITO POSITIVO. EDITE SUA CÓPIA NO DIRETÓRIO RAIZ DO REPOSITÓRIO EM VEZ DISSO.**
!!!

`config.yaml` é o arquivo de configuração principal para o servidor SillyTavern que você pode encontrar no diretório raiz do repositório após [completar a instalação](/Installation/index.md). É um arquivo YAML que contém várias configurações, como rede, segurança e opções específicas de backend. **As alterações feitas neste arquivo entrarão em vigor após reiniciar o servidor.**

Novas configurações que são adicionadas upstream são automaticamente populadas com valores padrão quando você executa `npm install` (especificamente, o script `post-install.js`) após [atualizar o repositório](/Installation/Updating/index.md). Você pode então modificar essas configurações conforme necessário.

Para configurações aninhadas, a notação de ponto é usada para indicar a hierarquia. Por exemplo, `protocol.ipv6: false` refere-se à configuração `ipv6` na seção `protocol` com um valor de `false`.

```yaml
protocol:
  ipv6: false
```

## Argumentos de Linha de Comando

Você pode passar argumentos de linha de comando ao iniciar o servidor SillyTavern para sobrescrever algumas configurações em [config.yaml](../Administration/config-yaml.md).

### Exemplos

```shell
node server.js --port 8000 --listen false
# or
npm run start -- --port 8000 --listen false
# or (Windows only)
Start.bat --port 8000 --listen false
```

### Argumentos suportados

!!!tip
Nenhum dos argumentos é obrigatório. Se você não fornecê-los, o SillyTavern usará as configurações em `config.yaml`.
!!!

| Option                          | Description                                                          | Type     |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | Mostra o número da versão                                            | boolean  |
| `--global`                      | Força o uso de caminhos do sistema para dados da aplicação           | boolean  |
| `--configPath`                  | Sobrescreve o caminho para o arquivo config.yaml (apenas modo standalone)    | string   |
| `--dataRoot`                    | Define o diretório raiz para armazenamento de dados (apenas modo standalone)      | string   |
| `--port`                        | Define a porta sob a qual o SillyTavern será executado                       | number   |
| `--listen`                      | Faz o SillyTavern ouvir em todas as interfaces de rede                   | boolean  |
| `--whitelist`                   | Habilita o modo whitelist                                               | boolean  |
| `--basicAuthMode`               | Habilita autenticação básica                                         | boolean  |
| `--enableIPv4`                  | Habilita o protocolo IPv4                                            | boolean  |
| `--enableIPv6`                  | Habilita o protocolo IPv6                                            | boolean  |
| `--listenAddressIPv4`           | Especifica o endereço IPv4 para ouvir                              | string   |
| `--listenAddressIPv6`           | Especifica o endereço IPv6 para ouvir                              | string   |
| `--dnsPreferIPv6`               | Prefere IPv6 para DNS                                                 | boolean  |
| `--ssl`                         | Habilita SSL                                                          | boolean  |
| `--certPath`                    | Define o caminho para seu arquivo de certificado                               | string   |
| `--keyPath`                     | Define o caminho para seu arquivo de chave privada                               | string   |
| `--browserLaunchEnabled`        | Inicia automaticamente o SillyTavern no navegador                    | boolean  |
| `--browserLaunchHostname`       | Define o hostname de inicialização do navegador                                     | string   |
| `--browserLaunchPort`           | Sobrescreve a porta para inicialização do navegador                                | string   |
| `--browserLaunchAvoidLocalhost` | Evita usar 'localhost' para inicialização do navegador em modo auto             | boolean  |
| `--corsProxy`                   | Habilita o proxy CORS                                               | boolean  |
| `--requestProxyEnabled`         | Habilita o uso de um proxy para requisições de saída                     | boolean  |
| `--requestProxyUrl`             | Define a URL do proxy de requisição (protocolos HTTP ou SOCKS)                 | string   |
| `--requestProxyBypass`          | Define a lista de bypass do proxy de requisição (lista separada por espaços de hosts)   | array    |
| `--disableCsrf`                 | Desabilita proteção CSRF (NÃO RECOMENDADO)                           | boolean  |

## Variáveis de Ambiente

A configuração também pode ser definida via variáveis de ambiente, que sobrescreverão os valores no arquivo `config.yaml`.

As variáveis de ambiente devem ser prefixadas com `SILLYTAVERN_` e usar letras maiúsculas para os nomes das configurações. Por exemplo, a configuração `dataRoot` pode ser sobrescrita com a variável de ambiente `SILLYTAVERN_DATAROOT`.

As configurações aninhadas devem ser separadas por underscores. Por exemplo, `protocol.ipv6` pode ser sobrescrito com a variável de ambiente `SILLYTAVERN_PROTOCOL_IPV6`.

!!!warning
Configurações que esperam arrays ou objetos devem ser stringified como JSON. Por exemplo, para sobrescrever a configuração `whitelist` com a variável de ambiente `SILLYTAVERN_WHITELIST`, você deve defini-la como uma string JSON: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`.
!!!

Se você estiver usando Node.js v20 ou posterior, você também pode armazenar variáveis de ambiente em um arquivo `.env` e passá-lo para o servidor com a flag `--env-file`. Por exemplo, para usar o arquivo `.env` localizado no diretório raiz do repositório, você pode iniciar o servidor com o seguinte comando:

```bash
node --env-file=.env server.js
```

Alternativamente, passe as variáveis de ambiente diretamente via linha de comando:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

Veja mais sobre o uso de variáveis de ambiente na [documentação do Node.js](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs).

## Configuração de Dados

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `dataRoot` | Diretório raiz para armazenamento de dados do usuário (apenas modo standalone) | `./data` | Qualquer caminho de diretório válido |
| `skipContentCheck` | Pular verificações de novo conteúdo padrão | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | Habilitar downloads de tokenizador sob demanda | `true` | `true`, `false` |

## Configuração de Registro

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | Nível mínimo de log para exibir no terminal | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | Escrever logs de acesso do servidor em um arquivo e no console | `true` | `true`, `false` |

## [Configuração de Rede](/Administration/remote-connections.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `listen` | Habilitar escuta de conexões recebidas | `false` | `true`, `false` |
| `port` | Porta de escuta do servidor | `8000` | Qualquer número de porta válido (1-65535) |
| `protocol.ipv4` | Habilitar escuta no protocolo IPv4 | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | Habilitar escuta no protocolo IPv6 | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | Ouvir em um endereço IPv4 específico | `0.0.0.0` | Endereço IPv4 válido |
| `listenAddress.ipv6` | Ouvir em um endereço IPv6 específico | `'[::]'` | Endereço IPv6 válido |
| `dnsPreferIPv6` | Preferir IPv6 para resolução DNS | `false` | `true`, `false` |

## Configuração SSL

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `ssl.enabled` | Habilitar criptografia SSL/TLS e protocolo HTTPS | `false` | `true`, `false` |
| `ssl.keyPath` | Caminho para chave privada SSL (relativo ao diretório do servidor) | `"./certs/privkey.pem"` | Caminho de arquivo válido |
| `ssl.certPath` | Caminho para certificado SSL (relativo ao diretório do servidor) | `"./certs/cert.pem"` | Caminho de arquivo válido |
| `ssl.keyPassphrase` | Frase secreta para a chave privada SSL. Deixe vazio se não for necessário | `""` | Qualquer string |

## Configuração de Segurança

### Whitelist de IP

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `whitelistMode` | Habilitar filtragem de whitelist de IP | `true` | `true`, `false` |
| `enableForwardedWhitelist` | Verificar cabeçalhos encaminhados para IPs na whitelist | `true` | `true`, `false` |
| `whitelist` | Lista de endereços IP permitidos | `["::1", "127.0.0.1"]` | Array de endereços IP válidos |
| `whitelistDockerHosts` | Adicionar automaticamente IPs de host Docker à whitelist | `true` | `true`, `false` |

### Whitelist de Host

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | Habilitar whitelist de host | `false` | `true`, `false` |
| `hostWhitelist.scan` | Registrar requisições recebidas de hosts não confiáveis | `true` | `true`, `false` |
| `hostWhitelist.hosts` | Lista de nomes de host confiáveis | `[]` | Array de nomes de host válidos |

### Sobrescritas de Segurança

!!!danger
**DESABILITAR MEDIDAS DE SEGURANÇA É ALTAMENTE DESENCORAJADO. POR FAVOR, CERTIFIQUE-SE DE QUE VOCÊ ENTENDE O QUE ESTÁ FAZENDO ANTES DE FAZER ALTERAÇÕES.**
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | Permitir exposição de chave de API não mascarada na UI | `false` | `true`, `false` |
| `disableCsrfProtection` | Desabilitar proteção CSRF (não recomendado) | `false` | `true`, `false` |
| `securityOverride` | Desabilitar verificações de segurança na inicialização (não recomendado) | `false` | `true`, `false` |

## [Autenticação de Usuário](/Administration/multi-user.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | Habilitar autenticação básica | `false` | `true`, `false` |
| `basicAuthUser.username` | Nome de usuário para autenticação básica | `"user"` | Qualquer string |
| `basicAuthUser.password` | Senha para autenticação básica | `"password"` | Qualquer string |
| `enableUserAccounts` | Habilitar modo multiusuário | `false` | `true`, `false` |
| `enableDiscreetLogin` | Ocultar a lista de usuários na tela de login | `false` | `true`, `false` |
| `sessionTimeout` | Timeout de sessão do usuário em segundos | `-1` (desabilitado) | Qualquer número (-1 para desabilitar, 0 para fechamento do navegador, >0 para timeout) |
| `perUserBasicAuth` | Usar credenciais da conta para autenticação básica | `false` | `true`, `false` |

### Login Automático SSO

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | Habilitar login automático baseado em Authelia. Veja: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Habilitar login automático baseado em Authentik. Veja: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## Configuração de Limitação de Taxa

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | Usar o cabeçalho X-Real-IP em vez do IP do socket para limitação de taxa | `false` | `true`, `false` |

## Configuração de Proxy de Requisição

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | Habilitar proxy para requisições de saída | `false` | `true`, `false` |
| `requestProxy.url` | URL do servidor proxy | `null` | URL de proxy válida (ex: `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | Hosts para ignorar o proxy | `["localhost", "127.0.0.1"]` | Array de nomes de host/IPs |

## Configuração de Proxy CORS

!!!
Um proxy CORS habilitado pode ser necessário para algumas extensões. Não é necessário para nenhum recurso integrado.
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | Habilitar middleware de proxy CORS | `false` | `true`, `false` |

## Configuração de Inicialização do Navegador

> Anteriormente conhecido como configurações de "Autorun".

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | Abrir o navegador automaticamente na inicialização do servidor | `true` | `true`, `false` |
| `browserLaunch.browser` | Navegador para usar ao abrir a URL | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | Sobrescrever o hostname para inicialização do navegador | `"auto"` | `"auto"`, qualquer hostname válido (ex: `"localhost"`, `"st.example.com"`) |
| `browserLaunch.port` | Sobrescrever a porta para inicialização do navegador | `-1` | `-1` (usar porta do servidor), qualquer número de porta válido |
| `browserLaunch.avoidLocalhost` | Evitar usar 'localhost' na URL de inicialização | `false` | `true`, `false` |

## Configuração de Desempenho

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | Carregamento preguiçoso de dados de personagem | `true` | `true`, `false` |
| `performance.useDiskCache` | Habilitar cache em disco para cartões de personagem | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | Capacidade máxima de cache de memória | `100mb` | Tamanho legível (ex: `100mb`, `1gb`) |

## Configuração de Cache Buster

!!!warning
Requer localhost ou um domínio com HTTPS, caso contrário não funcionará!
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | Limpar cache do navegador no primeiro carregamento ou após upload de arquivos de imagem | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | Apenas limpar o cache para user agents que correspondam ao padrão regex especificado. Exemplo: `'firefox'` (case-insensitive). | `''` | Qualquer string regex válida |

## Configuração de Miniaturas

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | Habilitar geração de miniaturas | `true` | `true`, `false` |
| `thumbnails.quality` | Qualidade de miniatura JPEG | `95` | 0-100 |
| `thumbnails.format` | Formato de imagem para miniaturas | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | Tamanho de miniaturas de fundo | `[160, 90]` | Array de dois números (largura, altura) |
| `thumbnails.dimensions.avatar` | Tamanho de miniaturas de avatar | `[96, 144]` |  Array de dois números (largura, altura) |
| `thumbnails.dimensions.persona` | Tamanho de miniaturas de persona | `[96, 144]` |  Array de dois números (largura, altura) |

## Configuração de Backup

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | Habilitar backups automáticos de chat | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | Verificar integridade dos arquivos de chat antes de salvar | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | Número de backups a manter | `50` | Qualquer inteiro positivo |
| `backups.chat.throttleInterval` | Intervalo de limitação de backup (ms) | `10000` | Qualquer inteiro positivo |
| `backups.chat.maxTotalBackups` | Máximo total de backups de chat a manter | `-1` | Qualquer inteiro positivo ou -1 |

## [Configuração de Extensões](/extensions/index.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | Habilitar extensões de UI | `true` | `true`, `false` |
| `extensions.autoUpdate` | Atualizar automaticamente extensões (se habilitado pelo manifesto da extensão) | `true` | `true`, `false` |
| `extensions.models.autoDownload` | Habilitar downloads automáticos de modelo | `true` | `true`, `false` |
| `extensions.models.classification` | ID do modelo HuggingFace para classificação | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | ID de modelo válido |
| `extensions.models.captioning` | ID do modelo HuggingFace para legendagem de imagem | `"Xenova/vit-gpt2-image-captioning"` | ID de modelo válido |
| `extensions.models.embedding` | ID do modelo HuggingFace para embeddings | `"Cohee/jina-embeddings-v2-base-en"` | ID de modelo válido |
| `extensions.models.speechToText` | ID do modelo HuggingFace para speech-to-text | `"Xenova/whisper-small"` | ID de modelo válido |
| `extensions.models.textToSpeech` | ID do modelo HuggingFace para text-to-speech | `"Xenova/speecht5_tts"` | ID de modelo válido |

## [Plugins de Servidor](/For_Contributors/Server-Plugins.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | Habilitar plugins do lado do servidor | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | Tentar atualizar automaticamente plugins de servidor na inicialização | `true` | `true`, `false` |

## [Configurações de Integração de API](/Usage/API_Connections/index.md)

### Configuração OpenAI

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | Mensagem padrão para prompts vazios | `"[Start a new chat]"` | Qualquer string |
| `openai.randomizeUserId` | Randomizar o ID do usuário para chamadas de API | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | Mensagem do sistema para conclusão de legenda | `""` | Qualquer string |

### Configuração MistralAI

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | Habilitar preenchimento de resposta. **O prefixo será ecoado na resposta** | `false` | `true`, `false` |

### Configuração Ollama

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | Duração de manutenção do modelo (segundos) | `-1` | `-1` (indefinido), `0` (descarregamento imediato), inteiro positivo |
| `ollama.batchSize` | Controla o parâmetro "num_batch" (tamanho do lote) da requisição de geração | `-1` | `-1` (padrão do modelo), inteiro positivo |

### Configuração Claude

!!!warning **IMPORTANTE!**

Use com cautela e apenas quando o prefixo do prompt for estático e não mudar entre requisições. Macro \{\{random\}\}, lorebooks, vetores, resumos, etc. provavelmente invalidarão o cache e você só desperdiçará dinheiro em cache misses. O comportamento pode ser imprevisível e nenhuma garantia pode ou será feita.

Veja: [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | Habilitar cache de prompt do sistema | `false` | `true`, `false` |
| `claude.cachingAtDepth` | Habilitar cache de histórico de mensagens | `-1` | `-1` (desabilitado), `0` ou inteiro positivo |
| `claude.extendedTTL` | Usar TTL de 1h em vez dos 5m padrão. Note que isso também aumenta o custo da requisição. | `false` | `true`, `false` |

### Configuração Google AI Studio

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | Versão do endpoint da API | `v1beta` | `v1beta`, `v1alpha` |

### Configuração DeepL

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `deepl.formality` | Nível de formalidade da tradução | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
