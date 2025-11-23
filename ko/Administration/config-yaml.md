---
icon: cpu
route: /administration/config-yaml/
label: 구성 파일
---

# 구성 파일

!!!warning 면책 조항

이 문서는 구식이거나 불완전하거나 부정확할 수 있습니다. 최신 설정 목록은 설치에 있는 [기본 config.yaml](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml)을 참조하십시오.

**경고: 기본 구성을 직접 편집하지 마십시오. 긍정적인 효과가 없습니다. 대신 리포지토리 루트에 있는 사본을 편집하십시오.**
!!!

`config.yaml`은 [설치 완료](/Installation/index.md) 후 리포지토리 루트 디렉터리에서 찾을 수 있는 SillyTavern 서버의 주요 구성 파일입니다. 네트워크, 보안 및 백엔드 관련 옵션과 같은 다양한 설정이 포함된 YAML 파일입니다. **이 파일에 대한 변경 사항은 서버를 재시작한 후에 적용됩니다.**

업스트림에 추가된 새 설정은 [리포지토리 업데이트](/Installation/Updating/index.md) 후 `npm install`을 실행할 때(특히 `post-install.js` 스크립트) 기본값으로 자동으로 채워집니다. 그런 다음 필요에 따라 이러한 설정을 수정할 수 있습니다.

중첩된 설정의 경우 점 표기법을 사용하여 계층 구조를 나타냅니다. 예를 들어 `protocol.ipv6: false`는 `protocol` 섹션 아래의 `ipv6` 설정을 값 `false`로 참조합니다.

```yaml
protocol:
  ipv6: false
```

## 명령줄 인수

SillyTavern 서버를 시작할 때 명령줄 인수를 전달하여 [config.yaml](../Administration/config-yaml.md)의 일부 설정을 재정의할 수 있습니다.

### 예제

```shell
node server.js --port 8000 --listen false
# or
npm run start -- --port 8000 --listen false
# or (Windows only)
Start.bat --port 8000 --listen false
```

### 지원되는 인수

!!!tip
인수는 필수가 아닙니다. 제공하지 않으면 SillyTavern은 `config.yaml`의 설정을 사용합니다.
!!!

| Option                          | Description                                                          | Type     |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | 버전 번호를 표시합니다                                             | boolean  |
| `--global`                      | 애플리케이션 데이터에 시스템 전체 경로 사용을 강제합니다             | boolean  |
| `--configPath`                  | config.yaml 파일 경로를 재정의합니다 (독립 실행형 모드만)    | string   |
| `--dataRoot`                    | 데이터 저장을 위한 루트 디렉터리를 설정합니다 (독립 실행형 모드만)      | string   |
| `--port`                        | SillyTavern이 실행될 포트를 설정합니다                       | number   |
| `--listen`                      | SillyTavern이 모든 네트워크 인터페이스에서 수신 대기하도록 합니다                   | boolean  |
| `--whitelist`                   | 화이트리스트 모드를 활성화합니다                                               | boolean  |
| `--basicAuthMode`               | 기본 인증을 활성화합니다                                         | boolean  |
| `--enableIPv4`                  | IPv4 프로토콜을 활성화합니다                                            | boolean  |
| `--enableIPv6`                  | IPv6 프로토콜을 활성화합니다                                            | boolean  |
| `--listenAddressIPv4`           | 수신 대기할 IPv4 주소를 지정합니다                              | string   |
| `--listenAddressIPv6`           | 수신 대기할 IPv6 주소를 지정합니다                              | string   |
| `--dnsPreferIPv6`               | DNS에 대해 IPv6를 선호합니다                                                 | boolean  |
| `--ssl`                         | SSL을 활성화합니다                                                          | boolean  |
| `--certPath`                    | 인증서 파일 경로를 설정합니다                               | string   |
| `--keyPath`                     | 개인 키 파일 경로를 설정합니다                               | string   |
| `--browserLaunchEnabled`        | 브라우저에서 SillyTavern을 자동으로 시작합니다                    | boolean  |
| `--browserLaunchHostname`       | 브라우저 시작 호스트 이름을 설정합니다                                     | string   |
| `--browserLaunchPort`           | 브라우저 시작을 위한 포트를 재정의합니다                                | string   |
| `--browserLaunchAvoidLocalhost` | 자동 모드에서 브라우저 시작에 'localhost' 사용을 피합니다             | boolean  |
| `--corsProxy`                   | CORS 프록시를 활성화합니다                                               | boolean  |
| `--requestProxyEnabled`         | 나가는 요청에 프록시 사용을 활성화합니다                     | boolean  |
| `--requestProxyUrl`             | 요청 프록시 URL을 설정합니다 (HTTP 또는 SOCKS 프로토콜)                 | string   |
| `--requestProxyBypass`          | 요청 프록시 우회 목록을 설정합니다 (공백으로 구분된 호스트 목록)   | array    |
| `--disableCsrf`                 | CSRF 보호를 비활성화합니다 (권장하지 않음)                           | boolean  |

## 환경 변수

구성은 환경 변수를 통해서도 설정할 수 있으며, 이는 `config.yaml` 파일의 값을 재정의합니다.

환경 변수는 `SILLYTAVERN_` 접두사를 붙이고 설정 이름에 대문자를 사용해야 합니다. 예를 들어 `dataRoot` 설정은 `SILLYTAVERN_DATAROOT` 환경 변수로 재정의할 수 있습니다.

중첩된 설정은 밑줄로 구분해야 합니다. 예를 들어 `protocol.ipv6`는 `SILLYTAVERN_PROTOCOL_IPV6` 환경 변수로 재정의할 수 있습니다.

!!!warning
배열이나 객체를 예상하는 구성은 JSON 문자열로 변환해야 합니다. 예를 들어 `SILLYTAVERN_WHITELIST` 환경 변수로 `whitelist` 설정을 재정의하려면 JSON 문자열로 설정해야 합니다: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`.
!!!

Node.js v20 이상을 사용하는 경우 `.env` 파일에 환경 변수를 저장하고 `--env-file` 플래그로 서버에 전달할 수도 있습니다. 예를 들어 리포지토리 루트에 있는 `.env` 파일을 사용하려면 다음 명령으로 서버를 시작할 수 있습니다:

```bash
node --env-file=.env server.js
```

또는 명령줄을 통해 환경 변수를 직접 전달합니다:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

환경 변수 사용에 대한 자세한 내용은 [Node.js 문서](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)를 참조하십시오.

## 데이터 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `dataRoot` | 사용자 데이터 저장을 위한 루트 디렉터리 (독립 실행형 모드만) | `./data` | 유효한 디렉터리 경로 |
| `skipContentCheck` | 새 기본 콘텐츠 확인을 건너뜁니다 | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | 온디맨드 토크나이저 다운로드를 활성화합니다 | `true` | `true`, `false` |

## 로깅 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `logging.minLogLevel` | 터미널에 표시할 최소 로그 레벨 | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | 서버 액세스 로그를 파일과 콘솔에 기록합니다 | `true` | `true`, `false` |

## [네트워크 구성](/Administration/remote-connections.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `listen` | 들어오는 연결 수신 대기를 활성화합니다 | `false` | `true`, `false` |
| `port` | 서버 수신 대기 포트 | `8000` | 유효한 포트 번호 (1-65535) |
| `protocol.ipv4` | IPv4 프로토콜에서 수신 대기를 활성화합니다 | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | IPv6 프로토콜에서 수신 대기를 활성화합니다 | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | 특정 IPv4 주소에서 수신 대기합니다 | `0.0.0.0` | 유효한 IPv4 주소 |
| `listenAddress.ipv6` | 특정 IPv6 주소에서 수신 대기합니다 | `'[::]'` | 유효한 IPv6 주소 |
| `dnsPreferIPv6` | DNS 확인에 IPv6를 선호합니다 | `false` | `true`, `false` |

## SSL 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `ssl.enabled` | SSL/TLS 암호화 및 HTTPS 프로토콜을 활성화합니다 | `false` | `true`, `false` |
| `ssl.keyPath` | SSL 개인 키 경로 (서버 디렉터리 기준) | `"./certs/privkey.pem"` | 유효한 파일 경로 |
| `ssl.certPath` | SSL 인증서 경로 (서버 디렉터리 기준) | `"./certs/cert.pem"` | 유효한 파일 경로 |
| `ssl.keyPassphrase` | SSL 개인 키의 암호문구. 필요하지 않으면 비워둡니다 | `""` | 문자열 |

## 보안 구성

### IP 화이트리스트

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `whitelistMode` | IP 화이트리스트 필터링을 활성화합니다 | `true` | `true`, `false` |
| `enableForwardedWhitelist` | 화이트리스트에 등록된 IP에 대해 전달된 헤더를 확인합니다 | `true` | `true`, `false` |
| `whitelist` | 허용된 IP 주소 목록 | `["::1", "127.0.0.1"]` | 유효한 IP 주소 배열 |
| `whitelistDockerHosts` | Docker 호스트 IP를 자동으로 화이트리스트에 추가합니다 | `true` | `true`, `false` |

### 호스트 화이트리스트

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `hostWhitelist.enabled` | 호스트 화이트리스트를 활성화합니다 | `false` | `true`, `false` |
| `hostWhitelist.scan` | 신뢰할 수 없는 호스트의 들어오는 요청을 기록합니다 | `true` | `true`, `false` |
| `hostWhitelist.hosts` | 신뢰할 수 있는 호스트 이름 목록 | `[]` | 유효한 호스트 이름 배열 |

### 보안 재정의

!!!danger
**보안 조치를 비활성화하는 것은 매우 권장하지 않습니다. 변경하기 전에 무엇을 하고 있는지 확실히 이해하십시오.**
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `allowKeysExposure` | UI에서 마스크되지 않은 API 키 노출을 허용합니다 | `false` | `true`, `false` |
| `disableCsrfProtection` | CSRF 보호를 비활성화합니다 (권장하지 않음) | `false` | `true`, `false` |
| `securityOverride` | 시작 보안 검사를 비활성화합니다 (권장하지 않음) | `false` | `true`, `false` |

## [사용자 인증](/Administration/multi-user.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `basicAuthMode` | 기본 인증을 활성화합니다 | `false` | `true`, `false` |
| `basicAuthUser.username` | 기본 인증 사용자 이름 | `"user"` | 문자열 |
| `basicAuthUser.password` | 기본 인증 비밀번호 | `"password"` | 문자열 |
| `enableUserAccounts` | 다중 사용자 모드를 활성화합니다 | `false` | `true`, `false` |
| `enableDiscreetLogin` | 로그인 화면에서 사용자 목록을 숨깁니다 | `false` | `true`, `false` |
| `sessionTimeout` | 사용자 세션 타임아웃(초) | `-1` (비활성화) | 숫자 (-1 비활성화, 0 브라우저 닫힘, >0 타임아웃) |
| `perUserBasicAuth` | 기본 인증에 계정 자격 증명을 사용합니다 | `false` | `true`, `false` |

### SSO 자동 로그인

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `sso.autheliaAuth` | Authelia 기반 자동 로그인을 활성화합니다. 참조: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Authentik 기반 자동 로그인을 활성화합니다. 참조: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## 속도 제한 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `rateLimiting.preferRealIpHeader` | 속도 제한에 소켓 IP 대신 X-Real-IP 헤더를 사용합니다 | `false` | `true`, `false` |

## 요청 프록시 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `requestProxy.enabled` | 나가는 요청에 프록시를 활성화합니다 | `false` | `true`, `false` |
| `requestProxy.url` | 프록시 서버 URL | `null` | 유효한 프록시 URL (예: `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | 프록시를 우회할 호스트 | `["localhost", "127.0.0.1"]` | 호스트 이름/IP 배열 |

## CORS 프록시 구성

!!!
활성화된 CORS 프록시는 일부 확장 프로그램에 필요할 수 있습니다. 내장 기능에는 필요하지 않습니다.
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableCorsProxy` | CORS 프록시 미들웨어를 활성화합니다 | `false` | `true`, `false` |

## 브라우저 시작 구성

> 이전에 "Autorun" 설정으로 알려져 있습니다.

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `browserLaunch.enabled` | 서버 시작 시 브라우저를 자동으로 엽니다 | `true` | `true`, `false` |
| `browserLaunch.browser` | URL 열기에 사용할 브라우저 | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | 브라우저 시작을 위한 호스트 이름을 재정의합니다 | `"auto"` | `"auto"`, 유효한 호스트 이름 (예: `"localhost"`, `"st.example.com"`) |
| `browserLaunch.port` | 브라우저 시작을 위한 포트를 재정의합니다 | `-1` | `-1` (서버 포트 사용), 유효한 포트 번호 |
| `browserLaunch.avoidLocalhost` | 시작 URL에서 'localhost' 사용을 피합니다 | `false` | `true`, `false` |

## 성능 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `performance.lazyLoadCharacters` | 캐릭터 데이터를 지연 로드합니다 | `true` | `true`, `false` |
| `performance.useDiskCache` | 캐릭터 카드에 대한 디스크 캐싱을 활성화합니다 | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | 최대 메모리 캐시 용량 | `100mb` | 사람이 읽을 수 있는 크기 (예: `100mb`, `1gb`) |

## 캐시 버스터 구성

!!!warning
localhost 또는 HTTPS가 있는 도메인이 필요하며, 그렇지 않으면 작동하지 않습니다!
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|------------------|
| `cacheBuster.enabled` | 처음 로드 시 또는 이미지 파일 업로드 후 브라우저 캐시를 지웁니다 | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | 지정된 정규식 패턴과 일치하는 사용자 에이전트에 대해서만 캐시를 지웁니다. 예: `'firefox'` (대소문자 구분 없음). | `''` | 유효한 정규식 문자열 |

## 썸네일 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `thumbnails.enabled` | 썸네일 생성을 활성화합니다 | `true` | `true`, `false` |
| `thumbnails.quality` | JPEG 썸네일 품질 | `95` | 0-100 |
| `thumbnails.format` | 썸네일 이미지 형식 | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | 배경 썸네일 크기 | `[160, 90]` | 두 숫자 배열 (너비, 높이) |
| `thumbnails.dimensions.avatar` | 아바타 썸네일 크기 | `[96, 144]` |  두 숫자 배열 (너비, 높이) |
| `thumbnails.dimensions.persona` | 페르소나 썸네일 크기 | `[96, 144]` |  두 숫자 배열 (너비, 높이) |

## 백업 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `backups.chat.enabled` | 자동 채팅 백업을 활성화합니다 | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | 저장하기 전에 채팅 파일의 무결성을 확인합니다 | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | 유지할 백업 수 | `50` | 양의 정수 |
| `backups.chat.throttleInterval` | 백업 조절 간격(ms) | `10000` | 양의 정수 |
| `backups.chat.maxTotalBackups` | 유지할 최대 총 채팅 백업 수 | `-1` | 양의 정수 또는 -1 |

## [확장 프로그램 구성](/extensions/index.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `extensions.enabled` | UI 확장 프로그램을 활성화합니다 | `true` | `true`, `false` |
| `extensions.autoUpdate` | 확장 프로그램 자동 업데이트 (확장 프로그램 매니페스트에서 활성화된 경우) | `true` | `true`, `false` |
| `extensions.models.autoDownload` | 자동 모델 다운로드를 활성화합니다 | `true` | `true`, `false` |
| `extensions.models.classification` | 분류를 위한 HuggingFace 모델 ID | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | 유효한 모델 ID |
| `extensions.models.captioning` | 이미지 캡셔닝을 위한 HuggingFace 모델 ID | `"Xenova/vit-gpt2-image-captioning"` | 유효한 모델 ID |
| `extensions.models.embedding` | 임베딩을 위한 HuggingFace 모델 ID | `"Cohee/jina-embeddings-v2-base-en"` | 유효한 모델 ID |
| `extensions.models.speechToText` | 음성-텍스트 변환을 위한 HuggingFace 모델 ID | `"Xenova/whisper-small"` | 유효한 모델 ID |
| `extensions.models.textToSpeech` | 텍스트-음성 변환을 위한 HuggingFace 모델 ID | `"Xenova/speecht5_tts"` | 유효한 모델 ID |

## [서버 플러그인](/For_Contributors/Server-Plugins.md)

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `enableServerPlugins` | 서버 측 플러그인을 활성화합니다 | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | 시작 시 서버 플러그인 자동 업데이트를 시도합니다 | `true` | `true`, `false` |

## [API 통합 설정](/Usage/API_Connections/index.md)

### OpenAI 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `promptPlaceholder` | 빈 프롬프트를 위한 기본 메시지 | `"[Start a new chat]"` | 문자열 |
| `openai.randomizeUserId` | API 호출을 위한 사용자 ID를 무작위화합니다 | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | 캡션 완성을 위한 시스템 메시지 | `""` | 문자열 |

### MistralAI 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `mistral.enablePrefix` | 응답 미리 채우기를 활성화합니다. **접두사가 응답에 반영됩니다** | `false` | `true`, `false` |

### Ollama 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `ollama.keepAlive` | 모델 keep-alive 기간(초) | `-1` | `-1` (무기한), `0` (즉시 언로드), 양의 정수 |
| `ollama.batchSize` | 생성 요청의 "num_batch" (배치 크기) 매개변수를 제어합니다 | `-1` | `-1` (모델 기본값), 양의 정수 |

### Claude 구성

!!!warning **중요!**

프롬프트 접두사가 정적이고 요청 간에 변경되지 않을 때만 주의해서 사용하십시오. \{\{random\}\} 매크로, 로어북, 벡터, 요약 등은 캐시를 무효화할 가능성이 높으며 캐시 미스로 인해 돈을 낭비하게 됩니다. 동작이 예측 불가능할 수 있으며 어떠한 보장도 할 수 없습니다.

참조: [프롬프트 캐싱](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `claude.enableSystemPromptCache` | 시스템 프롬프트 캐싱을 활성화합니다 | `false` | `true`, `false` |
| `claude.cachingAtDepth` | 메시지 기록 캐싱을 활성화합니다 | `-1` | `-1` (비활성화), `0` 또는 양의 정수 |
| `claude.extendedTTL` | 기본 5분 대신 1시간 TTL을 사용합니다. 이것은 요청 비용도 증가시킵니다. | `false` | `true`, `false` |

### Google AI Studio 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `gemini.apiVersion` | API 엔드포인트 버전 | `v1beta` | `v1beta`, `v1alpha` |

### DeepL 구성

| Setting | Description | Default | Permitted Values |
|---------|-------------|---------|-----------------|
| `deepl.formality` | 번역 격식 레벨 | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
