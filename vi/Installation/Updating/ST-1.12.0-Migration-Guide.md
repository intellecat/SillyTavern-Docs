---
order: 112
route: /installation/st-1.12.0-migration-guide/
---

# Hướng dẫn di chuyển 1.12.0

SillyTavern 1.12.0 (tên mã là bản cập nhật "Neo Server") bao gồm một số thay đổi quan trọng có thể ảnh hưởng đến cách bạn sử dụng SillyTavern.

Hướng dẫn này sẽ chuẩn bị cho bạn cho bản cập nhật và cung cấp một số hướng dẫn thêm.

## Cập nhật lưu trữ dữ liệu

1.12.0 thay đổi cách SillyTavern xử lý dữ liệu người dùng.

Trước đây, tất cả dữ liệu liên tục được lưu trữ cùng với phần frontend trong thư mục `/public`, điều này tạo ra sự nhầm lẫn và các điểm có thể gây lỗi, cũng như làm cho việc containerization và cài đặt ứng dụng toàn hệ thống khá khó khăn.

### Những gì đã thay đổi?

Tất cả thông tin liên tục từ `/public` như cài đặt và cuộc trò chuyện (danh sách đầy đủ bên dưới) đã được chuyển vào một thư mục riêng biệt với đường dẫn có thể cấu hình, làm cho nó di động và độc lập với chính web server. Khi cần thiết cho mục đích tương thích, ví dụ, để lưu trữ các tiện ích mở rộng, thẻ nhân vật kích thước đầy đủ, tải lên hình ảnh người dùng, v.v., một chuyển hướng thông minh đã được thiết lập để tự động lưu trữ các tệp người dùng từ thư mục data.

### Đặt data root

Bạn có thể cung cấp đường dẫn tuyệt đối hoặc tương đối (so với thư mục repository ST) đến data root bằng `config.yaml` hoặc bằng cách khởi động máy chủ với đối số console `--dataRoot`.

> Ví dụ YAML

```yaml
# -- DATA CONFIGURATION --
# Root directory for user data storage
dataRoot: C:\Users\Harry\Documents\ST-Data
```

> Ví dụ Console

```bash
node server.js --dataRoot="/Users/harry/ST-Data"
# OR
npm run start -- --dataRoot="/Users/harry/ST-Data"
```

Đường dẫn data root mặc định là `./data`, có nghĩa là thư mục `data` trong repository của SillyTavern.

!!!info Lưu ý
Đường dẫn data root phải là **đường dẫn tuyệt đối đầy đủ** hoặc **đường dẫn tương đối đầy đủ**. Bạn _không thể_ sử dụng các phím tắt đường dẫn như `~` hoặc `%APP_DATA%`, vì chúng được giải quyết bởi shell, không phải hệ điều hành.
!!!

### Di chuyển

#### **QUAN TRỌNG!** Trước khi chúng ta bắt đầu

1. **Chỉ khi bạn muốn di chuyển dataRoot từ vị trí mặc định. Nếu không, bỏ qua phần này.** Đặt data root _trước khi_ chạy máy chủ lần đầu tiên sau khi kéo bản cập nhật. Chạy `npm install` để `config.yaml` được điền với giá trị mới, hoặc truyền một đối số console.
2. Tất cả dữ liệu sẽ được di chuyển vào tài khoản `default-user`. Xem thêm về [Users](#users) bên dưới.

#### Cài đặt không có container (bare metal)

Bạn không phải làm gì cả! Một quá trình di chuyển tự động sẽ xử lý mọi thứ cho bạn khi bạn khởi động máy chủ ST và nó phát hiện định dạng lưu trữ cũ (bằng cách kiểm tra sự tồn tại của thư mục `/public/characters`).

Khi di chuyển bất kỳ tệp nào, một bản sao lưu tự động sẽ được tạo trong thư mục `/backups/_migration/YYYY-MM-DD` (được giải quyết thành ngày hiện tại), nhưng luôn là một thực hành tốt để tạo bản sao lưu thủ công đầy đủ trước khi chạy quá trình di chuyển.

#### Cài đặt có container (Docker)

Di chuyển dữ liệu trong các volume Docker hơi phức tạp hơn nhưng khá đơn giản. Trong khi `docker-compose.yml` được cung cấp với repo đã được cập nhật để phản ánh các thay đổi, bạn có thể cần điều chỉnh các quy trình làm việc/triển khai tùy chỉnh của mình.

**Bước 1.** Tạo một volume mới và gắn nó vào đường dẫn "/home/node/app/data" trong container. Đừng xóa volume `config`.

```yaml
volumes:
    - "./config:/home/node/app/config"
    - "./data:/home/node/app/data"
```

**Bước 2.** Di chuyển mọi thứ trừ tệp `config.yaml` từ volume `config` vào thư mục con `default-user` của volume `data`.

**Bước 3.** Xây dựng lại container và khởi động nó.

!!!info Lưu ý
Các liên kết mềm giữa thư mục `/public` và volume `config` không còn cần thiết nữa và không được xây dựng vào Docker container!
!!!

#### Những gì cần di chuyển?

Các tệp và thư mục sau đây là đối tượng của quá trình di chuyển dữ liệu. Giả định cấu hình mặc định, các đường dẫn trước và sau được cung cấp trong bảng dưới đây.

| Trước                                  | Sau                                  |
|----------------------------------------|--------------------------------------|
| /secrets.json                          | /data/default-user/secrets.json      |
| /thumbnails                            | /data/default-user/thumbnails        |
| /vectors                               | /data/default-user/vectors           |
| /public/settings.json                  | /data/default-user/settings.json     |
| /public/stats.json                     | /data/default-user/stats.json        |
| /public/assets                         | /data/default-user/assets            |
| /public/backgrounds                    | /data/default-user/backgrounds       |
| /public/characters                     | /data/default-user/characters        |
| /public/chats                          | /data/default-user/chats             |
| /public/context                        | /data/default-user/context           |
| /public/scripts/extensions/third-party | /data/default-user/extensions        |
| /public/group chats                    | /data/default-user/group chats       |
| /public/groups                         | /data/default-user/groups            |
| /public/instruct                       | /data/default-user/instruct          |
| /public/KoboldAI Settings              | /data/default-user/KoboldAI Settings |
| /public/movingUI                       | /data/default-user/movingUI          |
| /public/NovelAI Settings               | /data/default-user/NovelAI Settings  |
| /public/OpenAI Settings                | /data/default-user/OpenAI Settings   |
| /public/QuickReplies                   | /data/default-user/QuickReplies      |
| /public/TextGen Settings               | /data/default-user/TextGen Settings  |
| /public/themes                         | /data/default-user/themes            |
| /public/worlds                         | /data/default-user/worlds            |
| /default/content/content.log           | /data/default-user/content.log       |

## Người dùng

1.12.0 thêm khả năng (hoàn toàn tùy chọn) để tạo thiết lập đa người dùng trên cùng một máy chủ, cho phép nhiều người dùng sử dụng các instance SillyTavern riêng biệt hoàn toàn của họ ngay cả trong cùng một thời điểm. Tài khoản người dùng cũng có thể được bảo vệ bằng mật khẩu để có thêm lớp bảo mật.

Vui lòng tham khảo tài liệu [Users](/Administration/multi-user.md) để biết thêm thông tin.
