---
order: 25
icon: gear
expanded: true
route: /vi/administration/
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

## Cấu Trúc Dữ Liệu

Phần này cung cấp tổng quan về cấu trúc lưu trữ dữ liệu người dùng trong SillyTavern. Chỉ có cấu trúc dữ liệu mặc định (thư mục gốc dữ liệu là một thư mục con của thư mục cài đặt SillyTavern) được mô tả ở đây. Nếu bạn đã tùy chỉnh cấu trúc dữ liệu, hãy tham khảo cấu hình tùy chỉnh của bạn để biết chi tiết.

### `data/[user-handle]` (ví dụ: `data/default-user`)

Được tạo cho mỗi tài khoản người dùng, thư mục này chứa dữ liệu cụ thể của người dùng như tệp nhân vật, lịch sử trò chuyện và cài đặt.

### `data/_cache`

Nơi lưu trữ các tệp được máy chủ tải xuống, chẳng hạn như tệp tokenizer và các model transformers.js.

#### `data/_cache/characters`

Chứa dữ liệu nhân vật đã được phân tích khi cài đặt `performance.useDiskCache` được bật, được đồng bộ hóa khi khởi động và khi nhân vật được cập nhật.

Điều này cho phép tải dữ liệu nhân vật nhanh hơn với chi phí là dung lượng đĩa.

### `data/_css`

Nơi lưu trữ các tệp CSS tùy chỉnh.

Hiện tại, chỉ tệp `user.css` được hỗ trợ, cho phép bạn thêm các kiểu tùy chỉnh vào frontend.

### `data/_errors`

Nơi lưu trữ các tệp HTML chứa các trang lỗi cho các mã trạng thái HTTP khác nhau. Các tệp này được sử dụng để hiển thị các trang lỗi tùy chỉnh khi máy chủ gặp lỗi.

- `forbidden-by-whitelist.html`: Được hiển thị khi một yêu cầu bị chặn bởi whitelist địa chỉ IP.
- `host-not-allowed.html`: Được hiển thị khi một yêu cầu bị chặn bởi whitelist host.
- `unauthorized.html`: Được hiển thị khi xác thực cơ bản thất bại.
- `url-not-found.html`: Được hiển thị khi không tìm thấy tài nguyên được yêu cầu.

### `data/_storage`

Nơi lưu trữ [dữ liệu tài khoản người dùng](./multi-user.md).

Không nên chỉnh sửa các tệp này theo cách thủ công vì có thể dẫn đến hỏng dữ liệu.

### `data/_uploads`

Nơi lưu trữ tạm thời cho các tệp được người dùng tải lên, trong khi chúng đang được máy chủ xử lý.

Các tệp này được tự động xóa mỗi khi khởi động.

### `data/_webpack`

Nơi lưu trữ các tài sản webpack đã biên dịch và các tệp cache.

Nếu bạn gặp sự cố với frontend sau khi cập nhật, hãy thử xóa thư mục này để buộc xây dựng lại hoàn toàn các tài sản frontend.

### `data/access.log`

Một tệp log ghi lại các yêu cầu HTTP đến máy chủ, sau lần kết nối thành công đầu tiên.

Hãy kiểm tra tệp này thường xuyên để theo dõi bất kỳ hoạt động đáng ngờ nào.

### `data/cookie-secret.txt`

Chứa khóa bí mật được sử dụng để ký cookie trong máy chủ.

Tệp này được tự động tạo ra khi khởi động lần đầu nếu nó không tồn tại.

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
10. Cấu hình và bật [whitelist địa chỉ riêng tư](/Administration/config-yaml.md#private-address-whitelisting) để ngăn chặn các cuộc tấn công SSRF.

Tìm thêm thông tin về proxy an toàn trong hướng dẫn sau: [Reverse Proxying SillyTavern](reverse-proxying).
