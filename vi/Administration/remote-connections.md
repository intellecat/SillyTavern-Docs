---
icon: rss
order: -30
route: /vi/usage/remoteconnections/
---

# Kết nối từ xa

Thường là dành cho những người muốn sử dụng SillyTavern trên điện thoại di động của họ trong khi PC của họ chạy máy chủ ST trong cùng mạng WiFi.

Đây cũng là bước đầu tiên để cho phép các kết nối từ xa từ bên ngoài mạng cục bộ.

!!!warning
Bạn không nên sử dụng port forwarding để phơi bày máy chủ ST của bạn ra internet. Thay vào đó, hãy sử dụng VPN hoặc dịch vụ tunneling như Cloudflare Zero Trust, ngrok hoặc Tailscale. Xem hướng dẫn [VPN và Tunneling](tunneling.md) để biết thêm thông tin.
!!!

!!!danger Tuyên Bố Miễn Trừ Trách Nhiệm
**KHÔNG BAO GIỜ LƯU TRỮ BẤT KỲ INSTANCE NÀO TRÊN INTERNET MỞ MÀ KHÔNG ĐẢM BẢO CÁC BIỆN PHÁP BẢO MẬT THÍCH HỢP TRƯỚC.**

**CHÚNG TÔI KHÔNG CHỊU TRÁCH NHIỆM VỀ BẤT KỲ THIỆT HẠI HOẶC MẤT MÁT NÀO TRONG CÁC TRƯỜNG HỢP TRUY CẬP TRÁI PHÉP DO TRIỂN KHAI BẢO MẬT KHÔNG ĐÚNG HOẶC KHÔNG ĐẦY ĐỦ.**
!!!

## Cho phép kết nối từ xa

Theo mặc định, máy chủ ST chỉ chấp nhận các kết nối từ máy mà nó đang chạy (localhost). Để cho phép nó lắng nghe các kết nối từ các thiết bị khác, đặt tùy chọn `listen` trong `config.yaml` thành `true`.

!!! Nếu bạn tìm kiếm `config.yaml` trực tiếp trong thư mục SillyTavern, bạn có thể tìm thấy hai tệp.
Tất cả các sửa đổi đối với `config.yaml` trong tài liệu này đề cập đến tệp trong thư mục gốc SillyTavern (/SillyTavern/config.yaml), không phải `/SillyTavern/default/config.yaml`.
!!!

```yaml
# Lắng nghe các kết nối đến
listen: true
```

Khi ST đang lắng nghe các kết nối từ xa, bạn sẽ thấy thông báo này trong console:

```txt
SillyTavern is listening on IPv4: 0.0.0.0:8000
```

và một số giải thích về ý nghĩa của điều đó.

Khi ST **không** lắng nghe các kết nối từ xa, bạn sẽ thấy thông báo này trong console:

```txt
SillyTavern is listening on IPv4: 127.0.0.1:8000
```

## Cấu hình kiểm soát truy cập

Sau khi bật lắng nghe kết nối từ xa, bạn phải cấu hình ít nhất một phương pháp kiểm soát truy cập. Nếu không, máy chủ sẽ không khởi động.

### Kiểm soát truy cập dựa trên Whitelist

Để bật kiểm soát truy cập thông qua whitelist, chỉnh sửa tệp `config.yaml` trong thư mục gốc SillyTavern (`/SillyTavern/config.yaml`):

1. Khởi động SillyTavern ít nhất một lần để tạo các tệp cấu hình cần thiết.
2. Mở `/SillyTavern/config.yaml` trong trình chỉnh sửa văn bản.
3. Tìm phần `whitelist` và thêm các địa chỉ IP mà bạn muốn cho phép:
    * Liệt kê từng địa chỉ IP riêng biệt.
    * Đảm bảo `127.0.0.1` được bao gồm, nếu không bạn sẽ không thể kết nối từ máy chủ.
    * Hỗ trợ các IP riêng lẻ, mặt nạ CIDR (ví dụ: `10.0.0.0/24`) và phạm vi ký tự đại diện (`*`).
4. Lưu tệp `config.yaml`.
5. **Khởi động lại máy chủ SillyTavern của bạn.**

#### Ví dụ cấu hình whitelist `config.yaml`

1. Cho phép bất kỳ thiết bị nào trên mạng cục bộ:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 10.0.0.0/8
      - 172.16.0.0/12
      - 192.168.0.0/16
    ```

    Nếu không chắc chắn về phạm vi địa chỉ mạng cục bộ của bạn, hãy sử dụng whitelist ở trên.

2. Cho phép hai thiết bị cụ thể kết nối:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.2
      - 192.168.0.5
    ```

3. Cho phép bất kỳ thiết bị nào trên subnet `192.168.0.*` kết nối:

    ```yaml
    whitelist:
      - ::1
      - 127.0.0.1
      - 192.168.0.*
    ```

4. Cho phép kết nối mạng cho tất cả các thiết bị IPv4:

    ```yaml
    whitelist:
      - 0.0.0.0/0
    ```

### Vô hiệu hóa kiểm soát truy cập dựa trên whitelist

Để vô hiệu hóa kiểm soát truy cập thông qua whitelist:

* Đặt `whitelistMode` thành `false` trong `/SillyTavern/config.yaml`.
* Xóa hoặc đổi tên `whitelist.txt` (nếu nó tồn tại) trong thư mục cài đặt cơ sở SillyTavern.
* Khởi động lại máy chủ SillyTavern của bạn.

### Không khuyến nghị: sử dụng `whitelist.txt`

!!!info
Nếu `whitelist.txt` tồn tại, nó được ưu tiên hơn cài đặt whitelist trong `config.yaml`.

Tuy nhiên, vì tất cả các cấu hình khác được quản lý trong `config.yaml`, và `whitelist.txt` có thể gặp phải vấn đề về quyền hoặc bị khóa, hệ thống có thể âm thầm quay trở lại sử dụng whitelist `config.yaml`.

**Chỉnh sửa config.yaml trực tiếp vừa đơn giản hơn vừa đáng tin cậy hơn.**
!!!

Nếu bạn vẫn muốn sử dụng whitelist.txt:

1. Tạo một tệp văn bản mới có tên `whitelist.txt` trong thư mục cài đặt cơ sở SillyTavern.
2. Mở nó trong trình chỉnh sửa văn bản và thêm các địa chỉ IP được cho phép.
3. Lưu tệp và khởi động lại máy chủ SillyTavern của bạn.

#### Ví dụ cấu hình `whitelist.txt`

```txt
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.1
::1
```

Điều này cho phép bất kỳ thiết bị nào trên mạng cục bộ kết nối.

### Kiểm soát truy cập bằng HTTP Basic Authentication

!!!warning
HTTP Basic Authentication không cung cấp bảo mật mạnh.

Không có giới hạn tốc độ để ngăn chặn các cuộc tấn công brute-force. Nếu đây là mối quan tâm, nên sử dụng reverse proxy với TLS và giới hạn tốc độ, và một [dịch vụ xác thực](sso.md) chuyên dụng.
!!!

Máy chủ sẽ yêu cầu tên người dùng và mật khẩu bất cứ khi nào client kết nối qua HTTP. **Điều này chỉ hoạt động nếu Kết nối từ xa (listen: true) được bật.**

Để bật HTTP BA, Mở `config.yaml` trong thư mục cơ sở SillyTavern và tìm kiếm `basicAuthMode`. Đặt basicAuthMode thành true và đặt username và password. Lưu ý: `config.yaml` sẽ chỉ tồn tại nếu ST đã được thực thi ít nhất một lần trước đó.

```yaml
basicAuthMode: true
basicAuthUser:
  username: "MyUsername"
  password: "MyPassword"
```

Ngoài ra, bạn có thể bật xác thực cơ bản như sau:

```yaml
basicAuthMode: true
enableUserAccounts: true
perUserBasicAuth: true
```

Trong chế độ `perUserBasicAuth` này, tên người dùng và mật khẩu của xác thực cơ bản sẽ giống như bất kỳ tài khoản đa người dùng hợp lệ nào có mật khẩu. Ngoài ra, SillyTavern sẽ đăng nhập trực tiếp vào tài khoản đó. **Đảm bảo bạn có một tài khoản với mật khẩu trước khi bật `perUserBasicAuth`.**

Lưu tệp và khởi động lại SillyTavern nếu nó đã chạy. Bạn sẽ được nhắc nhập tên người dùng và mật khẩu khi kết nối với ST của bạn. Cả tên người dùng và mật khẩu đều được truyền dưới dạng văn bản thuần. Nếu bạn lo lắng về điều này, bạn có thể phục vụ ST qua HTTPS.

### Whitelist địa chỉ riêng tư

Trong khi việc thực hiện các yêu cầu HTTP đi ra đến các địa chỉ trong dải IP riêng tư (ví dụ: `192.168.x.x`, `10.x.x.x`) từ máy chủ được cho phép theo mặc định, bạn có thể hạn chế quyền truy cập đến các địa chỉ riêng tư cụ thể bằng cách sử dụng cấu hình whitelist. Điều này được khuyến nghị khi bạn có một API riêng tư đang chạy trên mạng cục bộ của bạn mà bạn muốn cho phép ST truy cập, nhưng bạn muốn ngăn ST truy cập các thiết bị khác trên mạng cục bộ.

#### Điều gì được coi là "địa chỉ riêng tư"?

* Địa chỉ loopback: `127.0.0.0/8` cho IPv4 và `::1/128` cho IPv6.
* Dải địa chỉ riêng tư IPv4: lớp A (`10.0.0.0/8`), lớp B (`172.16.0.0/12`), lớp C (`192.168.0.0/16`).
* Địa chỉ link-local: `169.254.0.0/16` cho IPv4 và `fe80::/10` cho IPv6.
* Địa chỉ local duy nhất: `fc00::/7` cho IPv6.

#### Bật/tắt whitelist địa chỉ riêng tư

Để bật whitelist địa chỉ riêng tư, chỉnh sửa tệp `config.yaml` trong thư mục gốc SillyTavern:

```yaml
privateAddressWhitelist:
    enabled: true
```

#### Thêm các địa chỉ riêng tư vào whitelist

Theo mặc định, điều này chỉ cho phép thực hiện các yêu cầu đến các địa chỉ loopback (`127.0.0.1` và `::1`) từ máy chủ. Để thêm nhiều địa chỉ riêng tư hơn vào whitelist, hãy đưa chúng vào phần `privateAddressWhitelist.allowedRanges`:

```yaml
privateAddressWhitelist:
  allowedRanges:
    - "127.0.0.0/8"
    - "::1/128"
    - "192.168.0.0/16"
```

Ví dụ này cho phép thực hiện các yêu cầu đến bất kỳ địa chỉ nào trong dải `192.168.x.x` và các địa chỉ loopback từ máy chủ, trong khi vẫn chặn quyền truy cập đến các dải IP riêng tư khác.

### Whitelist Host

Khi lưu trữ máy chủ qua mạng mà không có HTTPS, rất khuyến nghị bật xác minh host yêu cầu. Điều này giúp ngăn chặn các cuộc tấn công khác nhau, chẳng hạn như DNS rebinding. Theo mặc định, máy chủ SillyTavern sẽ ghi một thông báo console khi có kết nối đầu tiên từ một host không được nhận dạng.

#### Bật/tắt whitelist host

Để bật whitelist host, chỉnh sửa tệp `config.yaml` trong thư mục gốc SillyTavern:

```yaml
hostWhitelist:
    enabled: true
```

#### Thêm các host tin cậy

Để thêm tên host vào danh sách các host tin cậy, bao gồm nó trong phần `hostWhitelist.hosts`:

!!!tip Mẹo
Không thêm `localhost` hoặc IP (chẳng hạn như `127.0.0.1` hoặc `::1`). Những cái này luôn được coi là tin cậy.

Để thêm một dải các host, sử dụng dấu chấm đầu tiên. Ví dụ, thêm `.trycloudflare.com` sẽ tin cậy `trycloudflare.com` cũng như bất kỳ subdomain nào như `example.trycloudflare.com`.
!!!

```yaml
hostWhitelist:
  hosts:
    - "example.com"
    - ".trycloudflare.com"
```

#### Bật/tắt thông báo console

Để vô hiệu hóa thông báo console cho các host không được nhận dạng, đặt tùy chọn `hostWhitelist.scan` thành `false`:

```yaml
hostWhitelist:
    scan: false
```

## Kết nối với instance SillyTavern của bạn

### Lấy địa chỉ IP cho máy chủ ST

Sau khi whitelist được thiết lập, bạn sẽ cần IP của thiết bị lưu trữ ST.

Nếu thiết bị lưu trữ ST nằm trên cùng mạng wifi, bạn sẽ sử dụng IP wifi nội bộ của ST-host:

* Đối với Windows: nút windows > gõ `cmd.exe` vào thanh tìm kiếm > gõ `ipconfig` trong console, nhấn Enter > tìm kiếm danh sách `IPv4`.

Nếu bạn (hoặc người khác) muốn kết nối với ST được lưu trữ của bạn trong khi không ở trên cùng mạng, bạn sẽ cần IP công cộng của thiết bị lưu trữ ST của bạn.

* Trong khi sử dụng thiết bị lưu trữ ST, truy cập [trang này](https://whatismyipaddress.com/) và tìm kiếm `IPv4`. Đây là những gì bạn sẽ sử dụng để kết nối từ thiết bị từ xa.

### Kết nối với máy chủ ST

Bất kỳ IP nào bạn có cho tình huống của mình, bạn sẽ đặt địa chỉ IP và số cổng đó vào trình duyệt web của thiết bị từ xa.

Một địa chỉ điển hình cho một host ST trên cùng mạng wifi sẽ trông như:

`http://192.168.0.5:8000`

Sử dụng http:// KHÔNG PHẢI https://

### Ghi log kết nối

Các kết nối mới đến máy chủ được hiển thị trong cửa sổ console và được ghi vào tệp `access.log` trong thư mục dữ liệu SillyTavern.

Một thông báo console cho trình duyệt trên cùng máy với máy chủ trông như:

```txt
New connection from 127.0.0.1; User Agent: ...
```

Một thông báo console cho trình duyệt trên một máy khác trên cùng mạng với máy chủ có thể trông như:

```txt
New connection from 192.168.116.187; User Agent: ...
```

Nếu kết nối bị từ chối, thông báo console sẽ trông như:

```txt
New connection from 192.168.116.211; User Agent: ...

Forbidden: Connection attempt from 192.168.116.211. If you are attempting to connect,
please add your IP address in whitelist or disable whitelist mode in config.yaml in
root of SillyTavern folder.
```

`access.log` sẽ chứa thông tin kết nối, với dấu thời gian, nhưng không cho biết kết nối được chấp nhận hay bị từ chối.

### Xử lý sự cố

Vẫn không thể kết nối?

* Nếu nỗ lực kết nối [xuất hiện trong console](#connection-logging), nhưng bị cấm, đó là [vấn đề whitelist](#whitelist-based-access-control).
* Nếu ST đang lắng nghe các kết nối từ xa nhưng nỗ lực kết nối không xuất hiện trong console, đó là [vấn đề mạng](#network-issues).
* Nếu ST không lắng nghe các kết nối từ xa, đó là [vấn đề đọc](#allowing-remote-connections).

#### Vấn đề mạng

* Trên Windows, ứng dụng có thể bị chặn bởi tường lửa ứng dụng. Cách nhanh nhất để khắc phục điều này là gỡ cài đặt và cài đặt lại node.js, và khi được nhắc bởi tường lửa, cho phép nó truy cập mạng. Nếu không, bạn sẽ cần phải cho phép thủ công ứng dụng node.js thông qua tường lửa ứng dụng Windows.
* Trên Windows 11, bật loại hồ sơ Private Network trong Settings > Network and Internet > Ethernet. Điều này RẤT quan trọng đối với Windows 11, nếu không, bạn sẽ không thể kết nối ngay cả với các quy tắc tường lửa nói trên.
* Trên Linux, bạn có thể cần cho phép cổng thông qua tường lửa. Lệnh để làm điều này là `sudo ufw allow 8000`. Điều này sẽ cho phép lưu lượng truy cập trên cổng 8000.

Không sửa đổi cài đặt port forwarding trên router của bạn. Điều này không cần thiết để truy cập ST trong mạng cục bộ của bạn, và có thể phơi bày máy chủ của bạn ra internet.

Nếu bạn đang cố gắng truy cập máy chủ ST của mình từ [bên ngoài mạng cục bộ của bạn](remote-connections.md), và nó không hoạt động, hãy xác định xem vấn đề là giữa thiết bị từ xa và điểm cuối tunnel/VPN, hay giữa điểm cuối tunnel trên máy chủ và dịch vụ ST. Nếu không, bạn sẽ dành nhiều thời gian khắc phục sự cố sai.

## HTTPS

### Khởi động SillyTavern với TLS/SSL

!!!tip
SSL cũng có thể được cấu hình bằng cách sử dụng tệp `config.yaml`: [Cấu hình SSL](/Administration/config-yaml.md#ssl-configuration).
!!!

Để mã hóa lưu lượng từ và đến instance ST của bạn, hãy khởi động máy chủ với cờ `--ssl`.

Ví dụ:

```bash
node server.js --ssl
```

Theo mặc định, ST sẽ tìm kiếm chứng chỉ của bạn bên trong thư mục `certs`. Nếu tệp của bạn nằm ở nơi khác, bạn có thể sử dụng các đối số `--keyPath` và `--certPath`.

Ví dụ:

```bash
node server.js --ssl --keyPath /home/user/certificates/privkey.pem --certPath /home/user/certificates/cert.pem
```

Người dùng mà bạn đang chạy SillyTavern cần quyền đọc trên các tệp chứng chỉ.

### Cách lấy chứng chỉ

Cách đơn giản, nhanh nhất để lấy chứng chỉ là sử dụng [certbot](https://letsencrypt.org/getting-started/).

### Chứng chỉ trong Docker

!!!warning
Vì lý do bảo mật và quyền riêng tư, không đưa chứng chỉ SSL của bạn vào bên trong image Docker nếu bạn đang xây dựng một image. Thay vào đó, hãy sử dụng volume mount để cung cấp chứng chỉ khi chạy.
!!!

Khi chạy SillyTavern trong Docker, cách được khuyến nghị để cung cấp chứng chỉ SSL là đặt chúng trong volume mount `/config`. Điều này cho phép bạn quản lý chứng chỉ mà không cần xây dựng lại image container.

1. Đặt các tệp chứng chỉ của bạn (ví dụ: `privkey.pem` và `cert.pem`) vào thư mục cấu hình cục bộ của bạn được mount vào `/config` trong container.

2. Cập nhật `config.yaml` của bạn để tham chiếu đến chứng chỉ:

    ```yaml
    ssl:
      enabled: true
      certPath: ./config/cert.pem
      keyPath: ./config/privkey.pem
    ```

3. Khởi động lại container Docker của bạn để áp dụng các thay đổi.
