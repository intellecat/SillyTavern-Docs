---
order: tts-alltalk
route: /extensions/alltalk/
---
# AllTalk TTS V2

AllTalk là một hệ thống sao chép giọng nói dựa trên Coqui XTTS, F5-TTS, VITS, Piper và các công cụ mô hình TTS khác, được thiết kế để tạo ra chất lượng tái tạo giọng nói cao (cả nhân bản giọng nói zero shot hoặc giọng nói tích hợp sẵn). Trong AllTalk V2, các cập nhật quan trọng nâng cao chức năng và tính dễ sử dụng, bao gồm hỗ trợ nhiều công cụ TTS, mở rộng tùy chỉnh và tối ưu hóa hiệu suất. Để biết danh sách đầy đủ các tính năng, hãy tham khảo [AllTalk Wiki tại đây](https://github.com/erew123/alltalk_tts/wiki).

---

## 🟩 Tính năng chính trong AllTalk V2
- **Hỗ trợ đa công cụ**: Dễ dàng chuyển đổi giữa Coqui XTTS, VITS, Piper, Parler, F5 và các công cụ tùy chỉnh.
- **Chuyển đổi giọng nói (RVC)**: Đường ống nhân bản giọng nói dựa trên truy xuất được cải thiện.
- **Cài đặt tùy chỉnh**: Điều chỉnh cài đặt từng công cụ và lưu cấu hình khởi động.
- **Chức năng người kể**: Chỉ định giọng nói riêng biệt cho phần tường thuật và nhân vật.
- **Sử dụng độc lập và tích hợp**: Tích hợp liền mạch với SillyTavern.
- **Chế độ DeepSpeed và Low VRAM**: Tối ưu hóa hiệu suất cho môi trường hạn chế tài nguyên.
- **Ảnh chụp màn hình**: Xem giao diện AllTalk V2 [tại đây](https://github.com/erew123/alltalk_tts/discussions/237).

---

## 🟨 Các tùy chọn cài đặt và thiết lập

AllTalk cung cấp cả phương thức cài đặt độc lập và tích hợp. Cách thiết lập nhanh nhất là sử dụng một trong các tùy chọn cài đặt nhanh được cung cấp, với các script tự động hóa hầu hết quy trình.

- **Cài đặt độc lập**: Được khuyến nghị cho hầu hết người dùng ([Hướng dẫn cài đặt độc lập](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Standalone-Installation))
- **Tích hợp Text-generation-webui**: Để tích hợp vào Text-generation-webui ([Hướng dẫn cài đặt TGWUI](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Text%E2%80%90generation%E2%80%90webui-Installation))

#### 🟩 Cài đặt tự động
**Phương thức này chỉ dành cho người dùng Windows.**
Đối với người dùng mới muốn thiết lập nhanh, cài đặt tự động sử dụng SillyTavern-Launcher.
Lưu ý: Điều này giả định bạn đã cài đặt SillyTavern-Launcher. Nếu chưa, hãy truy cập https://github.com/SillyTavern/SillyTavern-Launcher và làm theo hướng dẫn trong file readme.md để cài đặt.
Sau khi cài đặt SillyTavern-Launcher:
1. Chạy Launcher.bat
2. Đi tới: `Home > Toolbox > App Installer > Voice Generation`
3. Chọn tùy chọn có nhãn: **Install AllTalk V2**

#### 🟩 Cài đặt thủ công
Đối với người dùng nâng cao yêu cầu kiểm soát chi tiết, hãy làm theo [Hướng dẫn cài đặt thủ công](https://github.com/erew123/alltalk_tts/wiki/Install-%E2%80%90-Manual-Installation-Guide) để thiết lập từng bước trên Windows, Linux hoặc Mac (chưa được kiểm tra).

#### 🟩 Cài đặt Google Colab
Chạy AllTalk trong môi trường đám mây với [Cài đặt Google Colab](https://github.com/erew123/alltalk_tts/wiki/Google-COLAB) dành cho người dùng không muốn cài đặt cục bộ.

---

## 🟨 Sử dụng AllTalk trong SillyTavern

Sau khi tải AllTalk, hãy chọn nó trong SillyTavern trên trang TTS, đảm bảo chọn đúng phiên bản máy chủ AllTalk trong cài đặt.

- **Quản lý cài đặt**: AllTalk có thể bật hoặc tắt các cài đặt cụ thể dựa trên cấu hình bạn đã chọn.
- **Trình tự tải**: Nếu SillyTavern được tải trước AllTalk, hãy tải lại trang tiện ích mở rộng TTS.
- **Tối ưu hóa hiệu suất**: Bật chế độ DeepSpeed và Low VRAM một cách có chọn lọc để cải thiện hiệu suất dựa trên tài nguyên hệ thống.
- **Chức năng người kể**: Chi tiết về chức năng người kể có thể được tìm thấy trên [AllTalk Wiki](https://github.com/erew123/alltalk_tts/wiki/Narrator-Function).

Chi tiết đầy đủ về tiện ích mở rộng AllTalk cho SillyTavern sẽ được cập nhật trên [trang AllTalk Wiki cho SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension)

Người dùng TGWUI sử dụng tiện ích mở rộng AllTalk cho TGWUI cần tắt `Enable TGWUI TTS` trong giao diện trò chuyện TGWUI, nếu không bạn sẽ có âm thanh TTS trùng lặp được tạo ra.

---

## 🟨 Khắc phục sự cố

Nếu bạn gặp sự cố mà bạn tin là liên quan đến AllTalk trong SillyTavern, vui lòng tham khảo [trang AllTalk Wiki cho SillyTavern](https://github.com/erew123/alltalk_tts/wiki/SillyTavern-Extension) để biết thông tin mới nhất.

---

### 🟪 Hỗ trợ, trợ giúp và yêu cầu tính năng

Để được hỗ trợ thêm:
- Tham khảo [Wiki](https://github.com/erew123/alltalk_tts/wiki) và tài liệu tích hợp sẵn.
- Tham gia thảo luận trên [Diễn đàn thảo luận](https://github.com/erew123/alltalk_tts/discussions/245).
- Gửi lỗi hoặc yêu cầu tính năng thông qua [Trình theo dõi vấn đề](https://github.com/erew123/alltalk_tts/issues).

---
