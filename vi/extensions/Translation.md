---
route: /extensions/translation/
templating: false
---

# Dịch Chat

## Tổng quan

Extension Dịch Chat cho phép dịch theo thời gian thực của các tin nhắn chat giữa các ngôn ngữ khác nhau bằng cách sử dụng các nhà cung cấp dịch khác nhau. Nó hỗ trợ cả chế độ dịch thủ công và tự động.

![Character message translated from English to Chinese using 'Translate Message/翻譯訊息' message action button](../static/extensions/translation/sensei.png)

+++ English
!["Translate Chat", "Translate Input"](../static/extensions/translation/wand-menu-en.png)
+++ 简体中文
!["翻译聊天", "翻译输入"](../static/extensions/translation/wand-menu-zh-cn.png)
+++ 繁體中文
!["翻譯聊天內容", "翻譯輸入內容"](../static/extensions/translation/wand-menu-zh-tw.png)
+++ 한국어
!["채팅 번역하기", "입력 번역하기"](../static/extensions/translation/wand-menu-ko.png)
+++ Русский
!["Перевести чат", "Перевести моё сообщение"](../static/extensions/translation/wand-menu-ru.png)
+++

## Cách sử dụng

Tất cả các cách để dịch các tin nhắn chat:

**<i class="fa-solid fa-language"></i> Dịch Chat** button trong menu **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- Dịch toàn bộ lịch sử chat cùng một lúc

**<i class="fa-solid fa-keyboard"></i> Dịch Input** button trong menu **<i class="fa-solid fa-magic-wand-sparkles"></i>
Extensions**

- Chỉ dịch text input hiện tại
- Hữu ích trước khi gửi tin nhắn

**<i class="fa-solid fa-language"></i> Dịch Tin nhắn** icon trong thanh **<i class="fa-solid fa-ellipsis"></i> Message
Actions** của bất kỳ tin nhắn nào

- Nhấp để dịch chỉ tin nhắn đó
- Nhấp lại để trở lại text gốc

**Cấu hình chế độ auto** trong ngăn kéo **Chat Translation** của panel **<i class="fa-solid fa-cubes"></i>
Extensions**

- Tự động dịch user inputs, phản hồi AI, hoặc cả hai

**/translate** slash command

- Sử dụng `/translate [target=language_code] text` để dịch text

## Cấu hình

Các tùy chọn cấu hình có sẵn trong ngăn kéo **Chat Translation** của panel **<i class="fa-solid fa-cubes"></i>
Extensions**.

#### Nhà cung cấp

- Chọn [dịch vụ dịch](#translation-providers) ưu tiên của bạn
- Nhấp vào biểu tượng **<i class="fa-solid fa-key"></i> API Key**, nếu xuất hiện, để nhập khóa API
- Nhấp vào biểu tượng **<i class="fa-solid fa-link"></i> URL Tùy chỉnh**, nếu xuất hiện, để nhập URL API tùy chỉnh

#### Ngôn ngữ Mục tiêu

Chọn ngôn ngữ bạn muốn viết tin nhắn của mình, hoặc đọc phản hồi AI.

#### Chế độ tự động

Cấu hình hành vi dịch tự động.

- **None**: Không dịch tự động
- **Dịch phản hồi**: Tự động dịch phản hồi AI sang ngôn ngữ mục tiêu
- **Dịch inputs**: Tự động dịch user inputs sang Tiếng Anh
- **Dịch cả hai**: Dịch cả user inputs và phản hồi AI

#### Xóa Dịch

Nút **<i class="fa-solid fa-trash-can"></i> Xóa Dịch** xóa tất cả các dịch từ tin nhắn trong chat hiện tại. Các tin nhắn gốc được bảo tồn.

### Ví dụ Cấu hình: Trò chuyện từ Tiếng Trung sang Tiếng Anh

Để thiết lập quy trình làm việc nơi một người dùng nói tiếng Trung có thể chat bằng tiếng Trung với một AI hoạt động bằng tiếng Anh:

1. Đặt Chế độ tự động thành "Dịch cả hai"
2. Đặt Ngôn ngữ Mục tiêu thành "Chinese (Simplified)" hoặc "Chinese (Traditional)"
3. Chọn một nhà cung cấp dịch với tự động phát hiện ngôn ngữ tốt (ví dụ: Google hoặc DeepL)

Cài đặt này sẽ:

- Dịch input Trung Quốc của người dùng sang Tiếng Anh cho AI
- Dịch phản hồi Tiếng Anh của AI lại thành Tiếng Trung cho người dùng

Cài đặt này dựa vào tự động phát hiện ngôn ngữ cho input. Để kiểm soát chính xác hơn, các cập nhật trong tương lai có thể bao gồm lựa chọn ngôn ngữ nguồn rõ ràng.

## Nhà cung cấp dịch

**:icon-cloud:** Cloud-based
**<i class="fa-solid fa-link"></i>** Local, custom URL
**<i class="fa-solid fa-key"></i>** Yêu cầu khóa API

| Nhà cung cấp                                                      | Vị trí                                                                        | Tính năng                                                                                               |
|---------------------------------------------------------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| [Libre Translate](https://libretranslate.com/)                      | :icon-cloud: <i class="fa-solid fa-key"></i> <i class="fa-solid fa-link"></i> | Tự lưu trữ (AGPL-3.0) thay thế cho các dịch vụ dịch độc quyền, với tier Pro được lưu trữ trong cloud     |
| [Google Translate](https://cloud.google.com/translate)              | :icon-cloud:                                                                  | Được sử dụng rộng rãi, hỗ trợ nhiều ngôn ngữ, độ chính xác tốt                                                    |
| [Lingva Translate](https://lingva.ml/)                              | <i class="fa-solid fa-link"></i>                                              | Front-end thay thế cho Google Translate, open source (AGPL-3.0), tập trung vào quyền riêng tư                    |
| [DeepL](https://www.deepl.com/)                                     | :icon-cloud: <i class="fa-solid fa-key"></i>                                  | Dịch chất lượng cao, đặc biệt cho các ngôn ngữ Châu Âu                                           |
| [DeepLX](https://github.com/OwO-Network/DeepLX)                     | <i class="fa-solid fa-link"></i>                                              | Proxy DeepL tự lưu trữ, open source (MIT), miễn phí nhưng proxying DeepL Pro yêu cầu khóa API DeepL         |
| [Bing Translator](https://www.bing.com/translator)                  | :icon-cloud:                                                                  | Dịch vụ dịch của Microsoft, tích hợp với các dịch vụ Azure                                        |
| [OneRing Translator](https://github.com/janvarev/OneRingTranslator) | <i class="fa-solid fa-link"></i>                                              | Front-end tự lưu trữ cho Google Translate và các nhà cung cấp khác, tập trung vào quyền riêng tư, open source (AGPL-3.0) |
| [Yandex Translate](https://translate.yandex.com/)                   | :icon-cloud:                                                                  | Tốt cho các ngôn ngữ Nga và Đông Âu                                                        |

### Cấu hình cụ thể DeepL

- Mức độ tính hình thức có sẵn cho tiếng Đức, tiếng Pháp, tiếng Ý, tiếng Tây Ban Nha, tiếng Hà Lan, tiếng Nhật và tiếng Nga
- Cấu hình qua `deepl.formality` trong [config.yaml](/Administration/config-yaml.md#deepl-configuration)

## Slash Commands

Sử dụng lệnh `/translate` để dịch nhanh. Cú pháp: `/translate [target=language_code] text`. Nếu không cung cấp ngôn ngữ mục tiêu, giá trị từ cài đặt extension sẽ được sử dụng.

### Cách sử dụng cơ bản

Dịch text thành ngôn ngữ mục tiêu hiện tại và hiển thị nó trong popup:

```
/translate Welcome to the Tavern | /echo
```

Dịch text sang tiếng Tây Ban Nha và thêm nó vào chat:

```
/translate target=es Hello world | /send
```

## Ghi chú kỹ thuật

- Hỗ trợ mã hóa UTF-8, ký tự đặc biệt và emojis
- Xử lý các tin nhắn lớn bằng cách chia thành các chunks khi cần
- Bảo tồn định dạng và hình ảnh nhúng trong tin nhắn
- Lưu trữ các dịch để tránh các lệnh gọi API dư thừa
