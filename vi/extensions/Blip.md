---
route: /vi/extensions/blip/
---

# Blip

Hướng dẫn này sẽ dẫn bạn qua quá trình thiết lập và tùy chỉnh tiện ích mở rộng blip cho trải nghiệm SillyTavern của bạn. Tiện ích mở rộng này tạo hiệu ứng động cho văn bản tin nhắn với tốc độ thay đổi và phát âm thanh kèm theo hiệu ứng. Bạn có thể sử dụng tệp âm thanh hoặc tạo âm thanh.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các điều kiện tiên quyết sau:

- Đảm bảo bạn đang sử dụng phiên bản mới nhất của SillyTavern.
- Cài đặt tiện ích mở rộng "Blip" từ menu "Download Extensions & Assets" trong bảng Extensions (biểu tượng khối xếp chồng).

## Cài đặt chung Blip

1. **Blip user message**:
   - Bật hộp kiểm để phát hiệu ứng trên tin nhắn người dùng.
   - Đặt hồ sơ cho người dùng hoặc hồ sơ mặc định nếu bạn muốn hiệu ứng blip cho người dùng.

2. **Blip only for certain text**:
   - Bật hộp kiểm để chỉ blip cho văn bản trong dấu ngoặc kép.
   - Bật hộp kiểm để bỏ qua mọi thứ bên trong dấu sao.

3. **Automatic scroll down**:
   - Bật hộp kiểm để làm cho cuộc trò chuyện cuộn xuống theo hiệu ứng văn bản, tắt nó nếu bạn muốn cuộn tự do trong khi hiệu ứng.

4. **Audio volume**
   - Tắt tiếng âm thanh nếu chỉ muốn hiệu ứng văn bản.
   - Bạn có thể điều chỉnh âm lượng chung của âm thanh blip.

## Hồ sơ hiệu ứng/giọng nói nhân vật

Bạn có thể lưu hồ sơ cho mỗi nhân vật:
   - bao gồm người dùng và hồ sơ mặc định tùy chọn sẽ được sử dụng khi nhân vật không có hồ sơ.
   - Nếu chỉ hiển thị các nhân vật trong cuộc trò chuyện hiện tại trong danh sách, hãy nhấp vào hộp kiểm để hiển thị tất cả nhân vật của bạn.

1. **Chọn nhân vật để gán/cập nhật hồ sơ**:
   - Chọn một nhân vật, nếu họ có hồ sơ nó sẽ được tải.
   - Nếu chưa có hồ sơ, các tham số hiện tại sẽ trở thành cài đặt hồ sơ của họ.
   - Bất kỳ hồ sơ nào cũng có thể bị xóa bằng nút remove.
   - Sử dụng nút refresh nếu nhân vật của bạn không xuất hiện trong danh sách.

2. **Cài đặt hiệu ứng văn bản**:
   - Đặt tốc độ văn bản: độ trễ tính bằng mili giây giữa mỗi chữ cái được in.
   - Đặt bộ nhân tốc độ Min/max khác với 1.0 để tạo tính ngẫu nhiên cho hiệu ứng tốc độ.
   - Đặt độ trễ dấu phẩy/cụm từ lớn hơn 0 để thêm khoảng dừng khi in ký tự đặc biệt, có thể thêm sự sống động hơn cho hiệu ứng. Âm thanh cũng bị tạm dừng trong trường hợp này.

3. **Tham số âm thanh**:
   - Đặt bộ nhân âm lượng chỉ ảnh hưởng đến hồ sơ giọng nói này nếu cần.
   - Đặt tốc độ âm thanh: độ trễ giữa mỗi âm thanh blip, độc lập với tốc độ văn bản.

4. **Nguồn Blip: Âm thanh được tạo**:
   - Sử dụng thanh trượt tần số min/max để tùy chỉnh âm thanh blip được phát.
   - Nếu min/max khác nhau, một âm thanh ngẫu nhiên trong phạm vi này sẽ được phát mỗi lần.

5. **Nguồn Blip: file**:
   - Chọn một tệp trong danh sách.
   - Bạn có thể tải các tài nguyên blip chính thức của ST từ menu tiện ích mở rộng assets.
   - Hoặc đặt tệp trực tiếp vào: `\SillyTavern\data\<user-handle>\assets\blip`.
   - Bật hộp kiểm để buộc đợi toàn bộ tệp được phát trước khi phát lại nếu cần.

Cảm ơn bạn đã làm theo hướng dẫn này! Trải nghiệm SillyTavern của bạn giờ đây được làm phong phú với hiệu ứng văn bản và giọng nói blip.
