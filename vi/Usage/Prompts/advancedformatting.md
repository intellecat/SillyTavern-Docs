---
order: 100
route: /vi/usage/core-concepts/advancedformatting/
---

# Advanced Formatting

Các cài đặt được cung cấp trong phần này cho phép kiểm soát nhiều hơn đối với chiến lược [xây dựng prompt](index.md), chủ yếu cho Text Completion APIs.

Hầu hết các cài đặt trong panel này không áp dụng cho Chat Completions APIs vì chúng được quản lý bởi hệ thống prompt manager thay thế.

+++ Text Completion APIs
* [System Prompt](#system-prompt)
* [Context Template](#context-template)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++ Chat Completion APIs
* System Prompt: không áp dụng, sử dụng [Prompt Manager](prompt-manager.md)
* Context Template: không áp dụng, sử dụng [Prompt Manager](prompt-manager.md)
* [Tokenizer](#tokenizer)
* [Custom Stopping Strings](#custom-stopping-strings)
+++

## Đặt lại Templates

Bạn có thể khôi phục các templates mặc định về trạng thái ban đầu. Điều này có thể được thực hiện thông qua giao diện hoặc bằng cách xóa thủ công các tệp dữ liệu liên quan.

### Đặt lại qua giao diện

1. Mở menu **<i class="fa-solid fa-font"></i> Advanced Formatting**.
2. Chọn template bạn muốn đặt lại.
3. Nhấp vào nút **<i class="fa-solid fa-recycle"></i> Restore current template**.
4. Xác nhận hành động khi được nhắc.

### Đặt lại thủ công

!!!
Đảm bảo cài đặt `skipContentCheck` được đặt thành `false` trong [config.yaml](/Administration/config-yaml.md#data-configuration), nếu không việc kiểm tra nội dung sẽ không được kích hoạt.
!!!

1. Điều hướng đến thư mục dữ liệu người dùng của bạn (xem [Data paths](/Installation/index.md#data-paths) để biết chi tiết).
2. Xóa tệp `content.log` từ thư mục gốc của dữ liệu người dùng. Tệp này theo dõi các tệp mặc định được sao chép cho người dùng của bạn.
3. Xóa các tệp JSON template từ các thư mục con liên quan (`context`, `instruct`, `sysprompt`, v.v.).
4. Khởi động lại server SillyTavern. Ứng dụng sẽ tự động tạo lại nội dung mặc định, khôi phục bất kỳ templates mặc định nào đã bị xóa.

## Templates được định nghĩa bởi Backend

!!! Áp dụng cho: Text Completion APIs
Không áp dụng cho Chat Completion APIs vì chúng sử dụng một prompt builder khác.
!!!

Một số nguồn Text Completion cung cấp khả năng tự động chọn templates được khuyến nghị bởi tác giả model. Điều này hoạt động bằng cách so sánh hash của chat template được định nghĩa trong tệp `tokenizer_config.json` của model với một trong các templates SillyTavern mặc định.

1. Tùy chọn **<i class="fa-solid fa-bolt"></i> Derive templates** phải được bật trong menu **<i class="fa-solid fa-font"></i> Advanced Formatting**. Điều này có thể được áp dụng cho Context, Instruct, hoặc cả hai.
2. Một backend được hỗ trợ phải được chọn làm nguồn Text Completion. Hiện tại chỉ llama.cpp và KoboldCpp hỗ trợ derive templates.
3. Model phải báo cáo chính xác metadata của nó khi kết nối với API được thiết lập. Nếu điều này không hoạt động, hãy thử cập nhật backend lên phiên bản mới nhất.
4. Hash chat template được báo cáo phải khớp với một trong các [templates SillyTavern đã biết](https://github.com/SillyTavern/SillyTavern/blob/release/public/scripts/chat-templates.js). Điều này chỉ bao gồm các templates mặc định, như Llama 3, Gemma 2, Mistral V7, v.v.
5. Nếu hash khớp, template sẽ được tự động chọn nếu nó tồn tại trong danh sách templates (tức là không bị đổi tên hoặc xóa).

## System Prompt

!!! Áp dụng cho: Text Completion APIs
Đối với các cài đặt tương đương trong Chat Completion APIs, sử dụng [Prompt Manager](prompt-manager.md). **Main Prompt** là tương đương với System Prompt trong Chat Completion APIs.
!!!

System Prompt định nghĩa các hướng dẫn chung cho model cần tuân theo. Nó đặt tông và ngữ cảnh cho cuộc trò chuyện. Ví dụ, nó cho model biết hoạt động như một trợ lý AI, một đối tác viết lách, hoặc một nhân vật hư cấu.

System Prompt là một phần của [Story String](context-template.md#story-string) và thường là phần đầu tiên của prompt mà model nhận được.

Xem [hướng dẫn prompting](index.md#main-prompt-system-prompt) để tìm hiểu thêm về System Prompt.

## Context Template

!!! Áp dụng cho: Text Completion APIs
Đối với các cài đặt tương đương trong Chat Completion APIs, sử dụng [Prompt Manager](prompt-manager.md).
!!!

Thông thường, các model AI yêu cầu bạn cung cấp dữ liệu nhân vật cho chúng theo một cách cụ thể. SillyTavern bao gồm một danh sách các quy tắc chuyển đổi được tạo sẵn cho các model khác nhau, nhưng bạn có thể tùy chỉnh chúng theo ý thích.

Các tùy chọn cho phần này được giải thích trong [Context Template](context-template.md).

## Tokenizer

Tokenizer là một công cụ chia một đoạn văn bản thành các đơn vị nhỏ hơn gọi là tokens. Các tokens này có thể là từng từ riêng lẻ hoặc thậm chí các phần của từ, chẳng hạn như tiền tố, hậu tố, hoặc dấu câu. Một quy tắc chung là một token thường tương ứng với 3~4 ký tự văn bản.

Các tùy chọn cho phần này được giải thích trong [Tokenizer](tokenizer.md).

## Custom Stopping Strings

Chấp nhận một mảng JSON-serialized của các stopping strings. Ví dụ: `["\n", "\nUser:", "\nChar:"]`. Nếu bạn không chắc chắn về định dạng, hãy sử dụng [online JSON validator](https://jsonlint.com/). Nếu output của model **kết thúc** bằng bất kỳ stop string nào, chúng sẽ bị loại bỏ khỏi output.

APIs được hỗ trợ:

1. KoboldAI Classic (phiên bản 1.2.2 trở lên) hoặc KoboldCpp
2. AI Horde
3. Text Completion APIs: Text Generation WebUI (ooba), Tabby, Aphrodite, Mancer, TogetherAI, Ollama, v.v.
4. NovelAI
5. OpenAI (tối đa 4 strings) và các APIs tương thích
6. OpenRouter (cả Text và Chat Completion)
7. Claude
8. Google AI Studio
9. MistralAI

## Start Reply With

!!! Lưu ý
Theo mặc định, tiền tố Start Reply With sẽ không được hiển thị trong tin nhắn kết quả. Bật "Show reply prefix in chat" để hiển thị nó.
!!!

### Text Completion APIs

Điền trước dòng cuối cùng của prompt, buộc model tiếp tục từ điểm đó. Điều này hữu ích để thực thi nội dung, chẳng hạn như hướng về [Model Reasoning](/Usage/Prompts/reasoning.md) với tiền tố được xác định:

```txt
<think>
Sure!
```

### Chat Completion APIs

Thêm một tin nhắn vai trò assistant vào cuối prompt. Đối với một số model backend, điều này tương đương với việc điền trước response của model, nhưng một số có thể không hỗ trợ điều đó và sẽ thất bại với lỗi validation. Nếu bạn không chắc chắn, hãy để trường này trống.
