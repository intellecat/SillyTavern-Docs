---
route: /vi/extensions/speech-recognition/
---

# Nhận dạng giọng nói

Hướng dẫn này sẽ hướng dẫn bạn cách thiết lập nhận dạng giọng nói để phiên âm giọng nói của bạn thành văn bản trong SillyTavern.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy chắc rằng bạn đã đáp ứng các điều kiện tiên quyết sau:

- Hãy chắc rằng bạn đang sử dụng phiên bản mới nhất của SillyTavern.
- Cài đặt extension "Speech Recognition" từ menu "Download Extensions & Assets" trong Extensions panel (icon stacked blocks).

## Thiết lập Nhận dạng Giọng nói (Trình duyệt)

1. **Cấu hình SillyTavern**:
   - Khởi chạy SillyTavern và đi tới **Extensions** > **Speech Recognition**.
   - Chọn "Browser" từ các tùy chọn dropdown.
   - Nếu trình duyệt của bạn không hỗ trợ nhận dạng giọng nói, một popup lỗi sẽ xuất hiện.

2. **Chọn Message Mode**:
   - Chọn "Message Mode" bạn muốn:
     - **Append**: Tin nhắn của bạn sẽ được nối thêm vào vùng văn bản tin nhắn người dùng hiện tại.
     - **Replace**: Tin nhắn của bạn sẽ thay thế tin nhắn người dùng hiện tại trong vùng văn bản.
     - **Auto send**: Tin nhắn của bạn sẽ được tự động gửi sau khi phát hiện được kết thúc giọng nói.

3. **Bật Message Mapping** *(Tùy chọn)*:
   - Thiết lập các mô tả của cụm từ cho các phím tắt giọng nói.
   - Ví dụ, bằng cách thêm "command delete = /del2", lệnh "/del2" sẽ thay thế tin nhắn giọng nói của bạn khi "command delete" được phát hiện.
   - Hữu ích khi kết hợp với chế độ auto send để kiểm soát giọng nói đầy đủ. Bật tính năng này bằng cách chọn "Enable messages mapping".

4. **Chọn Ngôn ngữ**:
   - Chọn ngôn ngữ bạn muốn nói (Lưu ý: không phải mọi trình duyệt đều hỗ trợ tất cả các ngôn ngữ).

5. **Ghi âm**:
   - Để bắt đầu ghi âm, nhấp vào nút microphone ở bên phải vùng tin nhắn bên cạnh nút send. Nhấp lại để dừng ghi âm. Ghi âm có thể dừng tự động nếu không phát hiện được giọng nói.

## Thiết lập Nhận dạng Giọng nói (Nguồn API)

Hỗ trợ các nguồn như OpenAI, MistralAI, Groq, Chutes, Z.AI và các nguồn khác cung cấp API chuyển giọng nói thành văn bản (speech-to-text).

Để thiết lập:

1. Cung cấp API key cho nhà cung cấp đã chọn trong cài đặt Chat Completion API.
2. Khởi chạy SillyTavern và đi tới **Extensions** > **Speech Recognition**.
3. Chọn nguồn API mong muốn từ các tùy chọn dropdown.
4. Cấu hình thêm các cài đặt cần thiết, tương tự như thiết lập nhà cung cấp "Browser".

## Thiết lập Nhận dạng Giọng nói (Extras) - Đã ngừng dùng

!!!
Yêu cầu ffmpeg binary được cài đặt. Xem [RVC setup](RVC.md#rvc-setup) để biết thêm chi tiết.
!!!

1. **Bật Provider**:
   - Bật nhà cung cấp nhận dạng giọng nói mong muốn trên máy chủ extras bằng lệnh sau:
     ```shell
     python server.py --enable-modules=whisper-stt
     ```
     hoặc
     ```shell
     python server.py --enable-modules=vosk-stt
     ```
   - Bạn cũng có thể sử dụng một mô hình tùy chỉnh bằng cách thêm tùy chọn `--stt-vosk-model-path` hoặc `--stt-whisper-model-path` với đường dẫn đến mô hình.

2. **Cấu hình SillyTavern**:
   - Khởi chạy SillyTavern và đi tới **Extensions** > **Speech Recognition**.
   - Chọn "Vosk" hoặc "Whisper" từ các tùy chọn dropdown (whisper chính xác hơn).
   - Các cài đặt tương tự như thiết lập nhà cung cấp "Browser" (ngoại trừ ngôn ngữ) xem trên.

## Thiết lập Nhận dạng Giọng nói (Streaming) - Đã ngừng dùng

!!!
Yêu cầu ffmpeg binary được cài đặt. Xem [RVC setup](RVC.md#rvc-setup) để biết thêm chi tiết.
!!!

1. **Bật Provider**:
   - Bật module nhận dạng giọng nói streaming trên Sillytavern-extras bằng lệnh sau:
     ```shell
     python server.py --enable-modules=streaming-stt
     ```

2. **Cấu hình SillyTavern**:
   - (Tùy chọn) Chỉ định một mô hình Whisper tùy chỉnh như trong thiết lập Whisper ở trên.
   - (Tùy chọn nhưng được khuyến nghị) Thiết lập các từ trigger trong SillyTavern. Chỉ các tin nhắn bắt đầu với các từ trigger này sẽ được gửi tới SillyTavern dưới dạng các tin nhắn thực tế. Điều này ngăn chặn các giọng nói ngẫu nhiên hoặc tiếng ồn không được phiên âm. Bật tính năng này bằng hộp kiểm. Các từ trigger có thể được bao gồm/loại trừ khỏi tin nhắn thực tế bằng cách sử dụng một hộp kiểm.
   - Các cài đặt khác tương tự như các nhà cung cấp khác.

Bây giờ bạn đã sẵn sàng để phiên âm giọng nói của bạn thành văn bản bằng nhận dạng giọng nói trong SillyTavern.
