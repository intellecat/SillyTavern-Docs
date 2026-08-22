---
order: tts-minimax
route: /vi/extensions/minimaxtts/
---

# MiniMax TTS

Trang này sẽ hướng dẫn bạn cách sử dụng đúng nhà cung cấp TTS MiniMax.

## Điều kiện tiên quyết

1. Tài khoản MiniMax với quyền truy cập API
2. API Key và Group ID hợp lệ từ MiniMax

## Lấy thông tin đăng nhập API

### 1. Tạo tài khoản MiniMax

1. Truy cập [trang web MiniMax (Quốc tế)](https://www.minimax.io/)
2. Nhấp "Sign Up" hoặc "Login"
3. Hoàn thành quá trình đăng ký tài khoản

!!!warning Sự khác biệt theo khu vực
MiniMax có các phiên bản tiếng Trung và Quốc tế riêng biệt. Vui lòng lưu ý:
- Phiên bản tiếng Trung không hỗ trợ tính năng nhân bản giọng nói
- Phiên bản tiếng Trung chỉ hỗ trợ máy chủ API `api.minimax.chat`
!!!

### 2. Lấy API Key và Group ID

1. Đăng nhập vào [console MiniMax (Quốc tế)](https://www.minimax.io/platform/user-center/basic-information)
2. Bạn có thể tìm thấy GroupId của mình trên trang Basic Information
3. Đi tới Settings → API Keys trên thanh bên trái để tạo và lấy API Key của bạn

## Cấu hình trong SillyTavern

### 1. Thiết lập cơ bản

1. Mở SillyTavern
2. Điều hướng đến "Extensions" → "TTS"
3. Chọn "MiniMax" làm nhà cung cấp TTS của bạn
4. Cấu hình các cài đặt sau:
    - **API Key**: API key MiniMax của bạn
    - **Group ID**: Group ID MiniMax của bạn
    - **API Host**: Chọn máy chủ phù hợp dựa trên khu vực của bạn:
        - `api.minimax.io` (Máy chủ quốc tế chính thức)
        - `api.minimaxi.chat` (Máy chủ quốc tế khác)
        - `api.minimax.chat` (Máy chủ Trung Quốc đại lục)

### 2. Lựa chọn mô hình

Các mô hình có sẵn bao gồm:
- **Speech-02-HD**: Tổng hợp giọng nói chất lượng cao (được khuyến nghị)
- **Speech-02-Turbo**: Tổng hợp giọng nói nhanh
- **Speech-01**: Mô hình cũ
- **Speech-01-240228**: Mô hình cũ (phiên bản cụ thể)

### 3. Tham số giọng nói

Điều chỉnh các tham số sau để tùy chỉnh đầu ra giọng nói:
- **Speed**: 0.5 - 2.0 (1.0 = tốc độ bình thường)
- **Volume**: 0.1 - 2.0 (1.0 = âm lượng bình thường)
- **Pitch**: 0.5 - 2.0 (1.0 = cao độ bình thường)
- **Audio Format**: MP3, WAV, FLAC

## Giọng nói tùy chỉnh

### 1. Lấy Voice ID

1. Truy cập [trang TTS MiniMax (Quốc tế)](https://www.minimax.io/audio/text-to-speech)
2. Nhấp "Voice" ở bên phải để vào giao diện Voice Selection
3. Tìm giọng nói bạn muốn sử dụng
4. Nhấp nút sao chép bên cạnh tên giọng nói để sao chép Voice ID

### 2. Thêm giọng nói tùy chỉnh

1. Trong cài đặt MiniMax TTS, tìm phần "Custom Voice Management"
2. Điền thông tin sau:
    - **Voice Name**: Chọn bất kỳ tên nào để nhận dạng
    - **Voice ID**: Voice ID lấy từ nền tảng MiniMax
    - **Language**: Chọn ngôn ngữ tương ứng cho giọng nói
3. Nhấp "Add Custom Voice"

## Mô hình tùy chỉnh

### 1. Thêm mô hình tùy chỉnh

1. Trong phần "Custom Model Management"
2. Điền:
    - **Model ID**: Mã định danh mô hình
    - **Model Name**: Tên hiển thị cho mô hình
3. Nhấp "Add Custom Model"

### 2. Lấy Model ID

1. Kiểm tra danh sách mô hình trong [tài liệu MiniMax](https://www.minimax.io/platform/document/Model?key=684261f14c5738213294faa7) chính thức
2. Hoặc xem các mô hình tùy chỉnh có sẵn trong console
3. Sao chép Model ID tương ứng

## Khắc phục sự cố

### Vấn đề phổ biến

1. **Xác thực API thất bại**
    - Xác minh rằng API Key tương ứng với API Host đúng
    - Xác nhận rằng Group ID là đúng
    - Kiểm tra xem tài khoản của bạn có đủ số dư không

2. **Tạo giọng nói thất bại**
    - Xác minh rằng Voice ID đã chọn là hợp lệ
    - Đảm bảo giọng nói tương thích với mô hình đã chọn của bạn

3. **Hết thời gian kết nối**
    - Thử chuyển sang API Host khác
    - Kiểm tra kết nối mạng của bạn
    - Xác minh cài đặt tường lửa

4. **Vấn đề chất lượng âm thanh**
    - Thử sử dụng mô hình khác (Speech-02-HD cho chất lượng tốt nhất)
    - Điều chỉnh tham số giọng nói (tốc độ, cao độ, âm lượng)
    - Kiểm tra khả năng tương thích định dạng âm thanh
