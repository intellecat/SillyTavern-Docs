---
label: Cập nhật
icon: repo-pull
order: -1
expanded: false
route: /vi/installation/updating/
---

# Cách cập nhật SillyTavern

Tìm hệ điều hành của bạn bên dưới và làm theo hướng dẫn để cập nhật ST.

!!! Để biết hướng dẫn cài đặt, xem trang [Cài đặt](/Installation/index.md).

Hướng dẫn này giả định bạn đã cài đặt và chạy SillyTavern ít nhất một lần.
!!!

----

## Linux/Termux hoặc MacOS

Bạn chắc chắn đã cài đặt qua git, vì vậy chỉ cần 'git pull' bên trong thư mục SillyTavern.

- `cd SillyTavern` để vào thư mục đúng.
- `git pull` để nhận bản cập nhật.
- `./start.sh` hoặc `bash start.sh` để khởi động ST.

----

## Windows

>Đầu tiên hãy thử sử dụng `UpdateAndStart.bat` nằm trong thư mục gốc cài đặt SillyTavern của bạn.

Nếu điều đó thất bại, quay lại đây và tiếp tục đọc.

### Phương pháp 1 - GIT

Chúng tôi luôn khuyến nghị người dùng cài đặt bằng 'git'. Đây là lý do:

Khi bạn đã cài đặt qua `git clone`, tất cả những gì bạn phải làm để cập nhật là gõ `git pull` [trong dòng lệnh trong thư mục ST](https://www.google.com/search?q=how+to+open+command+prompt+in+a+folder).
Ngoài ra, nếu dòng lệnh gây vấn đề cho bạn (và bạn đã cài đặt GitHub Desktop), bạn có thể sử dụng menu `Repository` và chọn `Pull`.

Các bản cập nhật được áp dụng tự động và an toàn.

#### "Giúp tôi với, tôi ban đầu cài đặt qua Zip và bây giờ muốn chuyển sang cài đặt Git"

Bạn đã chọn một con đường khôn ngoan.

Vì cài đặt của bạn được thực hiện qua Zip, bạn sẽ cần thực hiện cài đặt mới bằng git.

May mắn thay chúng tôi có [hướng dẫn](/Installation/Windows.md) về cách thực hiện điều đó.

Khi bạn đã sử dụng git để cài đặt MỘT SillyTavern MỚI vào MỘT thư mục KHÁC, quay lại trang này và tiến hành **Bước 4** của hướng dẫn 'Cập nhật Zip' bên dưới.

### Phương pháp 2 - ZIP

Nếu bạn khăng khăng cài đặt qua zip, đây là quy trình tẻ nhạt để thực hiện cập nhật:

1. Tải xuống zip bản phát hành mới.
2. Giải nén nó vào một thư mục BÊN NGOÀI cài đặt ST hiện tại của bạn.
3. Thực hiện quy trình thiết lập thông thường cho hệ điều hành của bạn để cài đặt các yêu cầu NodeJS.

4. Sao chép các tệp/thư mục sau khi cần thiết(*) từ cài đặt ST cũ của bạn:

    (*) 'Khi cần thiết' = "Nếu bạn đã tạo bất kỳ nội dung tùy chỉnh nào liên quan đến các thư mục đó".

    #### Cập nhật >=1.12.0

    Sao chép thư mục `/data` và tệp `config.yaml` từ cài đặt này sang cài đặt khác. Nếu bạn có các tiện ích mở rộng toàn server (được cài đặt cho "Tất cả người dùng") mà bạn muốn giữ lại, cũng sao chép thư mục `/public/scripts/extensions/third-party`.

    #### Cập nhật từ <1.12.0 đến >1.12.0

    1.12.0 bao gồm một quy trình di chuyển tự động. Các bước dưới đây chỉ cần thiết *nếu* quá trình di chuyển bị gián đoạn hoặc lỗi.

5. Chạy cài đặt máy chủ đã cập nhật ít nhất một lần để tạo thư mục `/data/default-user`.
6. Chuyển các tệp từ `/public` cũ sang `/data/default-user` mới khi cần thiết.

    Không có thư mục nào là bắt buộc, vì vậy chỉ sao chép những gì bạn cần.

    **LƯU Ý: ĐỪNG SAO CHÉP TOÀN BỘ THƯ MỤC /PUBLIC/**

    Làm như vậy có thể phá vỡ cài đặt mới và ngăn các tính năng mới xuất hiện.

    ```plaintext
    Assets
    Backgrounds
    Characters
    Chats
    Context
    Groups
    Group chats
    Instruct
    movingUI
    KoboldAI Settings
    NovelAI Settings
    OpenAI Settings
    QuickReplies
    TextGen Settings (textgen = ooba)
    Themes
    User Avatars
    Worlds
    User
    settings.json
    secrets.json <---- cái này nằm trong thư mục gốc, không phải /public/
    ```

7. Khi các thư mục/tệp đó được sao chép, dán chúng vào thư mục /data/default-user (với secrets.json đi vào thư mục gốc) của cài đặt mới.
8. Khởi động SillyTavern một lần nữa với phương pháp thích hợp cho hệ điều hành của bạn, và cầu nguyện bạn đã làm đúng.
9. Nếu mọi thứ hiển thị, bạn có thể xóa thư mục ST cũ một cách an toàn.

### Các vấn đề cập nhật phổ biến

#### "There are unresolved conflicts in the working directory."

Điều này có nghĩa là bạn đã sửa đổi các tệp mặc định đã được thay đổi trong repository từ xa (chẳng hạn như cài đặt preset).

Để khắc phục điều này, chạy lệnh này trong terminal. Sử dụng cẩn thận, vì nó có thể phá hủy. Đảm bảo có bản sao lưu nếu cần.

```bash
git merge --abort
git reset --hard
git pull --rebase --autostash
```

#### Các thay đổi tệp ngăn git pull

- Nếu bạn thay đổi các tệp hệ thống SillyTavern, `git pull` có thể không hoạt động.
- Đôi khi một bản cập nhật có thể yêu cầu chúng tôi thay đổi một tệp quan trọng, có thể gây ra vấn đề tương tự.
- Thường là các tệp preset mặc định hoặc `package-lock.json`.
- Trong trường hợp này, bạn có thể thử di chuyển tệp sang một thư mục khác (hoặc xóa tệp) và sau đó thực hiện `git pull`.
- Giải pháp khác là sử dụng `git pull --rebase --autostash`

#### Error: Cannot find module "***" when starting the server

- Điều này có nghĩa là SillyTavern đã thêm một yêu cầu gói npm mới.
- Chạy `npm install` trong thư mục SillyTavern để khắc phục điều này. Các script Start.bat và start.sh được cung cấp sẽ tự động thực hiện điều đó.
- Không giúp được? Xóa thư mục node_modules

**Windows**

```bash
rmdir /s /q node_modules
npm cache clean --force
npm install
```

**Unix/Linux**

```bash
rm -rf node_modules
npm cache clean --force
npm install
```

## Docker

1. Mở cửa sổ terminal và điều hướng đến thư mục docker của bạn `cd SillyTavern/docker`
2. Xóa container của bạn với `docker compose down`
3. Xóa image Docker SillyTavern khỏi bộ nhớ cache `docker rmi ghcr.io/sillytavern/sillytavern:latest` (Thay thế `sillytavern:latest` bằng `sillytavern:staging` nếu bạn đang nhắm đến nhánh staging.)
4. Xây dựng lại container với `sudo docker compose up -d`

Nếu mọi thứ diễn ra suôn sẻ, docker sẽ bắt đầu tải lại image và bạn sẽ chạy ngay. Nếu bạn gặp bất kỳ vấn đề nào, hãy tham khảo phần tiếp theo của hướng dẫn này.

### Các vấn đề cập nhật phổ biến
#### Tôi sử dụng Docker và tất cả dữ liệu của tôi đã biến mất sau khi cập nhật!

Bạn phải làm theo [Hướng dẫn di chuyển cho Docker containers](/Installation/Updating/ST-1.12.0-Migration-Guide.md#containerized-docker-installs)
 để cập nhật ánh xạ volume cho mô hình dữ liệu mới được giới thiệu trong 1.12.0

#### Permission denied khi chạy lệnh docker

Đây là vấn đề của Linux, và ngụ ý rằng quyền của bạn không được thiết lập đúng cách. Có hai cách để giải quyết điều này:

1. **Phương pháp dễ dàng**: Nếu bạn có quyền truy cập sudo trên người dùng của mình, chỉ cần thêm tiền tố lệnh bằng `sudo` (ví dụ: `sudo docker compose down`)
2. **Phương pháp đúng đắn**: Sửa quyền của bạn. Điều này khác nhau tùy thuộc vào phiên bản Linux bạn sử dụng. Có rất nhiều hướng dẫn trực tuyến để giúp bạn khắc phục vấn đề này.
