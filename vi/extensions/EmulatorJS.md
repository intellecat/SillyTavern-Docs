---
route: /vi/extensions/emulatorjs/
templating: false
---

# EmulatorJS

Tiện ích mở rộng này cho phép bạn chơi trò chơi console retro ngay từ cuộc trò chuyện SillyTavern.

## Cài đặt

**Điều kiện tiên quyết:**

- Phiên bản phát hành mới nhất của SillyTavern.
- Tệp ROM được tải xuống từ mạng. Bạn có thể tìm thấy chúng [ở bất cứ đâu](https://archive.org/details/ni-romsets).

**Cách cài đặt:**

1. Cài đặt bằng trình tải xuống tiện ích mở rộng của SillyTavern.
2. Hoặc sử dụng liên kết này: `https://github.com/SillyTavern/SillyTavern-EmulatorJS`

## Sử dụng

- Mở menu tiện ích mở rộng "EmulatorJS".
- Nhấp "Add ROM file". ROM được lưu vào bộ nhớ trình duyệt của bạn và không được lưu trên máy chủ.
- Chọn tệp trò chơi để thêm. Nhập tên và core (nếu nó không được tự động phát hiện). Nếu core yêu cầu tệp BIOS, hãy thêm nó.
- Nhấp nút "Play" trong danh sách hoặc khởi chạy qua menu wand.
- Bạn có thể tùy chỉnh điều khiển và cài đặt khác trong khung giả lập sau khi khởi chạy trò chơi.
- Sử dụng chức năng save/load state nếu bạn cần nghỉ ngơi.

Kiểm tra tài liệu EmulatorJS để xem danh sách các core có sẵn và yêu cầu của chúng: [Cores](https://emulatorjs.org/docs4devs/cores).

## Chế độ bình luận

Với sức mạnh của các mô hình đa phương thức, bot AI của bạn có thể nhìn thấy lối chơi của bạn và cung cấp những bình luận hóm hỉnh trong nhân vật.

### Yêu cầu

1. Trình duyệt hỗ trợ [ImageCapture](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture#browser_compatibility). Đã kiểm tra trên Chrome desktop. Firefox yêu cầu bật nó bằng config. Safari sẽ không hoạt động.
2. Chat Completion API với chế độ inline hình ảnh được khuyến nghị. Kiểm tra tài liệu API để xem mô hình đã chọn có hỗ trợ prompt đa phương thức không.
3. Nếu inline hình ảnh bị tắt, hãy đảm bảo tiện ích mở rộng [Image Captioning](./captioning.md#multimodal-source) được bật, sau đó chọn nguồn captioning "Multimodal".

### Cách bật bình luận

1. Đảm bảo bạn đặt khoảng thời gian cung cấp bình luận trong cài đặt tiện ích mở rộng EmulatorJS. Cài đặt này xác định tần suất truy vấn nhân vật để nhận bình luận bằng hình ảnh lối chơi hiện tại của bạn. Giá trị 0 cho biết không có bình luận được cung cấp.
2. Chọn cuộc trò chuyện nhân vật và khởi chạy trò chơi. Để có hiệu suất tốt nhất, hãy đảm bảo tệp ROM được đặt tên đúng để AI có thể có thêm ngữ cảnh nền.
3. Bắt đầu chơi như bình thường. Mô hình vision sẽ được truy vấn định kỳ để viết bình luận dựa trên ảnh chụp màn hình mới nhất mà nó "nhìn thấy".

### Cài đặt

1. Caption template - một prompt được sử dụng để mô tả ảnh chụp màn hình trong trò chơi. Các macro bổ sung `{{game}}` và `{{core}}` được hỗ trợ.
2. Comment template - một prompt được sử dụng để viết bình luận dựa trên caption được tạo. Các macro `{{game}}`, `{{core}}`, `{{caption}}` được hỗ trợ. Đối với chế độ inline hình ảnh, `{{caption}}` được thay thế bằng `see included image`.
3. Force captions - sẽ buộc sử dụng captioning đa phương thức ngay cả khi inline hình ảnh được hỗ trợ và bật.

### Tại sao tôi không thấy bình luận nào?

Bình luận tạm thời bị tạm dừng (bước khoảng thời gian bị bỏ qua) nếu:

1. Giả lập bị tạm dừng (bằng nút tạm dừng, không phải trong trò chơi).
2. Cửa sổ trình duyệt không được tập trung.
3. Vùng nhập người dùng không trống. Điều này để cho phép bạn gõ câu trả lời của mình một cách yên bình.
4. Một quá trình tạo phản hồi khác hiện đang diễn ra.
5. Giọng nói TTS đang được đọc to. Bình luận được giữ lại (tối đa 30 giây) cho đến khi kết thúc, nhưng không bị bỏ qua.
6. Một thẻ nhân vật hoặc nhóm hiện đang mở. Chế độ bình luận bị tắt khi bắt đầu trò chơi từ màn hình chào mừng.

Các vấn đề phổ biến khác:

1. Đảm bảo bạn đã đặt khoảng thời gian bình luận trước khi khởi chạy trò chơi.
2. Đảm bảo bạn đã đặt khóa API đa phương thức và không có lỗi trong console máy chủ ST.

Vẫn không hoạt động? Gửi cho chúng tôi log console debug trình duyệt của bạn (nhấn F12).

## Credits

- EmulatorJS engine (GPLv3): <https://github.com/EmulatorJS/EmulatorJS>
