---
route: /vi/extensions/translation/
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

![Popup in Chinese (Simplified), '欢迎来到酒馆/Welcome to the Tavern'](../static/extensions/translation/welcome-tavern.png)

Dịch text sang tiếng Tây Ban Nha và thêm nó vào chat:

```
/translate target=es Hello world | /send
```

![User message in Spanish, 'Hola Mundo/Hello world'](/static/extensions/translation/hola-mundo.png)

### Kiểm thử, dịch theo pipeline, bản địa hóa

Hỏi người dùng nhập một tin nhắn và một ngôn ngữ, dịch tin nhắn đó sang ngôn ngữ đó, sau đó dịch lại sang ngôn ngữ mục tiêu đã cấu hình và hiển thị cả hai bản dịch trong một popup. Ví dụ này sử dụng các lệnh `/input` và `/buttons` để thu thập dữ liệu nhập từ người dùng:

```shell
/input default="Hello, world!" <span data-i18n="Test Message">Sample text</span> | 
/let key=input ||
/buttons labels=["zh-CN", "zh-TW", "es", "hu", "en"] <span data-i18n="UI Language">Language</span> | 
/let key=lang ||
/translate target={{var::lang}} {{var::input}} | /let key=tx_target | 
/translate | /let key=tx_orig ||
/echo escapeHtml=false cssClass=wider_dialogue_popup
<b data-i18n="Test Message">Test message</b>: {{var::input}} <br/>
<b data-i18n="Output">Output</b> ({{var::lang}}): {{var::tx_target}} <br/>
<b data-i18n="Output">Output</b> (<span data-i18n="ext_translate_target_lang">target language</span>): {{var::tx_orig}} <br/>
```

Điều này hữu ích để kiểm tra chất lượng bản dịch sang một ngôn ngữ mà bạn không biết, trước khi viết nó vào một nơi quan trọng.

![Popup, 'Welcome to the Tavern/欢迎来到酒馆/welcome to the pub', en, zh-CN, en](../static/extensions/translation/welcome-tavern-en-cn.png)
![Popup, 'My hovercraft is full of eels/我的氣墊船裡裝滿了鰻魚/My hovercraft is filled with eels', en, zh-TW, en](../static/extensions/translation/eels-out-zh-tw.png)

Các điều khiển giao diện được hiển thị theo locale hiện tại, độc lập với ngôn ngữ mục tiêu đã cấu hình.

| `/input`                                                                                        | `/buttons`                                                                          |
|-------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| ![Input dialog, '发送测试消息/Send Test Message'](../static/extensions/translation/eels-input-zh.png) | ![Buttons dialog, '语言/Language'](../static/extensions/translation/eels-lang-zh.png) |

![Popup, '我的氣墊船裡裝滿了鰻魚/My hovercraft is full of eels', zh-TW -> en -> zh-TW](../static/extensions/translation/eels-out-tw-en.png)

Việc phát hiện ngôn ngữ đầu vào tương đối hiệu quả trong các ví dụ sau:

![Popup, '(My hovercraft is full of eels)/A légpárnás hajóm tele van angolnával/我的氣墊船裡裝滿了鰻魚', zh-TW -> hu -> zh-TW](../static/extensions/translation/eels-out-tw-hu.png)
![Popup, '我的氣墊船裡裝滿了鰻魚/Mi aerodeslizador está lleno de anguilas/My hovercraft is full of eels', zh-TW -> es -> en](../static/extensions/translation/eels-out-tw-es-en.png)
![Popup, 'Il mio hovercraft è pieno di anguille/我的气垫船里装满了鳗鱼/My hovercraft is filled with eels', it -> zh-CN -> en](../static/extensions/translation/eels-out-it-zhCN-en.png)

## Ghi chú kỹ thuật

- Hỗ trợ mã hóa UTF-8, ký tự đặc biệt và emojis
- Xử lý các tin nhắn lớn bằng cách chia thành các chunks khi cần
- Bảo tồn định dạng và hình ảnh nhúng trong tin nhắn
- Lưu trữ các dịch để tránh các lệnh gọi API dư thừa

### Ngôn ngữ đầu vào cho AI

`internal_language` kiểm soát ngôn ngữ mà tin nhắn của người dùng được tự động dịch sang trước khi gửi đến AI. Giá trị này được đặt cứng là 'en' trong cài đặt mặc định và không thể thay đổi qua giao diện người dùng. Do đó, ngôn ngữ mục tiêu dịch cho các tin nhắn *gửi đến AI* luôn là tiếng Anh. Các thử nghiệm trước đây cho thấy hiệu suất của AI tốt hơn khi nhận tin nhắn tiếng Anh, nhưng điều này có thể thay đổi khi ngày càng nhiều LLM được huấn luyện trên dữ liệu ngôn ngữ đa dạng hơn. Bạn có thể thử thay đổi `internal_language` trong `settings.json` để xem kết quả.

### Xử lý các biến thể tiếng Trung

Extension hỗ trợ cả tiếng Trung Giản thể và Phồn thể, nhưng không phải tất cả các nhà cung cấp dịch thuật đều hỗ trợ cả hai. Giao diện hiển thị chúng dưới dạng 'Chinese (Simplified)' và 'Chinese (Traditional)', với mã ngôn ngữ 'zh-CN' và 'zh-TW'. Chúng được ánh xạ đến các mã ngôn ngữ sau cho các nhà cung cấp dịch thuật:

* Libre Translate: 'zh-CN' thành 'zh' và 'zh-TW' thành 'zt'.
* DeepL và DeepLX: cả hai biến thể thành 'ZH'.
* Bing: 'zh-CN' thành 'zh-Hans', 'zh-TW' giữ nguyên.
* Các nhà cung cấp khác sử dụng 'zh-CN' và 'zh-TW' như được cung cấp.

### Giới hạn độ dài văn bản

Một số nhà cung cấp có giới hạn ký tự cho mỗi yêu cầu:

- Yandex: 5000 ký tự
- DeepLX: 1500 ký tự
- Bing: 1000 ký tự
- Google: 5000 ký tự
