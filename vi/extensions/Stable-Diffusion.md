---
route: /vi/extensions/stable-diffusion/
templating: false
---

# Tạo Hình ảnh

Sử dụng local hoặc cloud-based Stable Diffusion, FLUX hoặc DALL-E APIs để tạo hình ảnh.

Tự động tạo hình ảnh như các trả lời cho tin nhắn của bạn cho sự nhập vai toàn phần,
tạo từ lịch sử chat và thông tin nhân vật từ menu wand hoặc slash commands,
hoặc sử dụng lệnh `/sd (anything_here)` trong thanh nhập liệu chat để tạo một hình ảnh với prompt của chính bạn.

Hầu hết các cài đặt tạo Stable Diffusion phổ biến có thể tùy chỉnh trong UI SillyTavern.

- Hỗ trợ [nhiều nguồn tạo hình ảnh](#supported-sources), cả local và cloud-based
- Các [chế độ tạo hình ảnh](#generation-modes) khác nhau cho nhân vật, cảnh và các prompts tùy chỉnh
- [Slash commands](#how-to-generate-an-image) để tạo hình ảnh dễ dàng trong các chats
- [Chế độ tương tác](#use-interactive-mode) để kích hoạt tạo hình ảnh dựa trên các yêu cầu ngôn ngữ tự nhiên
- Các template prompt và [tiền tố](#common-prompt-prefix) có thể tùy chỉnh cho kiểu và chất lượng nhất quán
- [Tiền tố prompt cụ thể cho nhân vật](#character-specific-prompt-prefix) cho hình ảnh nhân vật được điều chỉnh
- [Cài đặt kiểu](#styles) để nhanh chóng chuyển đổi giữa các cài đặt tạo hình ảnh khác nhau
- [Các tùy chọn hiển thị](#chat-message-visibility) linh hoạt cho hình ảnh được tạo trong chat
- Tích hợp [ComfyUI](#comfyui-configuration) nâng cao cho các quy trình làm việc có độ tùy chỉnh cao
- Khả năng [xem tất cả hình ảnh được tạo](#view-all-generated-images) trong một thư viện ảnh nhân vật
- Tính năng [image swipes](#image-swipes) để tạo lại hình ảnh trong khi giữ nguyên prompt
- Tùy chọn [chỉnh sửa prompts trước khi tạo hình ảnh](#edit-prompts-before-generation) và [mở rộng prompts ở chế độ miễn phí](#extend-free-mode-prompts)
- Tích hợp với [function calling](#use-function-tool) AI để tự động phát hiện tạo hình ảnh

## Các nguồn được hỗ trợ

| Nguồn                                                                                             | Ghi chú                                                                                         |
|:--------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------|
| [AI/ML API](https://aimlapi.com/)                                                                 | Cloud, trả phí                                                                                     |
| [Black Forest Labs](https://bfl.ai/)                                                              | Cloud, trả phí                                                                                     |
| [Cloudflare Workers AI](https://www.cloudflare.com/developer-platform/products/workers-ai/)       | Cloud, trả phí, nhiều mô hình khác nhau có khả năng vision                                            |
| [Chutes](https://chutes.ai/)                                                                      | Cloud                                                                                           |
| [ComfyUI](https://github.com/comfyanonymous/ComfyUI)                                              | Local, open source (GPL3), miễn phí, xem [ComfyUI Configuration](#comfyui-configuration). |
| [Draw Things](https://drawthings.ai/)                                                             | Local, Mac/iOS, miễn phí                                                                                  |
| [Electron Hub](https://electronhub.ai/)                                                           | Cloud, trả phí                                                                                     |
| [FAL.AI](https://fal.ai/)                                                                         | Cloud, trả phí                                                                                     |
| [Google AI Studio](https://aistudio.google.com/) / [Google Vertex AI](https://cloud.google.com/vertex-ai) | Cloud, trả phí. Mô hình loạt Imagen. AI Studio hỗ trợ ít mô hình hơn.                       |
| [HuggingFace Serverless](https://huggingface.co/docs/api-inference/index)                         | Cloud, miễn phí                                                                           |
| [NanoGPT](https://nano-gpt.com/)                                                                  | Cloud, trả phí                                                                                     |
| [NovelAI Diffusion](https://novelai.net/)                                                         | Cloud, cần subscription hoạt động                                                          |
| [OpenAI](https://platform.openai.com/)                                                            | Cloud, trả phí                                                                                     |
| [OpenRouter](https://openrouter.ai/)                                                              | Cloud                                                                                           |
| [Pollinations](https://pollinations.ai/)                                                          | Cloud, open source (MIT), trả phí                                                        |
| [SD.Next / vladmandic](https://github.com/vladmandic/automatic)                                   | Local, open source (AGPL3), miễn phí                                                      |
| [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras)                           | Không được dùng nữa, không được khuyến nghị                                                                     |
| [stable-diffusion.cpp](https://github.com/leejet/stable-diffusion.cpp)                            | Local, open source (MIT), miễn phí                                                        |
| [Stability AI](https://platform.stability.ai/)                                                    | Cloud, trả phí                                                                                     |
| [Stable Diffusion WebUI / AUTOMATIC1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) | Local, open source (AGPL3), miễn phí                                                      |
| [Stable Horde](https://stablehorde.net/)                                                          | Cloud, open source (AGPL3), miễn phí                                                        |
| [TogetherAI](https://docs.together.ai/docs/serverless-models#image-models)                        | Cloud                                                                                           |
| [x.AI](https://x.ai/)                                                                             | Cloud, trả phí                                                                                     |
| [Z.AI](https://z.ai/)                                                                             | Cloud, trả phí                                                                                     |

## Chế độ tạo hình ảnh

| Wand menu item     | Slash command argument | Mô tả                                    | Ghi chú                               |
|:-------------------|:-----------------------|:-----------------------------------------------|:--------------------------------------|
| "Yourself"         | `you`                  | Chân dung toàn thân của nhân vật hiện tại. | -                                     |
| "Your Face"        | `face`                 | Chân dung cạnh nhân vật hiện tại.  | Buộc tỷ lệ khung hình chân dung.       |
| "Me"               | `me`                   | Chân dung của user persona.                | -                                     |
| "The Whole Story"  | `scene`                | Tóm tắt trực quan của các sự kiện chat.             | -                                     |
| "The Last Message" | `last`                 | Tóm tắt trực quan của tin nhắn chat cuối cùng.       | -                                     |
| "Raw Last Message" | `raw_last`             | Tin nhắn cuối cùng được sử dụng làm prompt nguyên bản.        | -                                     |
| "Background"       | `background`           | Nền chat dựa trên bối cảnh câu chuyện.      | Buộc tỷ lệ khung hình rộng ngang. |

## Cách tạo một hình ảnh

1. Sử dụng mục "Image Generation" trong context menu của extensions (wand).
2. Nhập slash command `/sd (argument)` với một argument từ bảng Generation modes. Bất cứ điều gì khác sẽ kích hoạt "free mode" để tạo SD bất cứ điều gì bạn đã yêu cầu. Ví dụ: `/sd apple tree` sẽ tạo một bức tranh của một cây táo.
3. Tìm một biểu tượng bàn chải trong các tác vụ bối cảnh cho các tin nhắn chat. Điều này sẽ buộc chế độ "Raw Message" cho tin nhắn được chọn.

Mỗi chế độ tạo hình ảnh ngoại trừ raw message và free mode sẽ kích hoạt tạo prompt bằng cách sử dụng API tạo chính được chọn hiện tại của bạn để chuyển đổi bối cảnh chat thành prompt SD.
Bạn có thể cấu hình mẫu hướng dẫn tạo prompts cho mỗi chế độ tạo hình ảnh bằng cách sử dụng ngăn kéo cài đặt "SD Prompt Templates" trong bảng điều khiển extensions.

### Mẹo và thủ thuật để sử dụng lệnh `/sd`

#### Xem tất cả hình ảnh được tạo

Để xem tất cả hình ảnh đã lưu cho một nhân vật (bao gồm các chats khác), mở một thư viện từ menu dropdown "More..." trên một bảng điều khiển thông tin nhân vật, hoặc sử dụng lệnh slash `/show-gallery`.

#### Chỉ định một prompt âm

Sử dụng một named argument `negative` trước prompt để thực thi một prompt âm cụ thể cho lần tạo này.

```stscript
/sd negative="fries" cute tater farmer holding a tayto in a spud-field
```

#### Bao gồm một tiền tố cụ thể cho nhân vật

Sử dụng một macro đặc biệt `{{charPrefix}}` trong chế độ free-prompt để bao gồm các tiền tố prompt dương và âm (nếu được định nghĩa) cho nhân vật hiện tại.

```stscript
/sd {{charPrefix}}, riding a bike
```

#### Triệt tiêu một tin nhắn chat

Bạn có thể tránh đăng một hình ảnh được tạo vào chat bằng cách chuyển một named argument `quiet=true`. Hình ảnh vẫn sẽ được thêm vào thư viện ảnh nhân vật, và lệnh sẽ tạo ra một URL tương đối cho hình ảnh có thể được sử dụng bởi các lệnh khác.

Ví dụ dưới đây sẽ gửi hình ảnh được tạo ra bằng cách sử dụng Markdown như một user persona.

```stscript
/sd quiet=true me | /send Here's a picture of me: ![my portrait]({{pipe}})
```

### Image swipes

Image swipes cho phép bạn cuộn lại tạo hình ảnh trong khi giữ nguyên prompt. Nếu một seed cố định được đặt, nó sẽ được ngẫu nhiên hóa cho lần tạo tiếp theo. Kích thước hình ảnh đã được ghi đè thông qua slash command `/sd` sẽ được giữ nguyên cho các hình ảnh được swipe.

Để chu kỳ qua các hình ảnh, di chuyển con trỏ chuột (nhấn trên di động) trên một hình ảnh được tạo để tiết lộ các nút mũi tên và bộ đếm swipes. Nhấp mũi tên bên phải trên hình ảnh mới nhất sẽ tạo ra một hình ảnh mới.

*'Swipes' ở đây chỉ là một tên, đừng thử cử chỉ swipe thực tế, vì điều này sẽ tạo lại tin nhắn chính nó, không phải hình ảnh đính kèm.*

## Tùy chọn

### Chỉnh sửa prompts trước khi tạo hình ảnh

Tùy chọn này cho phép bạn chỉnh sửa các prompts được tạo tự động trước khi chúng được gửi tới API Tạo Hình ảnh. Bạn cũng có thể chỉnh sửa hoặc loại bỏ prompt âm đã lưu và ghi đè độ phân giải khi tạo lại một hình ảnh ban đầu được tạo bằng lệnh `/sd`.

### Sử dụng function tool

Sử dụng [function calling](/extensions/Stable-Diffusion.md) để tự động phát hiện ý định tạo hình ảnh.

**Yêu cầu:**

1. Phải có tạo hình ảnh được cấu hình với một nguồn được hỗ trợ.
2. Phải sử dụng một mô hình Chat Completion API được hỗ trợ và có function tool calling được bật trong cài đặt AI Response.
3. Tùy chọn "Use function tool" phải được bật trong cài đặt Image Generation.
4. Người dùng nên bày tỏ ý định tạo hình ảnh trong tin nhắn chat, ví dụ: "Send me a picture of a cat".

!!!warning
Chế độ tương tác sẽ không kích hoạt khi function tool được bật.
!!!

### Sử dụng chế độ tương tác

Cho phép kích hoạt tạo hình ảnh thay vì văn bản là một trả lời cho một tin nhắn người dùng tuân theo mẫu đặc biệt:

1. Chứa một trong những động từ sau: send, mail, imagine, generate, make, create, draw, paint, render
2. Theo sau bởi một trong những danh từ sau (không quá 10 ký tự xa): pic, picture, image, drawing, painting, photo, photograph
3. Theo sau bởi một chủ đề mục tiêu của tạo hình ảnh, có thể tùy chọn được đặt trước bởi các cụm từ như "of a" hoặc "of this".

Ví dụ về các yêu cầu hợp lệ và các chủ đề được nắm bắt:

* `Can you please send me a picture of a cat` => `cat`
* `Generate a picture of the Eiffel tower` => `Eiffel tower`
* `Let's draw a painting of Mona Lisa` => `Mona Lisa`

Một số chủ đề đặc biệt kích hoạt một chế độ tạo hình ảnh được định nghĩa trước:

* 'you, 'yourself' => "Yourself"
* 'your face', 'your portrait', 'your selfie' => "Your Face"
* 'me', 'myself' => "Me"
* 'story', 'scenario', 'whole story' => "The Whole Story"
* 'last message' => "The Last Message"
* 'background', 'scene background', 'scene', 'scenery', 'surroundings', 'environment' => "Background"

### Mở rộng các prompts ở chế độ miễn phí

Khi sử dụng chế độ tương tác của slash command, tự động mở rộng các mô tả chủ đề tạo hình ảnh ở chế độ miễn phí bằng cách nhắc API chính của bạn.

### Minimal prompt processing

Khi được bật, giảm bớt quá trình xử lý được áp dụng cho các prompts do LLM trả về để tạo hình ảnh. Chỉ thực hiện chuẩn hóa và giảm khoảng trắng, bỏ qua việc làm sạch (sanitization) mạnh tay được thực hiện theo mặc định. Điều này hữu ích khi làm việc với các quy trình làm việc nâng cao (ví dụ: ComfyUI) chấp nhận các định dạng prompt có cấu trúc như JSON.

### Snap auto-adjusted resolutions

Snap các yêu cầu tạo hình ảnh có tỷ lệ khung hình buộc (chân dung, nền) đến độ phân giải nào gần đây nhất, trong khi cố gắng giữ nguyên số pixel tuyệt đối. Tham khảo dropdown "Resolution" để biết danh sách các tùy chọn có thể có.

**Được khuyến nghị cho các mô hình SDXL**.

## Tiền tố prompt chung

!!!tip Pro Tip
Sử dụng macro `{prompt}` để chỉ định chính xác nơi prompt được tạo sẽ được chèn.
!!!

Thêm vào trước mỗi prompt được tạo hoặc ở chế độ miễn phí. Thường được sử dụng để đặt kiểu tổng thể của bức tranh.

Ví dụ: `best quality, anime lineart`.

## Prompt âm

Các đặc điểm của hình ảnh bạn không muốn có mặt trong đầu ra.

Ví dụ: `bad quality, watermark`.

## Tiền tố prompt cụ thể cho nhân vật

!!!tip Pro Tip
Nếu được hỗ trợ bởi nguồn tạo hình ảnh, bạn cũng có thể sử dụng LoRAs/embeddings ở đây, ví dụ: `<lora:DonaldDuck:1>`.
!!!

Bất kỳ đặc điểm nào mô tả nhân vật được chọn hiện tại. Sẽ được thêm sau một tiền tố chung.

Ví dụ: `female, green eyes, brown hair, pink shirt`.

Bạn cũng có thể chỉ định một tiền tố prompt âm cho bất kỳ nội dung không mong muốn nào. Nó sẽ được kết hợp với prompt âm chung.

Hạn chế:
1. Hoạt động chỉ trong các chats 1-to-1. Sẽ không được sử dụng trong các nhóm.
2. Sẽ không được sử dụng cho hình nền và tạo hình ảnh ở chế độ miễn phí.

!!! Note
Để buộc bao gồm một tiền tố nhân vật vào một prompt ở chế độ miễn phí, sử dụng macro `{{charPrefix}}` ở bất kỳ nơi nào trong prompt.
!!!

Nếu bạn muốn chia sẻ các tiền tố với những người khác, tích hộp "Shareable". Điều này sẽ lưu chúng với dữ liệu nhân vật, thay vì cài đặt cục bộ của bạn.

## Kiểu

Sử dụng cái này để nhanh chóng lưu và khôi phục các cài đặt kiểu/chất lượng yêu thích của bạn để sử dụng chúng sau hoặc khi chuyển đổi giữa các mô hình. Sau đây được bao gồm trong cài đặt kiểu:

1. Tiền tố prompt chung
2. Prompt âm

Bạn cũng có thể chuyển đổi giữa các kiểu bằng lệnh `/imagine-style` (hoặc `/sd-style` hoặc `/img-style`).

## Hiển thị Tin nhắn Chat

Hình ảnh được tạo được chèn vào chat được ẩn trong các API prompts chính theo mặc định, nhưng điều này có thể được ghi đè cho từng người khởi tạo tạo ("Magic wand" icon, slash command, interactive mode). Điều này có thể được sử dụng để làm trải nghiệm phong phú hơn bằng cách cho phép các nhân vật "thừa nhận" hình ảnh. Các mô hình đa phương tiện trong Chat Completions API cũng có thể 'thấy' hình ảnh nếu "Send inline images" được bật.

Một tin nhắn văn bản có thể được tùy chỉnh bằng cách thay đổi "Chat Message Template" dưới Image Prompt Templates. Tất cả các macro thông thường có thể được sử dụng trong mẫu này, cộng với một macro đặc biệt `{{prompt}}` để chỉ định nơi prompt hình ảnh sẽ được thêm.

## ComfyUI Configuration

[ComfyUI](https://github.com/comfyanonymous/ComfyUI) là một tùy chọn nhanh và rất linh hoạt để tạo hình ảnh.

Nếu bạn quen thuộc với ComfyUI, tl;dr là: tạo quy trình làm việc của bạn trong ComfyUI, tải xuống nó **ở định dạng API**, và dán nó vào ComfyUI Workflow Editor của SillyTavern. ST sẽ gửi quy trình làm việc của bạn tới API ComfyUI và bạn sẽ nhận được một hình ảnh trong chat của bạn. Nhưng với sức mạnh lớn đi kèm trách nhiệm lớn, và trách nhiệm chính là chèn các trình giữ chỗ trong JSON quy trình làm việc của bạn để bạn có thể thay đổi các cài đặt từ SillyTavern.

Nếu bạn không quen thuộc với ComfyUI, bạn vẫn có thể sử dụng nó để tạo hình ảnh trong SillyTavern bằng quy trình làm việc mặc định. Sau đó, khi bạn muốn sức mạnh lớn, bạn có thể học cách sử dụng ComfyUI...

### Điều khiển

Bảng điều khiển này cho phép bạn cấu hình và quản lý tích hợp ComfyUI của bạn với SillyTavern.

#### Server Type

* Standard Server là khi bạn gọi ComfyUI trực tiếp, dù trên máy cục bộ hay được host ở nơi khác.
* RunPod Serverless Endpoint dùng để chạy ComfyUI thông qua [RunPod's serverless API](https://www.runpod.io/product/serverless). Serverless có thể là một lựa chọn tốt cho việc tạo hình ảnh từ xa vì bạn vẫn có cùng mức kiểm soát đối với các quy trình làm việc như một máy chủ tiêu chuẩn, nhưng có thể tận dụng các GPU được host mạnh mẽ hơn và chỉ bị tính phí khi bạn đang thực sự tạo hình ảnh. Phần lớn cách sử dụng là giống nhau. Sự khác biệt so với thiết lập và hành vi của máy chủ tiêu chuẩn được mô tả [bên dưới](#comfyui-runpod-setup).

#### Standard Server setup

Nhập URL của máy chủ ComfyUI của bạn vào trường nhập **ComfyUI URL**. Giá trị mặc định là `http://127.0.0.1:8188`.
Nếu bạn đang sử dụng [SwarmUI](https://github.com/mcmonkeyprojects/SwarmUI), cổng mặc định cho
[máy chủ ComfyUI được quản lý](https://github.com/mcmonkeyprojects/SwarmUI/blob/master/src/BuiltinExtensions/ComfyUIBackend/README.md) là `7821`,
20 cổng cao hơn cổng mặc định cho SwarmUI.

Sau khi nhập URL, chọn <i class="fa-solid fa-check"></i> **Connect** để xác thực và thiết lập kết nối. Máy chủ ComfyUI phải có thể truy cập được từ máy chủ SillyTavern.

#### ComfyUI RunPod Setup

* Bạn sẽ cần một tài khoản RunPod và nạp thêm tiền vào đó. Bạn có thể mong đợi khoảng 2 cent cho mỗi hình ảnh khi tạo hình ảnh Qwen trên RTX 4090, tuy kết quả thực tế có thể khác. $5 tín dụng nên dùng được một thời gian.
* <https://console.runpod.io/hub/runpod-workers/worker-comfyui> là một cấu hình flux1 dev mà bạn có thể sử dụng để tạo endpoint serverless của riêng mình.
  * Có thông tin ở đó về việc tạo cấu hình riêng của bạn nếu bạn muốn sử dụng một mô hình khác hoặc thêm LoRAs.
* Tạo một API key để truy cập endpoint serverless: <https://console.runpod.io/user/settings>

* Trong ST, chọn **ComfyUI** làm **Source** và **RunPod Serverless Endpoint** làm **Server Type**.
* Đặt **ComfyUI RunPod URL** thành URL của endpoint của bạn.
* Đặt API key.
* Nhấp **Connect**. Nếu API key và URL đúng, bạn sẽ nhận được các toast thông báo thành công.
* Luồng cấu hình quy trình làm việc ComfyUI giống như khi dùng cục bộ.
  * Sử dụng tùy chọn "Export (API)".
  * Tùy thuộc vào thiết lập cục bộ của bạn, bạn có thể cần/muốn chọn một biến thể của mô hình để sử dụng trên RunPod. Ví dụ, nếu bạn sử dụng một GGUF đã lượng hóa cục bộ, nhưng muốn sử dụng phiên bản fp16 trên RunPod. Quy trình làm việc JSON bạn sử dụng trong ST cần có thay đổi này.
  * Model, samplers, VAE, v.v. không thể được xác định động nên quy trình làm việc của bạn cần có các giá trị này được viết cứng (không dùng thay thế `%model%`).
  * Các thay thế khác sẽ hoạt động giống như khi dùng cục bộ.

!!!info Note
Cấu hình serverless hiện chưa nhúng quy trình làm việc vào hình ảnh đầu ra. Tức là, bạn sẽ không thể kéo/thả hình ảnh vào ComfyUI cục bộ để xem seed hoặc prompt. Đây chỉ là một hạn chế của trình xử lý RunPod và là một khả năng có thể được thêm vào ở phía đó.
!!!

### Quản lý Quy trình Làm việc

Chọn một quy trình làm việc ComfyUI từ menu dropdown. Hai quy trình làm việc mặc định được cung cấp:

- Default_Comfy_Workflow.json: Một quy trình làm việc text-to-image cơ bản hỗ trợ hầu hết các cài đặt tạo hình ảnh phổ biến.
- Char_Avatar_Comfy_Workflow.json: Một quy trình làm việc image-to-image mẫu sử dụng avatar nhân vật, cộng với prompt, để tạo một hình ảnh.

Sử dụng các nút sau để quản lý các quy trình làm việc của bạn:

- <i class="fa-solid fa-pen-to-square"></i> **Open workflow editor** để xem và sửa đổi quy trình làm việc được chọn.
- <i class="fa-solid fa-plus"></i> **Create new workflow** để tạo một quy trình làm việc mới với tên tùy chỉnh.
- <i class="fa-solid fa-trash-can"></i> **Delete workflow** để xóa quy trình làm việc được chọn.

### Workflow Editor

ComfyUI Workflow Editor cho phép bạn xem và sửa đổi các quy trình làm việc ComfyUI để sử dụng với SillyTavern.

Thành phần chính của editor là một vùng văn bản lớn nơi bạn có thể chèn hoặc chỉnh sửa quy trình làm việc ComfyUI của bạn ở định dạng JSON.

Để thêm một quy trình làm việc ComfyUI vào editor, làm theo các bước sau:

1. Bật 'Dev Mode' trong cài đặt ComfyUI.
2. Sử dụng tùy chọn 'Save (API Format)' trong ComfyUI để tải xuống dữ liệu JSON.
3. Tạo một quy trình làm việc mới trong SillyTavern và mở editor.
4. Dán dữ liệu JSON được tải xuống vào vùng văn bản.
5. Thay thế các giá trị cụ thể bằng trình giữ chỗ khi cần cho trường hợp sử dụng của bạn.

!!!tip Tips
Bạn có thể thêm tệp API-format JSON trực tiếp vào thư mục `data/default-user/user/workflows` trong cài đặt SillyTavern của bạn. Điều này sẽ giúp bạn tránh các bước 3 và 4.

Giữ lại tệp JSON gốc. Nếu bạn cần mở quy trình làm việc lại trong ComfyUI để thực hiện các thay đổi, sẽ thuận tiện hơn nhiều khi chỉnh sửa tệp gốc so với tệp có tất cả các trình giữ chỗ.
!!!

### Trình giữ chỗ

Editor cung cấp một danh sách các trình giữ chỗ được định nghĩa trước có thể được sử dụng trong JSON quy trình làm việc của bạn. Các trình giữ chỗ này được thay thế bằng các giá trị động khi quy trình làm việc được thực hiện trong SillyTavern.

Trình giữ chỗ được đánh dấu bằng ✅ có mặt trong JSON quy trình làm việc của bạn. Trình giữ chỗ được đánh dấu bằng ❌ không có mặt trong JSON quy trình làm việc của bạn. Bạn có thể thêm các trình giữ chỗ này vào JSON quy trình làm việc của bạn khi cần thiết. Bạn không cần phải thêm tất cả các trình giữ chỗ, chỉ những cái mà quy trình làm việc của bạn sử dụng và bạn muốn thay thế động.

#### Prompts

Các trình giữ chỗ `%prompt%` và `%negative_prompt%` được sử dụng để chèn các prompt tạo hình ảnh vào quy trình làm việc. Chúng chứa các prompts cuối cùng được tạo bởi SillyTavern, bao gồm prompt được tạo cho chế độ `/sd` của bạn, tiền tố prompt chung, prompt âm và tiền tố prompt cụ thể cho nhân vật.

Ví dụ, bạn có thể đã kiểm tra quy trình làm việc của bạn với một prompt như "forest elf" trong ComfyUI. Để sử dụng quy trình làm việc này trong SillyTavern, bạn có thể thay thế prompt "forest elf" bằng trình giữ chỗ `%prompt%`:

+++ JSON with placeholder
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "%prompt%"
    }
}
```
+++ Original JSON
```json
{
    "class_type": "CLIPTextEncode",
    "inputs": {
        "clip": ["4", 1],
        "text": "forest elf"
    }
}
```
+++

Lưu ý rằng trình giữ chỗ được bao quanh bởi dấu ngoặc kép. Điều này rất quan trọng đối với định dạng JSON, và được yêu cầu bởi hệ thống thay thế trình giữ chỗ của SillyTavern. Ngay cả đối với các số, bạn phải sử dụng dấu ngoặc kép trong JSON mẫu.

Đôi khi prompt (hoặc giá trị khác) không xuất hiện nơi bạn có thể mong đợi. ComfyUI sẽ xóa các nút khỏi phiên bản API của quy trình làm việc nếu chúng không cần thiết để quy trình làm việc hoạt động ở chế độ API.

Ví dụ, quy trình làm việc này sử dụng một [nút tải LoRA tag loader](https://github.com/badjeff/comfyui_lora_tag_loader) với một nguyên thủy prompt để quy trình làm việc rõ ràng hơn ở chế độ UI:

![Prompt primitive and LoRA loader](/static/extensions/sd-comfy-prompt-primitive.png)

Nút prompt primitive sẽ bị xóa khỏi phiên bản API của quy trình làm việc, vì vậy bạn chèn trình giữ chỗ vào nút LoraTagLoader. Tìm văn bản "apple tree" trong quy trình làm việc và thay thế nó bằng trình giữ chỗ `%prompt%`:

+++ JSON with placeholder
```json
{
    "inputs": {
      "text": "%prompt%",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++ Original JSON
```json
{
    "inputs": {
      "text": "apple tree",
      "model": ["112", 0],
      "clip": ["112", 1]
    },
    "class_type": "LoraTagLoader",
    "_meta": {"title": "Load LoRA Tag"}
}
```
+++

Trong một số trường hợp, bạn có thể cần phải thực hiện một số thay thế trong JSON quy trình làm việc, ngay cả khi prompt chỉ xuất hiện một lần trong UI.

#### Mô hình

Trình giữ chỗ `%model%` sẽ chèn giá trị của mô hình được chọn vào cài đặt tạo hình ảnh.

Một ví dụ từ quy trình làm việc text-to-image mặc định:

+++ JSON with placeholder
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "%model%"
    }
}
```
+++ Original JSON
```json
{
    "class_type": "CheckpointLoaderSimple",
    "inputs": {
        "ckpt_name": "sd15.safetensors"
    }
}
```
+++

Để tải UNets được lượng hóa GGUF, sử dụng một nút [UNet Loader (GGUF)](https://github.com/city96/ComfyUI-GGUF) trong quy trình làm việc của bạn,
chọn một mô hình `GGUF` trong dropdown mô hình SillyTavern, và sử dụng trình giữ chỗ `%model%` trong cài đặt nút như thế này:

+++ JSON with placeholder
```json
{
    "inputs": {
      "unet_name": "%model%"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++ Original JSON
```json
{
    "inputs": {
      "unet_name": "flux1-dev-Q4_0.gguf"
    },
    "class_type": "UnetLoaderGGUF",
    "_meta": {
      "title": "Unet Loader (GGUF)"
    }
}
```
+++

!!!info Nếu bạn có các loại mô hình khác ngoài các checkpoint SD thông thường trong ComfyUI
Các checkpoint Stable Diffusion, SD UNets và UNets được lượng hóa GGUF đều xuất hiện trong dropdown Mô hình.
Các mô hình của một loại sẽ không hoạt động với các nút quy trình làm việc/loader mong đợi một loại khác.
Nếu bạn chọn một loại mô hình không tương thích trong ST, ComfyUI sẽ báo cáo một vấn đề với nút loader.
!!!

#### Avatar images

Sử dụng các trình giữ chỗ `%user_avatar%` và `%char_avatar%` để bao gồm các avatar người dùng và nhân vật trong quy trình làm việc. Các trình giữ chỗ này được thay thế bằng dữ liệu PNG của các avatar khi quy trình làm việc được thực hiện. Dữ liệu hình ảnh được mã hóa ở định dạng base64, vì vậy bạn phải giải mã nó trong quy trình làm việc của bạn. Một lựa chọn phổ biến cho công việc này là nút [Load image (Base64)](https://github.com/Acly/comfyui-tooling-nodes).

Trong ví dụ này, avatar nhân vật được tải bằng nút `Load Image (Base64)`. Nó cũng sử dụng một nút Image Resize để thay đổi kích thước hình ảnh theo bất kỳ kích thước nào được chỉ định trong cài đặt tạo hình ảnh:

![Load image from base64 string and resize](/static/extensions/sd-comfy-load-b64.png)

Chèn các trình giữ chỗ `%char_avatar%`, `%width%` và `%height%` vào JSON cho các nút Load Image (Base64) và Image Resize:

```json
{
    "97": {
        "inputs": {
            "image": "%char_avatar%"
        },
        "class_type": "ETN_LoadImageBase64",
        "_meta": {"title": "Load Image (Base64)"}
    },
    "98": {
        "inputs": {
            "mode": "resize",
            "resize_width": "%width%",
            "resize_height": "%height%",
            "image": ["97", 0]
        },
        "class_type": "Image Resize",
        "_meta": {"title": "Resize image"}
    }
}
```

Để lấy một chuỗi hình ảnh được mã hóa base64 để kiểm tra quy trình làm việc của bạn trong ComfyUI, hãy sử dụng bất kỳ công cụ trực tuyến nào chuyển đổi hình ảnh thành chuỗi base64.
Đây là một ví dụ về chuỗi bạn có thể sử dụng cho việc kiểm tra ban đầu: [sd-comfy-base64-test-string.txt](/static/extensions/sd-comfy-base64-test-string.txt).

#### Other placeholders

Hầu hết các trình giữ chỗ khác sử dụng các giá trị của các điều khiển tương ứng trong cài đặt tạo hình ảnh, hoặc các giá trị mà bạn chỉ định với lệnh `/sd`:

- `%vae%`, nhưng hầu hết các mô hình SD bao gồm một VAE vì vậy các quy trình làm việc mặc định không sử dụng trình giữ chỗ này. Sử dụng nó với các quy trình làm việc tùy chỉnh để tải một VAE bên cạnh một UNet, ghi đè VAE mặc định, v.v.
- `%sampler%`
- `%scheduler%`
- `%steps%`
- `%scale%`
- `%width%`
- `%height%`
- `%denoise%`: cho quy trình làm việc hình ảnh-to-hình ảnh mẫu, thay đổi lượng denoising giữa khoảng 0.5 (những thay đổi khó nhận thấy đối với hình ảnh nguồn) và 1.0 (một hình ảnh hoàn toàn khác như thể không sử dụng hình ảnh nguồn nào). Không được sử dụng bởi quy trình làm việc text-to-image mặc định vì không có điểm sử dụng giá trị khác ngoài 1.0 cho text-to-image.
- `%clip_skip%`: không được sử dụng bởi các quy trình làm việc mặc định nhưng có sẵn cho các quy trình làm việc tùy chỉnh.

Trình giữ chỗ `%seed%` sẽ chèn giá trị seed từ điều khiển nếu bạn đã chỉ định một. Nếu bạn đặt seed thành `-1`, SillyTavern sẽ tạo một seed ngẫu nhiên mới cho mỗi hình ảnh trong `%seed%`.

#### Trình giữ chỗ tùy chỉnh

Bạn có thể thêm các trình giữ chỗ tùy chỉnh vào quy trình làm việc của bạn:

1. Tìm phần "Custom" bên dưới các trình giữ chỗ được định nghĩa trước.
2. Nhấp nút "+" để thêm một trình giữ chỗ tùy chỉnh mới.
3. Nhập tên cho trình giữ chỗ vào trường `find`.
4. Nhập giá trị mà bạn muốn thay thế trình giữ chỗ bằng vào trường `replace`.

Các trình giữ chỗ tùy chỉnh sẽ xuất hiện trong một danh sách riêng biệt bên dưới những cái được định nghĩa trước.

Ví dụ, bạn có thể thay thế tiền tố "SillyTavern" cho các tên tệp hình ảnh đã lưu trong quy trình làm việc mặc định bằng một trình giữ chỗ tùy chỉnh. Thêm một trình giữ chỗ tùy chỉnh mới với `find` được đặt thành `filename_prefix` và `replace` được đặt thành `ServiceTensor`. Chèn trình giữ chỗ mới `%filename_prefix%` vào JSON quy trình làm việc của bạn. Bây giờ bạn có thể thay đổi tiền tố tên tệp từ SillyTavern thành ServiceTensor bằng cách thay đổi giá trị của trình giữ chỗ tùy chỉnh.

+++ JSON with placeholder
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "%filename_prefix%",
        "images": ["8", 0]
    }
}
```
+++ Original JSON
```json
{
    "class_type": "SaveImage",
    "inputs": {
        "filename_prefix": "SillyTavern",
        "images": ["8", 0]
    }
}
```
+++

### Comfy tricks

Đọc tất cả thông tin chung trên trang này để bạn quen thuộc với các tùy chọn tạo hình ảnh. Các tùy chọn như các kiểu có thể chuyển đổi và các tiền tố prompt chung, khi kết hợp với tính linh hoạt toàn bộ của các quy trình làm việc ComfyUI, cho phép bạn tạo nhiều loại cài đặt tạo hình ảnh.

#### Loading LoRAs

Sử dụng một nút tải thẻ LoRA (chẳng hạn như [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader)) để tải bất kỳ LoRAs nào được chỉ định trong prompt.
Bây giờ bạn có thể thêm bao nhiêu LoRAs bạn thích vào prompt của bạn bằng các thẻ như `<lora:CroissantStyle:0.8>`, và chúng sẽ được tải vào quy trình làm việc của bạn.
Điều này cũng sẽ tạo cho "pro-tip" sử dụng LoRAs trong [tiền tố prompt cụ thể cho nhân vật](#character-specific-prompt-prefix) hoạt động với ComfyUI.

#### Setting workflow values from styles or slash-commands

Bạn có thể sử dụng macros trong giá trị trình giữ chỗ tùy chỉnh. Như một ví dụ thực tế,
hãy nói rằng bạn đôi khi muốn tạo hình ảnh mà không có nền, và bạn thích điều này có thể được chuyển đổi bằng
một slash-command hoặc kiểu hình ảnh. Đây là cách bạn có thể làm điều này:

1. Tạo một quy trình làm việc ComfyUI loại bỏ nền hình ảnh, hoặc không, tùy thuộc vào giá trị của một đầu vào
2. Sử dụng một trình giữ chỗ tùy chỉnh để đặt giá trị của đầu vào đó, nhưng sử dụng `{{getvar::remove_background}}` làm giá trị thay thế
3. Bây giờ bạn có thể đặt giá trị của `remove_background` bằng `/setvar key=remove_background true` hoặc `/setvar key=remove_background false` trước khi tạo hình ảnh
4. Quy trình làm việc sẽ sử dụng giá trị bạn đặt để xác định liệu có loại bỏ nền
5. Tạo kiểu hình ảnh "No background" có tiền tố prompt chung `{{setvar::remove_background::true}}`
6. Sử dụng điều khiển kiểu hoặc `/imagine-style No background` để đặt giá trị của `remove_background` thành `true` trước khi tạo hình ảnh
