---
order: 100
icon: person-fill
route: /vi/usage/characters/
---

# Nhân vật

Nhân vật là các danh tính AI mà bạn có thể tạo và quản lý để định hình vai trò của AI trong cuộc trò chuyện. Mỗi nhân vật có tên, tính cách và lịch sử trò chuyện. Bạn có thể tạo bao nhiêu nhân vật tùy thích và chuyển đổi giữa chúng bất cứ lúc nào.

Nhân vật có thể được sử dụng trong các cuộc chat solo hoặc thêm nhiều nhân vật vào một group chat để cho họ tương tác với nhau.

## Bảng quản lý nhân vật

Mở bảng <i class="fa-solid fa-address-card"></i> **Characters** từ navbar để truy cập danh sách nhân vật. Nhấp vào một nhân vật hoặc group để chat với họ hoặc chỉnh sửa họ, hoặc chọn <i class="fa-solid fa-user-plus"></i> **Create New Character** để thêm một nhân vật mới.

### Điều khiển bảng

* <i class="fa-solid fa-lock"></i> **Pin Panel**: Giữ bảng mở trong khi tương tác
* <i class="fa-solid fa-list-ul"></i> **Character List**: Quay lại chế độ xem danh sách nhân vật
* **HotSwap Bar**: Truy cập nhanh vào các nhân vật yêu thích

### Danh sách nhân vật

* <i class="fa-solid fa-user-plus"></i> **Create New Character**: Thêm một nhân vật mới
* <i class="fa-solid fa-file-import"></i> **Import Character**: Tải nhân vật từ tệp
* <i class="fa-solid fa-cloud-arrow-down"></i> **External Import**: Nhập từ URL
* <i class="fa-solid fa-users-gear"></i> **Create Group**: Bắt đầu một group chat mới

#### Tìm kiếm và sắp xếp

* **Search Bar**: Lọc nhân vật theo tên hoặc thuộc tính
* **Sort Dropdown**: Nhiều tùy chọn sắp xếp:
    - Theo bảng chữ cái (A-Z, Z-A)
    - Theo thời gian (Mới nhất, Cũ nhất)
    - Dựa trên sử dụng (Gần đây, Nhiều/Ít chat nhất)
    - Dựa trên kích thước (Nhiều/Ít token nhất)
    - Đặc biệt (Yêu thích, Ngẫu nhiên)

#### Lọc nhân vật theo loại hoặc tag

* <i class="fa-solid fa-star"></i> **Favorites Filter**: Hiển thị các nhân vật yêu thích
* <i class="fa-solid fa-users"></i> **Groups Filter**: Chỉ hiển thị group chats
* <i class="fa-solid fa-folder-plus"></i> **Tags as Folders**: Tổ chức theo thứ bậc tag
* <i class="fa-solid fa-gear"></i> **Manage Tags**: [Cấu hình tag](/Usage/Characters/Tags.md)
* <i class="fa-solid fa-tags"></i> **Tag List**: Xem tất cả các tag có sẵn
* <i class="fa-solid fa-filter-circle-xmark"></i> **Clear Filters**: Đặt lại tất cả các bộ lọc

### Bảng tạo/chỉnh sửa nhân vật

* **Avatar Image**: Tải lên và xem trước ảnh hồ sơ nhân vật
* **Token Count**: [Sử dụng token](characterdesign.md#character-tokens) cho nhân vật
* <i class="fa-solid fa-ranking-star"></i> **Stats**: Lịch sử chat và thống kê sử dụng
* [Quản lý tag](/Usage/Characters/Tags.md)

#### Hành động nhanh

- <i class="fa-solid fa-star"></i> Chuyển đổi yêu thích
- <i class="fa-solid fa-book"></i> Định nghĩa nâng cao
- <i class="fa-solid fa-globe"></i> [Character lore](/Usage/worldinfo.md#character-lore)
- <i class="fa-solid fa-passport"></i> [Chat lore](/Usage/worldinfo.md#chat-lorebook): liên kết chat với World Info
- <i class="fa-solid fa-file-export"></i> Xuất nhân vật
- <i class="fa-solid fa-clone"></i> Nhân bản
- <i class="fa-solid fa-skull"></i> Xóa

#### Tùy chọn mở rộng

* Liên kết World Info
* Nhập card lore
* Ghi đè scenario
* Chuyển đổi persona
* Đổi tên nhân vật
* Liên kết nguồn
* Thay thế/Cập nhật
* Nhập tag
* Xem gallery

#### Các trường nội dung

* **[Character Description](characterdesign.md#character-description)**: Tóm tắt ngắn gọn về nhân vật
* **[First Message](characterdesign.md#first-message)**: Lời chào hoặc prompt ban đầu khi bắt đầu một chat mới
* **Alternative greetings**: Xác định nhiều first messages mà bạn có thể swipe giữa chúng khi bắt đầu chat

### Bảng định nghĩa nâng cao

Nhấp vào nút <i class="fa-solid fa-book"></i> **Advanced Definitions** để truy cập các cài đặt nhân vật mở rộng.

#### Ghi đè Prompt (Chat Completion/Instruct Mode)

* **Main Prompt**: Thay thế [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) mặc định, có thể sử dụng placeholder \{\{original\}\} để bao gồm prompt gốc
* **Post-History Instructions**: Ghi đè [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) mặc định

#### Metadata của người tạo

Thông tin không phải prompt về nhân vật:

- Tên/liên hệ người tạo
- Phiên bản nhân vật
- Ghi chú của người tạo
- Danh sách tags nhúng

#### Tính cách nhân vật

* **[Personality Summary](characterdesign.md#personality-summary)**: Tổng quan ngắn gọn về các đặc điểm của nhân vật
* **[Scenario](characterdesign.md#scenario)**: Bối cảnh và hoàn cảnh của cuộc đối thoại
* **Character's Note**: Tin nhắn tùy chỉnh với độ sâu và vai trò tin nhắn có thể chọn (xem thêm [Author's Note](/Usage/Characters/Author's-Note.md))
* **Talkativeness** (Group Chats): Thanh trượt cho Shy → Normal → Chatty
* **Example Messages**: Ví dụ về phong cách viết của nhân vật

### Quản lý Group Chat

Nếu đây là một group chat, bạn có thể quản lý các thành viên và cài đặt của group từ bảng này.

Xem [Group Chats](/Usage/Characters/groupchats.md) để biết thêm chi tiết.
