---
order: 90
route: /usage/core-concepts/macros/
---

# Macros (thẻ thay thế)

!!! Lưu ý
Danh sách này có thể không đầy đủ hoặc lỗi thời. Sử dụng lệnh slash `/help macros` trong bất kỳ chat SillyTavern nào để nhận danh sách các macros hoạt động trong instance của bạn.
!!!

Macros có thể được sử dụng trong mô tả nhân vật, author's notes, world info và nhiều nơi khác và được thay thế bằng các giá trị tương ứng khi tạo phản hồi. Chúng có thể được sử dụng để chèn nội dung động vào prompt, chẳng hạn như tên người dùng, mô tả nhân vật hoặc thời gian hiện tại. Macros được đặt trong dấu ngoặc nhọn kép, ví dụ: `{{user}}` và thường không phân biệt chữ hoa chữ thường. **Xin lưu ý rằng hiện tại không hỗ trợ macro lồng nhau.**

Lưu ý: một số extension cũng có thể thêm các macros đặc biệt theo ngữ cảnh chỉ hoạt động trong các khu vực nhất định (tức là các placeholder đặc biệt cho các prompt extension). Những cái này sẽ không được ghi lại ở đây trừ khi macro không bị ràng buộc với một chức năng cụ thể.

## Macros chung

| Macro | Mô tả |
|-------|-------------|
| `{{pipe}}` | Chỉ dành cho xử lý lô lệnh slash. Được thay thế bằng kết quả được trả về của lệnh trước đó. |
| `{{newline}}` | Chèn một dòng mới. |
| `{{trim}}` | Cắt các dòng mới xung quanh macro này. |
| `{{noop}}` | Không có thao tác, chỉ là một chuỗi rỗng. |
| `{{user}}` hoặc `<USER>` | Tên của người dùng. |
| `{{charPrompt}}` | Ghi đè Main Prompt của nhân vật. |
| `{{charJailbreak}}` | Ghi đè Post-History Instructions Prompt của nhân vật. |
| `{{group}}` hoặc `{{charIfNotGroup}}` | Danh sách tên thành viên group được phân tách bằng dấu phẩy hoặc tên nhân vật trong các cuộc chat solo. |
| `{{groupNotMuted}}` | Giống như `{{group}}` nhưng loại trừ các thành viên bị tắt tiếng. |
| `{{notChar}}` | Danh sách được phân tách bằng dấu phẩy của tất cả người tham gia chat ngoại trừ người nói hiện tại (`{{char}}`). Trong group chats, điều này vẫn bao gồm các nhân vật bị tắt tiếng, và khi không có tin nhắn nào được tạo, nó liệt kê mọi nhân vật trong danh sách. |
| `{{char}}` hoặc `<BOT>` | Tên của nhân vật. |
| `{{description}}` | Mô tả của nhân vật. |
| `{{scenario}}` | Scenario của nhân vật hoặc ghi đè scenario của chat (nếu được đặt). |
| `{{personality}}` | Tính cách của nhân vật. |
| `{{persona}}` | Mô tả persona của người dùng. |
| `{{mesExamples}}` | Ví dụ đối thoại của nhân vật (định dạng instruct). |
| `{{mesExamplesRaw}}`  | Ví dụ đối thoại của nhân vật (không thay đổi và không chia). |
| `{{charVersion}}` | Số phiên bản của nhân vật. |
| `{{charDepthPrompt}}` | Prompt at-depth của nhân vật. |
| `{{model}}` | Tên model tạo văn bản cho API hiện đang được chọn. **Có thể không chính xác!** |
| `{{lastMessageId}}` | ID tin nhắn chat cuối cùng. |
| `{{lastMessage}}` | Văn bản tin nhắn chat cuối cùng. |
| `{{firstIncludedMessageId}}` | ID của tin nhắn đầu tiên được bao gồm trong context. Yêu cầu generation phải được chạy ít nhất một lần trong phiên hiện tại. |
| `{{lastCharMessage}}` | Tin nhắn chat cuối cùng được gửi bởi nhân vật. |
| `{{lastUserMessage}}` | Tin nhắn chat cuối cùng được gửi bởi người dùng. |
| `{{currentSwipeId}}` | ID dựa trên 1 của swipe tin nhắn cuối cùng hiện đang được hiển thị. |
| `{{lastSwipeId}}` | Số lượng swipes trong tin nhắn chat cuối cùng. |
| `{{lastGenerationType}}` | Loại yêu cầu generation được xếp hàng cuối cùng. Giá trị: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue". |
| `{{original}}` | Có thể được sử dụng trong các trường Prompt Overrides để bao gồm prompt mặc định từ cài đặt hệ thống. Chỉ áp dụng cho Chat Completion APIs và chế độ Instruct. |
| `{{time}}` | Thời gian hệ thống hiện tại. |
| `{{time_UTC±X}}` | Thời gian hiện tại trong múi giờ UTC được chỉ định (timezone), ví dụ: đối với UTC+02:00 sử dụng `{{time_UTC+2}}`. |
| `{{timeDiff::(time1)::(time2)}}` | Chênh lệch thời gian giữa time1 và time2. Chấp nhận các macros time và date. |
| `{{date}}` | Ngày hệ thống hiện tại. |
| `{{input}}` | Nội dung của thanh nhập của người dùng. |
| `{{weekday}}` | Ngày trong tuần hiện tại. |
| `{{isotime}}` | Thời gian ISO hiện tại (đồng hồ 24 giờ). |
| `{{isodate}}` | Ngày ISO hiện tại (YYYY-MM-DD). |
| `{{datetimeformat ...}}` | Ngày/giờ hiện tại theo định dạng được chỉ định (ví dụ: `{{datetimeformat DD.MM.YYYY HH:mm}}`). |
| `{{idle_duration}}` | Chèn một chuỗi được nhân hóa của khoảng thời gian kể từ tin nhắn người dùng cuối cùng được gửi (ví dụ: 4 hours, 1 day). |
| `{{random:(args)}}` | Trả về một mục ngẫu nhiên từ danh sách (ví dụ: `{{random:1,2,3,4}}` sẽ trả về 1 trong 4 số ngẫu nhiên). |
| `{{random::arg1::arg2}}` | Cú pháp thay thế cho random hỗ trợ dấu phẩy trong các đối số của nó. |
| `{{pick::(args)}}` | Thay thế cho random, nhưng đối số được chọn ổn định khi đánh giá tiếp theo trong chat hiện tại nếu chuỗi nguồn không thay đổi. |
| `{{roll:(formula)}}` | Tạo một giá trị ngẫu nhiên bằng cú pháp xúc xắc D&D: XdY+Z (ví dụ: `{{roll:d6}}` tạo một giá trị 1-6). |
| `{{bias "text here"}}` | Đặt một bias hành vi cho AI cho đến lần nhập người dùng tiếp theo. Yêu cầu dấu ngoặc kép xung quanh văn bản. |
| `{{// (note)}}` | Cho phép để lại ghi chú sẽ được thay thế bằng nội dung trống. Không hiển thị cho AI. |
| `{{banned "text here"}}` | Thêm động văn bản được trích dẫn vào các chuỗi từ bị cấm cho backend Text Generation WebUI. Không làm gì cho các backend khác. Yêu cầu dấu ngoặc kép. |
| `{{reverse:(content)}}` | Đảo ngược nội dung của macro. |
| `{{outlet::(name)}}` | Được thay thế bằng nội dung của [World Info outlet](/Usage/worldinfo.md#outlet-name) được đặt tên, sẽ chứa các entries được kích hoạt được phân tách bằng các dòng mới. |

## Macros Instruct Mode và Context Template

(được bật trong cài đặt Advanced Formatting)

| Macro | Mô tả |
|-------|-------------|
| `{{exampleSeparator}}` | Dấu phân tách ví dụ đối thoại của context template. |
| `{{chatStart}}` | Dòng bắt đầu chat của context template. |
| `{{instructSystemPrompt}}` | System prompt của Instruct. |
| `{{instructSystemPromptPrefix}}` | Chuỗi tiền tố system prompt. |
| `{{instructSystemPromptSuffix}}` | Chuỗi hậu tố system prompt. |
| `{{instructUserPrefix}}` | Chuỗi tiền tố tin nhắn người dùng. |
| `{{instructAssistantPrefix}}` | Chuỗi tiền tố tin nhắn assistant. |
| `{{instructSystemPrefix}}` | Chuỗi tiền tố tin nhắn system. |
| `{{instructUserSuffix}}` | Chuỗi hậu tố tin nhắn người dùng. |
| `{{instructAssistantSuffix}}` | Chuỗi hậu tố tin nhắn assistant. |
| `{{instructSystemSuffix}}` | Chuỗi hậu tố tin nhắn system. |
| `{{instructFirstAssistantPrefix}}` | Chuỗi đầu ra đầu tiên của assistant. |
| `{{instructLastAssistantPrefix}}` | Chuỗi đầu ra cuối cùng của assistant. |
| `{{instructFirstUserPrefix}}` | Chuỗi đầu vào đầu tiên của người dùng Instruct. |
| `{{instructLastUserPrefix}}` | Chuỗi đầu vào cuối cùng của người dùng Instruct. |
| `{{instructSystemInstructionPrefix}}` | Chuỗi tiền tố hướng dẫn system. |
| `{{instructUserFiller}}` | Văn bản tin nhắn điền của người dùng. |
| `{{instructStop}}` | Chuỗi dừng Instruct. |
| `{{maxPrompt}}` | Kích thước tối đa của prompt tính bằng token (độ dài context giảm bởi độ dài phản hồi). |
| `{{systemPrompt}}` | Nội dung system prompt, bao gồm ghi đè prompt nhân vật nếu được phép và có sẵn. |
| `{{defaultSystemPrompt}}` | Nội dung system prompt (không bao gồm ghi đè prompt nhân vật). |

## Macros biến Chat

- Local variables = duy nhất cho chat hiện tại
- Global variables = hoạt động trong bất kỳ chat nào cho bất kỳ nhân vật nào

| Macro | Mô tả |
|-------|-------------|
| `{{getvar::name}}` | Được thay thế bằng giá trị của biến cục bộ "name". |
| `{{setvar::name::value}}` | Được thay thế bằng chuỗi rỗng, đặt biến cục bộ "name" thành "value". Cho phép giá trị trống. |
| `{{addvar::name::increment}}` | Được thay thế bằng chuỗi rỗng, thêm giá trị số của "increment" vào biến cục bộ "name". |
| `{{incvar::name}}` | Được thay thế bằng kết quả của việc tăng giá trị của biến "name" lên 1. |
| `{{decvar::name}}` | Được thay thế bằng kết quả của việc giảm giá trị của biến "name" xuống 1. |
| `{{getglobalvar::name}}` | Được thay thế bằng giá trị của biến toàn cục "name". |
| `{{setglobalvar::name::value}}` | Được thay thế bằng chuỗi rỗng, đặt biến toàn cục "name" thành "value". Cho phép giá trị trống. |
| `{{addglobalvar::name::value}}` | Được thay thế bằng chuỗi rỗng, thêm giá trị số của "increment" vào biến toàn cục "name". |
| `{{incglobalvar::name}}` | Được thay thế bằng kết quả của việc tăng giá trị của biến toàn cục "name" lên 1. |
| `{{decglobalvar::name}}` | Được thay thế bằng kết quả của việc giảm giá trị của biến toàn cục "name" xuống 1. |
| `{{var::name}}` | Được thay thế bằng giá trị của biến scoped "name" (chỉ STscript). |
| `{{var::name::index}}` | Được thay thế bằng giá trị tại index của biến scoped "name" (cho arrays/objects trong STscript). |

## Macros đặc biệt cho Extension

Được thêm bởi các extensions và chỉ hoạt động trong các điều kiện nhất định.

| Macro | Mô tả |
|-------|-------------|
| `{{summary}}` | Được thay thế bằng bản tóm tắt của phiên chat hiện tại (nếu có). |
| `{{authorsNote}}` | Được thay thế bằng nội dung của Author's Note. |
| `{{charAuthorsNote}}` | Được thay thế bằng nội dung của Character's Author's Note. |
| `{{defaultAuthorsNote}}` | Được thay thế bằng nội dung của default Author's Note. |
| `{{charPrefix}}` | Được thay thế bằng tiền tố prompt positive Image Generation đặc biệt cho nhân vật (nếu có). |
| `{{charNegativePrefix}}` | Được thay thế bằng tiền tố prompt negative Image Generation đặc biệt cho nhân vật (nếu có). |
