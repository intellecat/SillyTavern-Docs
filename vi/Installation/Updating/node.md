---
order: -50
route: /installation/updating/node/
---

# Cách cập nhật Node.js

Việc giữ cho runtime Node.js của bạn được cập nhật là quan trọng vì lý do bảo mật và hiệu suất. Dưới đây là các bước để cập nhật Node.js tùy thuộc vào hệ điều hành của bạn.

Chúng tôi khuyến nghị sử dụng phiên bản Long Term Support (LTS) mới nhất, mà bạn có thể tìm thấy trên [trang web chính thức của Node.js](https://nodejs.org/en/about/previous-releases).

## Cách kiểm tra phiên bản Node.js hiện tại của bạn

1. Mở terminal hoặc command prompt của bạn.
2. Gõ lệnh sau và nhấn Enter:

```bash
node -v
```

## nvm (Node Version Manager) - Đa nền tảng

Nếu bạn đang sử dụng `nvm`:

1. Mở terminal của bạn.
2. Gõ lệnh sau:

[**Unix/Linux/macOS:**](https://github.com/nvm-sh/nvm)

```bash
nvm install --lts
nvm use --lts
```

[**Windows:**](https://github.com/coreybutler/nvm-windows)

```bash
nvm install lts
nvm use lts
```

## Windows - Cài đặt thông thường

1. Truy cập [trang tải xuống](https://nodejs.org/en/download/) của Node.js.
2. Tải Windows Installer cho phiên bản LTS.
3. Chạy trình cài đặt và làm theo lời nhắc để hoàn tất cài đặt.

## Windows - SillyTavern Launcher

Nếu bạn đã cài đặt bằng SillyTavern Launcher:

1. Mở SillyTavern Launcher.
2. Điều hướng đến `Toolbox / App Installer / Core Utilities / Install Node.js`.

**HOẶC:**

Thực hiện thủ công bằng winget trong PowerShell:

```powershell
winget install --id=OpenJS.NodeJS.LTS  -e
```

## Android - Termux

1. Mở ứng dụng Termux.
2. Gõ các lệnh sau:

```bash
pkg update
pkg upgrade nodejs-lts
```

Đừng quên chấp nhận bất kỳ lời nhắc nào có thể xuất hiện trong quá trình cập nhật bằng cách nhấn `Y` trên bàn phím ảo.

## macOS - Cài đặt thông thường

1. Truy cập [trang tải xuống](https://nodejs.org/en/download/) của Node.js.
2. Tải macOS Installer cho phiên bản LTS.
3. Chạy tệp `.pkg` và làm theo lời nhắc để hoàn tất cài đặt.

## macOS - Homebrew

Nếu bạn đã cài đặt Homebrew, bạn có thể cập nhật Node.js bằng các lệnh sau:

```bash
brew update
brew upgrade node
```

## Linux - Package Manager

Phương pháp cập nhật Node.js trên Linux phụ thuộc vào bản phân phối của bạn.

Nhưng vì phiên bản Node.js trong các repository chính thức có thể không phải là mới nhất, chúng tôi khuyến nghị sử dụng [Node Version Manager (nvm)](https://github.com/nvm-sh/nvm) hoặc [repository NodeSource](https://github.com/nodesource/distributions).

## Docker

Không cần hành động nào. Image Docker được xây dựng sẵn mà chúng tôi cung cấp được biên dịch với phiên bản Node.js cập nhật.
