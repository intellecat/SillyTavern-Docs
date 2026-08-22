---
title: Chế độ đa người dùng
icon: people
order: -10
route: /vi/administration/multi-user/
---

Chế độ đa người dùng cho phép nhiều người sử dụng một máy chủ SillyTavern. Mỗi người dùng có cài đặt, tiện ích mở rộng và dữ liệu riêng của họ. Tài khoản người dùng cũng có thể được bảo vệ bằng mật khẩu.

!!!warning
Mật khẩu người dùng cung cấp quyền riêng tư cơ bản giữa những người dùng trong thiết lập đa người dùng. Chúng không phải là một tính năng bảo mật và không nên được coi như vậy. Tất cả dữ liệu người dùng (bao gồm lịch sử chat, khóa API và thông tin nhạy cảm khác) được lưu trữ dưới dạng văn bản thuần trên máy chủ. Nó có thể được xem và sửa đổi bởi bất kỳ ai có quyền truy cập vào hệ thống tệp của máy chủ. **Không sử dụng SillyTavern trên máy chủ công cộng hoặc với những người dùng không tin cậy.**
!!!

## Cấu Hình

Để bật và sử dụng chế độ đa người dùng, chỉnh sửa tệp `config.yaml`:

```yaml
# Bật chế độ đa người dùng
enableUserAccounts: true
# Bật chế độ đăng nhập kín đáo: ẩn danh sách người dùng trên màn hình đăng nhập
enableDiscreetLogin: true
```

1. Khi cài đặt tài khoản người dùng bị vô hiệu hóa, một tài khoản quản trị viên dự phòng `default-user` được sử dụng để lưu trữ dữ liệu người dùng.
2. Khi cài đặt đăng nhập kín đáo bị vô hiệu hóa, danh sách người dùng hoạt động sẽ được hiển thị trên màn hình đăng nhập. Nếu được bật, người dùng phải nhập handle của họ thủ công.

!!!info
Bạn không thể _xóa_ tài khoản `default-user` khỏi danh sách người dùng vì nó được sử dụng để phục vụ dữ liệu người dùng trong trường hợp `enableUserAccounts` được đặt thành `false`. Nhưng bạn có thể _vô hiệu hóa_ nó để ẩn nó khỏi danh sách và không cho phép đăng nhập.
!!!

## Handle Người Dùng

Handle là định danh duy nhất của người dùng. Nó chỉ có thể bao gồm các chữ cái thường, số và dấu gạch ngang.

Đường dẫn đến thư mục dữ liệu người dùng giả định sử dụng mẫu sau: `%DATA_ROOT%/%USER_HANDLE%`.

Ví dụ về các handle người dùng hợp lệ:

- default-user
- juan555
- flux-the-cat
- cool-guy1337

## Vai Trò

- **Admin** - có thể quản lý (tạo, xóa, sửa đổi) người dùng khác. Có thể cài đặt tiện ích mở rộng cho tất cả người dùng.
- **User** - không thể quản lý người dùng khác. Chỉ có thể cài đặt tiện ích mở rộng cho chính họ.

Ngoài việc có quyền truy cập bảng điều khiển quản trị viên, cả hai vai trò người dùng đều giống hệt nhau về mặt chức năng và có thể sử dụng đầy đủ các tính năng của SillyTavern mà không có bất kỳ hạn chế nào. Việc triển khai quyền người dùng vẫn đang được phát triển.

Tất cả tài khoản người dùng được tạo dưới dạng người dùng thông thường trước, sau đó có thể được thăng cấp lên quản trị viên nếu cần.

### Màn hình đăng nhập

Tại đây bạn có thể chọn tài khoản người dùng để sử dụng. Có hai phong cách, tùy thuộc vào giá trị cấu hình `enableDiscreetLogin`.

Màn hình đăng nhập sẽ được bỏ qua và không hiển thị khi bạn chỉ có một người dùng hoạt động và nó không được bảo vệ bằng mật khẩu.

### Hồ sơ người dùng

Bạn có thể truy cập menu tự quản lý tài khoản bằng cách sử dụng nút "Account" trong bảng "User settings" trên thanh menu trên cùng.

1. Tên hiển thị - được sử dụng trong màn hình đăng nhập, có thể thay đổi. Không liên quan đến persona và không hiển thị với các API AI - bạn vẫn có thể sử dụng nhiều persona tùy thích.
2. Ảnh hồ sơ - được sử dụng trong màn hình đăng nhập. Bạn có thể sử dụng ảnh tùy chỉnh, ảnh persona mặc định (nếu được đặt), hoặc persona được sử dụng lần cuối.
3. Mật khẩu - biểu tượng khóa phản ánh trạng thái bảo vệ tài khoản (khóa mở = không có mật khẩu). Mật khẩu có thể được đặt, thay đổi hoặc xóa bằng nút "Change Password".
4. Settings Snapshots - truy cập và xem lại các bản sao lưu của tệp `settings.json` của bạn, với khả năng tạo hoặc khôi phục các snapshot.
5. Download Backup - tải xuống một kho lưu trữ thư mục dữ liệu người dùng của bạn.
6. Reset Settings - đặt lại cài đặt mặc định từ nhà máy, trong khi vẫn giữ nguyên dữ liệu khác (nhân vật, chat).

## Khôi phục mật khẩu

1. Mật khẩu có thể được khôi phục từ màn hình đăng nhập. Bạn cần truy cập vào console máy chủ để lấy mã khôi phục một lần (gồm 4 chữ số).
2. Ngoài ra, bạn có thể sử dụng script tiện ích trong máy chủ SillyTavern để đặt lại mật khẩu bằng cách cung cấp handle người dùng.

```txt
Usage: node recover.js [account] (password)
Example: node recover.js admin SecurePassword
```

## Content scaffolding

Để thêm nội dung tùy chỉnh cho người dùng, bạn có thể sử dụng tính năng content scaffolding. Tính năng này cho phép bạn xác định một tập hợp các tệp sẽ được sao chép vào thư mục dữ liệu của mỗi người dùng khi máy chủ khởi động.

Bạn phải tạo một tệp `index.json` trong thư mục `/default/scaffold` để tính năng này hoạt động. Cú pháp giống như đối với nội dung mặc định. Tất cả các đường dẫn tệp phải tương đối với thư mục `/default/scaffold`, và bạn có thể tổ chức các tệp bằng cách sử dụng các thư mục con.

Các tệp scaffolded được sao chép trước các tệp mặc định, có nghĩa là chúng sẽ ghi đè bất kỳ tệp mặc định nào (presets/settings/etc.) có cùng tên tệp.

!!!tip
Mỗi thư mục dữ liệu người dùng có một tệp `content.log` liệt kê tất cả các tệp được sao chép từ các thư mục scaffold và mặc định. Xóa tệp này để buộc máy chủ đồng bộ hóa nội dung lại khi khởi động lần tiếp theo.
!!!

### Các loại nội dung được nhận dạng

| Loại                          | Giá Trị              |
|-------------------------------|----------------------|
| settings.json                 | `'settings'`         |
| Character card                | `'character'`        |
| Character sprites             | `'sprites'`          |
| Background image              | `'background'`       |
| World Info file               | `'world'`            |
| Persona avatar                | `'avatar'`           |
| UI theme                      | `'theme'`            |
| ComfyUI workflow              | `'workflow'`         |
| KoboldAI Classic preset       | `'kobold_preset'`    |
| Chat Completion preset        | `'openai_preset'`    |
| NovelAI preset                | `'novel_preset'`     |
| Text Completion preset        | `'textgen_preset'`   |
| Instruct Mode template        | `'instruct'`         |
| Context Formatting template   | `'context'`          |
| MovingUI preset               | `'moving_ui'`        |
| Quick Replies set             | `'quick_replies'`    |
| System Prompt template        | `'sysprompt'`        |
| Reasoning Formatting template | `'reasoning'`        |

### Ví dụ (`/default/scaffold/index.json`)

```json
[
    {
        "filename": "themes/Midnight.json",
        "type": "theme"
    },
    {
        "filename": "backgrounds/city.png",
        "type": "background"
    },
    {
        "filename": "characters/Charlie.png",
        "type": "character"
    }
]
```
