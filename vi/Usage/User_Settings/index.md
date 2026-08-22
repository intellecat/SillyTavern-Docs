---
order: 120
icon: gear
route: /vi/usage/user-settings/
---

# Cài đặt Người dùng


:::callout
**[Tùy chỉnh Giao diện](uicustomization.md)**

Thay đổi chủ đề, giao diện và cảm nhận của giao diện chat theo sở thích của bạn.
:::



:::callout
**[Chế độ Visual Novel](Visual-Novel.md)**

Trò chuyện với các nhân vật có sprite, giống như trong các visual novel như Doki Doki Literature Club và các trò chơi VN nổi tiếng khác.
:::


## Cài đặt Chung

Đây là các cài đặt cốt lõi ảnh hưởng đến trải nghiệm tổng thể của SillyTavern.

### Ngôn ngữ Giao diện

Giao diện người dùng của SillyTavern có sẵn bằng nhiều ngôn ngữ. Bộ chọn ngôn ngữ cung cấp các tùy chọn sau:
* **Default**: Sử dụng ngôn ngữ hệ thống nếu có sẵn
* **English**: Bắt buộc giao diện tiếng Anh bất kể cài đặt hệ thống
* Các ngôn ngữ khác có sẵn thông qua menu thả xuống

Lưu ý: Cài đặt này chỉ ảnh hưởng đến văn bản giao diện người dùng. Để dịch cuộc trò chuyện AI, vui lòng sử dụng tiện ích mở rộng [Chat Translation](../../extensions/Translation.md).

### Phiên bản Phần mềm

Phiên bản hiện tại của SillyTavern được hiển thị ở góc trên bên phải. Thông tin này rất quan trọng cho:
* Khắc phục sự cố
* Đảm bảo tương thích với các tiện ích mở rộng
* Xác định xem có bản cập nhật nào không

Để cập nhật SillyTavern lên phiên bản mới nhất, vui lòng tham khảo tài liệu [Updating](/Installation/Updating).

### Quản lý Tài khoản

Kiểm soát tài khoản người dùng SillyTavern của bạn, sao lưu cài đặt và dữ liệu người dùng, và quản lý vai trò và quyền người dùng trong [chế độ đa người dùng](/Administration/multi-user.md).

#### <i class="fa-fw fa-solid fa-user-shield"></i> Tài khoản

Trong hộp thoại Tài khoản, bạn có thể xem và chỉnh sửa thông tin hồ sơ, thay đổi mật khẩu và quản lý cài đặt tài khoản.

**Thông tin Hồ sơ**

* Tên hiển thị (có thể chỉnh sửa qua biểu tượng bút chì)
* Ảnh đại diện người dùng (cũng có thể thay đổi bằng [Personas](/Usage/personas.md))
* Tên tài khoản
* Vai trò người dùng
* Ngày tạo tài khoản
* Trạng thái mật khẩu (biểu tượng khóa/mở khóa cho biết bảo vệ)

**Hành động Tài khoản**

* **Settings Snapshots**: Tạo, quản lý và khôi phục bản sao lưu cài đặt người dùng của bạn
* **Download Backup**: Xuất bản sao lưu hoàn chỉnh của tất cả dữ liệu người dùng
* **Change Password**: Cập nhật thông tin bảo mật tài khoản

**Vùng Nguy hiểm**

Các thao tác tài khoản quan trọng cần được sử dụng cẩn thận:
* **Reset Settings**: Khôi phục tất cả cài đặt về mặc định
* **Reset Everything**: Xóa hoàn toàn tài khoản và khôi phục cài đặt gốc

#### <i class="fa-fw fa-solid fa-user-tie"></i> Bảng điều khiển Admin

!!! Áp dụng cho: [chế độ đa người dùng](/Administration/multi-user.md)

Các tính năng đa tài khoản yêu cầu `enableUserAccounts` được đặt thành true trong config.yaml.
!!!

Chọn **Manage Users** để xem và quản lý các tài khoản người dùng hiện có.

##### Hồ sơ Người dùng

- Quản lý ảnh đại diện tùy chỉnh (tải lên/xóa)
- Tên hiển thị và tên tài khoản
- Thông tin vai trò và trạng thái
- Ngày tạo tài khoản
- Trạng thái bảo vệ mật khẩu

##### Điều khiển Tài khoản

- <i class="fa-fw fa-solid fa-pencil"></i> Chỉnh sửa tên hiển thị
- <i class="fa-fw fa-solid fa-check"></i> Bật tài khoản
- <i class="fa-fw fa-solid fa-ban"></i> Tắt tài khoản
- <i class="fa-fw fa-solid fa-arrow-up"></i> Thăng cấp lên admin
- <i class="fa-fw fa-solid fa-arrow-down"></i> Hạ cấp xuống người dùng thông thường

##### Hành động Quản lý

- <i class="fa-fw fa-solid fa-download"></i> Tải xuống bản sao lưu dữ liệu người dùng
- <i class="fa-fw fa-solid fa-key"></i> Thay đổi mật khẩu người dùng
- <i class="fa-fw fa-solid fa-trash"></i> Xóa tài khoản

##### Người dùng Mới

Chọn **New User** để tạo tài khoản người dùng mới.

* Tên Hiển thị* (ví dụ: "John Snow")
* Tên Tài khoản* (chỉ chữ cái thường, số và dấu gạch ngang)
* Mật khẩu (tùy chọn)
* Xác nhận Mật khẩu

Tạo người dùng mới sẽ tự động tạo một thư mục con trong thư mục /data/ sử dụng tên tài khoản làm tên thư mục.

#### <i class="fa-fw fa-solid fa-right-from-bracket"></i> Đăng xuất

!!! Áp dụng cho: [chế độ đa người dùng](/Administration/multi-user.md)
!!!

Đăng xuất khỏi phiên hiện tại.

### Tìm kiếm Cài đặt

Thanh tìm kiếm tiện lợi giúp bạn nhanh chóng tìm các cài đặt cụ thể:
* Nhập bất kỳ từ khóa nào để lọc và làm nổi bật cài đặt ở bất kỳ đâu trong User Settings
* Tìm kiếm qua tên và mô tả cài đặt
* Giúp điều hướng các cài đặt phức tạp hiệu quả hơn

## Chủ đề Giao diện

Thay đổi giao diện của giao diện chat theo sở thích của bạn.

Để biết thêm thông tin về các cài đặt trong phần này của <i class="fa-fw fa-solid fa-user-gear" title="User Settings icon"></i> **User Settings**, xem [Tùy chỉnh Giao diện](uicustomization.md#ui-theme).

## Xử lý Nhân vật

* **Char List Subheader**: Chọn thông tin bổ sung nào để hiển thị dưới tên nhân vật trong danh sách [<i class="fa-fw fa-solid fa-address-card" title="Characters icon"></i> Characters](/Usage/Characters/characterdesign.md):
    - Character Version
    - Created by
* **Import Card Tags**: Kiểm soát cách xử lý thẻ khi nhập thẻ nhân vật:
    - Ask - Hiển thị hộp thoại cho mỗi lần nhập
    - None - Không nhập thẻ nào
    - All - Nhập tất cả thẻ
    - Existing - Chỉ nhập các thẻ đã tồn tại
* **Advanced Character Search**: Khi được bật, sử dụng tìm kiếm mờ và tìm kiếm tất cả các trường dữ liệu nhân vật, không chỉ tên.
* **Prefer Char. Prompt**: Nếu được bật, sử dụng System Prompt ghi đè từ thẻ nhân vật khi có sẵn.
* **Prefer Char. Instructions**: Nếu được bật, sử dụng Post-History Instructions ghi đè từ thẻ nhân vật khi có sẵn.
* **Never resize avatars**: Ngăn cắt/thay đổi kích thước ảnh nhân vật đã nhập. Khi tắt, ảnh sẽ được thay đổi kích thước thành 512x768.
* **Show avatar filenames**: Hiển thị tên tệp thực tế của ảnh đại diện nhân vật trong danh sách nhân vật.
* **Spoiler Free Mode**: Ẩn định nghĩa nhân vật sau nút spoiler trong bảng chỉnh sửa.

## Linh tinh

* **Reload Chat**: Tải lại và vẽ lại cuộc trò chuyện hiện tại.
* **[Debug Menu](#debug-menu)**: Truy cập các tùy chọn gỡ lỗi.
* **Smooth Streaming**: Làm mượt mà việc tạo văn bản stream bằng cách hiển thị văn bản từng chữ cái một. Bao gồm thanh trượt điều khiển tốc độ. Để loại trừ các reasoning block khỏi smooth streaming, bật tùy chọn "Exclude 'Thinking...'".
* **Stream Fade-In**: Áp dụng hiệu ứng fade-in cho văn bản stream. Có thể sử dụng với hoặc không có Smooth Streaming.
* **[Message Sound](uicustomization.md#message-sound)**: Phát âm thanh khi hoàn thành tạo tin nhắn.
    - **Background Sound Only**: Chỉ phát âm thanh khi tab trình duyệt không được focus.
* **Relaxed API URLs**: Giảm yêu cầu định dạng cho URL API.
* **Lorebook Import Dialog**: Hiển thị hộp thoại nhập cho World Info/Lorebook khi nhập nhân vật có lore nhúng.
* **Auto-select Input Text**: Tự động chọn văn bản trong các trường nhập nhất định khi được nhấp.
* **Markdown Hotkeys**: Bật phím tắt cho định dạng markdown.
* **Restore User Input**: Bảo tồn đầu vào người dùng chưa lưu khi trang được làm mới.
* **MovingUI**: Cho phép sắp xếp lại các phần tử giao diện bằng cách kéo (chỉ PC).
    - Nút <i class="fa-solid fa-recycle" title="Reset icon"></i> **Reset** để khôi phục vị trí mặc định
    - Hệ thống preset để lưu/tải bố cục giao diện

## Xử lý Chat/Tin nhắn

### Cài đặt Hiển thị Tin nhắn

Kiểm soát cách tin nhắn được tải và hiển thị trong giao diện chat. Các cài đặt này ảnh hưởng đến trải nghiệm chat tổng thể và hiệu suất.
* **# Messages to Load**: Số lượng tin nhắn lịch sử chat để tải trước khi phân trang (0 = Tất cả)
* **Streaming FPS**: Tốc độ cập nhật văn bản stream (5-100 FPS)
* **Example Messages Behavior**:
    - Gradual push-out
    - Always include examples
    - Never include examples
* **Image Swipe Behavior** (kiểm soát hành động vuốt cho hình ảnh theo kiểu gallery):
    - Generate new: Cho phép tạo hình ảnh mới bằng tiện ích mở rộng [Image Generation](/extensions/Stable-Diffusion.md)
    - Roll over: Xoay vòng qua các hình ảnh hiện có, quay lại từ đầu khi đến cuối

### Điều khiển Đầu vào & Phản hồi

Các cài đặt xác định cách gửi tin nhắn và cách AI tiếp tục phản hồi.
* **Enter to Send**: Chọn giữa Disabled, Automatic (PC), hoặc Enabled
* **"Send" to Continue**: Sử dụng nút Send để tiếp tục phản hồi AI
* **Quick "Continue" button**: Hiển thị nút để mở rộng tin nhắn cuối cùng của AI
* **Quick "Impersonate" button**: Hiển thị nút để giả mạo nhân vật trong một tin nhắn
* **Swipes**: Hiển thị nút mũi tên cho các phản hồi thay thế của AI (PC và di động)
* **Gestures**: Bật cử chỉ vuốt cho việc tạo (chỉ Di động)

### Quản lý Tự động

Các tính năng tự động giúp quản lý luồng chat và nội dung.
* **Auto-load Last Chat**: Tự động tải cuộc trò chuyện gần nhất khi khởi động
* **Auto-scroll Chat**: Tự động cuộn đến tin nhắn mới nhất
* **Auto-save Message Edits**: Lưu chỉnh sửa tin nhắn mà không cần xác nhận
* **Confirm message deletion**: Nhắc nhở trước khi xóa tin nhắn
* **Auto-fix Markdown**: Tự động sửa định dạng markdown

#### Auto-swipe

Tự động từ chối và tạo lại tin nhắn AI dựa trên tiêu chí có thể cấu hình.
* **Enable Auto-swipe**: Công tắc chính cho chức năng auto-swipe
* **Minimum generated message length**: Kích hoạt auto-swipe nếu tin nhắn ngắn hơn giá trị này
* **Blacklisted words**: Danh sách các từ có thể kích hoạt auto-swipe, phân tách bằng dấu phẩy
* **Blacklisted word count to swipe**: Số lượng từ trong danh sách đen tối thiểu phải được phát hiện để kích hoạt auto-swipe

#### Auto-Continue

Tự động tiếp tục phản hồi nếu mô hình dừng trước khi đạt đến độ dài nhất định.

Điều này cho phép AI của bạn viết phản hồi dài trong nhiều phần, để bạn có thể có [cài đặt độ dài phản hồi](/Usage/Common-Settings.md#response-tokens) ngắn trong khi vẫn nhận được phản hồi dài.

Nó sẽ không làm cho AI viết nhiều hơn mức nó có thể viết. Yêu cầu AI tiếp tục tin nhắn mà nó coi là "đã hoàn thành" thường không hiệu quả. Xem [Làm thế nào để AI viết nhiều hơn?](/Usage/faq.md#how-to-make-the-ai-write-more) để biết thêm ý tưởng.

* **Enable Auto-continue**: Công tắc chính cho tính năng tự động tiếp tục
* **Allow for Chat Completion APIs**: Bật chức năng auto-continue cho các endpoint Chat Completion API
* **Target length (tokens)**: Độ dài tin nhắn mong muốn theo token - sẽ kích hoạt tiếp tục nếu tin nhắn ngắn hơn giá trị này (0-1024)

### Định dạng & Hiển thị Tin nhắn

Kiểm soát cách tin nhắn được định dạng và nội dung nào được hiển thị.
* **Forbid External Media**: Chặn phương tiện nhúng từ các domain bên ngoài
* **Show {\{char}}: in responses**: Giữ tiền tố tên nhân vật trong phản hồi nếu được tạo
* **Show {\{user}}: in responses**: Giữ tiền tố tên người dùng trong phản hồi nếu được tạo
* **Experimental Macro Engine**: Bật xử lý macro nâng cao có hỗ trợ lồng nhau
* **Show tags in responses**: Cho phép (một số) thẻ HTML trong phản hồi được hiển thị dưới dạng HTML
* **Relax message trim in Groups**: Cho phép AI nói thay cho các nhân vật khác trong chat nhóm, thay vì dừng tạo phản hồi
* **Show group chat queue**: Hiển thị thứ tự phản hồi trong danh sách nhân vật cho chat nhóm
* **Pin greeting message styles**: Luôn hiển thị thẻ style từ lời chào, ngay cả khi tin nhắn bị dỡ tải do lazy loading.

### Kiểm tra Prompt và Gỡ lỗi

* **Log prompts to console**: Xuất prompt ra console trình duyệt
* **Request token probabilities**: Yêu cầu xác suất token cho phản hồi AI từ API. Khi có sẵn, những thông tin này có thể được xem trong <i class="fa-solid fa-bars" title="Burger Menu icon"></i> [Token Probabilities](../../Usage/Chatting/index.md#token-probabilities-panel).

### AutoComplete

- Auto-hide details
- Matching style (Starts with/Includes/Fuzzy)
- Visual style (Theme/Dark/Light)
- Keyboard selection options
- Font scaling
- Width controls

## Cài đặt STscript

Các tùy chọn cấu hình cho [trình phân tích cú pháp STscript](/For_Contributors/st-script.md#parser-flags).

### STRICT_ESCAPING

* Ký tự pipe không cần được escape trong giá trị trích dẫn.
* Dấu gạch chéo ngược phía trước ký hiệu có thể được escape để cung cấp dấu gạch chéo ngược theo nghĩa đen theo sau là ký hiệu chức năng.

Xem [Strict Escaping](/For_Contributors/st-script.md#strict-escaping) để biết thêm thông tin.

### REPLACE_GETVAR

Giúp tránh thay thế kép khi giá trị biến chứa văn bản có thể được hiểu là macro.

Xem [Replace Variable Macros](/For_Contributors/st-script.md#replace-variable-macros) để biết thêm thông tin.

## Menu Dọn dẹp

Menu Dọn dẹp cung cấp công cụ bảo trì dữ liệu giúp bạn xác định và xóa các tệp không cần thiết khỏi cài đặt SillyTavern của bạn. Tính năng này giúp giữ cho thư mục dữ liệu của bạn được tổ chức và có thể giải phóng không gian đĩa đáng kể.

!!! warning "Cảnh báo Quan trọng"
Công cụ Dọn dẹp sẽ xóa vĩnh viễn các tệp. **Hành động này không thể hoàn tác!**

Các tệp tải lên thủ công vào thư mục `/data/user/files/` và `/data/user/images/` sẽ bị xóa nếu chúng không được liên kết với tin nhắn chat hoặc mục Data Bank.

Nếu không chắc chắn, hãy sao lưu dữ liệu của bạn trước khi sử dụng menu Dọn dẹp.
!!!

### Cách sử dụng Dọn dẹp

1. Nhấp vào nút **Clean-Up** dưới phần **Miscellaneous**
2. Nhấp **Scan** để phân tích cài đặt của bạn. Điều này có thể mất một chút thời gian tùy thuộc vào kích thước thư mục dữ liệu của bạn
3. Xem lại các danh mục tệp tìm thấy
4. Sử dụng **View** để xem trước nội dung tệp trước khi xóa
5. Sử dụng **Download** để lưu tệp trước khi xóa
6. Xóa từng tệp hoặc toàn bộ danh mục khi cần

### Các Danh mục Dọn dẹp

Công cụ Dọn dẹp quét các tệp rời vào các danh mục sau:

#### Files

* **Tìm thấy gì**: Các tệp không được liên kết với tin nhắn chat hoặc mục Data Bank
* **Vị trí**: `/data/<user-handle>/user/files/`
* **Rủi ro**: ⚠️ **SẼ XÓA CÁC TẢI LÊN THỦ CÔNG** không được tham chiếu trong chat
* **Khi nào nên dọn dẹp**: An toàn để xóa nếu bạn không cần các tệp không được tham chiếu

#### Images

* **Tìm thấy gì**: Hình ảnh không được liên kết với tin nhắn chat
* **Vị trí**: `/data/<user-handle>/user/images/`
* **Rủi ro**: ⚠️ **SẼ XÓA CÁC TẢI LÊN THỦ CÔNG** không được tham chiếu trong chat
* **Khi nào nên dọn dẹp**: An toàn để xóa nếu bạn không cần các hình ảnh không được tham chiếu

#### Chats

* **Tìm thấy gì**: Các tệp chat liên kết với nhân vật đã xóa
* **Vị trí**: `data/<user-handle>/chats/`
* **Rủi ro**: ⚠️ **Chat mồ côi sẽ bị mất vĩnh viễn**
* **Khi nào nên dọn dẹp**: An toàn để xóa nếu bạn đã cố ý xóa nhân vật và không còn cần lịch sử chat của họ

#### Group Chats

* **Tìm thấy gì**: Các tệp chat liên kết với nhóm đã xóa
* **Vị trí**: `data/<user-handle>/group chats/`
* **Rủi ro**: ⚠️ **Chat nhóm mồ côi sẽ bị mất vĩnh viễn**
* **Khi nào nên dọn dẹp**: An toàn để xóa nếu bạn đã cố ý xóa nhóm và không còn cần lịch sử chat của họ

#### Avatar Thumbnails

* **Tìm thấy gì**: Hình thu nhỏ cho ảnh đại diện của nhân vật bị thiếu hoặc đã xóa
* **Vị trí**: `data/<user-handle>/thumbnails/avatar`
* **Rủi ro**: ✅ **An toàn để xóa** - hình thu nhỏ sẽ tự động tạo lại khi cần
* **Khi nào nên dọn dẹp**: Luôn an toàn để dọn dẹp, giúp giải phóng không gian

#### Background Thumbnails

* **Tìm thấy gì**: Hình thu nhỏ cho nền bị thiếu hoặc đã xóa
* **Vị trí**: `data/<user-handle>/thumbnails/bg`
* **Rủi ro**: ✅ **An toàn để xóa** - hình thu nhỏ sẽ tự động tạo lại khi cần
* **Khi nào nên dọn dẹp**: Luôn an toàn để dọn dẹp, giúp giải phóng không gian

#### Chat Backups

* **Tìm thấy gì**: Bản sao lưu chat được tạo tự động
* **Vị trí**: `data/<user-handle>/backups/chat_*`
* **Rủi ro**: ⚠️ **Các tệp sao lưu sẽ bị mất vĩnh viễn**
* **Khi nào nên dọn dẹp**: Cân nhắc giữ các bản sao lưu gần đây, nhưng các bản cũ hơn có thể xóa an toàn

#### Settings Backups

* **Tìm thấy gì**: Bản sao lưu cài đặt được tạo tự động
* **Vị trí**: `data/<user-handle>/backups/settings_*`
* **Rủi ro**: ⚠️ **Các tệp sao lưu cài đặt sẽ bị mất vĩnh viễn**
* **Khi nào nên dọn dẹp**: Cân nhắc giữ các bản sao lưu gần đây, nhưng các bản cũ hơn có thể xóa an toàn

## Menu Debug

!!!warning Các chức năng này chỉ dành cho người dùng nâng cao.

Không sử dụng chúng trừ khi bạn hiểu đầy đủ hậu quả của chúng.
!!!

Menu Debug cung cấp chức năng cho mục đích khắc phục sự cố, bảo trì và phát triển. Các chức năng này nên được sử dụng cẩn thận vì chúng có thể ảnh hưởng đáng kể đến cài đặt SillyTavern của bạn.

Vì các tiện ích mở rộng có thể thêm các chức năng debug, các tùy chọn có sẵn sẽ khác nhau tùy thuộc vào các tiện ích mở rộng bạn đã cài đặt.

### Chức năng Dịch & Locale
* **Get missing translations**: Phân tích locale hiện tại (hoặc tất cả locale nếu tiếng Anh được chọn) để tìm bản dịch bị thiếu và xuất kết quả ra console trình duyệt
* **Apply locale**: Buộc làm mới cài đặt ngôn ngữ hiện tại bằng cách áp dụng lại locale đã chọn
### Quản lý Cache & Bộ nhớ
* **Clear WebSearch cache**: Xóa tất cả kết quả tìm kiếm đã lưu từ cache cục bộ
* **Purge all vector indices**: Xóa hoàn toàn tất cả vector đã lưu trữ trên tất cả các nguồn
* **Reset token cache**: Xóa số lượng token đã lưu, buộc tokenize lại hoàn toàn tất cả chat
* **Delete itemized prompts**: Xóa tất cả prompt đã phân mục từ bộ nhớ cục bộ
### Dữ liệu & Thống kê
* **Refresh Stat File**: Xây dựng lại tệp thống kê bằng dữ liệu chat hiện có
* **Backfill token counters**: Tính toán lại số lượng token cho tất cả tin nhắn trong chat hiện tại
    - Hữu ích khi chuyển đổi giữa các mô hình có tokenizer khác nhau
    - Kích hoạt tải lại chat sau khi hoàn thành
    - Chỉ thay đổi trực quan, không sửa đổi nội dung chat
### Kiểm thử API & Tiện ích
* **Change Mancer base URL**: Sửa đổi URL cơ sở cho máy chủ Mancer API
* **Test WebSearch extension**: Thực hiện tìm kiếm thử nghiệm bằng cài đặt hiện tại
* **Send a generation request**: Kiểm tra tạo văn bản bằng API đã chọn hiện tại
### Công cụ Hệ thống & Debug
* **Force onboarding**: Khởi động lại quy trình onboarding
* **Toggle event tracing**: Bật/tắt theo dõi sự kiện để gỡ lỗi
* **Copy ST setup**: [Đang phát triển] Sao chép dữ liệu cấu hình hệ thống vào clipboard cho báo cáo lỗi

Mỗi chức năng có thể được thực thi bằng nút "Execute" bên dưới mô tả của nó. Cân nhắc sao lưu dữ liệu của bạn trước khi sử dụng các công cụ này, vì một số thao tác không thể hoàn tác.
