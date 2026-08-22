---
order: 50
templating: false
route: /vi/usage/prompts/prompt-manager/
---

# Prompt Manager

Prompt Manager là một hệ thống cung cấp nhiều kiểm soát hơn đối với chiến lược [xây dựng prompt](index.md) cho Chat Completion APIs.

!!! Áp dụng cho: Chat Completion APIs
Đối với các cài đặt tương đương trong Text Completion APIs, sử dụng [Advanced Formatting](advancedformatting.md).
!!!

!!!tip Đặt tên Presets
Nếu một preset chia sẻ tên với một trong các character cards của bạn, nó sẽ tự động được chọn khi bắt đầu chat với nhân vật đó. Đặt tên presets là thứ gì đó độc đáo để tránh hành vi này.
!!!

Truy cập Prompt Manager bằng cách nhấp vào nút "AI Response Configuration" trong thanh điều hướng. Prompt Manager nằm bên dưới panel [cài đặt chung](/Usage/Common-Settings.md).

## Quick Prompts Edit

Cung cấp không gian để nhanh chóng chỉnh sửa các phần prompt phổ biến, chẳng hạn như **Main Prompt**, **Auxiliary Prompt**, và **Post-History Instructions**. Thêm thông tin về các prompts này có thể được tìm thấy trên trang [xây dựng prompt](index.md).

## Utility Prompts

Các prompts này được gửi đến Chat Completion model để giúp nó hiểu thông tin được gửi đến nó, hoặc hướng dẫn nó hoạt động theo các cách cụ thể trong các loại tương tác nhất định.

### Format Templates

!!!tip
Nếu format template không được đặt, thông tin sẽ được gửi nguyên trạng, không có bất kỳ wrapping nào.
!!!

Đây là các string templates được sử dụng để bọc thông tin được lấy từ [World Info](/Usage/worldinfo.md) và [Character Cards](/Usage/Characters/characterdesign.md).

Một marker đặc biệt được sử dụng để chỉ ra nơi thông tin nên được chèn:

- `{0}` cho World Info format template.
- `{{scenario}}` cho Scenario format template.
- `{{personality}}` cho Personality format template.

### Group Nudge Prompt Template

Chỉ được sử dụng trong group chats. Được đặt ở cuối prompt để buộc reply từ một nhân vật cụ thể.

Để trống để vô hiệu hóa chức năng Group Nudge.

### New Chat, New Group Chat, New Example Chat

Chúng được gửi trước lịch sử chat và trước mỗi block [Example Dialogue](/Usage/Characters/characterdesign.md#examples-of-dialogue) để thông báo cho model biết nơi thông tin nền kết thúc và lịch sử chat bắt đầu.

- **New Chat:** Được sử dụng cho chats cá nhân.
- **New Group Chat:** Được sử dụng cho group chats.
- **New Example Chat:** Được sử dụng cho các block example dialogue.

Để trống để vô hiệu hóa chức năng này.

### Continue Nudge

Được gửi ở cuối prompt để hướng dẫn model về những gì cần làm khi Continue được kích hoạt, chẳng hạn như khi nút Continue được nhấn hoặc khi được kích hoạt bởi STScript.

!!! Chat Completion 'Continues'
Hãy nhớ rằng các model Chat Completion xử lý Continues khác với các model **Text Completion**, và có thể không luôn cung cấp kết quả liền mạch bất kể Continue Nudge của bạn.
!!!

### Replace Empty Message

Gửi nội dung của trường này thay vì tin nhắn trống khi text box trống và **Send a message** được nhấn.

## Character Names Behavior

Cung cấp các chiến lược khác nhau để hướng dẫn model về cách liên kết tin nhắn với nhân vật. Nếu một Chat Completion model gặp khó khăn trong việc xác định tin nhắn nào thuộc về nhân vật nào, nó có thể cần một chiến lược khác được chọn.

## Continue Postfix

Khi Continue được kích hoạt, tin nhắn 'continued' được trả về bởi model sẽ có Continue Postfix được chọn được thêm vào đầu. Ví dụ, nó có thể thêm một khoảng trắng trước văn bản tiếp tục.

## Additional Settings

### Wrap in Quotes

!!!warning
Tùy chọn không được khuyến khích. Ưu tiên [Regex scripts](/extensions/Regex.md) thay thế.
!!!

Bọc toàn bộ tin nhắn user trong dấu ngoặc kép ẩn trước khi gửi. Điều này hữu ích cho các phiên mà nhân vật không sử dụng dấu ngoặc kép để chỉ lời nói. Nếu phiên của bạn sử dụng dấu ngoặc kép để chỉ lời nói, hãy bỏ chọn điều này.

### Continue Prefill

!!!warning
Có thể không hoạt động với tất cả các nguồn Chat Completion.
!!!

Gửi Continue Nudge như một tin nhắn vai trò Assistant thay vì tin nhắn System. Nếu điều này được bật, prompt Continue Nudge sẽ không được sử dụng.

### Squash system messages

!!!warning
Tùy chọn không được khuyến khích. Ưu tiên [Prompt Post-Processing](/Usage/API_Connections/openai.md#prompt-post-processing) thay thế.
!!!

Kết hợp các tin nhắn System liên tiếp thành một tin nhắn kết hợp duy nhất (loại trừ Example Dialogue).

### Enable web search

!!!
Không nên nhầm lẫn với [Web Search extension](/extensions/WebSearch.md).
!!!

Kích hoạt khả năng tìm kiếm web được cung cấp bởi Chat Completion backend. Prompt thường được làm giàu với kết quả tìm kiếm bởi nhà cung cấp model và có thể phát sinh chi phí bổ sung.

### Enable function calling

Xem [Function Calling](/For_Contributors/Function-Calling.md)

### Send inline images, Send inline videos

!!!
Không nên nhầm lẫn với [Image Captioning extension](/extensions/captioning.md).
!!!

Nếu Chat Completion model có khả năng đa phương thức để xử lý hình ảnh và video được gửi, điều này chuyển đổi khả năng của nó để làm như vậy. Để thêm media vào prompt, sử dụng tùy chọn **Attach A File** trong menu "Magic Wand".

### Request inline images

!!!
Không nên nhầm lẫn với [Image Generation extension](/extensions/Stable-Diffusion.md).
!!!

Cho phép model trả về các tệp đính kèm hình ảnh.

### Use system prompt

!!!
Chỉ được hỗ trợ bởi Google Gemini và Anthropic Claude backends.

Mặc dù có các cài đặt rất giống nhau cho hai điều này, chúng về mặt kỹ thuật là các tùy chọn riêng biệt, vì vậy chúng có thể được cấu hình riêng.
!!!

Hợp nhất tất cả tin nhắn system cho đến tin nhắn đầu tiên với vai trò không phải system (User/Assistant) và gửi chúng như một trường system instruction riêng biệt.

## Reasoning Settings

Nếu Chat Completion model sử dụng reasoning, các cài đặt này ảnh hưởng đến khả năng hiển thị và chức năng của nó.

### Request model reasoning

Xem [Adding Reasoning: By Backend](/Usage/Prompts/reasoning.md#by-backend).

### Reasoning Effort

Xem [Reasoning Effort](/Usage/Prompts/reasoning.md#reasoning-effort).

## "Prompts"

Prompt Manager tạo thành xương sống của prompt được gửi đến Chat Completion model. Nó kiểm soát những gì được gửi cũng như *thứ tự* mà nó được gửi.

### Dropdown 'Prompts'

Chứa một danh sách dropdown của tất cả các prompts (không mặc định) mà Chat Completion preset hiện tại bao gồm. Để một trong các prompts này được thêm vào tin nhắn đi, nó cần được chọn từ danh sách dropdown và sau đó được thêm vào Prompt Manager bằng cách nhấn nút **Insert prompt**. Để tạo một prompt mới để thêm vào danh sách dropdown này, nhấn nút **New prompt**. Sau khi prompt mới được viết và lưu, nó được thêm vào dropdown và sau đó có thể được chèn.

### Danh sách Prompts

Đây là một giao diện kéo và thả liệt kê các prompts được chọn để có thể được gửi đến Chat Completion model. Các prompts được đặt gần **đầu** của giao diện được gửi sớm hơn. **Cuối** danh sách là **điều cuối cùng** được gửi đến model (thường, đây sẽ là **Post-History Instructions** của bạn).

!!! Prompts 'Pinned' = Prompts mặc định
Các prompts mặc định không thể bị loại bỏ khỏi danh sách các prompts đã chọn. Điều này bao gồm Main Prompt, World Info (before/after), Persona Description, Character Description, Character Personality, Scenario, Enhance Definitions, Auxiliary Prompt, Chat Examples, Chat History, và Post-History Instructions. Nếu những điều này không được mong muốn, chúng có thể được **chuyển đổi 'OFF'**, nhưng không bị loại bỏ hoặc xóa hoàn toàn.
!!!

## Chỉnh sửa một Prompt

Nhấp vào **nút bút chì** trên một prompt sẽ đưa bạn đến **giao diện Edit**. Tại đây, bạn có thể chỉnh sửa prompt trực tiếp.

!!! Hãy chắc chắn lưu các thay đổi của bạn!
Để lưu vĩnh viễn các thay đổi cho các prompts này trong Chat Completion preset của bạn, bạn phải nhấp vào nút **Save** ở góc dưới bên phải của **giao diện Edit**, cũng như lưu preset chính nó bằng cách sử dụng nút **Save** nằm ở đầu phần **AI Response Configuration**! Nếu không, các thay đổi được thực hiện sẽ bị mất khi Chat Completion preset được chuyển sang một preset khác.
!!!

### Name

Tên của prompt. Điều này không được gửi đến Chat Completion model; nó chỉ để bạn tham khảo trong Prompt Manager.

### Role

Vai trò nào gửi prompt. Bạn có thể chọn giữa System, AI Assistant, hoặc User.

### Triggers

Các loại generation mà prompt này được gửi. Nếu không có gì được chọn, prompt sẽ được gửi cho tất cả các loại generation. Nếu một hoặc nhiều được chọn, prompt sẽ chỉ được gửi cho các loại generation cụ thể đó:

- **Normal:** Yêu cầu generation tin nhắn thông thường.
- **Continue:** Khi nút Continue được nhấn.
- **Impersonate:** Khi nút Impersonate được nhấn.
- **Swipe:** Khi generation được kích hoạt bằng cách swipe.
- **Regenerate:** Khi nút Regenerate được nhấn trong solo chats.
- **Quiet:** Yêu cầu generation nền, thường được kích hoạt bởi [extensions](/extensions/index.md) hoặc lệnh [STscript](/For_Contributors/st-script.md).

!!!
Trigger "Regenerate" không có sẵn trong group chats vì nó sử dụng logic regeneration khác: tất cả tin nhắn từ reply cuối cùng bị xóa, và tin nhắn được xếp hàng sử dụng loại generation "Normal" theo [chiến lược Group reply](/Usage/Characters/groupchats.md#reply-order-strategies) đã chọn.
!!!

### Position

Khi Position được đặt thành **Relative**, prompt này được gửi ở vị trí nó nằm trong giao diện kéo và thả với tất cả các prompts khác. Khi nó được đặt thành **In-Chat** và được đưa ra một **Depth**, thay vào đó nó được gửi **trong Chat History** với Role đã chọn, và **bỏ qua** thứ tự của giao diện kéo và thả.

### Depth

Khi Position được đặt thành **In-Chat**, điều này định nghĩa độ sâu prompt được gửi trong lịch sử chat. Số càng cao, nó càng được gửi sâu hơn. Ví dụ, Depth là 0 sẽ được gửi sau tin nhắn chat cuối cùng, Depth là 1 sẽ được gửi trước tin nhắn chat cuối cùng, và Depth là 2 sẽ được gửi trước tin nhắn chat thứ hai từ cuối, v.v.

### Order

!!!
Các prompts có cùng Role và Depth sẽ được nhóm lại với nhau và được sắp xếp theo giá trị Order của chúng.
Thứ tự như sau (từ trên xuống dưới): User, AI Assistant, System.
!!!

Khi Position được đặt thành **In-Chat**, điều này định nghĩa thứ tự mà prompt được gửi trong lịch sử chat. Số càng thấp, nó càng được gửi sớm hơn.

## Xây dựng Prompt của bạn: Mẹo và Thủ thuật

Truy cập phần [xây dựng prompt](index.md) của tài liệu SillyTavern để biết thêm thông tin về cách viết prompts hiệu quả. Thông tin phần lớn có thể được áp dụng cho các Chat Completion presets.
