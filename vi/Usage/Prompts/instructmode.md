---
order: 80
route: /usage/core-concepts/instructmode/
---

# Instruct Mode

Instruct Mode cho phép bạn điều chỉnh prompting cho các model tuân theo hướng dẫn được huấn luyện trên nhiều định dạng prompt khác nhau, chẳng hạn như Alpaca, ChatML, Llama2, v.v.

!!! Áp dụng cho: Text Completion APIs
Đối với các cài đặt tương đương trong Chat Completion APIs, sử dụng [Prompt Manager](prompt-manager.md).
!!!

## Hỗ trợ API

### Text Completion API

Được hỗ trợ đầy đủ. Điều này bao gồm:

* Tất cả các nguồn dưới Text Completion
* KoboldAI Classic
* AI Horde

#### Chọn một định dạng

Template instruct được chọn phải khớp với kỳ vọng của model thực tế đang chạy trên backend.

Điều này thường được phản ánh trong model card trên HuggingFace, và một số thậm chí cung cấp các tệp JSON tương thích với SillyTavern.

Ví dụ: [NeverSleep/Noromaid-13b-v0.1.1](https://huggingface.co/NeverSleep/Noromaid-13b-v0.1.1#prompt-template-custom-format-or-alpaca)

### Chat Completion API (OpenAI, Claude, v.v.)

Điều này không được hỗ trợ **(và không cần thiết)** cho Chat Completion APIs. Chúng sử dụng một prompt builder hoàn toàn khác.

### NovelAI

Mặc dù *về mặt kỹ thuật* được hỗ trợ cho NovelAI, không có model nào của họ được huấn luyện để hiểu định dạng instruct. Các model NovelAI có thể sử dụng một instruct module đặc biệt được kích hoạt *tự động* khi một hướng dẫn được bọc trong dấu ngoặc nhọn được gặp trong tin nhắn chat, vì vậy việc sử dụng Instruct Mode cho toàn bộ prompt sẽ dẫn đến **chất lượng giảm** của các outputs.

Đây là một ví dụ tự động kích hoạt instruct module cho NovelAI:

```txt
User: { Write a happy song about Nintendo Switch. }
```

## Cài đặt Instruct Mode

### System Prompt

!!!warning Thay đổi gần đây
System Prompt hiện là một thực thể riêng biệt. Xem trang [Advanced Formatting](advancedformatting.md#system-prompt) để biết thêm chi tiết.
!!!

### Templates

Cung cấp các templates sẵn có với sequences cho một số model instruct nổi tiếng.

*Thay đổi template sẽ đặt lại các cài đặt chưa lưu về trạng thái đã lưu cuối cùng! Đừng quên lưu template của bạn nếu bạn đã thực hiện bất kỳ thay đổi nào bạn không muốn mất.*

### Activation Regex

Nếu được định nghĩa là một regular expression hợp lệ, khi kết nối với một model và tên của nó khớp với regex này, sẽ tự động chọn template này.

Instruct mode cần được bật trước. Chỉ regex match đầu tiên trên các templates sẽ được chọn (được đánh giá theo thứ tự bảng chữ cái).

### Wrap Sequences with Newline

Mỗi văn bản sequence sẽ được bọc với các ký tự dòng mới khi được chèn vào prompt. Bắt buộc cho Alpaca và các dẫn xuất của nó.

Tắt nếu bạn muốn có toàn quyền kiểm soát các ký tự kết thúc dòng.

### Replace Macro in Sequences

Nếu được bật, các thay thế \{\{macro\}\} đã biết sẽ được thay thế nếu được định nghĩa trong các sequences bọc tin nhắn.

Ngoài ra, một macro đặc biệt \{\{name\}\} có thể được sử dụng trong các tiền tố tin nhắn để tham chiếu tên thực tế được đính kèm với một tin nhắn (thay vì \{\{char\}\} hoặc \{\{user\}\} hiện đang hoạt động), có thể hữu ích khi sử dụng group chats hoặc lệnh /sendas. Nếu không thể xác định tên, "System" được sử dụng làm placeholder dự phòng.

### Include Names

Nếu được bật, thêm tên nhân vật và user vào logs lịch sử chat sau sequence tiền tố.

Các tùy chọn sau có sẵn:

* **Never**: Không thêm tiền tố tên trước nội dung tin nhắn.
* **Groups and Past Personas**: Chỉ thêm tiền tố tên cho tin nhắn từ nhân vật nhóm và personas trong quá khứ.
* **Always**: Luôn thêm tiền tố tên trước nội dung tin nhắn.

### Sequences: Story String Wrapping

!!!warning Thay đổi gần đây
System Prompt wrapping đã bị loại bỏ và thay thế bằng Story String wrapping.
!!!

Định nghĩa cách Story String sẽ được bọc khi Position được đặt thành "Default (top of context)"

#### Story String Prefix

Được chèn trước Story String.

#### Story String Suffix

Được chèn sau Story String.

### Sequences: Chat Messages Wrapping

Các cài đặt này định nghĩa cách tin nhắn thuộc các vai trò khác nhau sẽ được bọc khi xây dựng prompt.

Tất cả các sequences tiền tố cũng sẽ tự động được sử dụng làm stopping strings.

#### User Message Prefix

Được chèn trước tin nhắn User và là dòng prompt cuối cùng khi impersonating.

#### User Message Suffix

Được chèn sau tin nhắn User.

#### Assistant Message Prefix

Được chèn trước tin nhắn Assistant và là dòng prompt cuối cùng khi tạo AI reply.

#### Assistant Message Suffix

Được chèn sau tin nhắn Assistant

#### System Message Prefix

Được chèn trước tin nhắn System (được thêm bởi slash commands hoặc extensions).

#### System Message Suffix

Được chèn sau tin nhắn System.

#### System same as User

Nếu được đánh dấu là true, tin nhắn System sẽ sử dụng sequences tin nhắn vai trò User.

Nếu không, tin nhắn System sử dụng các sequences riêng của chúng (nếu không trống) hoặc sẽ không thực hiện bất kỳ wrapping nào (nếu trống).

### Misc. Sequences

Các cấu hình nâng cao khác nhau để điều chỉnh tinh hơn việc xây dựng prompt

#### First Assistant Prefix

Được chèn trước tin nhắn đầu tiên của Assistant.

!!!info
Chỉ tin nhắn đầu tiên của **lịch sử chat** được tính, không phải tin nhắn thực sự đi vào prompt đầu tiên!
!!!

#### Last Assistant Prefix

Được chèn trước tin nhắn cuối cùng của Assistant hoặc là dòng prompt cuối cùng khi tạo AI reply.

!!!info
Không được sử dụng khi tạo văn bản trong nền (ví dụ: Stable Diffusion prompts hoặc Summaries). System Instruction Prefix hoặc Regular Assistant Prefix sẽ được sử dụng thay thế.
!!!

#### System Instruction Prefix

Được chèn làm dòng prompt cuối cùng khi tạo văn bản neutral/system trong nền (ví dụ: Stable Diffusion prompts hoặc Summaries).

#### User Filler Message

Sẽ được chèn vào đầu lịch sử chat nếu nó không bắt đầu bằng tin nhắn User.

**Trường hợp sử dụng:** khi một định dạng instruct *nghiêm ngặt yêu cầu* prompts phải là user-first và chỉ có tin nhắn với các vai trò xen kẽ, ví dụ: Llama 2 Chat, Mistral Instruct.

#### Stop Sequence

Văn bản biểu thị sự kết thúc của reply. Cũng được gửi như một stopping string đến backend API.

Nếu một stop sequence được tạo ra, mọi thứ sau nó sẽ bị loại bỏ khỏi output (bao gồm cả sequence chính nó).
