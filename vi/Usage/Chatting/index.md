---
icon: report
order: 170
expanded: false
route: /vi/usage/chatting/
---

# Trò chuyện

Khi bạn đã [kết nối với API](/Usage/API_Connections/index.md), hãy gửi tin nhắn đến AI bằng cách nhập vào thanh chat ở cuối màn hình. Sau đó nhấp vào <i class="fa-solid fa-paper-plane"></i> **Send** hoặc nhấn Enter.
![Chat bar](/static/chatbox.png)

AI sẽ phản hồi bằng một tin nhắn tiếp tục cuộc trò chuyện.

![Chat message](/static/chatmessage.png)

Bây giờ bạn có thể:

* **Gửi tin nhắn khác**
* **Vuốt phản hồi**: Nhấp vào nút <i class="fa-solid fa-chevron-right"></i> **Swipe** trên tin nhắn để tạo một phản hồi khác.
* **Chỉnh sửa tin nhắn**: Nhấp vào nút <i class="fa-solid fa-pencil"></i> **Edit** trên bất kỳ tin nhắn nào để [chỉnh sửa nội dung tin nhắn](#edit-message-content).
* **Hành động tin nhắn**: Nhấp vào nút <i class="fa-solid fa-ellipsis"></i> **Message actions** trên một tin nhắn để xem thêm [tùy chọn tin nhắn](#message-actions-panel) như [dịch thuật](../../extensions/Translation.md), tạo hình ảnh và phân nhánh câu chuyện.
* **Tùy chọn chat**: Nhấp vào nút <i class="fa-solid fa-bars"></i> **Options** bên cạnh thanh chat để xem thêm [tùy chọn chat](#chat-options-panel) như ghi chú của tác giả và quản lý tệp chat.

!!! Chỉnh sửa và vuốt
Nếu bạn muốn mình đã nói điều gì đó khác, bạn có thể chỉnh sửa tin nhắn của mình và sau đó vuốt phản hồi của AI để nhận một phản hồi mới.
!!!

!!! Phím tắt
Bạn cũng có thể sử dụng phím mũi tên **Right** để vuốt, và phím mũi tên **Up** để chỉnh sửa tin nhắn cuối cùng trong cuộc trò chuyện. Để biết thêm phím tắt, hãy sử dụng [lệnh slash](/Usage/Chatting/slashcommands.md) `/help hotkeys` trong chat hoặc xem trang [Phím tắt](/Usage/Chatting/hotkeys.md).
!!!

## Bảng hành động tin nhắn

Quản lý các tin nhắn chat riêng lẻ thông qua nút dấu ba chấm (•••) trên tin nhắn.

Để hiển thị các tùy chọn này cho tất cả tin nhắn trong cuộc trò chuyện của bạn, hãy bật cài đặt [Expand Message Actions](/Usage/User_Settings/uicustomization.md#theme-toggles) trong cài đặt người dùng của bạn.

### Chức năng cốt lõi

* <i class="fa-solid fa-language"></i> **Translate**: Chuyển đổi tin nhắn sang ngôn ngữ khác
* <i class="fa-solid fa-paintbrush"></i> **Generate Image**: [Tạo hình ảnh](/extensions/Stable-Diffusion.md) từ nội dung tin nhắn
* <i class="fa-solid fa-bullhorn"></i> **Narrate**: Chuyển đổi [văn bản thành giọng nói](/extensions/TTS.md)
* <i class="fa-solid fa-square-poll-horizontal"></i> **Prompt**: Xem prompt tạo và sử dụng token

### Hiển thị tin nhắn

* <i class="fa-solid fa-eye"></i> **Included**: AI nhìn thấy tin nhắn này; nhấp để loại trừ
* <i class="fa-solid fa-eye-slash"></i> **Excluded**: AI không nhìn thấy tin nhắn này; nhấp để bao gồm

### Quản lý nội dung

* <i class="fa-solid fa-paperclip"></i> **Embed**: [Đính kèm tệp hoặc hình ảnh](/Usage/Characters/data-bank.md#about-documents)
* <i class="fa-solid fa-flag-checkered"></i> **Checkpoint**: Tạo điểm lưu câu chuyện
* <i class="fa-solid fa-flag"></i> **Checkpoint Navigation**: Nhấp để mở chat checkpoint, Shift+Nhấp để cập nhật
  checkpoint hiện có
* <i class="fa-solid fa-code-branch"></i> **Branch**: Bắt đầu nhánh câu chuyện thay thế
* <i class="fa-solid fa-copy"></i> **Copy**: Sao chép văn bản tin nhắn
* <i class="fa-solid fa-pencil"></i> **Edit**: Chỉnh sửa nội dung tin nhắn

## Chỉnh sửa nội dung tin nhắn

Một bảng điều khiển nhỏ gọn các công cụ thao tác tin nhắn xuất hiện khi bạn <i class="fa-solid fa-pencil"></i> **Edit** một tin nhắn
chat.

### Hành động cốt lõi

* <i class="fa-solid fa-check"></i> **Confirm**: Lưu thay đổi tin nhắn
* <i class="fa-solid fa-xmark"></i> **Cancel**: Hủy thay đổi tin nhắn

### Thao tác tin nhắn

* <i class="fa-solid fa-copy"></i> **Copy**: Sao chép nội dung tin nhắn
* <i class="fa-solid fa-trash-can"></i> **Delete**: Xóa tin nhắn

### Vị trí tin nhắn

* <i class="fa-solid fa-chevron-up"></i> **Move Up**: Dịch chuyển tin nhắn lên trên trong chat
* <i class="fa-solid fa-chevron-down"></i> **Move Down**: Dịch chuyển tin nhắn xuống dưới trong chat

Lưu ý: Các điều khiển di chuyển có thể bị vô hiệu hóa dựa trên vị trí tin nhắn trong lịch sử chat.

## Bảng tùy chọn chat

Quản lý cài đặt và thao tác chat thông qua nút <i class="fa-solid fa-bars"></i> **Options** ở dưới bên trái của
giao diện chat.

### Điều khiển hiển thị

* <i class="fa-lg fa-solid fa-times"></i> **Close chat**: Thoát phiên chat hiện tại
* <i class="fa-lg fa-solid fa-cog"></i> **Toggle Panels**: Hiển thị/ẩn [bảng giao diện](/Usage/index.md#control-panels)

### Cài đặt tạo

* <i class="fa-lg fa-solid fa-note-sticky"></i> **[Author's Note](/Usage/Characters/Author's-Note.md)**: Hướng dẫn ngữ cảnh tùy chỉnh
* <i class="fa-lg fa-solid fa-scale-balanced"></i> **[CFG Scale](/Usage/Prompts/CFG.md)**: Điều chỉnh tính sáng tạo của phản hồi
* <i class="fa-lg fa-solid fa-pie-chart"></i> **[Token Probabilities](#token-probabilities-panel)**: Xem thống kê tạo token

### Điều hướng chat

* <i class="fa-lg fa-solid fa-left-long"></i> **Back to parent chat**: Quay lại cuộc trò chuyện chính
* <i class="fa-lg fa-solid fa-flag"></i> **Save checkpoint**: Tạo điểm lưu câu chuyện
* <i class="fa-lg fa-solid fa-people-arrows"></i> **Convert to group**: Chuyển đổi thành [chat nhóm](/Usage/Characters/groupchats.md)

### Quản lý chat

* <i class="fa-lg fa-solid fa-comments"></i> **Start new chat**: Bắt đầu cuộc trò chuyện mới
* <i class="fa-lg fa-solid fa-address-book"></i> **Manage chat files**: [Thao tác tệp chat](/Usage/Characters/chatfilemanagement.md) như nhập, xuất và đổi tên

### Điều khiển tin nhắn

* <i class="fa-lg fa-solid fa-trash-can"></i> **Delete messages**: Chọn và xóa nhiều tin nhắn
* <i class="fa-lg fa-solid fa-repeat"></i> **Regenerate**: Tạo phản hồi mới
* <i class="fa-lg fa-solid fa-user-secret"></i> **Impersonate**: AI viết tin nhắn thay cho người dùng
* <i class="fa-lg fa-solid fa-arrow-right"></i> **Continue**: Mở rộng tin nhắn cuối cùng

Lưu ý: Một số tùy chọn có thể bị ẩn tùy thuộc vào ngữ cảnh và trạng thái chat.

## Bảng xác suất Token

Bảng Token Probabilities cho phép bạn xem quy trình lấy mẫu của AI để tạo văn bản. Nó cho bạn thấy không chỉ những gì AI đã viết, mà còn những lựa chọn khác mà nó đã xem xét tại mỗi điểm trong văn bản.

Để mở nó, hãy nhấp vào nút <i class="fa-solid fa-pie-chart"></i> **Token Probabilities** trong bảng <i class="fa-solid fa-bars" title="Burger Menu icon"></i> **Chat Options**.

![Example message](/static/token-probs/fling-msg.png){ width=500}

![Token probabilities display for example message](/static/token-probs/fling-probs.png){ width=500}

Khi bạn nhấp vào bất kỳ token nào (từ, dấu câu hoặc ký tự định dạng) trong văn bản được tạo, bảng sẽ hiển thị các token thay thế mà AI đã xem xét tại vị trí đó, cùng với điểm xác suất của chúng. Điều này giúp bạn hiểu rõ hơn về "quy trình suy nghĩ" của AI và cho thấy các hướng khác mà phản hồi có thể đi theo. Xem xét các lựa chọn thay thế này có thể giúp bạn hiểu liệu có nhiều tùy chọn khả thi hay chỉ có một lựa chọn rõ ràng.

![Alternative tokens and probabilities](/static/token-probs/fling-probs-logprob.png){ width=500}

Nếu bạn thấy một token mà bạn nghĩ AI nên chọn khác đi, hãy chọn một lựa chọn thay thế và tin nhắn sẽ được tạo lại từ điểm đó trở đi, có khả năng mang lại cho bạn một phản hồi khác.

### Tạo lại

Nếu bạn thay đổi một token cụ thể và tạo lại phản hồi, phần của phản hồi mới trước token đã thay đổi sẽ giống với phản hồi gốc. Phần này được hiển thị màu xám. Vì nó không được tạo, nên không có thông tin xác suất cho phần này.

Bạn có thể muốn xem các phản hồi khác có thể được tạo dựa trên token thay thế của bạn.

Bạn có thể nhấp vào phần màu xám để "tạo lại" việc tạo, mang lại cho bạn một biến thể mới của văn bản. Nhấp vào bất kỳ phần nào của phần màu xám sẽ giữ toàn bộ phần màu xám và tạo lại toàn bộ phần trắng/có màu.

Giữ Ctrl trong khi nhấp vào một token trong phần màu xám sẽ giữ phần màu xám cho đến token đã nhấp và tạo lại phần còn lại của văn bản. Lựa chọn token thay thế của bạn không thể được giữ trong trường hợp này.

### Điều khiển

**Hiển thị Token**:

* Văn bản được tạo được chia thành các token riêng lẻ
* Mỗi token có thể tương tác, nhấp vào một token để xem các lựa chọn thay thế được AI xem xét
* Các token được tô màu như một công cụ hỗ trợ trực quan nhưng điều này không cho biết xác suất
* Các ký tự đặc biệt (khoảng trắng, xuống dòng) được đánh dấu rõ ràng

**Chọn Token**:

* Nhấp vào một token để xem các lựa chọn thay thế
* Nhấp vào một lựa chọn thay thế để thay thế token và tạo lại phản hồi
* Di chuột qua một token để xem điểm log-probability thô của nó

**Điều khiển cửa sổ**:

* <i class="fa-solid fa-grip"></i> Tay cầm kéo để định vị lại bảng (chỉ MovingUI)
* <i class="fa-solid fa-window-maximize"></i> Phóng to/khôi phục kích thước bảng
* <i class="fa-solid fa-circle-chevron-up"></i> Mở rộng/thu gọn nội dung bảng
* <i class="fa-solid fa-circle-xmark"></i> Đóng bảng

### Tính khả dụng

Bạn phải chọn **Request token probabilities** trong [User Settings](/Usage/User_Settings/index.md#chatmessage-handling) để bật tính năng này.

Xác suất token chỉ khả dụng cho tin nhắn gần nhất và không được lưu vào chat. Nếu thông tin xác suất token không còn khả dụng cho một tin nhắn, bảng sẽ hiển thị thông báo chỉ ra điều này.

Xác suất token không khả dụng khi sử dụng Smooth Streaming.

Xác suất token không khả dụng từ tất cả các API. Nếu bạn đang sử dụng API không hỗ trợ xác suất token, bảng sẽ mở nhưng sẽ không hiển thị bất kỳ thông tin nào.

#### Text Completion
* **LlamaCPP**: Khả dụng
* **Text Generation WebUI** (oobabooga): Khả dụng
* **TabbyAPI**: Khả dụng
* **NovelAI**: Khả dụng
* **KoboldCPP**: Khả dụng
* **Ollama**: Dường như không khả dụng
* **OpenRouter Text**: Dường như không khả dụng

#### Chat Completion
* **OpenAI** hoặc **Custom**: Khả dụng, nhưng tạo lại không được hỗ trợ
* **Anthropic**: Dường như không khả dụng
* **Google AI Studio**: Dường như không khả dụng
* **OpenRouter Chat**: Dường như không khả dụng
