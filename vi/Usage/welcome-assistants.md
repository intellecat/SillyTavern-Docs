---
tags: ['>=1.13.0']
icon: people
route: /vi/usage/welcome-assistants/
---

# Welcome Page Assistants

SillyTavern có màn hình chào mừng có thể chào đón bạn với một nhân vật "Assistant" được chỉ định. Màn hình này xuất hiện khi bạn khởi động SillyTavern mà không có cuộc trò chuyện đang hoạt động hoặc sau khi bạn đóng phiên trò chuyện cuối cùng.

!!! Note
Nếu bạn không thấy màn hình chào mừng khi khởi động ứng dụng, hãy đảm bảo tùy chọn "Auto-Load Last Chat" bị tắt trong phần "Chat/Message Handling" của bảng điều khiển **<i class="fa-solid fa-user-cog"></i> User Settings**. Nếu tùy chọn này được bật, SillyTavern sẽ tự động tải cuộc trò chuyện cuối cùng của bạn thay vì hiển thị màn hình chào mừng.
!!!

## Màn hình chào mừng

Khi không có cuộc trò chuyện nào đang hoạt động, màn hình chào mừng cung cấp một số yếu tố hữu ích:

* **Phiên bản SillyTavern:** Hiển thị logo ứng dụng và phiên bản hiện tại.
* **Liên kết nhanh:** Truy cập dễ dàng đến:
  * **Docs:** Mở tài liệu chính thức của SillyTavern (bạn đang ở đây!).
  * **GitHub:** Đưa bạn đến repository GitHub của SillyTavern (<https://github.com/SillyTavern/SillyTavern>).
  * **Discord:** Cung cấp liên kết đến máy chủ Discord chính thức của SillyTavern (<https://discord.gg/sillytavern>).
* **Nút Temporary Chat:** Cho phép bạn nhanh chóng bắt đầu phiên trò chuyện tạm thời mới với assistant trung lập mặc định, phiên này sẽ không được lưu vào lịch sử trò chuyện của bạn trừ khi bạn lưu nó một cách rõ ràng.
* **Phần Recent Chats:** Liệt kê các cuộc trò chuyện gần đây của bạn để truy cập nhanh. Bạn có thể:
  * Hiển thị hoặc ẩn phần này.
  * Mở rộng danh sách nếu có hơn 3 cuộc trò chuyện (tối đa 15 cuộc trò chuyện gần đây).

## Temporary Chat

!!! Note
Do giới hạn kỹ thuật, tính năng Temporary Chat sẽ không sử dụng Welcome Page Assistant tùy chỉnh của bạn. Nó sẽ luôn bắt đầu một cuộc trò chuyện trống mà không có bất kỳ prompt hoặc thông tin nhân vật bổ sung nào.
!!!

Nút Temporary Chat cho phép bạn nhanh chóng bắt đầu phiên trò chuyện mới mà không lưu nó vào lịch sử trò chuyện. Điều này hữu ích cho việc thử nghiệm hoặc các cuộc trò chuyện thông thường mà không làm lộn xộn các cuộc trò chuyện đã lưu của bạn. Cuộc trò chuyện này sẽ bị xóa ngay khi bạn đóng nó hoặc chuyển sang cuộc trò chuyện khác.

* Nút **Save** sẽ cho phép bạn xuất cuộc trò chuyện tạm thời dưới dạng tệp JSONL, sau đó bạn có thể nhập lại sau.
* Nút **Load** sẽ cho phép bạn khôi phục tệp trò chuyện tạm thời đã lưu trước đó.

## Welcome Page Assistant là gì?

Welcome Page Assistant là một nhân vật bạn chọn để hiển thị trên màn hình chào mừng. Điều này cho phép lời chào được cá nhân hóa và một cách nhanh chóng để bắt đầu trò chuyện với một nhân vật quen thuộc ngay từ đầu.

### Thiết lập và hủy thiết lập Assistant

Bạn có thể chọn bất kỳ nhân vật nào của mình để đóng vai trò làm Welcome Page Assistant.

**Để thiết lập assistant:**

1. Điều hướng đến bảng điều khiển **Character Management** (thường được tìm thấy trong thanh bên phải thông qua biểu tượng <i class="fa-solid fa-address-card"></i>).
2. Tìm nhân vật bạn muốn đặt làm assistant trong danh sách.
3. Nhấp vào "More..." và chọn **"Set / Unset as Welcome Page Assistant"** từ menu thả xuống.
4. Một biểu tượng nhỏ (<i class="fa-solid fa-user-graduate"></i>) sẽ xuất hiện bên cạnh tên nhân vật, cho biết họ hiện là Welcome Page Assistant đang hoạt động của bạn.

**Để hủy thiết lập assistant:**

1. Vào bảng điều khiển Character Management.
2. Tìm Welcome Page Assistant hiện tại của bạn (họ sẽ có biểu tượng <i class="fa-solid fa-user-graduate"></i>).
3. Nhấp vào "More..." và chọn **"Set / Unset as Welcome Page Assistant"** một lần nữa.
4. Nhân vật sẽ không còn là assistant của bạn, và biểu tượng <i class="fa-solid fa-user-graduate"></i> sẽ biến mất.
5. SillyTavern sẽ quay lại sử dụng Default Assistant (xem bên dưới).

### Tương tác với Assistant

Khi màn hình chào mừng được hiển thị với assistant đã chọn của bạn, chỉ cần nhập tin nhắn của bạn vào thanh nhập trò chuyện ở cuối màn hình và nhấn Enter hoặc nhấp vào nút send. Điều này sẽ bắt đầu phiên trò chuyện mới với Welcome Page Assistant của bạn.

Để mở cuộc trò chuyện trước đó với assistant, sử dụng phần Recent Chats hoặc tìm cuộc trò chuyện trong hộp thoại **Manage chat files** (có thể truy cập qua menu **<i class="fa-solid fa-bars"></i> Options**).

## Default Assistant

SillyTavern sẽ tự động tạo một nhân vật mặc định có tên "Assistant" khi bạn tương tác với màn hình chào mừng lần đầu tiên. Nhân vật này đóng vai trò là tùy chọn dự phòng nếu bạn chưa đặt một nhân vật cụ thể làm Welcome Page Assistant.

Default assistant không có bất kỳ prompt cụ thể nào được đính kèm, và bạn có thể tự do tùy chỉnh nó theo ý thích (ví dụ: đổi tên, thêm hình ảnh, hoặc đặt tính cách).

* Nếu bạn chưa đặt rõ ràng một nhân vật làm Welcome Page Assistant, default assistant này sẽ được sử dụng.
* Nếu bạn hủy thiết lập assistant đã chọn, hệ thống sẽ quay lại default assistant này.
* Nếu một nhân vật bạn đã đặt làm assistant bị xóa, hệ thống cũng sẽ quay lại default assistant.

**Lưu ý:** Bạn không thể "hủy thiết lập" default system assistant theo cách giống như bạn hủy thiết lập một nhân vật bạn đã chọn. Để thay đổi từ default assistant, bạn phải đặt một trong các nhân vật khác của mình làm assistant.
