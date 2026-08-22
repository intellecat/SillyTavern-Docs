---
label: VPN 및 터널링
order: -40
icon: lock
route: /ko/administration/tunneling/
---

VPN과 터널은 세계 어디서나 홈 네트워크에 안전하게 액세스할 수 있는 방법입니다. 이 가이드는 VPN 또는 터널을 사용하여 어디서나 SillyTavern 인스턴스에 액세스하는 방법을 보여줍니다.

## 방법

1. **자체 제작 VPN**을 사용합니다.

   여러 라우터는 라우터 관리 페이지에서 VPN 서버(주로 OpenVPN 또는 WireGuard)를 호스팅하는 기능을 제공합니다. 라우터 설명서를 참조하여 VPN을 설정하고 장치를 VPN에 추가하십시오. 연결되면 SillyTavern용으로 설정한 사설 IP로 이동하면 정상적으로 연결할 수 있습니다. 사용자와 Windows 사용에 더 쉽습니다.

2. [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/)를 사용합니다.

   Cloudflare Zero Trust는 50명의 사용자를 추가할 수 있는 Cloudflare의 무료 조직 기능입니다. 이것은 Cloudflare를 통해 트래픽을 프록시하며 `cloudflared`를 사용하여 ST PC를 터널로 추가하면 집에 있는 것처럼 ST 인스턴스에 연결할 수 있습니다.

   터널을 만든 후 라우터의 사설 IP 주소로 경로를 추가하고 IP CIDR 값을 계산하여 Cloudflare Zero Trust를 사용하여 이동 중에 완전한 로컬 액세스를 해야 합니다.

3. 독립형 **Cloudflare** 또는 **[ngrok](https://ngrok.com)** 터널을 사용합니다.

   AI 백엔드가 연결할 수 있는 방법과 유사하게 Cloudflare 터널을 통해 ST 인스턴스를 연결하고 Cloudflare 터널 페이지를 열 수도 있습니다. 그러나 이동 중에 ST를 사용하려고 할 때마다 Cloudflare/NGROK에서 생성한 각 새 링크를 복사하여 붙여넣어야 합니다.

4. **Tailscale**을 사용합니다.

   Tailscale은 PC에 안전한 원격 연결을 가능하게 하는 VPN 제공업체입니다.

## Tailscale 설정

Tailscale은 PC에 안전한 원격 연결을 가능하게 하는 VPN 제공업체입니다. Tailscale 서버의 오픈 소스 구현이 존재하며 [Headscale](https://github.com/juanfont/headscale)을 사용하여 서버를 호스팅할 수도 있지만 이것은 이 튜토리얼의 범위를 벗어납니다.

### 1. 계정 만들기

* [Tailscale 웹사이트](https://tailscale.com/)로 이동하여 새 계정을 만듭니다.

**참고:** 한 사람의 일상적인 사용을 위해 Tailscale은 영구적으로 무료입니다. 숨겨진 비용이 걱정되면 결제 옵션을 추가하지 마십시오.

### 2. 클라이언트 설정

* [Tailscale 다운로드 페이지](https://tailscale.com/download)로 이동하여 SillyTavern이 실행 중인 장치와 원격 위치에서 사용하려는 장치에 클라이언트/앱을 다운로드합니다.
* 이전에 만든 계정으로 두 장치에 로그인합니다.
* [Tailscale 관리 페이지](https://login.tailscale.com/admin/machines)로 이동하여 두 장치를 승인합니다.
* 연결된 두 장치의 이름을 기록해 둡니다.

### 3. 화이트리스트에 장치 추가

* [화이트리스트 IP 관리](./remote-connections.md#whitelist-based-access-control)에 따라 연결 장치의 컴퓨터 이름(SillyTavern과 함께 사용하려는 장치)을 SillyTavern의 화이트리스트에 추가합니다.

**참고:** [원격 연결](./remote-connections.md#allowing-remote-connections)을 참조하지 않았다면 구성을 편집할 때 listen을 true로 설정해야 합니다.

### 4. 연결

이제 어디서나 SillyTavern을 사용하려면 다음과 같이 하면 됩니다:

* SillyTavern을 호스팅하는 PC와 원격으로 사용하려는 장치 모두에서 Tailscale을 켭니다.
* 연결하려는 장치에서 브라우저를 열고 `http://<st를 실행하는 PC의 컴퓨터 이름>:8000/`으로 이동합니다.

### 5. 친구와 SillyTavern 인스턴스 공유 (선택 사항)

* 친구에게 자신의 Tailscale 계정을 만들고 장치에 클라이언트를 다운로드하도록 알립니다.
* [Tailscale 관리 페이지](https://login.tailscale.com/admin/machines)로 이동합니다.
* SillyTavern을 호스팅하는 PC의 3점 버튼 위로 마우스를 가져가서 "Share..."를 누르거나 3점 버튼을 누르고 "Sharing settings..."를 누릅니다.
* "Allow use as an exit node"를 선택 해제합니다(친구가 PC를 통해 모든 인터넷 트래픽을 라우팅할 수 있도록 하지 않으려면).
* 이메일로 링크를 보내거나 탭을 "Copy share link"로 변경하고 같은 텍스트가 있는 큰 파란색 버튼을 누르고 다른 방법으로 친구에게 보냅니다.
* 공유 링크를 클릭하면 친구의 Tailscale 네트워크에 PC가 나타납니다.
* 마지막 단계에서 설명한 것과 동일한 SillyTavern 액세스 링크를 친구에게 보냅니다.

**참고:** 이렇게 하면 친구가 SillyTavern, automatic1111 등과 같이 PC에서 로컬로 실행되는 모든 서비스에 완전히 액세스할 수 있습니다. 정말로 친구를 신뢰하는 경우에만 이렇게 하십시오.
