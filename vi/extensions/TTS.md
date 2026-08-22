---
order: tts
route: /vi/extensions/tts/
---

# TTS

SillyTavern có một loạt các tùy chọn TTS (text-to-speech) được sử dụng để có một giọng nói kể lại các phần của chat của bạn. Trang này giải thích cách thiết lập và sử dụng.

## Cấu hình TTS

### Lựa chọn Nhà cung cấp TTS

Được sử dụng để chọn dịch vụ TTS nào bạn muốn sử dụng. Một số tùy chọn là miễn phí, một số yêu cầu subscription trả phí, và một số chạy locally trên PC của bạn.

Các tùy chọn có sẵn (danh sách có thể thay đổi theo thời gian):

- **AllTalk** - miễn phí, open source local installation, cung cấp nhiều TTS engines. Xem trang [AllTalk](./AllTalk.md) để biết hướng dẫn thiết lập.
- **Azure TTS** - cùng giọng như Microsoft Edge. Yêu cầu một tài khoản Azure và một subscription trả phí.
- **Coqui-TTS** (không được dùng nữa) - miễn phí, yêu cầu Extras API để chạy. Các mô hình Text2Speech hiệu suất cao (Tacotron, Tacotron2, Glow-TTS, SpeedySpeech) cũng như Bark.
- **Edge** - miễn phí, chạy qua Azure. Khi chạy với "Plugin" được chọn làm nhà cung cấp, bạn cũng cần cài đặt [plugin server này](https://github.com/SillyTavern/SillyTavern-EdgeTTS-Plugin). Tùy chọn khác yêu cầu Extras API (không được dùng nữa) để chạy.
- **Electron Hub** - tái sử dụng khóa [Electron Hub](https://electronhub.ai/) API của bạn để truy cập các giọng nói trong cloud (GPT-4o Mini TTS, các giọng nói neural Microsoft, v.v.) với các điều khiển trên mỗi mô hình.
- **ElevenLabs** - yêu cầu subscription trả phí. Lấy khóa API từ [ElevenLabs](https://elevenlabs.io/).
- **Google Translate** - một giọng nói miễn phí được cung cấp bởi Google, một cho mỗi ngôn ngữ, chất lượng có thể thay đổi rộng rãi.
- **Google Gemini TTS** - yêu cầu khóa API từ [Vertex AI](/Usage/API_Connections/google.md#google-vertex-ai) hoặc [AI Studio](/Usage/API_Connections/google.md#google-ai-studio), sử dụng các mô hình [Gemini TTS](https://cloud.google.com/text-to-speech/docs/gemini-tts).
- **Kokoro** - miễn phí, sử dụng [kokoro.js](https://www.npmjs.com/package/kokoro-js) để chạy mô hình locally trong trình duyệt của bạn. Tuy nhiên, [một số trình duyệt](https://caniuse.com/webgpu) có thể không hỗ trợ WebGPU cho tùy chọn thiết bị.
- **MiniMax** - yêu cầu khóa API từ [MiniMax](https://www.minimax.io/). Xem trang [MiniMax TTS](./MiniMaxTTS.md) để biết hướng dẫn thiết lập.
- **Novel** - yêu cầu subscription NovelAI trả phí, được tạo bởi TTS engine của NovelAI
- **OpenAI** - yêu cầu khóa API trả phí, sử dụng các mô hình TTS của OpenAI.
- **Pollinations** - truy cập miễn phí vào các mô hình OpenAI TTS, nhưng có một rate limit. [Website](https://pollinations.ai/).
- **Silero** - miễn phí, chạy trên PC của bạn, chất lượng có thể thay đổi rộng rãi. Yêu cầu một [dedicated API server](https://github.com/ouoertheo/silero-api-server) installation hoặc Extras API (không được dùng nữa).
- **System** - sử dụng OS TTS engine của bạn, nếu có một. Chất lượng có thể thay đổi rộng rãi tùy thuộc vào OS.
- **XTTS** - miễn phí, yêu cầu một dedicated API server installation. Xem trang [XTTS](./XTTS.md) để biết hướng dẫn thiết lập.

### Các hộp kiểm

- **Enabled** - bật/tắt phát TTS
- **Auto Generation** - cho phép TTS bắt đầu phát tự động khi một tin nhắn mới vào chat
- **Only narrate "quotes"** - Giới hạn phát TTS chỉ bao gồm văn bản trong `"quotation marks"`. Điều này sẽ `*include "quotes" within asterisk lines*` (tên biến nội bộ = `narrate_quoted_only`)
- **Ignore \*text, even "quotes", inside asterisks\*** - TTS sẽ không phát bất kỳ văn bản nào trong `*asterisks*`, thậm chí "quotes" (tên biến nội bộ = `narrate_dialogues_only`)
- *nếu cả hai hộp kiểm "only narrate quotes" và "ignore asterisks" được chọn sẽ dẫn đến TTS chỉ đọc "quotes" không ở trong asterisks, và bỏ qua tất cả mọi thứ khác.*
- **Narrate only the translated text** - điều này sẽ làm cho TTS chỉ kể lại văn bản được dịch.
- **Apply regex** - áp dụng một mẫu regex được cung cấp cho văn bản trước khi gửi tới nhà cung cấp TTS. Hữu ích để loại bỏ các phần không mong muốn khỏi văn bản đầu vào, chẳng hạn như emoji hoặc các ký tự ngôn ngữ không phải bản địa mà TTS engine không xử lý tốt.

Với ví dụ văn bản: `*Cohee approaches you with a faint "nya"* "Good evening, senpai", she says.`
Đây là một bảng cho thấy cách văn bản sẽ được sửa đổi dựa trên các trạng thái boolean của **Ignore \*text, even "quotes", inside asterisks\*** và **Only narrate "quotes"**:

| **Ignore \*text, even "quotes", inside asterisks\*** 	 | **Only narrate "quotes"**	 | **Output**                                                                |
|:-------------------------------------------------------|:---------------------------|:--------------------------------------------------------------------------|
| Disabled                                               | 	Disabled	                 | Cohee approaches you with a faint "nya" "Good evening, senpai", she says. |
| Disabled                                               | Enabled	                   | "nya"... "Good evening, senpai"                                           |
| Enabled	                                               | Disabled	                  | "Good evening, senpai", she says.                                         |
| Enabled	                                               | Enabled	                   | "Good evening, senpai"                                                    |

### Các thanh trượt

Chúng sẽ thay đổi tùy thuộc vào API bạn chọn.

### Nút bấm

- **Apply** - phải được nhấp sau khi đặt một TTS API và sau khi chỉnh sửa voice map.
- **Refresh** - tải lại danh sách các giọng nói từ TTS API được chọn.
- **Available voices** - tải một popup với tất cả các giọng nói có sẵn cho API được chọn của bạn, và cho phép bạn xem trước chúng với các dialogue mẫu.

## Sử dụng TTS

1. Nhấp vào hộp kiểm "Enable", hoặc không có gì sẽ bao giờ xảy ra.
2. Nhấp vào hộp kiểm "Auto-generation" nếu bạn muốn TTS bắt đầu tự động mỗi khi một tin nhắn mới đến chat.
3. Tùy chọn, nhấp vào biểu tượng megaphone bên trong phía trên-phải của bất kỳ tin nhắn nào để phát lại theo yêu cầu.
4. Nhấp vào nút "Stop" phía dưới-phải (được tìm thấy bên trong wand menu) để dừng bất kỳ phát lại nào.

### Voice Map

Bạn phải cung cấp một voice map cho TTS sử dụng, nếu không, nó sẽ không biết giọng nào nên được sử dụng cho mỗi nhân vật. Để thiết lập voice map, trước tiên hãy mở một chat với một nhân vật bạn muốn gán một giọng nói và/hoặc chọn một user persona để gán một giọng nói, sau đó chọn một giọng nói được liệt kê bởi một nhà cung cấp TTS từ dropdown. Nếu bạn không thấy một danh sách các giọng nói và/hoặc nhân vật, hãy chắc rằng nhà cung cấp TTS của bạn được cấu hình đúng cách và nhấp "Refresh". Một số nhà cung cấp (như OpenAI-compatible hoặc NovelAI) yêu cầu bạn điền danh sách giọng nói theo cách thủ công.
