---
route: /extensions/captioning/
templating: false
---

# Chú thích hình ảnh

Chú thích hình ảnh cho phép SillyTavern tự động tạo các mô tả văn bản cho các hình ảnh được sử dụng trong các chats.

Sử dụng Chú thích hình ảnh khi bạn muốn nhân vật AI của bạn "nhìn" và phản hồi nội dung trực quan trong các cuộc trò chuyện của bạn.

- Tạo chú thích cho hình ảnh bạn tải lên hoặc dán vào tin nhắn
- Thêm bối cảnh vào hình ảnh hiện có trong lịch sử chat
- Sử dụng nhiều nguồn khác nhau để tạo, bao gồm các mô hình local, cloud APIs và các mạng crowdsourced

Có các tùy chọn không yêu cầu thiết lập, không có tiền và không có GPU. Cũng có các tùy chọn yêu cầu một số hoặc tất cả những điều đó. Chọn cái phù hợp với nhu cầu và tài nguyên của bạn.

Extension chú thích hình ảnh được tích hợp trong SillyTavern và không cần cài đặt riêng.

## Bắt đầu nhanh

1. Thiết lập:
    - Mở panel **Image Captioning** trong panel **<i class="fa-solid fa-cubes"></i> Extensions**
    - Chọn một nguồn chú thích (rất có khả năng là "Local" hoặc "Multimodal")
    - Đối với "Multimodal", hãy chắc rằng bạn đã thiết lập kết nối trong tab **<i class="fa-solid fa-plug"></i> API Connections**
2. Tạo một chú thích:
    - Chọn "**Generate Caption**" từ popup menu **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions**
    - Chọn một file hình ảnh khi được nhắc
    - Chờ chú thích được tạo
3. Xem xét và gửi:
    - Hình ảnh được chú thích sẽ được chèn vào tin nhắn của bạn
    - Xem chú thích bằng image tooltip
    - Nhấp **<i class="fa-solid fa-paper-plane"></i> Send** để xem những gì nhân vật của bạn nghĩ về hình ảnh!

## Kiểm soát panel

### Lựa chọn Nguồn

Chọn nguồn cho chú thích hình ảnh. Các tùy chọn được hỗ trợ:

| Nguồn                           | Mô tả                                                                                                                                                                                                  |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Multimodal](#multimodal-source) | **Cloud**: OpenAI, Anthropic, Google, MistralAI và các nhà cung cấp khác. <br>**Local**: Ollama, llama.cpp, KoboldCpp, Text Generation WebUI và vLLM. <br>Hỗ trợ các prompts tùy chỉnh để bạn có thể hỏi hình ảnh của bạn. |
| [Local](#local-source)           | Sử dụng [transformers.js](https://huggingface.co/docs/transformers.js/en/index) chạy locally bên trong máy chủ SillyTavern của bạn. Không cần thiết lập!                                                                     |
| Horde                            | Sử dụng mạng [AI Horde](https://aihorde.net/), một mạng phân tán crowdsourced của các mô hình tạo hình ảnh. Không cần tải xuống, cấu hình hoặc trả tiền. Thời gian phản hồi có thay đổi.                       |
| Extras                           | Dự án Extras đã bị ngưng dùng vào tháng 4 năm 2024 và không được duy trì hoặc hỗ trợ.                                                                                                                        |

### Cấu hình Chú thích
- **Caption Prompt**: Nhập một custom prompt cho chú thích. Default prompt là "What's in this image?"
- **Ask every time**: Toggle để yêu cầu một custom prompt cho mỗi chú thích hình ảnh

### Message Template
- **Message Template**: Tùy chỉnh mẫu tin nhắn chú thích. Sử dụng macro `{{caption}}` để chèn chú thích được tạo. Mẫu mặc định là `[{{user}} sends {{char}} a picture that contains: {{caption}}]`

### Auto-captioning
- **Automatically caption images**: Toggle để bật chú thích tự động của hình ảnh được dán hoặc gắn vào tin nhắn
- **Edit captions before saving**: Toggle để cho phép chỉnh sửa chú thích trước khi chúng được lưu

## Chú thích hình ảnh

Tất cả các cách để chú thích hình ảnh trong SillyTavern:

* Chọn "**Generate Caption**" từ popup menu **<i class="fa-solid fa-magic-wand-sparkles"></i> Extensions** và chọn một file hình ảnh khi được nhắc
* Nhấp vào <i class="fa-solid fa-envelope-open-text"></i> **Caption** icon ở trên một hình ảnh đã có trong một tin nhắn
* Dán một hình ảnh trực tiếp vào chat input với [auto-captioning](#auto-captioning) được bật
* Gắn một file hình ảnh vào một tin nhắn bằng nút <i class="fa-solid fa-paperclip"></i> **Embed File or Image** trong các tác vụ của một tin nhắn.
* Gửi một tin nhắn với một hình ảnh nhúng
* Sử dụng slash command `/caption` (#slash-command-caption)

## Auto-Captioning
Tính năng auto-captioning cho phép bạn tự động tạo chú thích cho hình ảnh khi chúng được thêm vào chat, mà không cần kích hoạt quy trình chú thích thủ công mỗi lần.

Để bật, chọn hộp kiểm "Automatically caption images" trong panel Image Captioning. Bạn cũng có thể chọn chỉnh sửa chú thích trước khi chúng được lưu bằng cách chọn hộp "Edit captions before saving".

Một khi được bật, auto-captioning sẽ kích hoạt trong các tình huống sau:

- Khi một hình ảnh được dán trực tiếp vào chat input.
- Khi một file hình ảnh được gắn vào một tin nhắn.
- Khi một tin nhắn với một hình ảnh nhúng được gửi.

Hệ thống sẽ sử dụng nguồn chú thích được chọn của bạn (Local, Extras, Horde hoặc Multimodal) và các cài đặt được cấu hình để tạo một chú thích cho hình ảnh.

### Chỉnh sửa chú thích trước khi lưu (Chế độ Refine)

Nếu bạn đã bật tùy chọn "Edit captions before saving":
1. Sau khi một hình ảnh được thêm vào, một popup sẽ xuất hiện với chú thích được tạo.
2. Bạn có thể xem xét và chỉnh sửa chú thích khi cần thiết.
3. Nhấp "OK" để áp dụng chú thích, hoặc "Cancel" để loại bỏ chú thích mà không lưu.

### Caption sending
Chú thích được tạo (và tùy chọn được chỉnh sửa) sẽ tự động được chèn vào prompt bằng cách sử dụng Message Template bạn đã cấu hình. Theo mặc định, nó sẽ được gửi ở định dạng này:

```
[BaronVonUser sends Seraphina a picture that contains: ...]
```

## Slash Command: /caption
Extension cung cấp một slash command `/caption` để sử dụng trong chatbox hoặc trong các scripts.

### Cách sử dụng

```
/caption [quiet=true|false]? [mesId=number]? [prompt]
```

- `prompt` (tùy chọn): Một custom prompt cho mô hình chú thích. Chỉ được hỗ trợ bởi các nguồn multimodal.
- `quiet=true|false`: Nếu được đặt thành true, triệt tiêu gửi một tin nhắn được chú thích vào chat. Mặc định là false.
- `mesId=number`: Chỉ định một message ID để chú thích một hình ảnh từ một tin nhắn hiện có thay vì tải lên một cái mới.

Nếu không cung cấp `mesId`, lệnh sẽ nhắc bạn tải lên một hình ảnh. Khi `quiet` là false (mặc định), một tin nhắn mới với hình ảnh được chú thích sẽ được gửi vào chat. Chú thích được tạo có thể được sử dụng làm input cho các lệnh khác.

### Ví dụ
Chú thích một hình ảnh mới với cài đặt mặc định:

```
/caption
```

Chú thích một hình ảnh mới với một custom prompt:

```
/caption Describe the main colours and shapes in this image
```

Chú thích một hình ảnh từ tin nhắn #5 mà không gửi một tin nhắn mới:

```
/caption mesId=5 quiet=true
```

Chú thích một hình ảnh từ tin nhắn #10 với một custom prompt, sau đó [tạo một hình ảnh mới](/extensions/Stable-Diffusion.md) dựa trên chú thích:

```
/caption mesId=10 Describe this image using comma-separated keywords | /imagine
```

## Nguồn Local

Bạn có thể thay đổi mô hình trong [config.yaml](/Administration/config-yaml.md#extensions-configuration). Khóa được gọi là `extensions.models.captioning`. Nhập ID mô hình Hugging Face mà bạn muốn sử dụng. Mặc định là `Xenova/vit-gpt2-image-captioning`.

Bạn có thể sử dụng bất kỳ mô hình nào hỗ trợ chú thích hình ảnh (`VisionEncoderDecoderModel` hoặc pipeline "image-to-text"). Mô hình cần tương thích với thư viện transformers.js. Đó là, nó cần trọng số ONNX. Tìm các mô hình có các thẻ `ONNX` và `image-to-text`, hoặc có một thư mục được gọi là `onnx` đầy đủ các file `.onnx`.

## Nguồn Multimodal

### Cấu hình chung

- **Model**: Chọn mô hình cho chú thích hình ảnh. Các tùy chọn thay đổi dựa trên API được chọn.
- **Allow reverse proxy**: Toggle để cho phép sử dụng một reverse proxy nếu được định nghĩa và hợp lệ (OpenAI, Anthropic, Google, Mistral, xAI)

Khóa API và URL endpoint cho các nguồn chú thích được quản lý trong panel [API Connections](/Usage/API_Connections/index.md). Thiết lập kết nối trong API Connections trước, sau đó chọn nó làm nguồn chú thích của bạn trong Captioning.

!!!warning Thiết lập nó trong panel API Connections trước
Lần cuối cùng: cấu hình khóa API/địa chỉ/cổng trong **<i class="fa-solid fa-plug"></i> API Connections** và sử dụng kết nối trong Captioning.

Bạn vẫn có thể sử dụng Claude cho chats và Google AI Studio cho chú thích hình ảnh, hoặc bất cứ thứ gì. Chỉ cần thiết lập cả hai *cả hai* trong tab 'API Connections' trước. Sau đó, lật Chat Completion nguồn của bạn sang Claude và nguồn Captioning của bạn sang Google AI Studio.
!!!

Đối với hầu hết các local backends, bạn sẽ cần đặt một số tùy chọn trong backend mô hình chứ không phải trong SillyTavern. Nếu backend của bạn chỉ có thể chạy một mô hình cùng một lúc và không hỗ trợ chuyển đổi tự động, bạn có một số tùy chọn để sử dụng các mô hình khác nhau cho chat và chú thích:

1. **Secondary endpoints:** Sử dụng tính năng secondary endpoint (xem phần [Secondary endpoints](#secondary-endpoints) bên dưới) để kết nối tới một máy chủ API khác cho chú thích
2. **Multiple connection types:** Kết nối tới backend của bạn bằng cách sử dụng cả chế độ Text Completion và Chat Completion trong API Connections - điều này cung cấp cho bạn hai kết nối riêng biệt với cùng một loại backend

### Nguồn

Để sử dụng một trong các nguồn chú thích này, chọn Multimodal trong dropdown Source.

* "Tôi muốn chú thích tốt nhất có thể, và tôi không phiền chi trả tiền": Anthropic
* "Tôi không muốn trả tiền bất cứ điều gì hoặc chạy bất cứ điều gì": Google AI Studio free tier
* "Tôi muốn chú thích hình ảnh locally và nó chỉ hoạt động": Ollama
* "Tôi muốn giữ giấc mơ của AI cục bộ sống": [KoboldCpp](#koboldcpp)
* "Tôi muốn phàn nàn khi nó không hoạt động": ~~Extras~~

## Ghi chú kỹ thuật

- Hỗ trợ mã hóa UTF-8, ký tự đặc biệt và emojis
- Xử lý các tin nhắn lớn bằng cách chia thành các chunks khi cần
- Bảo tồn định dạng và hình ảnh nhúng trong tin nhắn
- Lưu trữ các chú thích để tránh các lệnh gọi API dư thừa
