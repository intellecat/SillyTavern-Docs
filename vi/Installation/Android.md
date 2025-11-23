---
label: Android (Termux)
route: /installation/android-(termux)/
---

# Cài đặt Android (Termux)

SillyTavern có thể chạy trực tiếp trên thiết bị Android bằng Termux.

## Cài đặt Termux

!!!tip
Tránh cài đặt Termux từ Google Play Store, phiên bản đó không còn được bảo trì nữa.
Thay vào đó, sử dụng F-Droid (khuyến nghị) hoặc GitHub releases để có phiên bản mới nhất.
!!!

1. Tải Termux từ [F-Droid](https://f-droid.org/en/packages/com.termux/) hoặc [GitHub releases](https://github.com/termux/termux-app/releases).
2. Cài đặt tệp APK đã tải xuống.
3. Mở Termux và chạy lệnh đầu tiên:

   ```bash
   termux-change-repo
   ```

4. Chọn "Mirror group" và chọn các máy chủ gần nhất với bạn. Bạn có thể chạm vào màn hình hoặc sử dụng cử chỉ vuốt với [Unexpected Keyboard](https://play.google.com/store/apps/details?id=juloo.keyboard2&hl=en).
5. Cập nhật Termux:

   ```bash
   pkg update && pkg upgrade
   ```

## Cài đặt các phụ thuộc

Cài đặt các gói yêu cầu:

```bash
pkg install git nodejs-lts nano
```

!!!warning
Nếu bạn đang chạy Android 32-bit, xem phần [Lỗi phổ biến](#common-errors) bên dưới để biết các bước bổ sung.
!!!

## Cài đặt SillyTavern

Clone repository SillyTavern ([Cách chọn nhánh](/Installation/index.md#branches)):

- **Nhánh Release:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b release
    ```

- **Nhánh Staging:**

    ```bash
    git clone https://github.com/SillyTavern/SillyTavern -b staging
    ```

## Chạy SillyTavern

Để chạy SillyTavern, di chuyển đến thư mục đã clone và chạy script khởi động:

```bash
cd ~/SillyTavern
bash start.sh
```

Để cập nhật SillyTavern, di chuyển đến thư mục SillyTavern và chạy:

```bash
cd ~/SillyTavern
git pull --rebase --autostash
```

Xem phần [Aliases](#optional-create-aliases) bên dưới để tạo các phím tắt giúp đơn giản hóa quy trình này.

## Lỗi phổ biến

### Unsupported platform: android arm LEtime-web

Android 32-bit yêu cầu một phụ thuộc bên ngoài không thể cài đặt qua npm.

Sử dụng lệnh sau để cài đặt nó:

```bash
pkg install esbuild
```

Sau đó tiếp tục với các bước cài đặt ở trên.

### Điều chỉnh hiệu suất

!!!info
Để biết các mẹo chung về cải thiện hiệu suất, xem phần [FAQ tương ứng](/Usage/faq.md#performance-tips).
!!!

Do giới hạn phần cứng trên thiết bị Android, bạn có thể muốn điều chỉnh các cài đặt [config.yaml](/Administration/config-yaml.md) sau của SillyTavern để sử dụng bộ nhớ, lưu trữ và CPU tốt hơn:

```yaml
performance:
  # Tránh tải tất cả dữ liệu nhân vật cho đến khi cần thiết
  lazyLoadCharacters: true
  # Tắt bộ nhớ đệm đĩa để giảm việc sử dụng lưu trữ
  useDiskCache: false
backups:
  chat:
    # Tùy chọn: Tắt sao lưu chat tự động để tiết kiệm dung lượng lưu trữ
    enabled: false
```

!!!tip
Sử dụng trình soạn thảo văn bản `nano` đi kèm với Termux để chỉnh sửa tệp `config.yaml`: `nano ~/SillyTavern/config.yaml`
!!!

## Tùy chọn: Tạo Aliases

Bạn có thể tạo các phím tắt cho các lệnh thường dùng để làm cho công việc của bạn dễ dàng hơn.

1. Mở trình soạn thảo để sửa đổi tệp `.bashrc` của bạn:

   ```bash
   nano ~/.bashrc
   ```

2. Thêm các dòng sau để tạo aliases:

   ```bash
   # Update Termux packages
   alias pkgup="pkg update && pkg upgrade"
   #Start SillyTavern
   alias st='cd ~/SillyTavern && bash start.sh'
   # Update SillyTavern
   alias stup='cd ~/SillyTavern && git pull --rebase --autostash'
   ```

3. Lưu tệp và thoát khỏi trình soạn thảo (trong nano, nhấn `CTRL + X`, sau đó `Y`, rồi `Enter`).

4. Để áp dụng các thay đổi, chạy:

   ```bash
   source ~/.bashrc
   ```

Bây giờ bạn có thể sử dụng các lệnh sau:

- `st` để khởi động SillyTavern
- `stup` để cập nhật SillyTavern
- `pkgup` để cập nhật các gói Termux

## Đọc thêm

!!!info
Các hướng dẫn được liên kết dưới đây không được duy trì bởi nhóm SillyTavern.
!!!

- Hướng dẫn SillyTavern trong Termux bởi ArroganceComplex#2659: <https://rentry.org/STAI-Termux>
- Truy cập tệp Termux với Material Files: <https://www.learntermux.tech/2020/10/Termux-File-Manager.html>
- Ngăn quá trình Termux ngủ sâu: <https://wiki.termux.com/wiki/Termux-wake-lock>
