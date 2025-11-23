---
icon: cpu
route: /administration/config-yaml/
---

# Tệp Cấu Hình

!!!warning Tuyên Bố Miễn Trừ Trách Nhiệm

Tài liệu này có thể lỗi thời, không đầy đủ hoặc không chính xác. Vui lòng tham khảo [tệp config.yaml mặc định](https://github.com/SillyTavern/SillyTavern/blob/release/default/config.yaml) trong bản cài đặt của bạn để có danh sách cài đặt cập nhật nhất.

**CẢNH BÁO: KHÔNG CHỈNH SỬA TRỰC TIẾP TẬP CẤU HÌNH MẶC ĐỊNH. ĐIỀU NÀY SẼ KHÔNG CÓ TÁC DỤNG TÍCH CỰC. THAY VÀO ĐÓ, HÃY CHỈNH SỬA BẢN SAO CỦA NÓ TRONG THƯ MỤC GỐC CỦA REPOSITORY.**
!!!

`config.yaml` là tệp cấu hình chính cho máy chủ SillyTavern mà bạn có thể tìm thấy trong thư mục gốc của repository sau khi [hoàn thành cài đặt](/Installation/index.md). Đây là tệp YAML chứa các cài đặt khác nhau, chẳng hạn như mạng, bảo mật và các tùy chọn cụ thể của backend. **Các thay đổi được thực hiện cho tệp này sẽ có hiệu lực sau khi khởi động lại máy chủ.**

Các cài đặt mới được thêm vào upstream sẽ tự động được điền với các giá trị mặc định khi bạn chạy `npm install` (cụ thể là script `post-install.js`) sau khi [cập nhật repository](/Installation/Updating/index.md). Sau đó, bạn có thể sửa đổi các cài đặt này theo nhu cầu.

Đối với các cài đặt lồng nhau, ký hiệu dấu chấm được sử dụng để biểu thị hệ thống phân cấp. Ví dụ, `protocol.ipv6: false` đề cập đến cài đặt `ipv6` trong phần `protocol` với giá trị là `false`.

```yaml
protocol:
  ipv6: false
```

## Đối Số Dòng Lệnh

Bạn có thể truyền các đối số dòng lệnh khi khởi động máy chủ SillyTavern để ghi đè một số cài đặt trong [config.yaml](../Administration/config-yaml.md).

### Ví Dụ

```shell
node server.js --port 8000 --listen false
# hoặc
npm run start -- --port 8000 --listen false
# hoặc (chỉ Windows)
Start.bat --port 8000 --listen false
```

### Các đối số được hỗ trợ

!!!tip
Không có đối số nào là bắt buộc. Nếu bạn không cung cấp chúng, SillyTavern sẽ sử dụng các cài đặt trong `config.yaml`.
!!!

| Tùy Chọn                        | Mô Tả                                                                | Loại     |
|---------------------------------|----------------------------------------------------------------------|----------|
| `--version`                     | Hiển thị số phiên bản                                                | boolean  |
| `--global`                      | Buộc sử dụng đường dẫn hệ thống cho dữ liệu ứng dụng                | boolean  |
| `--configPath`                  | Ghi đè đường dẫn đến tệp config.yaml (chỉ chế độ standalone)         | string   |
| `--dataRoot`                    | Đặt thư mục gốc để lưu trữ dữ liệu (chỉ chế độ standalone)           | string   |
| `--port`                        | Đặt cổng mà SillyTavern sẽ chạy                                      | number   |
| `--listen`                      | Làm cho SillyTavern lắng nghe trên tất cả các giao diện mạng         | boolean  |
| `--whitelist`                   | Bật chế độ whitelist                                                 | boolean  |
| `--basicAuthMode`               | Bật xác thực cơ bản                                                  | boolean  |
| `--enableIPv4`                  | Bật giao thức IPv4                                                   | boolean  |
| `--enableIPv6`                  | Bật giao thức IPv6                                                   | boolean  |
| `--listenAddressIPv4`           | Chỉ định địa chỉ IPv4 để lắng nghe                                   | string   |
| `--listenAddressIPv6`           | Chỉ định địa chỉ IPv6 để lắng nghe                                   | string   |
| `--dnsPreferIPv6`               | Ưu tiên IPv6 cho DNS                                                 | boolean  |
| `--ssl`                         | Bật SSL                                                              | boolean  |
| `--certPath`                    | Đặt đường dẫn đến tệp chứng chỉ của bạn                              | string   |
| `--keyPath`                     | Đặt đường dẫn đến tệp khóa riêng của bạn                             | string   |
| `--browserLaunchEnabled`        | Tự động khởi chạy SillyTavern trong trình duyệt                      | boolean  |
| `--browserLaunchHostname`       | Đặt hostname khởi chạy trình duyệt                                   | string   |
| `--browserLaunchPort`           | Ghi đè cổng cho khởi chạy trình duyệt                                | string   |
| `--browserLaunchAvoidLocalhost` | Tránh sử dụng 'localhost' để khởi chạy trình duyệt ở chế độ tự động | boolean  |
| `--corsProxy`                   | Bật proxy CORS                                                       | boolean  |
| `--requestProxyEnabled`         | Bật sử dụng proxy cho các yêu cầu đi ra                              | boolean  |
| `--requestProxyUrl`             | Đặt URL proxy yêu cầu (giao thức HTTP hoặc SOCKS)                    | string   |
| `--requestProxyBypass`          | Đặt danh sách bỏ qua proxy yêu cầu (danh sách máy chủ ngăn cách bởi khoảng trắng) | array    |
| `--disableCsrf`                 | Tắt bảo vệ CSRF (KHÔNG KHUYẾN NGHỊ)                                 | boolean  |

## Biến Môi Trường

Cấu hình cũng có thể được đặt thông qua các biến môi trường, điều này sẽ ghi đè các giá trị trong tệp `config.yaml`.

Các biến môi trường phải được đặt tiền tố với `SILLYTAVERN_` và sử dụng chữ in hoa cho tên cài đặt. Ví dụ, cài đặt `dataRoot` có thể được ghi đè bằng biến môi trường `SILLYTAVERN_DATAROOT`.

Các cài đặt lồng nhau phải được phân tách bằng dấu gạch dưới. Ví dụ, `protocol.ipv6` có thể được ghi đè bằng biến môi trường `SILLYTAVERN_PROTOCOL_IPV6`.

!!!warning
Các cấu hình mong đợi mảng hoặc đối tượng phải được chuyển đổi thành chuỗi JSON. Ví dụ, để ghi đè cài đặt `whitelist` bằng biến môi trường `SILLYTAVERN_WHITELIST`, bạn nên đặt nó dưới dạng chuỗi JSON: `SILLYTAVERN_WHITELIST='["127.0.0.1", "::1"]'`.
!!!

Nếu bạn đang sử dụng Node.js v20 trở lên, bạn cũng có thể lưu trữ các biến môi trường trong tệp `.env` và truyền nó cho máy chủ bằng cờ `--env-file`. Ví dụ, để sử dụng tệp `.env` nằm trong thư mục gốc của repository, bạn có thể khởi động máy chủ bằng lệnh sau:

```bash
node --env-file=.env server.js
```

Hoặc, truyền các biến môi trường trực tiếp qua dòng lệnh:

```bash
SILLYTAVERN_LISTEN=true SILLYTAVERN_PORT=8000 node server.js
```

Xem thêm về việc sử dụng biến môi trường trong [tài liệu Node.js](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs).

## Cấu Hình Dữ Liệu

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `dataRoot` | Thư mục gốc để lưu trữ dữ liệu người dùng (chỉ chế độ standalone) | `./data` | Bất kỳ đường dẫn thư mục hợp lệ nào |
| `skipContentCheck` | Bỏ qua kiểm tra nội dung mặc định mới | `false` | `true`, `false` |
| `enableDownloadableTokenizers` | Bật tải xuống tokenizer theo yêu cầu | `true` | `true`, `false` |

## Cấu Hình Ghi Log

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `logging.minLogLevel` | Mức log tối thiểu để hiển thị trong terminal | `0` (DEBUG) | (DEBUG = 0, INFO = 1, WARN = 2, ERROR = 3) |
| `logging.enableAccessLog` | Ghi log truy cập máy chủ vào tệp và console | `true` | `true`, `false` |

## [Cấu Hình Mạng](/Administration/remote-connections.md)

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `listen` | Bật lắng nghe các kết nối đến | `false` | `true`, `false` |
| `port` | Cổng lắng nghe của máy chủ | `8000` | Bất kỳ số cổng hợp lệ nào (1-65535) |
| `protocol.ipv4` | Bật lắng nghe trên giao thức IPv4 | `true` | `true`, `false`, `auto` |
| `protocol.ipv6` | Bật lắng nghe trên giao thức IPv6 | `false` | `true`, `false`, `auto` |
| `listenAddress.ipv4` | Lắng nghe trên một địa chỉ IPv4 cụ thể | `0.0.0.0` | Địa chỉ IPv4 hợp lệ |
| `listenAddress.ipv6` | Lắng nghe trên một địa chỉ IPv6 cụ thể | `'[::]'` | Địa chỉ IPv6 hợp lệ |
| `dnsPreferIPv6` | Ưu tiên IPv6 cho phân giải DNS | `false` | `true`, `false` |

## Cấu Hình SSL

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `ssl.enabled` | Bật mã hóa SSL/TLS và giao thức HTTPS | `false` | `true`, `false` |
| `ssl.keyPath` | Đường dẫn đến khóa riêng SSL (tương đối với thư mục máy chủ) | `"./certs/privkey.pem"` | Đường dẫn tệp hợp lệ |
| `ssl.certPath` | Đường dẫn đến chứng chỉ SSL (tương đối với thư mục máy chủ) | `"./certs/cert.pem"` | Đường dẫn tệp hợp lệ |
| `ssl.keyPassphrase` | Cụm mật khẩu cho khóa riêng SSL. Để trống nếu không yêu cầu | `""` | Bất kỳ chuỗi nào |

## Cấu Hình Bảo Mật

### Whitelist IP

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `whitelistMode` | Bật lọc whitelist IP | `true` | `true`, `false` |
| `enableForwardedWhitelist` | Kiểm tra các header được chuyển tiếp cho các IP được phép | `true` | `true`, `false` |
| `whitelist` | Danh sách các địa chỉ IP được phép | `["::1", "127.0.0.1"]` | Mảng các địa chỉ IP hợp lệ |
| `whitelistDockerHosts` | Tự động whitelist các IP máy chủ Docker | `true` | `true`, `false` |

### Whitelist Host

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `hostWhitelist.enabled` | Bật whitelist host | `false` | `true`, `false` |
| `hostWhitelist.scan` | Ghi log các yêu cầu đến từ các host không tin cậy | `true` | `true`, `false` |
| `hostWhitelist.hosts` | Danh sách các hostname tin cậy | `[]` | Mảng các hostname hợp lệ |

### Ghi Đè Bảo Mật

!!!danger
**VIỆC VÔ HIỆU HÓA CÁC BIỆN PHÁP BẢO MẬT LÀ RẤT KHÔNG KHUYẾN NGHỊ. VUI LÒNG ĐẢM BẢO BẠN HIỂU NHỮNG GÌ BẠN ĐANG LÀM TRƯỚC KHI THỰC HIỆN THAY ĐỔI.**
!!!

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `allowKeysExposure` | Cho phép hiển thị khóa API không che trong giao diện | `false` | `true`, `false` |
| `disableCsrfProtection` | Tắt bảo vệ CSRF (không khuyến nghị) | `false` | `true`, `false` |
| `securityOverride` | Tắt kiểm tra bảo mật khi khởi động (không khuyến nghị) | `false` | `true`, `false` |

## [Xác Thực Người Dùng](/Administration/multi-user.md)

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `basicAuthMode` | Bật xác thực cơ bản | `false` | `true`, `false` |
| `basicAuthUser.username` | Tên người dùng xác thực cơ bản | `"user"` | Bất kỳ chuỗi nào |
| `basicAuthUser.password` | Mật khẩu xác thực cơ bản | `"password"` | Bất kỳ chuỗi nào |
| `enableUserAccounts` | Bật chế độ đa người dùng | `false` | `true`, `false` |
| `enableDiscreetLogin` | Ẩn danh sách người dùng trên màn hình đăng nhập | `false` | `true`, `false` |
| `sessionTimeout` | Thời gian chờ phiên người dùng tính bằng giây | `-1` (vô hiệu hóa) | Bất kỳ số nào (-1 để vô hiệu hóa, 0 khi đóng trình duyệt, >0 cho thời gian chờ) |
| `perUserBasicAuth` | Sử dụng thông tin đăng nhập tài khoản cho xác thực cơ bản | `false` | `true`, `false` |

### Đăng Nhập Tự Động SSO

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `sso.autheliaAuth` | Bật đăng nhập tự động dựa trên Authelia. Xem: [SSO](/Administration/sso.md) | `false` | `true`, `false` |
| `sso.authentikAuth` | Bật đăng nhập tự động dựa trên Authentik. Xem: [SSO](/Administration/sso.md) | `false` | `true`, `false` |

## Cấu Hình Giới Hạn Tốc Độ

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `rateLimiting.preferRealIpHeader` | Sử dụng header X-Real-IP thay vì IP socket để giới hạn tốc độ | `false` | `true`, `false` |

## Cấu Hình Request Proxy

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `requestProxy.enabled` | Bật proxy cho các yêu cầu đi ra | `false` | `true`, `false` |
| `requestProxy.url` | URL máy chủ proxy | `null` | URL proxy hợp lệ (ví dụ: `"socks5://username:password@example.com:1080"`) |
| `requestProxy.bypass` | Các host để bỏ qua proxy | `["localhost", "127.0.0.1"]` | Mảng các hostname/IP |

## Cấu Hình CORS Proxy

!!!
Một proxy CORS được bật có thể được yêu cầu bởi một số tiện ích mở rộng. Nó không được yêu cầu bởi bất kỳ tính năng tích hợp nào.
!!!

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `enableCorsProxy` | Bật middleware proxy CORS | `false` | `true`, `false` |

## Cấu Hình Khởi Chạy Trình Duyệt

> Trước đây được gọi là cài đặt "Autorun".

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `browserLaunch.enabled` | Tự động mở trình duyệt khi khởi động máy chủ | `true` | `true`, `false` |
| `browserLaunch.browser` | Trình duyệt sử dụng để mở URL | `"default"` | `"default"`, `"chrome"`, `"firefox"`, `"edge"`, `"brave"` |
| `browserLaunch.hostname` | Ghi đè hostname để khởi chạy trình duyệt | `"auto"` | `"auto"`, bất kỳ hostname hợp lệ nào (ví dụ: `"localhost"`, `"st.example.com"`) |
| `browserLaunch.port` | Ghi đè cổng để khởi chạy trình duyệt | `-1` | `-1` (sử dụng cổng máy chủ), bất kỳ số cổng hợp lệ nào |
| `browserLaunch.avoidLocalhost` | Tránh sử dụng 'localhost' trong URL khởi chạy | `false` | `true`, `false` |

## Cấu Hình Hiệu Năng

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `performance.lazyLoadCharacters` | Tải dữ liệu nhân vật theo cách lazy-load | `true` | `true`, `false` |
| `performance.useDiskCache` | Bật bộ nhớ đệm đĩa cho thẻ nhân vật | `true` | `true`, `false` |
| `performance.memoryCacheCapacity` | Dung lượng bộ nhớ cache tối đa | `100mb` | Kích thước có thể đọc được (ví dụ: `100mb`, `1gb`) |

## Cấu Hình Cache Buster

!!!warning
Yêu cầu localhost hoặc một domain với HTTPS, nếu không sẽ không hoạt động!
!!!

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `cacheBuster.enabled` | Xóa bộ nhớ cache trình duyệt khi tải lần đầu hoặc sau khi tải lên tệp hình ảnh | `false` | `true`, `false` |
| `cacheBuster.userAgentPattern` | Chỉ xóa bộ nhớ cache cho các user agent khớp với mẫu regex được chỉ định. Ví dụ: `'firefox'` (không phân biệt chữ hoa chữ thường). | `''` | Bất kỳ chuỗi regex hợp lệ nào |

## Cấu Hình Thumbnailing

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `thumbnails.enabled` | Bật tạo thumbnail | `true` | `true`, `false` |
| `thumbnails.quality` | Chất lượng thumbnail JPEG | `95` | 0-100 |
| `thumbnails.format` | Định dạng hình ảnh cho thumbnail | `jpg` | `jpg`, `png` |
| `thumbnails.dimensions.bg` | Kích thước thumbnail nền | `[160, 90]` | Mảng hai số (chiều rộng, chiều cao) |
| `thumbnails.dimensions.avatar` | Kích thước thumbnail avatar | `[96, 144]` | Mảng hai số (chiều rộng, chiều cao) |
| `thumbnails.dimensions.persona` | Kích thước thumbnail persona | `[96, 144]` | Mảng hai số (chiều rộng, chiều cao) |

## Cấu Hình Sao Lưu

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `backups.chat.enabled` | Bật sao lưu chat tự động | `true` | `true`, `false` |
| `backups.chat.checkIntegrity` | Xác minh tính toàn vẹn của các tệp chat trước khi lưu | `true` | `true`, `false` |
| `backups.common.numberOfBackups` | Số lượng bản sao lưu được giữ lại | `50` | Bất kỳ số nguyên dương nào |
| `backups.chat.throttleInterval` | Khoảng thời gian điều tiết sao lưu (ms) | `10000` | Bất kỳ số nguyên dương nào |
| `backups.chat.maxTotalBackups` | Số lượng bản sao lưu chat tối đa được giữ lại | `-1` | Bất kỳ số nguyên dương nào hoặc -1 |

## [Cấu Hình Extensions](/extensions/index.md)

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `extensions.enabled` | Bật UI extensions | `true` | `true`, `false` |
| `extensions.autoUpdate` | Tự động cập nhật extensions (nếu được bật bởi extension manifest) | `true` | `true`, `false` |
| `extensions.models.autoDownload` | Bật tải xuống model tự động | `true` | `true`, `false` |
| `extensions.models.classification` | ID model HuggingFace cho phân loại | `"Cohee/distilbert-base-uncased-go-emotions-onnx"` | ID model hợp lệ |
| `extensions.models.captioning` | ID model HuggingFace cho chú thích hình ảnh | `"Xenova/vit-gpt2-image-captioning"` | ID model hợp lệ |
| `extensions.models.embedding` | ID model HuggingFace cho embeddings | `"Cohee/jina-embeddings-v2-base-en"` | ID model hợp lệ |
| `extensions.models.speechToText` | ID model HuggingFace cho speech-to-text | `"Xenova/whisper-small"` | ID model hợp lệ |
| `extensions.models.textToSpeech` | ID model HuggingFace cho text-to-speech | `"Xenova/speecht5_tts"` | ID model hợp lệ |

## [Server Plugins](/For_Contributors/Server-Plugins.md)

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `enableServerPlugins` | Bật plugins phía máy chủ | `false` | `true`, `false` |
| `enableServerPluginsAutoUpdate` | Cố gắng tự động cập nhật server plugins khi khởi động | `true` | `true`, `false` |

## [Cài Đặt Tích Hợp API](/Usage/API_Connections/index.md)

### Cấu Hình OpenAI

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `promptPlaceholder` | Thông báo mặc định cho các prompt trống | `"[Start a new chat]"` | Bất kỳ chuỗi nào |
| `openai.randomizeUserId` | Ngẫu nhiên hóa ID người dùng cho các lời gọi API | `false` | `true`, `false` |
| `openai.captionSystemPrompt` | Thông điệp hệ thống cho hoàn thành chú thích | `""` | Bất kỳ chuỗi nào |

### Cấu Hình MistralAI

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `mistral.enablePrefix` | Bật prefill phản hồi. **Tiền tố sẽ được echo lại trong phản hồi** | `false` | `true`, `false` |

### Cấu Hình Ollama

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `ollama.keepAlive` | Thời lượng giữ model hoạt động (giây) | `-1` | `-1` (vô hạn), `0` (gỡ bỏ ngay lập tức), số nguyên dương |
| `ollama.batchSize` | Kiểm soát tham số "num_batch" (batch size) của yêu cầu tạo | `-1` | `-1` (mặc định của model), số nguyên dương |

### Cấu Hình Claude

!!!warning **QUAN TRỌNG!**

Sử dụng cẩn thận và chỉ khi tiền tố prompt là tĩnh và không thay đổi giữa các yêu cầu. Macro \{\{random\}\}, lorebooks, vectors, summaries, v.v. có thể làm vô hiệu cache và bạn sẽ chỉ lãng phí tiền vào các cache miss. Hành vi có thể không thể dự đoán được và không có bảo đảm nào có thể hoặc sẽ được đưa ra.

Xem: [Prompt Caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)
!!!

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `claude.enableSystemPromptCache` | Bật cache prompt hệ thống | `false` | `true`, `false` |
| `claude.cachingAtDepth` | Bật cache lịch sử tin nhắn | `-1` | `-1` (vô hiệu hóa), `0` hoặc số nguyên dương |
| `claude.extendedTTL` | Sử dụng TTL 1h thay vì 5m mặc định. Lưu ý rằng điều này cũng làm tăng chi phí của yêu cầu. | `false` | `true`, `false` |

### Cấu Hình Google AI Studio

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `gemini.apiVersion` | Phiên bản endpoint API | `v1beta` | `v1beta`, `v1alpha` |

### Cấu Hình DeepL

| Cài Đặt | Mô Tả | Mặc Định | Giá Trị Cho Phép |
|---------|-------|----------|------------------|
| `deepl.formality` | Mức độ trang trọng của bản dịch | `"default"` | `"default"`, `"more"`, `"less"`, `"prefer_more"`, `"prefer_less"` |
