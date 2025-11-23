---
route: /usage/api-connections/google/
---

# Google Gemini

Gemini là LLM đa phương thức tiên tiến của Google, có sẵn thông qua một số API, bao gồm Google Vertex AI và Google AI Studio (trước đây là MakerSuite). Hướng dẫn này sẽ giúp bạn thiết lập kết nối Gemini API trong SillyTavern.

## Google AI Studio

AI Studio là cách nhanh nhất và thân thiện nhất với người dùng để dùng thử các mô hình Google AI mới nhất mà không cần thiết lập dự án Google Cloud Platform (GCP). Nó cung cấp một API key đơn giản mà bạn có thể sử dụng để truy cập các mô hình Gemini.

### Bước 1: Tạo Google AI Studio Key

1. Truy cập trang [Google AI Studio](https://aistudio.google.com/apikey) và đăng nhập bằng tài khoản Google của bạn.
2. Nhấp vào "Get API Key", chấp nhận các điều khoản và điều kiện.
3. Nhấp "Create API Key" để tạo API key của bạn.
4. Sao chép API key vào clipboard của bạn.

### Bước 2: Đưa API Key vào SillyTavern

1. Trong SillyTavern, vào trang "API Connections".
2. Chọn "Chat Completion" làm loại API.
3. Chọn "Google AI Studio" từ menu dropdown.
4. Nhập API key bạn đã sao chép trước đó vào hộp văn bản "API Key".
5. Nhấp nút "Connect" để lưu key.

Bây giờ bạn có thể sử dụng Google AI Studio API với SillyTavern.

## Google Vertex AI

Vertex AI là dịch vụ được cung cấp bởi Google Cloud Platform (GCP). Nó cung cấp quyền truy cập vào nhiều mô hình AI khác nhau, bao gồm cả series Gemini.

Có nhiều cách để thiết lập Vertex AI API, và các mô hình có sẵn có thể khác nhau tùy thuộc vào phương pháp được sử dụng.

### Service Account

Google Cloud Platform (GCP) yêu cầu service account để truy cập Vertex AI, các API key đơn giản sẽ không hoạt động. Một token sẽ được tạo từ file JSON của service account, sau đó sẽ được sử dụng để xác thực các yêu cầu đến Vertex AI API.

Bạn có thể tạo service account bằng cách làm theo các bước sau:

**Yêu cầu:**

1. Bạn phải có tài khoản Google Cloud Platform (GCP).
2. Bạn phải có một dự án được tạo trong tài khoản GCP của bạn.
3. Bạn phải bật billing cho dự án đó.

#### Bước 1: Bật Vertex AI API

Trước khi key của bạn có thể hoạt động, API phải được bật cho dự án của bạn.

1. Truy cập Google Cloud Console: <https://console.cloud.google.com/>
2. Đảm bảo dự án đúng được chọn trong thanh trên cùng.
3. Điều hướng đến trang Vertex AI API: <https://console.cloud.google.com/apis/library/aiplatform.googleapis.com>
4. Nếu chưa được bật, nhấp nút "Enable".

#### Bước 2: Tạo Service Account

Đây là định danh sẽ được sử dụng để truy cập Vertex AI API.

1. Trong Google Cloud Console, điều hướng đến trang "Service Accounts". Bạn có thể tìm kiếm nó trong thanh tìm kiếm trên cùng hoặc sử dụng liên kết trực tiếp này: <https://console.cloud.google.com/iam-admin/serviceaccounts>
2. Chọn dự án GCP của bạn và nhấp "+ CREATE SERVICE ACCOUNT".
3. Tên service account: Đặt một tên mô tả, như `my-vertex-ai-client`.
4. Nhấp "CREATE AND CONTINUE".
5. Cấp quyền truy cập cho service account này vào dự án: Trong dropdown "Role", tìm và chọn Vertex AI User. Role này cấp các quyền cần thiết để chạy mô hình mà không cấp quá nhiều quyền truy cập.
6. Nhấp "CONTINUE", sau đó nhấp "DONE".

#### Bước 3: Tạo JSON Key

Đây là file "mật khẩu" bạn cần. Nó chứa thông tin nhạy cảm, vì vậy đừng chia sẻ hoặc tải lên bất cứ đâu công khai.

1. Bây giờ bạn nên quay lại danh sách Service Accounts. Tìm tài khoản bạn vừa tạo (ví dụ: sillytavern-vertex-ai).
2. Nhấp menu ba chấm (⋮) ở phía bên phải của hàng đó và chọn "Manage keys".
3. Nhấp "ADD KEY" -> "Create new key".
4. Đảm bảo Key type được đặt thành JSON.
5. Nhấp "CREATE".

Một file .json sẽ ngay lập tức được tải xuống máy tính của bạn. Giữ nó an toàn, vì key này không thể phục hồi nếu bị mất.

#### Bước 4: Đưa Nội Dung JSON vào SillyTavern

File JSON bạn tải xuống chứa tất cả thông tin cần thiết để xác thực với Vertex AI API. Nó sẽ trông giống như thế này:

```json
{
    "type": "service_account",
    "project_id": "your-gcp-project-name",
    "private_key_id": "...",
    "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
    "client_email": "sillytavern-vertex-ai@your-gcp-project-name.iam.gserviceaccount.com",
    "client_id": "...",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "..."
}
```

1. Mở file .json bạn vừa tải xuống bằng trình soạn thảo văn bản đơn giản (như Notepad trên Windows, TextEdit trên Mac, hoặc VS Code).
2. Chọn tất cả văn bản trong file (Ctrl+A hoặc Cmd+A).
3. Sao chép văn bản vào clipboard của bạn (Ctrl+C hoặc Cmd+C).
4. Trong SillyTavern, vào trang "API Connections", chọn "Chat Completion" làm loại API, sau đó chọn "Google Vertex AI" từ menu dropdown. Chuyển phương thức xác thực sang "Service Account".
5. Dán toàn bộ nội dung đã sao chép vào hộp văn bản "Service Account JSON Content".
6. Nhấp nút "Validate JSON" để đảm bảo bạn đã sao chép đúng.
7. Cuối cùng, cuộn xuống và nhấp "Connect" ở cuối trang cài đặt API.

Bây giờ bạn có thể sử dụng Google Vertex AI API với SillyTavern.

### Express Mode

Express mode là cách nhanh nhất để bắt đầu sử dụng Generative AI trên Google Cloud. Nó cho phép bạn sử dụng Gemini API mà không cần thiết lập service account. Thay vào đó, bạn có thể sử dụng API key trực tiếp.

Xem tài liệu chính thức để biết thêm chi tiết: [Vertex AI in express mode overview](https://cloud.google.com/vertex-ai/generative-ai/docs/start/express-mode/overview).

#### Bước 1: Đảm bảo tài khoản của bạn đủ điều kiện cho Express Mode

Bạn phải có tài khoản Google chưa được sử dụng trước đó để tạo dự án Google Cloud.
Nếu bạn có dự án Google Cloud hiện có (bao gồm cả bản dùng thử miễn phí), bạn có thể tạo một dự án mới cho mục đích này.

#### Bước 2: Kích hoạt Vertex AI Express Mode

1. Truy cập trang web sau: [Vertex AI Studio](https://cloud.google.com/generative-ai-studio).
2. Nhấp vào "Try it free".
3. Chấp nhận các điều khoản và điều kiện và đăng nhập bằng tài khoản Google của bạn.
4. Chọn quốc gia của bạn và nhấp "Agree & start free". Đợi quá trình thiết lập hoàn tất.

#### Bước 3: Tạo API Key

1. Xác minh rằng Google Cloud console của bạn đang chạy ở Express Mode. Bạn sẽ thấy một banner ở góc trên bên trái của trang.
2. Nhấp vào liên kết "API Keys" trong thanh bên trái.
3. Nhấp vào nút "Create API Key".
4. Một API key mới sẽ được tạo. Sao chép key này vào clipboard của bạn.

#### Bước 4: Đưa API Key vào SillyTavern

1. Trong SillyTavern, vào trang "API Connections".
2. Chọn "Chat Completion" làm loại API.
3. Chọn "Google Vertex AI" từ menu dropdown.
4. Chuyển phương thức xác thực sang "Express Mode (API Key)".
5. Dán API key bạn đã sao chép trước đó vào hộp văn bản "API Key".
6. Nhấp nút "Connect" để lưu key.

Bây giờ bạn có thể sử dụng Google Vertex AI API ở Express Mode với SillyTavern.
