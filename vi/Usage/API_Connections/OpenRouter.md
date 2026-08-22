---
order: 10
route: /vi/usage/api-connections/openrouter/
---

# OpenRouter

!!!info
OpenRouter có sẵn dưới dạng cả nguồn Text Completion và Chat Completion. Tất cả các mô hình đều có sẵn thông qua cả hai API, nhưng các tính năng của chúng có thể khác nhau tùy thuộc vào loại API bạn chọn. Ví dụ, inline hình ảnh và tool calling chỉ có sẵn với Chat Completion API.
!!!

Không muốn đăng ký hàng tá dịch vụ API, nhưng vẫn muốn truy cập tất cả các mô hình mới nhất? Hãy sử dụng OpenRouter.

OpenRouter hoạt động bằng cách cho phép bạn sử dụng một endpoint duy nhất để truy cập các mô hình như DeepSeek, Claude và Gemini, tất cả trong một dịch vụ với một pool tín dụng chung.

Nó có bản dùng thử miễn phí (khoảng $1) và trả phí sau đó. Không có đăng ký hoặc hóa đơn hàng tháng - bạn chỉ trả cho những gì bạn thực sự sử dụng. Một số mô hình có quyền truy cập miễn phí với số lượng yêu cầu hàng ngày giới hạn.

!!!tip
Để có quyền truy cập vĩnh viễn vào các mô hình miễn phí với giới hạn hàng ngày hào phóng, bạn cần mua ít nhất $10 tín dụng **một lần**.

Xem thêm chi tiết trên [OpenRouter FAQ page](https://openrouter.ai/docs/faq).
!!!

- Tạo tài khoản OpenRouter: [openrouter.ai](https://openrouter.ai/)
- [Danh sách mô hình OpenRouter](https://openrouter.ai/models?order=pricing-low-to-high)

![OpenRouter-ConnectionPanel](/static/openrouter-connection.png)

Từ trên xuống dưới (xem hình trên):

1. Chọn API 'Chat Completion'.
2. Chọn OpenRouter làm nguồn.
3. Nhấp "Authorize" để lấy key bằng cách sử dụng luồng OAuth. Hoặc, tạo API key [tại đây](https://openrouter.ai/keys) và dán vào hộp.
4. Nhấp "Connect" và chọn một mô hình.
5. (Tùy chọn) Sử dụng nút "Test Message" để xác minh kết nối của bạn.
