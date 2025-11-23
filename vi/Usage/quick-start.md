---
order: 190
icon: rocket
route: /usage/quick-start/
---

# Khởi động nhanh

!!!light
Tôi không biết gì cả. Hãy chỉ cho tôi cách dễ nhất và nhanh nhất để bắt đầu sử dụng SillyTavern. -- *Ẩn danh*
!!!

Bạn có thể bắt đầu với SillyTavern chỉ trong vài phút. Dưới đây là hai cách dễ dàng để bắt đầu:

* Bạn có thể [sử dụng AI Horde](#khoi-dong-nhanh-voi-ai-horde) miễn phí. AI Horde là một dịch vụ AI cộng đồng cung cấp quyền truy cập vào nhiều mô hình AI khác nhau.

* Nếu bạn có tài khoản OpenAI hoặc muốn đăng ký một tài khoản, bạn có thể [sử dụng OpenAI](#khoi-dong-nhanh-voi-openai).

## Khởi động nhanh với AI Horde

1. Làm theo [Hướng dẫn cài đặt](/Installation/index.md) để cài đặt và khởi động SillyTavern.

2. Trong màn hình giới thiệu của SillyTavern, nhập tên cho persona của bạn. Tên này sẽ được sử dụng trong cuộc trò chuyện.

   ![This is an optional caption](/static/quick-start/1_name.png)
3. Nhấp vào nút API Connections ở thanh trên cùng.

   ![This is an optional caption](/static/quick-start/2_api_conn.png)
4. Nhập khóa API cho AI Horde. Bạn có thể sử dụng `0000000000` hiện tại, hoặc lấy khóa miễn phí từ [AI Horde](https://aihorde.net/).

   ![This is an optional caption](/static/quick-start/3_horde_key.png)
5. Chọn một số mô hình AI để sử dụng. Chỉ cần chọn một vài mô hình từ đầu danh sách. Bạn luôn có thể thay đổi chúng sau.

   ![This is an optional caption](/static/quick-start/4_horde_models.png)
6. Đóng cửa sổ API Connections. Nhập tin nhắn vào ô trò chuyện ở phía dưới và nhấn Enter.

   ![This is an optional caption](/static/quick-start/5_msg.png)
7. AI của bạn sẽ phản hồi trong vài giây. Bạn có thể tiếp tục [trò chuyện](/Usage/Chatting/index.md) với nó. Thành công!

   ![This is an optional caption](/static/quick-start/6_success.png)

## Khởi động nhanh với OpenAI

### Cài đặt SillyTavern

Làm theo [Hướng dẫn cài đặt](/Installation/index.md) để cài đặt và khởi động SillyTavern.

### Truy cập OpenAI

1. Đăng ký tài khoản OpenAI.
2. Truy cập <https://platform.openai.com>
3. Nhấp vào biểu tượng tài khoản của bạn ở góc trên bên phải, sau đó chọn View API Keys.
4. Nhấp vào "Create new secret key". Sao chép ngay lập tức. **KHÔNG CHIA SẺ KHÓA NÀY. BẤT KỲ AI CÓ NÓ CÓ THỂ SỬ DỤNG TÀI KHOẢN CỦA BẠN ĐỂ SỬ DỤNG GPT CHI PHÍ CỦA BẠN.**

### Cấu hình SillyTavern để sử dụng API của bạn

1. Trong thanh trên cùng của SillyTavern, nhấp vào API Connections.
2. Trong mục API, chọn Chat Completion (OpenAI).
3. Trong mục Chat Completion Source, chọn OpenAI.
4. Dán khóa API bạn đã lưu ở bước trước.
5. Nhấp vào nút Connect. Xác nhận rằng nó hiển thị Valid.
6. Mặc định, SillyTavern sẽ sử dụng GPT-4 Turbo. Bạn có thể chọn một mô hình khác, nhưng hãy tìm hiểu về giá cả.

### Kiểm tra thiết lập của bạn

1. Trong thanh trên cùng của SillyTavern, nhấp vào Character Management ở bên phải nhất.
2. Chọn một nhân vật hiện có như Seraphina.
3. Trong hộp văn bản ở phía dưới, viết điều gì đó cho Seraphina, sau đó nhấn Enter hoặc nhấp vào nút Send.

Nếu bạn làm mọi thứ đúng, sau vài giây, Seraphina sẽ phản hồi.
