---
label: VPN và Tunneling
order: -40
icon: lock
route: /administration/tunneling/
---

VPN và tunnel là một cách an toàn để truy cập mạng gia đình của bạn từ bất kỳ đâu trên thế giới. Hướng dẫn này sẽ chỉ cho bạn cách sử dụng VPN hoặc tunnel để truy cập instance SillyTavern của bạn từ bất kỳ đâu.

## Phương Pháp

1. Sử dụng **VPN tự tạo**.

   Một số router đi kèm với khả năng lưu trữ máy chủ VPN (chủ yếu là OpenVPN hoặc WireGuard) trong trang quản trị router. Tham khảo hướng dẫn sử dụng router của bạn để thiết lập VPN và thêm thiết bị của bạn vào VPN. Sau khi kết nối, chỉ cần truy cập IP riêng mà bạn đã đặt cho SillyTavern và bạn có thể kết nối dễ dàng. Dễ dàng hơn cho người dùng và cho việc sử dụng Windows.

2. Sử dụng [**Cloudflare Zero Trust**](https://developers.cloudflare.com/cloudflare-one/).

   Cloudflare Zero Trust là một tính năng tổ chức miễn phí trong Cloudflare cho phép bạn thêm 50 người dùng. Điều này sẽ proxy lưu lượng truy cập của bạn thông qua Cloudflare và bằng cách thêm PC ST của bạn làm tunnel bằng cách sử dụng `cloudflared`, bạn có thể kết nối với instance ST của mình như thể bạn đang ở nhà.

   Lưu ý rằng sau khi tạo tunnel, bạn sẽ phải thêm một route đến địa chỉ IP riêng của router và tính toán các giá trị IP CIDR để có quyền truy cập cục bộ đầy đủ khi di chuyển bằng Cloudflare Zero Trust.

3. Sử dụng tunnel **Cloudflare** hoặc **[ngrok](https://ngrok.com)** độc lập.

   Tương tự như cách các backend AI có thể kết nối, bạn cũng có thể kết nối instance ST của mình qua Cloudflare Tunnel và mở trang Cloudflare Tunnel. Tuy nhiên, bạn sẽ phải sao chép và dán mỗi liên kết mới được tạo bởi Cloudflare/NGROK mỗi khi bạn muốn sử dụng ST khi di chuyển.

4. Sử dụng **Tailscale**.

   Tailscale là một nhà cung cấp VPN cho phép kết nối từ xa an toàn đến PC của bạn.

## Thiết lập Tailscale

Tailscale là một nhà cung cấp VPN cho phép kết nối từ xa an toàn đến PC của bạn. Một triển khai mã nguồn mở của máy chủ Tailscale tồn tại và bạn cũng có thể lưu trữ máy chủ bằng cách sử dụng [Headscale](https://github.com/juanfont/headscale), nhưng điều đó nằm ngoài phạm vi của hướng dẫn này.

### 1. Tạo tài khoản

* Truy cập [trang web của Tailscale](https://tailscale.com/) và tạo một tài khoản mới.

**LƯU Ý:** Để sử dụng hàng ngày bởi một người, Tailscale sẽ miễn phí vĩnh viễn. Nếu bạn lo sợ chi phí ẩn, chỉ cần không thêm bất kỳ tùy chọn thanh toán nào.

### 2. Thiết lập client

* Truy cập [trang tải xuống của Tailscale](https://tailscale.com/download) và tải xuống client/app trên thiết bị mà bạn có SillyTavern đang chạy và trên thiết bị mà bạn muốn sử dụng từ vị trí từ xa.
* Đăng nhập trên cả hai thiết bị bằng tài khoản bạn đã tạo trước đó.
* Truy cập [trang quản trị của Tailscale](https://login.tailscale.com/admin/machines) và phê duyệt cả hai thiết bị.
* Ghi nhớ tên của cả hai thiết bị đã kết nối.

### 3. Thêm thiết bị của bạn vào whitelist

* Thêm tên máy của thiết bị kết nối của bạn (thiết bị mà bạn muốn sử dụng SillyTavern) vào whitelist của SillyTavern bằng cách làm theo [Quản lý IP được whitelist](./remote-connections.md#whitelist-based-access-control).

### 4. Kết nối

Bây giờ bất cứ khi nào bạn muốn sử dụng SillyTavern từ bất kỳ đâu, tất cả những gì bạn phải làm là:

* Bật Tailscale trên cả PC lưu trữ SillyTavern và thiết bị của bạn muốn sử dụng nó từ xa.
* Mở trình duyệt trên thiết bị muốn kết nối và truy cập `http://<machine name of PC running st>:8000/`

### 5. Chia sẻ instance SillyTavern với bạn bè (tùy chọn)

* Yêu cầu bạn của bạn tạo tài khoản Tailscale của riêng họ và tải xuống client trên thiết bị của họ.
* Truy cập [trang quản trị của Tailscale](https://login.tailscale.com/admin/machines).
* Di chuột qua nút ba chấm trên PC lưu trữ SillyTavern của bạn và nhấn "Share..." hoặc nhấn nút ba chấm và nhấn "Sharing settings...".
* Bỏ chọn "Allow use as an exit node" (trừ khi bạn muốn bạn của mình có thể định tuyến tất cả lưu lượng internet của họ thông qua PC của bạn).
* Gửi liên kết dưới dạng email hoặc chuyển tab sang "Copy share link", nhấn nút lớn màu xanh có cùng văn bản, và gửi nó cho bạn của bạn theo bất kỳ cách nào khác.
* Sau khi nhấp vào liên kết chia sẻ của bạn, bạn của bạn sẽ thấy PC của bạn xuất hiện trong mạng Tailscale của họ.
* Gửi cho bạn của bạn cùng liên kết mà bạn sử dụng để truy cập SillyTavern như đã giải thích trong bước cuối cùng.

**LƯU Ý:** Điều này sẽ cấp cho bạn của bạn quyền truy cập đầy đủ vào bất kỳ dịch vụ nào đang chạy cục bộ trên PC của bạn như SillyTavern, automatic1111, v.v. Chỉ làm điều này nếu bạn thực sự tin tưởng bạn của mình.
