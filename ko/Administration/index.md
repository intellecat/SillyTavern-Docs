---
order: 25
icon: gear
expanded: true
route: /ko/administration/
label: 관리
---

# 관리

!!!warning
많은 보안 모범 사례를 따르고 있음에도 불구하고, SillyTavern 서버는 공개 인터넷에 노출되기에는 충분히 안전하지 않습니다.

**적절한 보안 조치를 먼저 보장하지 않고 공개 인터넷에 인스턴스를 절대 호스팅하지 마십시오.**

**부적절하거나 불충분한 보안 구현으로 인한 무단 액세스로 인한 손상이나 손실에 대해 우리는 책임지지 않습니다.**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

SillyTavern의 주요 구성 파일입니다. 네트워크, 보안 및 백엔드 관련 옵션과 같은 다양한 설정이 포함되어 있습니다.
:::

:::callout
**[다중 사용자](multi-user)**

SillyTavern 인스턴스를 다른 사람들과 공유하려면 여러 사용자 계정을 만들 수 있습니다. 각 사용자는 자신만의 설정, 확장 프로그램 및 데이터를 가지고 있습니다. 사용자 계정은 비밀번호로 보호할 수도 있습니다.
:::

:::callout
**[원격 액세스](remote-connections)**

휴대폰, 태블릿 또는 다른 컴퓨터에서 SillyTavern 인스턴스에 액세스할 수 있습니다.
:::

:::callout
**[VPN 및 터널링](tunneling.md)**

인터넷에서 SillyTavern 인스턴스에 액세스하려면 VPN 또는 Cloudflare Zero Trust, ngrok, Tailscale과 같은 터널링 서비스를 사용할 수 있습니다.
:::

:::callout
**[리버스 프록시](reverse-proxying)**

열정적인 사용자는 인터넷에서 SillyTavern 인스턴스에 액세스하기 위해 리버스 프록시를 설정할 수 있습니다.
:::

## 데이터 레이아웃

이 섹션에서는 SillyTavern의 사용자 데이터 저장 구조에 대한 개요를 제공합니다. 여기서는 기본 데이터 레이아웃(데이터 루트가 SillyTavern 설치 디렉터리의 하위 디렉터리인 경우)만 설명합니다. 데이터 레이아웃을 커스터마이징한 경우 해당 사용자 지정 구성을 참조하십시오.

### `data/[user-handle]` (예: `data/default-user`)

각 사용자 계정마다 생성되며, 이 폴더에는 캐릭터 파일, 대화 기록, 설정과 같은 사용자별 데이터가 포함됩니다.

### `data/_cache`

토크나이저 파일 및 transformers.js 모델과 같이 서버가 다운로드한 파일을 위한 저장소입니다.

#### `data/_cache/characters`

`performance.useDiskCache` 설정이 활성화된 경우 파싱된 캐릭터 데이터를 포함하며, 시작 시 및 캐릭터가 업데이트될 때 동기화됩니다.

디스크 공간을 사용하는 대신 캐릭터 데이터를 더 빠르게 로드할 수 있습니다.

### `data/_css`

사용자 지정 CSS 파일을 위한 저장소입니다.

현재는 `user.css` 파일만 지원되며, 이를 통해 프런트엔드에 사용자 지정 스타일을 추가할 수 있습니다.

### `data/_errors`

다양한 HTTP 상태 코드에 대한 오류 페이지를 포함하는 HTML 파일의 저장소입니다. 이 파일들은 서버에서 오류가 발생했을 때 사용자 지정 오류 페이지를 표시하는 데 사용됩니다.

- `forbidden-by-whitelist.html`: IP 주소 화이트리스트에 의해 요청이 차단되었을 때 표시됩니다.
- `host-not-allowed.html`: 호스트 화이트리스트에 의해 요청이 차단되었을 때 표시됩니다.
- `unauthorized.html`: 기본 인증이 실패했을 때 표시됩니다.
- `url-not-found.html`: 요청된 리소스를 찾을 수 없을 때 표시됩니다.

### `data/_storage`

[사용자 계정 데이터](./multi-user.md)를 위한 저장소입니다.

이 파일들을 수동으로 편집하는 것은 권장되지 않으며 데이터 손상을 초래할 수 있습니다.

### `data/_uploads`

서버에서 처리되는 동안 사용자가 업로드한 파일을 위한 임시 저장소입니다.

이 파일들은 시작할 때마다 자동으로 지워집니다.

### `data/_webpack`

컴파일된 webpack 에셋과 캐시 파일을 위한 저장소입니다.

업데이트 후 프런트엔드에 문제가 발생하면 이 폴더를 지워서 프런트엔드 에셋의 전체 재빌드를 강제해 보십시오.

### `data/access.log`

첫 연결 성공 후 서버로 들어오는 HTTP 요청을 기록하는 로그 파일입니다.

의심스러운 활동을 모니터링하기 위해 이 파일을 정기적으로 점검하십시오.

### `data/cookie-secret.txt`

서버에서 쿠키에 서명하는 데 사용되는 비밀 키를 포함합니다.

이 파일이 존재하지 않으면 첫 시작 시 자동으로 생성됩니다.

## 보안 체크리스트

**이것들은 단지 권장사항입니다. ST 인스턴스를 실제로 운영하기 전에 웹 애플리케이션 보안 전문가에게 상담하십시오.**

1. 운영 체제와 Node.js와 같은 런타임 소프트웨어를 최신 상태로 유지하십시오. 이렇게 하면 시스템에 최신 보안 패치와 수정 사항이 적용되어 잠재적인 취약점을 방지하는 데 도움이 됩니다.
2. [화이트리스트](/Administration/config-yaml.md#ip-whitelisting)와 네트워크 방화벽을 사용하십시오. 신뢰할 수 있는 IP 범위만 서버에 액세스할 수 있도록 허용하십시오.
3. [기본 인증](/Administration/config-yaml.md#user-authentication)을 활성화하십시오. 프론트엔드 앱에 액세스하기 전에 "마스터 비밀번호" 역할을 합니다.
4. 또는 외부 인증을 구성하십시오. 이를 위한 알려진 서비스로는 [Authelia](https://www.authelia.com/)와 [authentik](https://goauthentik.io/)이 있습니다. 자세한 내용은 [SSO 가이드](sso.md)를 참조하십시오.
5. 비밀번호 없이 관리자 계정을 남겨두지 마십시오. 보호되지 않은 관리자 계정이 있으면 서버가 시작 시 경고합니다.
6. 로컬 네트워크 외부에서는 신중한 로그인 설정을 사용하십시오. 이렇게 하면 잠재적인 외부인으로부터 사용자 목록이 숨겨집니다.
7. 액세스 로그를 자주 확인하십시오. 로그는 서버 콘솔과 `access.log` 파일에 기록되며 IP 주소 및 사용자 에이전트와 같은 들어오는 연결에 대한 정보를 제공합니다.
8. HTTPS를 구성하십시오. localhost 서버의 경우 자체 서명 인증서를 생성하여 사용할 수 있습니다. 그렇지 않으면 [Traefik](https://traefik.io/) 또는 [Caddy](https://caddyserver.com/docs/getting-started)와 같은 리버스 프록시 웹 서버를 배포해야 할 수 있습니다.
9. [호스트 화이트리스트](/Administration/config-yaml.md#host-whitelisting)를 구성하고 활성화하십시오. 특히 로컬 네트워크에서 HTTPS 암호화를 사용하지 않는 경우 더욱 그렇습니다.
10. SSRF 공격을 방지하기 위해 [사설 주소 화이트리스트](/Administration/config-yaml.md#private-address-whitelisting)를 구성하고 활성화하십시오.

보안 프록시에 대한 자세한 정보는 다음 가이드에서 확인할 수 있습니다: [SillyTavern 리버스 프록시](reverse-proxying).
