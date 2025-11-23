---
label: Single Sign-On (SSO)
icon: key
order: -20
route: /administration/sso/
---

# Single Sign-On (SSO)

SSO cho phép bạn tạo người dùng và bảo mật nhiều trang khác nhau bằng cách sử dụng một cổng đăng nhập được hiển thị trên các trang bạn muốn bảo mật. Mặc dù phức tạp để thiết lập, đây là một cách tốt để vừa học SSO vừa bảo mật instance ST của bạn trên internet một cách tốt hơn.

SSO cũng có thể thay thế [HTTP Basic Authentication](/Administration/config-yaml.md#user-authentication) như một cơ chế kiểm soát truy cập cho [kết nối từ xa](/Administration/remote-connections.md).

Điều này được khuyến nghị vì SSO cung cấp bảo mật và chức năng tốt hơn HTTP Basic Authentication.

[**Authelia**](https://www.authelia.com/) và [**Authentik**](https://goauthentik.io/) là các nhà cung cấp SSO mã nguồn mở có thể được sử dụng với SillyTavern.

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
