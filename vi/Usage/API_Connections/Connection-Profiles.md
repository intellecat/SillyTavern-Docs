---
icon: paperclip
route: /vi/usage/core-concepts/connection-profiles/
order: 100
---

# Hồ Sơ Kết Nối

Lưu Hồ Sơ Kết Nối để nhanh chóng chuyển đổi giữa các API, mô hình và template định dạng khác nhau. Điều này hữu ích khi bạn thường xuyên sử dụng nhiều kết nối API hoặc cần chuyển đổi giữa các cấu hình khác nhau mà không cần phải tìm kiếm trong các menu.

## Truy Cập Hồ Sơ Kết Nối

Tính năng này được bật mặc định từ SillyTavern 1.12.6 trở lên dưới dạng extension tích hợp, và có sẵn trong menu API Connections. Nếu bạn muốn *tắt* tính năng này, hãy mở panel Extensions, nhấp vào "Manage extensions", tìm Connection Profiles trong danh sách, bỏ chọn hộp kiểm "Enabled", sau đó nhấp "Close".

## Những Gì Được Lưu

Hồ Sơ Kết Nối lưu các lựa chọn sau.

### Chung

* [Loại API, mô hình và URL máy chủ](/Usage/API_Connections/index.md)
* [Secret Key](/Usage/faq.md#where-are-my-api-keys-stored-why-cant-i-see-them)
* [Settings preset](/Usage/Common-Settings.md)
* [Start Reply With](/Usage/Prompts/advancedformatting.md#start-reply-with) (có thể để trống một cách rõ ràng)
* [Custom Stopping Strings](/Usage/Prompts/advancedformatting.md#custom-stopping-strings) (có thể để trống một cách rõ ràng)
* [Reasoning Formatting](/Usage/Prompts/reasoning.md#configuration)

### Text Completion APIs

* [System Prompt và trạng thái của nó](/Usage/Prompts/advancedformatting.md#system-prompt)
* [Trạng thái và template của Instruct Mode](/Usage/Prompts/instructmode.md)
* [Context Template](/Usage/Prompts/advancedformatting.md#context-template)
* [Tokenizer](/Usage/Prompts/advancedformatting.md#tokenizer)

### Chat Completion APIs

* [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing)
* Proxy preset

## Quản Lý Hồ Sơ Kết Nối

!!!info Lưu ý
Hồ sơ chỉ lưu lựa chọn trong các trường dropdown, không biết gì về cài đặt bên dưới. Điều này có nghĩa là bạn sẽ mất các thay đổi chưa lưu khi chuyển sang hồ sơ khác. Để tránh điều này, hãy đảm bảo cập nhật tất cả presets và templates nếu bạn không muốn mất các thay đổi tạm thời.
!!!

* Để lưu hồ sơ, thiết lập tất cả các cài đặt cần thiết và nhấp nút "Create". Sau đó xem lại cài đặt và đặt tên cho hồ sơ. **Tên phải là duy nhất.**
* Để xem thông tin chi tiết về hồ sơ đã chọn, nhấp vào nút "Information". Nhấp lại để ẩn chi tiết.
* Cài đặt Hồ Sơ Kết Nối được lưu vào `settings.json` mà không thay đổi file lưu hồ sơ liên quan cho đến khi bạn nhấn nút "Update". Điều này có nghĩa là nếu bạn thiết lập hồ sơ, nhưng sau đó chuyển sang hồ sơ khác mà không cập nhật, bạn sẽ mất tất cả các thay đổi trước đó.
* Để khôi phục các lựa chọn đã thay đổi từ hồ sơ đã lưu, nhấp nút "Reload".
* Để xóa hồ sơ, nhấp nút "Delete" và xác nhận xóa. **Hành động này không thể hoàn tác.**

## Lệnh Slash

Hồ sơ kết nối có thể được quản lý bằng các lệnh slash sau.

1. `/profile [name]` - chuyển sang hồ sơ nếu có tham số, hoặc lấy tên hồ sơ hiện tại nếu không.
2. `/profile-create [name]` - lưu cài đặt hiện tại dưới dạng hồ sơ mới với tên được cung cấp.
3. `/profile-list` - trả về mảng JSON của các tên hồ sơ có sẵn.
4. `/profile-get [name]` - lấy chi tiết của hồ sơ với tên được cung cấp dưới dạng đối tượng JSON.
5. `/profile-update` - cập nhật hồ sơ đã chọn với cài đặt hiện tại.
