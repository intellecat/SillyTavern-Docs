---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /vi/administration/sso/
---

# Single Sign-On (SSO)

!!!warning
Mặc dù có nhiều lợi ích, SSO là một hệ thống phức tạp để thiết lập và duy trì. Nếu không được cấu hình đúng cách, nó có thể dẫn đến truy cập trái phép vào instance của bạn. Hãy đảm bảo cấu hình và kiểm tra đúng thiết lập của bạn trước khi bật SSO. Nếu bạn không chắc chắn về các tác động bảo mật, nên giữ đăng nhập tự động SSO ở trạng thái tắt và sử dụng các phương thức xác thực khác thay thế.
!!!

SSO cho phép bạn tạo người dùng và bảo mật nhiều trang khác nhau bằng cách sử dụng một cổng đăng nhập được hiển thị trên các trang bạn muốn bảo mật. Mặc dù phức tạp để thiết lập, đây là một cách tốt để vừa học SSO vừa bảo mật instance ST của bạn trên internet một cách tốt hơn.

SSO cũng có thể thay thế [HTTP Basic Authentication](/Administration/config-yaml.md#user-authentication) như một cơ chế kiểm soát truy cập cho [kết nối từ xa](/Administration/remote-connections.md).

Điều này được khuyến nghị vì SSO cung cấp bảo mật và chức năng tốt hơn HTTP Basic Authentication.

[**Authelia**](https://www.authelia.com/) và [**Authentik**](https://goauthentik.io/) là các nhà cung cấp SSO mã nguồn mở có thể được sử dụng với SillyTavern.

## Cấu hình các proxy tin cậy

Chỉ các yêu cầu từ các địa chỉ IP được cấu hình là proxy tin cậy mới có thể xác thực người dùng bằng cách chuyển tiếp các header cần thiết. Theo mặc định, cả địa chỉ loopback IPv4 và IPv6 đều được tin cậy. Để cho phép các IP khác xác thực bằng các header SSO, hãy thêm chúng vào danh sách `sso.trustedProxies` trong tệp [config.yaml](/Administration/config-yaml.md#sso-auto-login) của bạn:

!!!tip
Cũng hỗ trợ ký hiệu CIDR và ký tự đại diện để chỉ định nhiều IP hoặc dải IP, ví dụ: `192.168.0.*` hoặc `10.0.0.0/24`.
!!!

```yaml
sso:
  trustedProxies:
    - ::1           # Địa chỉ loopback IPv6 - được tin cậy theo mặc định
    - 127.0.0.1     # Địa chỉ loopback IPv4 - được tin cậy theo mặc định
    - '192.168.0.1' # Ví dụ về địa chỉ IP của một proxy tin cậy
```

## Đăng nhập bằng SSO

Nếu tên người dùng do SSO cung cấp của bạn **khớp chính xác** với handle người dùng của tài khoản người dùng SillyTavern, bạn có thể đăng nhập vào SillyTavern với tư cách người dùng đó bằng SSO. Để bật tính năng này, thay đổi một trong các tùy chọn sau vào tệp [config.yaml](/Administration/config-yaml.md#sso-auto-login) của bạn:

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

Cả hai tùy chọn đều bổ sung hoặc thay thế thành phần [quản lý mật khẩu](/Usage/User_Settings/index.md#account-management) tích hợp của thiết lập [chế độ đa người dùng](/Administration/multi-user.md).
