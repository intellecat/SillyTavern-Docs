---
order: 90
templating: false
route: /usage/prompts/context-template/
---

# Context Template

!!! Áp dụng cho: Text Completion APIs
Đối với các cài đặt tương đương trong Chat Completion APIs, sử dụng [Prompt Manager](prompt-manager.md).
!!!

Thông thường, các model AI yêu cầu bạn cung cấp dữ liệu nhân vật cho chúng theo một cách cụ thể. SillyTavern bao gồm một danh sách các quy tắc chuyển đổi được tạo sẵn cho các model khác nhau, nhưng bạn có thể tùy chỉnh chúng theo ý thích.

Chỉnh sửa các cài đặt này trong panel "[Advanced Formatting](advancedformatting.md)".

## Story String

Trường này là một template cho phần mở đầu prompt (được biết nội bộ là một story string). Đây là cách chính để thêm thông tin được định nghĩa trong [Character Cards](/Usage/Characters/index.md) cho các model text completion và instruct.

Template hỗ trợ cú pháp Handlebars, chèn văn bản tùy chỉnh hoặc định dạng, và bất kỳ [macros](/Usage/Characters/macros.md) nào khác. Xem tài liệu ngôn ngữ tại đây: <https://handlebarsjs.com/guide/>

Chúng tôi cung cấp các tham số sau cho Handlebars evaluator (được bọc trong dấu ngoặc nhọn kép):

1. `{{anchorBefore}}`: Các prompts được đặt để sử dụng vị trí "Before Story String".
2. `{{anchorAfter}}`: Các prompts được đặt để sử dụng vị trí "After Story String".
3. `{{description}}`: [Description](/Usage/Characters/characterdesign.md#character-description) của nhân vật.
4. `{{scenario}}`: [Scenario](/Usage/Characters/characterdesign.md#scenario) của nhân vật.
5. `{{personality}}`: [Personality](/Usage/Characters/characterdesign.md#personality-summary) của nhân vật.
6. `{{system}}`: [system prompt](advancedformatting.md#system-prompt) HOẶC ghi đè [main prompt](/Usage/Characters/characterdesign.md#prompt-overrides) của nhân vật (nếu nó tồn tại và "Prefer Char. Prompt" được bật trong User Settings).
7. `{{persona}}`: [persona's description](/Usage/personas.md#persona-description) đã chọn.
8. `{{char}}`: Tên của nhân vật.
9. `{{user}}`: Tên của persona đã chọn.
10. `{{wiBefore}}` hoặc `{{loreBefore}}`: Các mục [World Info](/Usage/worldinfo.md) được kích hoạt kết hợp với Position được đặt thành "Before Char Defs".
11. `{{wiAfter}}` hoặc `{{loreAfter}}`: Các mục [World Info](/Usage/worldinfo.md) được kích hoạt kết hợp với Position được đặt thành "After Char Defs".
12. `{{mesExamples}}`: (Tùy chọn) [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) của nhân vật, được định dạng instruct với một separator.
13. `{{mesExamplesRaw}}`: [Example Dialogues](/Usage/Characters/characterdesign.md#examples-of-dialogue) của nhân vật ở định dạng thô, không có bất kỳ định dạng nào.

!!!tip **Quan trọng**
Khi sử dụng `{{mesExamples}}` trong Story String, đặt **"Example Messages Behavior"** trong panel **<i class="fa-solid fa-user-cog"></i> User Settings** thành **"Never include examples"** để tránh trùng lặp example messages trong prompt.
!!!

Một macro đặc biệt `{{trim}}` được hỗ trợ để loại bỏ bất kỳ dòng mới nào xung quanh nó. Sử dụng nó nếu bạn muốn một phần của văn bản không được phân tách khỏi dòng trước bằng một dòng mới (_khoảng trắng **không** bị loại bỏ_).

**CẢNH BÁO**: Nếu bất kỳ tham số nào ở trên bị thiếu trong template story string, chúng sẽ không được gửi trong prompt.

### Prompt Anchors

`{{anchorBefore}}` và `{{anchorAfter}}` là placeholders chung cho các prompts được thêm bởi các extension khác nhau và các tính năng khác trong một vị trí tĩnh đã chọn, ví dụ:

* [Author's Note](/Usage/Characters/Author's-Note.md)
* [Summaries](/extensions/Summarize.md)
* [Chat Vectorization](/extensions/Chat-vectorization.md) / [Data Bank](/Usage/Characters/data-bank.md)
* [STscript injections](/For_Contributors/st-script.md#prompt-injections)
* [Web Search](/extensions/WebSearch.md)

### Vị trí Story String

Theo mặc định, story string được render (với tất cả các placeholders được thay thế) được đặt ở đầu prompt, theo sau là example messages và lịch sử chat hiển thị.

Ngoài ra, bạn có thể di chuyển nó đến một vị trí động bằng cách chọn tùy chọn "In-chat @ Depth", đặt story string ở một độ sâu cụ thể trong context chat.

!!!warning **Chú ý**
Nếu template chứa các phần tử prompt tĩnh (tiền tố hoặc hậu tố cụ thể của model) để bọc story string, việc sử dụng vị trí "In-Chat @ Depth" sẽ khiến nó bị bọc sai với các chuỗi trùng lặp, có thể dẫn đến kết quả không mong muốn.

Trong trường hợp này, bạn có thể sửa lỗi bằng một trong các cách sau:

1. **Templates có sẵn**: Đặt lại templates về mặc định bằng các bước được mô tả trong [Advanced Formatting](/Usage/Prompts/advancedformatting.md#resetting-templates).
2. **Templates tùy chỉnh**: Di chuyển các phần tử tĩnh từ template story string sang [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
!!!

### Bọc Story String

!!!
Phần sau chỉ áp dụng khi **Instruct Mode** được BẬT.
!!!

* Vị trí **Default**: Story String được render sẽ được bọc bằng các chuỗi được định nghĩa trong [Story String Sequences](/Usage/Prompts/instructmode.md#sequences-story-string-wrapping).
* Vị trí **In-chat @ Depth**: Story String được render sẽ được bọc bằng các chuỗi được định nghĩa trong [Chat Messages Sequences](/Usage/Prompts/instructmode.md#sequences-chat-messages-wrapping) cho một vai trò đã chọn (mặc định: System).

## Example Separator

Được sử dụng làm header block và separator giữa các block example dialogue. Bất kỳ instance nào của thẻ `<START>` trong example dialogues sẽ được thay thế bằng nội dung của trường này.

## Chat Start

Được chèn làm separator sau story string được render và sau các block example dialogues, nhưng trước tin nhắn đầu tiên trong context.

## Separators as Stop Strings

Thêm "Example Separator" và "Chat Start" vào danh sách stop strings.

Hữu ích nếu model có xu hướng ảo giác hoặc rò rỉ toàn bộ các block example dialogue đứng trước separator.

## Names as Stop Strings

Thêm tên Character và User Persona vào danh sách stop strings.

Khuyên nên giữ bật để ngăn model giả mạo.

## Always add character's name to prompt

!!!info
Cài đặt này không có hiệu lực khi Instruct Mode được BẬT. Hành vi tên thay vào đó được định nghĩa bởi tùy chọn [Include Names](/Usage/Prompts/instructmode.md#include-names) được chọn.
!!!

Thêm tên của nhân vật vào prompt để buộc model hoàn thành tin nhắn với vai trò nhân vật:

```txt
** OTHER CONTEXT HERE **
Character:
```
