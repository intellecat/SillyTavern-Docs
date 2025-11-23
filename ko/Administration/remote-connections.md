---
icon: rss
order: -30
route: /usage/remoteconnections/
label: 원격 연결
---

# 원격 연결

대부분의 경우 이것은 동일한 WiFi 네트워크 내에서 PC가 ST 서버를 실행하는 동안 휴대폰에서 SillyTavern을 사용하려는 사람들을 위한 것입니다.

로컬 네트워크 외부에서 원격 연결을 허용하는 첫 번째 단계이기도 합니다.

!!!warning
포트 포워딩을 사용하여 ST 서버를 인터넷에 노출해서는 안 됩니다. 대신 VPN 또는 Cloudflare Zero Trust, ngrok, Tailscale과 같은 터널링 서비스를 사용하십시오. 자세한 내용은 [VPN 및 터널링](tunneling.md) 가이드를 참조하십시오.
!!!

!!!danger 면책 조항
**적절한 보안 조치를 먼저 보장하지 않고 공개 인터넷에 인스턴스를 절대 호스팅하지 마십시오.**

**부적절하거나 불충분한 보안 구현으로 인한 무단 액세스의 경우 손상이나 손실에 대해 우리는 책임지지 않습니다.**
!!!

## 원격 연결 허용

기본적으로 ST 서버는 실행 중인 컴퓨터(localhost)에서만 연결을 허용합니다. 다른 장치의 연결을 수신하도록 하려면 `config.yaml`의 `listen` 옵션을 `true`로 설정하십시오.

!!! SillyTavern 폴더에서 `config.yaml`을 직접 검색하면 두 개의 파일을 찾을 수 있습니다.
이 문서에서 `config.yaml`에 대한 모든 수정은 `/SillyTavern/default/config.yaml`이 아닌 SillyTavern 루트 디렉터리(/SillyTavern/config.yaml)의 것을 참조합니다.
!!!

```yaml
# Listen for incoming connections
listen: true
```

ST가 원격 연결을 수신 대기 중일 때 콘솔에 다음 메시지가 표시됩니다:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

그리고 그것이 무엇을 의미하는지에 대한 몇 가지 설명이 있습니다.

ST가 원격 연결을 수신 대기하지 **않을** 때 콘솔에 다음 메시지가 표시됩니다:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## 액세스 제어 구성

원격 연결 수신을 활성화한 후 최소한 하나의 액세스 제어 방법을 구성해야 합니다. 그렇지 않으면 서버가 시작되지 않습니다.

### 화이트리스트 기반 액세스 제어

화이트리스트를 통한 액세스 제어를 활성화하려면 SillyTavern 루트 디렉터리(`/SillyTavern/config.yaml`)의 `config.yaml` 파일을 편집하십시오:

1. 필요한 구성 파일을 생성하려면 SillyTavern을 최소한 한 번 시작하십시오.
2. 텍스트 편집기에서 `/SillyTavern/config.yaml`을 엽니다.
3. `whitelist` 섹션을 찾아 허용하려는 IP 주소를 추가합니다:
    * 각 IP 주소를 별도로 나열합니다.
    * `127.0.0.1`이 포함되어 있는지 확인하십시오. 그렇지 않으면 호스트 컴퓨터에서 연결할 수 없습니다.
    * 개별 IP, CIDR 마스크(예: `10.0.0.0/24`) 및 와일드카드(`*`) 범위를 지원합니다.
4. `config.yaml` 파일을 저장합니다.
5. **SillyTavern 서버를 재시작합니다.**

#### 예제 `config.yaml` 화이트리스트 구성

1. 로컬 네트워크의 모든 장치 허용:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    로컬 네트워크의 주소 범위를 잘 모르는 경우 위의 화이트리스트를 사용하십시오.

2. 두 개의 특정 장치만 연결 허용:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. `192.168.0.*` 서브넷의 모든 장치 연결 허용:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. 모든 IPv4 장치에 대한 네트워크 연결 허용:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### 화이트리스트 기반 액세스 제어 비활성화

화이트리스트를 통한 액세스 제어를 비활성화하려면:

* `/SillyTavern/config.yaml`에서 `whitelistMode`를 `false`로 설정합니다.
* SillyTavern 기본 설치 폴더에 있는 `whitelist.txt`를 제거하거나 이름을 변경합니다(존재하는 경우).
* SillyTavern 서버를 재시작합니다.

### 권장하지 않음: `whitelist.txt` 사용

!!!info
`whitelist.txt`가 존재하면 `config.yaml`의 화이트리스트 설정보다 우선합니다.

그러나 다른 모든 구성은 `config.yaml` 내에서 관리되며 `whitelist.txt`는 권한 문제가 발생하거나 잠길 수 있으므로 시스템이 자동으로 `config.yaml` 화이트리스트를 사용하도록 되돌릴 수 있습니다.

**config.yaml을 직접 편집하는 것이 더 간단하고 안정적입니다.**
!!!

여전히 whitelist.txt를 사용하려면:

1. SillyTavern 기본 설치 폴더에 `whitelist.txt`라는 새 텍스트 파일을 만듭니다.
2. 텍스트 편집기에서 열고 허용된 IP 주소를 추가합니다.
3. 파일을 저장하고 SillyTavern 서버를 재시작합니다.

#### 예제 `whitelist.txt` 구성

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

이렇게 하면 로컬 네트워크의 모든 장치가 연결할 수 있습니다.

### HTTP 기본 인증을 통한 액세스 제어

!!!warning
HTTP 기본 인증은 강력한 보안을 제공하지 않습니다.

무차별 대입 공격을 방지하기 위한 속도 제한이 없습니다. 이것이 우려되는 경우 TLS 및 속도 제한이 있는 리버스 프록시와 전용 [인증 서비스](sso.md)를 사용하는 것이 좋습니다.
!!!

서버는 클라이언트가 HTTP를 통해 연결할 때마다 사용자 이름과 비밀번호를 요청합니다. **이것은 원격 연결(listen: true)이 활성화된 경우에만 작동합니다.**

HTTP BA를 활성화하려면 SillyTavern 기본 디렉터리에서 `config.yaml`을 열고 `basicAuthMode`를 검색하십시오. basicAuthMode를 true로 설정하고 사용자 이름과 비밀번호를 설정합니다. 참고: `config.yaml`은 ST가 이전에 최소한 한 번 실행된 경우에만 존재합니다.

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

또는 다음과 같이 기본 인증을 활성화할 수 있습니다:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

이 `perUserBasicAuth` 모드에서 기본 인증의 사용자 이름과 비밀번호는 비밀번호가 있는 유효한 다중 사용자 계정과 동일합니다. 또한 SillyTavern은 해당 계정에 직접 로그인합니다. **`perUserBasicAuth`를 활성화하기 전에 비밀번호가 있는 계정이 있는지 확인하십시오.**

파일을 저장하고 이미 실행 중인 경우 SillyTavern을 재시작합니다. ST에 연결할 때 사용자 이름과 비밀번호를 묻는 메시지가 표시됩니다. 사용자 이름과 비밀번호는 모두 일반 텍스트로 전송됩니다. 이것이 우려되는 경우 HTTPS를 통해 ST를 제공할 수 있습니다.

### 호스트 화이트리스트

HTTPS 없이 네트워크를 통해 서버를 호스팅할 때 요청 호스트 확인을 활성화하는 것이 좋습니다. 이것은 DNS 리바인딩과 같은 다양한 공격을 방지하는 데 도움이 됩니다. 기본적으로 SillyTavern 서버는 인식되지 않은 호스트에서 처음 연결할 때 콘솔 메시지를 기록합니다.

### 호스트 화이트리스트 토글

호스트 화이트리스트를 활성화하려면 SillyTavern 루트 디렉터리의 `config.yaml` 파일을 편집하십시오:

```yaml
hostWhitelist:
    enabled: true
```

### 신뢰할 수 있는 호스트 추가

신뢰할 수 있는 호스트 목록에 호스트 이름을 추가하려면 `hostWhitelist.hosts` 섹션에 포함하십시오:

!!!tip 팁
`localhost` 또는 IP(예: `127.0.0.1` 또는 `::1`)를 추가하지 마십시오. 이들은 항상 신뢰할 수 있는 것으로 간주됩니다.

호스트 범위를 추가하려면 선행 점을 사용하십시오. 예를 들어 `.trycloudflare.com`을 추가하면 `trycloudflare.com`과 `example.trycloudflare.com`과 같은 모든 하위 도메인을 신뢰합니다.
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

### 콘솔 메시지 토글

인식되지 않은 호스트에 대한 콘솔 메시지를 비활성화하려면 `hostWhitelist.scan` 옵션을 `false`로 설정하십시오:

```yaml
hostWhitelist:
    scan: false
```

## SillyTavern 인스턴스에 연결

### ST 호스트 컴퓨터의 IP 주소 가져오기

화이트리스트가 설정된 후 ST 호스팅 장치의 IP가 필요합니다.

ST 호스팅 장치가 동일한 WiFi 네트워크에 있는 경우 ST 호스트의 내부 WiFi IP를 사용합니다:

* Windows의 경우: Windows 버튼 > 검색창에 `cmd.exe` 입력 > 콘솔에 `ipconfig` 입력, Enter 키 누르기 > `IPv4` 목록을 찾습니다.

사용자(또는 다른 사람)가 동일한 네트워크에 있지 않은 동안 호스팅된 ST에 연결하려면 ST 호스팅 장치의 공용 IP가 필요합니다.

* ST 호스팅 장치를 사용하는 동안 [이 페이지](https://whatismyipaddress.com/)에 액세스하여 `IPv4`를 찾습니다. 이것은 원격 장치에서 연결하는 데 사용할 것입니다.

### ST 서버에 연결

상황에 따라 얻은 IP가 무엇이든 해당 IP 주소와 포트 번호를 원격 장치의 웹 브라우저에 입력합니다.

동일한 WiFi 네트워크의 ST 호스트에 대한 일반적인 주소는 다음과 같습니다:

`http://192.168.0.5:8000`

https://가 아닌 http://를 사용하십시오.

### 연결 로깅

서버에 대한 새 연결은 콘솔 창에 표시되고 SillyTavern 데이터 디렉터리의 `access.log` 파일에 기록됩니다.

서버와 동일한 컴퓨터의 브라우저에 대한 콘솔 메시지는 다음과 같습니다:

```txt
New connection from 127.0.0.1; User Agent: ...
```

서버와 동일한 네트워크의 다른 컴퓨터의 브라우저에 대한 콘솔 메시지는 다음과 같을 수 있습니다:

```txt
New connection from 192.168.116.187; User Agent: ...
```

연결이 거부되면 콘솔 메시지는 다음과 같습니다:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log`에는 타임스탬프와 함께 연결 정보가 포함되지만 연결이 승인되었는지 거부되었는지는 포함되지 않습니다.

### 문제 해결

여전히 연결할 수 없습니까?

* 연결 시도가 [콘솔에 나타나지만](#connection-logging) 금지된 경우 [화이트리스트 문제](#whitelist-based-access-control)입니다.
* ST가 원격 연결을 수신 대기 중이지만 연결 시도가 콘솔에 나타나지 않으면 [네트워크 문제](#network-issues)입니다.
* ST가 원격 연결을 수신 대기하지 않는 경우 [읽기 문제](#allowing-remote-connections)입니다.

#### 네트워크 문제

* Windows에서는 애플리케이션이 애플리케이션 방화벽에 의해 차단될 수 있습니다. 이 문제를 해결하는 가장 빠른 방법은 node.js를 제거하고 다시 설치하는 것이며, 방화벽에서 메시지가 표시되면 네트워크에 액세스할 수 있도록 허용하는 것입니다. 그렇지 않으면 Windows 애플리케이션 방화벽을 통해 node.js 애플리케이션을 수동으로 허용해야 합니다.
* Windows 11에서는 설정 > 네트워크 및 인터넷 > 이더넷에서 개인 네트워크 프로필 유형을 활성화하십시오. 이것은 Windows 11에 매우 중요합니다. 그렇지 않으면 위에서 언급한 방화벽 규칙을 사용하더라도 연결할 수 없습니다.
* Linux에서는 방화벽을 통해 포트를 허용해야 할 수 있습니다. 이를 위한 명령은 `sudo ufw allow 8000`입니다. 이렇게 하면 포트 8000에서 트래픽이 허용됩니다.

라우터의 포트 포워딩 설정을 수정하지 마십시오. 로컬 네트워크 내에서 ST에 액세스하는 데 필요하지 않으며 서버가 인터넷에 노출될 수 있습니다.

[로컬 네트워크 외부](remote-connections.md)에서 ST 서버에 액세스하려고 하는데 작동하지 않는 경우 문제가 원격 장치와 터널/VPN 엔드포인트 간인지 서버의 터널 엔드포인트와 ST 서비스 간인지 확인하십시오. 그렇지 않으면 잘못된 것을 많은 시간을 들여 문제 해결하게 될 것입니다.

## HTTPS

### TLS/SSL로 SillyTavern 시작

!!!tip
SSL은 `config.yaml` 파일을 사용하여 구성할 수도 있습니다: [SSL 구성](/Administration/config-yaml.md#ssl-configuration).
!!!

ST 인스턴스와 주고받는 트래픽을 암호화하려면 `--ssl` 플래그로 서버를 시작하십시오.

예제:

```bash
node server.js --ssl
```

기본적으로 ST는 `certs` 폴더 안에서 인증서를 검색합니다. 파일이 다른 곳에 있는 경우 `--keyPath` 및 `--certPath` 인수를 사용할 수 있습니다.

예제:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

SillyTavern을 실행하는 사용자는 인증서 파일에 대한 읽기 권한이 필요합니다.

### 인증서를 얻는 방법

인증서를 얻는 가장 간단하고 빠른 방법은 [certbot](https://letsencrypt.org/getting-started/)을 사용하는 것입니다.
