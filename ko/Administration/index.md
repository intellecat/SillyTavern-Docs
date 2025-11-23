---
order: 25
icon: gear
expanded: true
route: /administration/
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

보안 프록시에 대한 자세한 정보는 다음 가이드에서 확인할 수 있습니다: [SillyTavern 리버스 프록시](reverse-proxying).
