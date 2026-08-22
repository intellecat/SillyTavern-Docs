---
route: /vi/usage/api-connections/horde/
---

# AI Horde

## Tuyên Bố Từ Chối Trách Nhiệm

- AI Horde là cụm GPU phân tán, crowdsourced được vận hành hoàn toàn bởi các tình nguyện viên.
- Theo mặc định, đầu vào của bạn được gửi ẩn danh và phản hồi không thể được nhìn thấy bởi người chạy Horde Worker.
- Tuy nhiên, vì nó là một chương trình mã nguồn mở, các Worker độc hại có thể sửa đổi mã để:
  - ghi lại hoạt động của bạn (prompt đầu vào, phản hồi AI).
  - tạo ra phản hồi xấu hoặc xúc phạm.

!!!warning
Khi sử dụng Horde **không bao giờ gửi** bất kỳ thông tin cá nhân nào như tên, địa chỉ email, v.v.
!!!

Bật hộp kiểm "Trusted Workers Only" sẽ giới hạn lựa chọn các worker có sẵn chỉ cho những người đã lưu trữ trên Horde một thời gian và thường được coi là đáng tin cậy. Nhưng họ vẫn có thể nhìn thấy prompt, ví dụ bằng cách lưu trữ bằng phần mềm không được kiểm soát.

Để giúp giảm vấn đề này, SillyTavern đã tích hợp tính năng sau:

- Khi phản hồi trò chuyện được tạo bởi Horde Worker, SillyTavern ghi lại ID của Worker và mô hình họ đang sử dụng.
- Thông tin này có thể được xem bằng cách di chuột qua mục trò chuyện (xem hình dưới đây).
- Nếu bạn tin rằng mình nhận được phản hồi độc hại, bạn có thể chuyển thông tin này cho quản trị viên Horde trên [AI Horde Discord](https://discord.gg/3DxrhksKzn) để xem xét và có thể xử lý kỷ luật Worker đó.

![Horde Worker Info Popup](/static/horde-worker.png)

## Thiết Lập

- SillyTavern có thể kết nối với Horde ngay lập tức mà không cần thiết lập bổ sung.
- Chọn 'AI Horde' từ API Dropdown Selector trong ST API Panel.
- Chọn một hoặc nhiều mô hình ('bộ não AI' cho các nhân vật) từ Model Selector ở cuối panel.
- Chọn một nhân vật và bắt đầu trò chuyện.

![ST Kobold Horde API Connection Panel](/static/horde-config.png)

!!!warning
Theo mặc định, instance SillyTavern của bạn kết nối với 'guest account' có độ ưu tiên thấp của Horde.
Điều này có nghĩa là bạn có thể phải đợi lâu để nhận được phản hồi.
Để giảm thời gian chờ, hãy làm theo các mẹo dưới đây.
!!!

## Mẹo

- [Đăng ký tài khoản trên trang web Horde](https://aihorde.net/register) sau đó thêm Horde key của bạn vào hộp Horde API Key của SillyTavern.
- [Thiết lập Horde Worker](https://github.com/Haidra-Org/AI-Horde-Worker#readme) để cung cấp GPU của bạn cho người khác.
  - Để người khác sử dụng GPU của bạn kiếm cho bạn ['Kudos', một loại tiền tệ chỉ dành cho Horde](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#kudos).
  - Tài khoản của bạn có càng nhiều kudos, bạn sẽ nhận được phản hồi trò chuyện từ các Horde Worker khác càng nhanh.
  - Kudos cũng có thể được sử dụng để tạo hình ảnh AI trên [Stable Horde](https://stablehorde.net).
    - SillyTavern hỗ trợ tạo hình ảnh Stable Horde ngay lập tức.
- Nếu GPU của bạn không đủ mạnh để chạy AI, hoặc bạn không có máy tính, bạn vẫn có thể [tham gia cộng đồng Horde để kiếm Kudos theo nhiều cách khác nhau](https://github.com/Haidra-Org/AI-Horde/blob/main/FAQ.md#i-dont-have-a-powerful-gpu-how-can-i-get-kudos).
