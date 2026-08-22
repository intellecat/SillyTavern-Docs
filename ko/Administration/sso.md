---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /ko/administration/sso/
---

# Single Sign-On (SSO)

!!!warning
SSO는 여러 장점에도 불구하고 설정하고 유지하기 복잡한 시스템입니다. 올바르게 구성하지 않으면 인스턴스에 대한 무단 액세스로 이어질 수 있습니다. SSO를 활성화하기 전에 설정을 올바르게 구성하고 테스트하십시오. 보안적 영향이 확실하지 않다면 SSO 자동 로그인을 비활성화한 상태로 유지하고 다른 인증 방법을 사용하는 것을 권장합니다.
!!!

SSO를 사용하면 사용자를 만들고 보안을 유지하려는 사이트에 제공되는 로그인 포털을 사용하여 여러 페이지를 보호할 수 있습니다. 설정이 복잡하지만 SSO를 배우고 인터넷에서 ST 인스턴스를 더 안전하게 보호하는 좋은 방법입니다.

SSO는 [원격 연결](/Administration/remote-connections.md)을 위한 액세스 제어 메커니즘으로 [HTTP 기본 인증](/Administration/config-yaml.md#user-authentication)을 대체할 수도 있습니다.

SSO가 HTTP 기본 인증보다 더 나은 보안과 기능을 제공하므로 권장됩니다.

[**Authelia**](https://www.authelia.com/)와 [**Authentik**](https://goauthentik.io/)은 SillyTavern과 함께 사용할 수 있는 오픈 소스 SSO 제공업체입니다.

## 신뢰할 수 있는 프록시 구성

필요한 헤더를 전달하여 사용자를 인증할 수 있는 것은 신뢰할 수 있는 프록시로 구성된 IP 주소의 요청뿐입니다. 기본적으로 IPv4와 IPv6 루프백 주소가 모두 신뢰됩니다. 다른 IP가 SSO 헤더로 인증할 수 있도록 하려면 [config.yaml](/Administration/config-yaml.md#sso-auto-login) 파일의 `sso.trustedProxies` 목록에 추가하십시오:

!!!tip
여러 IP 또는 범위를 지정하기 위해 CIDR 및 와일드카드 표기법도 지원합니다. 예: `192.168.0.*` 또는 `10.0.0.0/24`.
!!!

```yaml
sso:
  trustedProxies:
    - ::1           # IPv6 loopback address - trusted by default
    - 127.0.0.1     # IPv4 loopback address - trusted by default
    - '192.168.0.1' # Example IP address of a trusted proxy
```

## SSO로 로그인

SSO 제공 사용자 이름이 SillyTavern 사용자 계정의 사용자 핸들과 **정확히** 일치하면 SSO로 해당 사용자로 SillyTavern에 로그인할 수 있습니다. 이 기능을 활성화하려면 [config.yaml](/Administration/config-yaml.md#sso-auto-login) 파일에서 다음 옵션 중 하나를 변경하십시오:

### Authelia

```yaml
sso:
  autheliaAuth: true
```

### Authentik

```yaml
sso:
  authentikAuth: true
```

두 옵션 모두 [다중 사용자 모드](/Administration/multi-user.md) 설정의 내장 [비밀번호 관리](/Usage/User_Settings/index.md#account-management) 구성 요소를 보강하거나 대체합니다.
