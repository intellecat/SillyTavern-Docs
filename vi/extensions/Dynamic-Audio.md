---
route: /vi/extensions/dynamic-audio/
---

# Dynamic Audio

Hướng dẫn này sẽ dẫn bạn qua quá trình thiết lập và tùy chỉnh tài nguyên âm thanh động cho trải nghiệm SillyTavern của bạn.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các điều kiện tiên quyết sau:

- Đảm bảo bạn đang sử dụng phiên bản mới nhất của SillyTavern.
- Cài đặt tiện ích mở rộng "Dynamic Audio" từ menu "Download Extensions & Assets" trong bảng Extensions (biểu tượng khối xếp chồng).

## Thiết lập Dynamic Audio (Trình duyệt)

1. **Kết nối với Kho tài nguyên**:
   - Khởi chạy SillyTavern và điều hướng đến **Extensions** > **Assets**.
   - Nhấp vào nút "Connect" để thiết lập kết nối với kho tài nguyên chính thức.
   - Tải xuống các tài nguyên âm thanh mong muốn, chẳng hạn như nhạc nền (BGM) hoặc âm thanh xung quanh, tương ứng với các nền bạn định sử dụng.

2. **Bật tiện ích mở rộng Dynamic Audio**:
   - Trong SillyTavern, đi tới **Extensions** > **Dynamic Audio**.
   - Bật tiện ích mở rộng, bỏ tắt tiếng và điều chỉnh âm lượng của BGM và âm thanh xung quanh theo sở thích của bạn.
   - Khi bgm kết thúc, một bài khác sẽ phát ngẫu nhiên, nhấp vào nút vòng lặp để giữ bgm hiện tại tiếp tục phát
   - Nhấp vào nút roll để chọn một bgm khác ngẫu nhiên

3. **BGM dựa trên biểu cảm**:
   - Bật chuyển đổi BGM biểu cảm nếu bạn muốn bgm theo dõi biểu cảm nhân vật (yêu cầu bgm trong thư mục nhân vật xem bên dưới).
   - Điều chỉnh bộ đếm thời gian cooldown (tính bằng giây) giữa các cập nhật BGM. Tăng nó nếu bạn thấy BGM thay đổi quá thường xuyên trong cuộc trò chuyện nhóm hoặc khi sử dụng BGM dành riêng cho nhân vật với phát hiện cảm xúc.

## Nhập nhạc cho nhân vật

Để thiết lập nhạc tùy chỉnh cho cảm xúc của nhân vật, hãy làm theo các bước sau:

1. **Điều hướng đến thư mục nhân vật**:
   - Đi tới thư mục characters, ví dụ: `\SillyTavern\data\<user-handle>\characters\Seraphina`.

2. **Tạo thư mục BGM**:
   - Bên trong thư mục nhân vật, tạo một thư mục con có tên `bgm`.

3. **Nhập nhạc cảm xúc**:
   - Trong thư mục `bgm`, nhập các tệp nhạc cho mỗi cảm xúc. Các phần mở rộng âm thanh được hỗ trợ bao gồm `.mp3`, `.ogg` và `.wav`.
   - Quy ước đặt tên: `[emotion]_[number].mp3`, ví dụ: `anger_0.mp3`, `joy_0.mp3`.

4. **Nhiều bài hát cho các cảm xúc**:
   - Bạn có thể nhập nhiều bài hát cho cùng một cảm xúc bằng cách tăng số, ví dụ: `neutral_1.mp3`, `neutral_2.mp3`.

5. **Chọn nhạc mặc định**:
   - Khi không phát hiện cảm xúc, một bài hát neutral ngẫu nhiên sẽ phát làm mặc định. Các cảm xúc được phát hiện tương tự như cập nhật sprites; tham khảo [tài liệu expression images](/extensions/Expression-Images.md) để biết chi tiết.

## Thay đổi nhạc BGM mặc định

Nếu một nhân vật không có BGM tùy chỉnh trong thư mục của họ, một bài hát mặc định sẽ phát. Đây là cách bạn có thể thay đổi nó:

1. **Điều hướng đến thư mục BGM**:
   - Đi tới thư mục sau: `\SillyTavern\data\<user-handle>\assets\bgm`.

2. **Thay thế/Thêm nhạc**:
   - Thay thế hoặc thêm tệp nhạc (`.mp3`, `.ogg`, `.wav`) vào thư mục này.
   - Đây là các tài nguyên âm thanh chính thức được tải xuống bằng tiện ích mở rộng assets.
   - Một trong những bài hát này sẽ phát ngẫu nhiên khi không tìm thấy BGM dành riêng cho nhân vật (cuộc trò chuyện đơn hoặc nhóm).

## Thay đổi âm thanh xung quanh

Âm thanh xung quanh thêm chiều sâu cho cảnh của bạn. Đây là cách bạn có thể tùy chỉnh chúng:

1. **Điều hướng đến thư mục Ambient**:
   - Đi tới thư mục sau: `\SillyTavern\data\<user-handle>\assets\ambient`.

2. **Quy ước đặt tên tệp**:
   - Tên tệp âm thanh xung quanh tương ứng với tên tệp hình ảnh nền, thay thế khoảng trắng bằng dấu gạch ngang.
   - Ví dụ: `"bedroom-clean.mp3"` tương ứng với nền "bedroom clean.jpg".
   - Nếu nút khóa được mở khóa, tệp âm thanh tương ứng với nền sẽ phát. Kích hoạt khóa sẽ giữ âm thanh xung quanh hiện tại tiếp tục phát.

3. **Ambient tùy chỉnh**:
   - Bạn có thể thêm âm thanh xung quanh của riêng mình cho các nền tùy chỉnh hoặc hiện có bằng cách làm theo cùng một mẫu đặt tên.

Cảm ơn bạn đã làm theo hướng dẫn này! Trải nghiệm SillyTavern của bạn giờ đây được làm phong phú với âm thanh động.
