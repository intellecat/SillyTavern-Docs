---
order: 50
icon: package
expanded: true
route: /installation/
---

# Cài đặt

Làm theo hướng dẫn cài đặt cho nền tảng của bạn:

* [Windows](/Installation/Windows.md)
* [Linux và Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Các nhánh

SillyTavern đang được phát triển bằng hệ thống hai nhánh để đảm bảo trải nghiệm mượt mà cho tất cả người dùng.

* `release` -🌟 **Khuyến nghị cho hầu hết người dùng.** Đây là nhánh ổn định nhất và được khuyến nghị, chỉ được cập nhật khi các bản phát hành chính được đẩy lên. Nó phù hợp với phần lớn người dùng. Thường được cập nhật mỗi tháng một lần.
* `staging` - ⚠️ **Không khuyến nghị cho sử dụng bình thường.** Nhánh này có các tính năng mới nhất, nhưng hãy thận trọng vì nó có thể bị lỗi bất cứ lúc nào. Chỉ dành cho người dùng power và những người đam mê. Cập nhật nhiều lần mỗi ngày.

## Chế độ Global / Standalone

Có hai chế độ chạy SillyTavern khác nhau về cách chúng xử lý đường dẫn cấu hình và dữ liệu.

* **Chế độ Standalone** (mặc định) - sử dụng tệp `config.yaml` và thư mục `data` trong thư mục máy chủ. Tất cả dữ liệu sẽ được giới hạn trong đường dẫn cài đặt. Đây là chế độ được khuyến nghị cho hầu hết người dùng.
* **Chế độ Global** - sử dụng đường dẫn toàn hệ thống cho cấu hình và dữ liệu. Điều này hữu ích để cài đặt SillyTavern như một gói hoặc khi bạn muốn chia sẻ cùng một cấu hình và dữ liệu trên nhiều cài đặt.

!!!info
Các cài đặt được thực hiện bằng cách sử dụng [gói npm chính thức](https://www.npmjs.com/package/sillytavern) (ví dụ: `npx sillytavern@latest`) sẽ chạy ở chế độ global theo mặc định.
!!!

### Đường dẫn dữ liệu

Đường dẫn **chế độ Standalone** có liên quan đến thư mục cài đặt SillyTavern:

* **Đường dẫn Config**: `./config.yaml`
* **Thư mục gốc Data**: `./data/`

Đường dẫn **chế độ Global** phụ thuộc vào hệ điều hành:

* **Linux**: `~/.local/share/SillyTavern/config.yaml` (hoặc `$XDG_DATA_HOME/SillyTavern/config.yaml`) và `~/.local/share/SillyTavern/data/` (hoặc `$XDG_DATA_HOME/SillyTavern/data/`)
* **Windows**: `%APPDATA%\SillyTavern\config.yaml` và `%APPDATA%\SillyTavern\data\`
* **MacOS**: `~/Library/Application Support/SillyTavern/config.yaml` và `~/Library/Application Support/SillyTavern/data/`

### Cách chạy ở chế độ global

!!!warning
`dataRoot` và `configPath` không thể bị ghi đè bằng [đối số CLI](../Administration/config-yaml.md#command-line-arguments) hoặc [config.yaml](../Administration/config-yaml.md) khi chạy ở chế độ global.
!!!

1. Truyền đối số `--global` cho lệnh khởi động máy chủ (ví dụ: `node server.js --global`).
2. Truyền đối số `--global` cho script khởi động shell (ví dụ: `Start.bat --global` hoặc `./start.sh --global`).
3. Sử dụng script `start:global` trong tệp `package.json` (ví dụ: `npm run start:global`).
