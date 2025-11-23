---
label: MacOS & Linux
order: 5
route: /installation/linuxmacos/
---

# Cài đặt Linux/MacOS

## Cài đặt Git thủ công

Đối với MacOS / Linux, tất cả các bước này sẽ được thực hiện trong Terminal.

1. Cài đặt git và nodeJS (phương pháp thực hiện điều này sẽ khác nhau tùy thuộc vào hệ điều hành của bạn)
2. Clone repository

   - cho Nhánh Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - cho Nhánh Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

3. `cd SillyTavern` để điều hướng vào thư mục cài đặt.
4. Chạy script `start.sh` với một trong các lệnh sau:

- `./start.sh`
- `bash start.sh`

## SillyTavern Launcher

### Cho người dùng Linux
1. Mở terminal yêu thích của bạn và cài đặt git
2. Tải SillyTavern Launcher với: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
3. Điều hướng đến SillyTavern-Launcher với: `cd SillyTavern-Launcher`
4. Bắt đầu launcher cài đặt với: `chmod +x install.sh && ./install.sh` và chọn những gì bạn muốn cài đặt
5. Sau khi cài đặt, khởi động launcher với: `chmod +x launcher.sh && ./launcher.sh`

### Cho người dùng Mac
1. Mở terminal và cài đặt brew với: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Sau đó cài đặt git với: `brew install git`
3. Tải SillyTavern Launcher với: `git clone https://github.com/SillyTavern/SillyTavern-Launcher.git`
4. Điều hướng đến SillyTavern-Launcher với: `cd SillyTavern-Launcher`
5. Bắt đầu launcher cài đặt với: `chmod +x install.sh && ./install.sh` và chọn những gì bạn muốn cài đặt
6. Sau khi cài đặt, khởi động launcher với: `chmod +x launcher.sh && ./launcher.sh`
