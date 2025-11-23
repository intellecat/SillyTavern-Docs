---
order: 25
icon: gear
expanded: true
route: /administration/
---

# Quản Trị

!!!warning
Mặc dù tuân theo nhiều phương pháp hay nhất về bảo mật, máy chủ SillyTavern không đủ an toàn để tiếp xúc với internet công cộng.

**KHÔNG BAO GIỜ LƯU TRỮ BẤT KỲ INSTANCE NÀO TRÊN INTERNET MỞ MÀ KHÔNG ĐẢM BẢO CÁC BIỆN PHÁP BẢO MẬT THÍCH HỢP TRƯỚC.**

**CHÚNG TÔI KHÔNG CHỊU TRÁCH NHIỆM VỀ BẤT KỲ THIỆT HẠI HOẶC MẤT MÁT NÀO DO TRUY CẬP TRÁI PHÉP DO TRIỂN KHAI BẢO MẬT KHÔNG ĐÚNG HOẶC KHÔNG ĐẦY ĐỦ.**
!!!

:::callout
**[config.yaml](./config-yaml.md)**

Tệp cấu hình chính cho SillyTavern. Nó chứa nhiều cài đặt khác nhau, chẳng hạn như mạng, bảo mật và các tùy chọn cụ thể của backend.
:::

:::callout
**[Đa người dùng](multi-user)**

Để chia sẻ instance SillyTavern của bạn với người khác, bạn có thể tạo nhiều tài khoản người dùng. Mỗi người dùng có cài đặt, tiện ích mở rộng và dữ liệu riêng của họ. Tài khoản người dùng cũng có thể được bảo vệ bằng mật khẩu.
:::

:::callout
**[Truy cập từ xa](remote-connections)**

Bạn có thể truy cập instance SillyTavern của mình từ điện thoại, máy tính bảng hoặc máy tính khác.
:::

:::callout
**[VPN và Tunneling](tunneling.md)**

Để truy cập instance SillyTavern của bạn từ internet, bạn có thể sử dụng VPN hoặc dịch vụ tunneling như Cloudflare Zero Trust, ngrok hoặc Tailscale.
:::

:::callout
**[Reverse proxying](reverse-proxying)**

Những người đam mê có thể thiết lập reverse proxy để truy cập instance SillyTavern của họ từ internet.
:::

## Danh sách kiểm tra bảo mật

**Đây chỉ là những khuyến nghị. Vui lòng tham khảo ý kiến chuyên gia bảo mật ứng dụng web trước khi đưa instance ST của bạn lên mạng.**

1. Giữ cho hệ điều hành và phần mềm runtime của bạn, chẳng hạn như Node.js, được cập nhật. Điều này đảm bảo hệ thống của bạn có các bản vá và sửa lỗi bảo mật mới nhất, giúp ngăn chặn các lỗ hổng tiềm ẩn.
2. Sử dụng [whitelist](/Administration/config-yaml.md#ip-whitelisting) và tường lửa mạng. Chỉ cho phép các dải IP tin cậy truy cập máy chủ.
3. Bật [xác thực cơ bản](/Administration/config-yaml.md#user-authentication). Nó hoạt động như một "mật khẩu chính" trước khi bạn có thể truy cập ứng dụng front-end.
4. Ngoài ra, cấu hình xác thực bên ngoài. Một số dịch vụ đã biết cho việc này là [Authelia](https://www.authelia.com/) và [authentik](https://goauthentik.io/). Xem [hướng dẫn SSO](sso.md) để biết chi tiết.
5. Không bao giờ để tài khoản quản trị viên không có mật khẩu. Máy chủ sẽ cảnh báo bạn khi khởi động nếu bạn có bất kỳ tài khoản quản trị viên nào không được bảo vệ.
6. Sử dụng cài đặt đăng nhập kín đáo bên ngoài mạng cục bộ. Điều này ẩn danh sách người dùng khỏi những người ngoài tiềm năng.
7. Kiểm tra log truy cập thường xuyên. Chúng được ghi vào console máy chủ và vào tệp `access.log` và cung cấp thông tin về các kết nối đến, chẳng hạn như địa chỉ IP và user agent.
8. Cấu hình HTTPS. Đối với máy chủ localhost, bạn có thể tạo và sử dụng chứng chỉ tự ký. Nếu không, bạn có thể cần triển khai máy chủ web reverse-proxy như [Traefik](https://traefik.io/) hoặc [Caddy](https://caddyserver.com/docs/getting-started).
9. Cấu hình và bật [whitelist host](/Administration/config-yaml.md#host-whitelisting), đặc biệt nếu bạn không sử dụng mã hóa HTTPS trên mạng cục bộ.

Tìm thêm thông tin về proxy an toàn trong hướng dẫn sau: [Reverse Proxying SillyTavern](reverse-proxying).
