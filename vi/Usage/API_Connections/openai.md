---
order: 20
route: /vi/usage/api-connections/openai/
---

# Chat Completions

## Hướng dẫn cụ thể theo nguồn

!!!warning **Quan trọng!**
Hầu hết các nền tảng API cho phép bạn xem API key đã tạo chỉ một lần, tại thời điểm tạo. Nếu bạn mất nó, bạn sẽ cần tạo key mới. Hãy đảm bảo giữ nó an toàn!
!!!

### OpenAI

Sử dụng nền tảng nhà phát triển của OpenAI để truy cập các mô hình OpenAI khác nhau, bao gồm gpt-4o, gpt-4.1, o3, v.v.

**Cách lấy API key:**

1. Truy cập [OpenAI](https://platform.openai.com/) và đăng nhập.
2. Sử dụng tùy chọn "[View API keys](https://platform.openai.com/account/api-keys)" để tạo API key mới.

### Claude

Claude là một họ mô hình AI được phát triển bởi Anthropic. Bạn có thể truy cập các mô hình Claude thông qua Anthropic console.

**Cách lấy API key:**

1. Truy cập [Anthropic Console](https://console.anthropic.com/) và đăng nhập.
2. Sử dụng phần "[Get API Key](https://console.anthropic.com/settings/keys)" để tạo API key mới.

### Mistral AI

Mistral AI là một nhóm phát triển cả các mô hình mở và độc quyền với tiêu chuẩn khoa học cao và tập trung vào sự cởi mở. Bạn có thể chạy các mô hình của họ cục bộ hoặc thông qua dịch vụ API của họ, La Plateforme.

**Cách lấy API key:**

1. Bước đầu tiên là tạo tài khoản trên [La Plateforme](https://console.mistral.ai/).
2. Sau khi hoàn tất, bạn có thể chọn một [plan](https://console.mistral.ai/billing/plans) và thiết lập thông tin thanh toán của bạn hoặc chọn Free Tier.
3. Tiếp theo, bạn có thể tạo [API key](https://console.mistral.ai/api-keys) của mình. Bạn có thể cần đợi vài phút trước khi key trở nên hợp lệ!

### DeepSeek

DeepSeek Platform cung cấp quyền truy cập vào các mô hình DeepSeek mới nhất thông qua API. Họ cung cấp nhiều mô hình, bao gồm DeepSeek V3 và DeepSeek R1.

**Cách lấy API key:**

1. Đăng ký trên [DeepSeek Platform](https://platform.deepseek.com/).
2. Sau khi đăng ký và nạp tiền vào tài khoản của bạn, bạn có thể tạo API key trong phần "[API keys](https://platform.deepseek.com/api_keys)".

### AI21

AI21 Labs cung cấp nhiều mô hình AI, bao gồm series Jamba hàng đầu của họ. Bạn có thể truy cập các mô hình của họ thông qua AI21 Studio API.

**Cách lấy API key:**

1. Truy cập [AI21 Studio](https://studio.ai21.com/) và đăng nhập.
2. Điều hướng đến phần "Settings => API Keys" để tạo API key mới.

### Cohere

Cohere cung cấp một bộ mô hình AI cho nhiều tác vụ khác nhau, bao gồm tạo văn bản và embeddings. Bạn có thể truy cập các mô hình của họ thông qua Cohere API.

**Cách lấy API key:**

1. Truy cập [Cohere](https://cohere.com/) và đăng nhập.
2. Điều hướng đến phần "[API Keys](https://dashboard.cohere.com/api-keys)" trong cài đặt tài khoản của bạn để tạo API key mới.

### Perplexity

Perplexity AI cung cấp quyền truy cập vào các mô hình Sonar hỗ trợ online thông qua API của họ để nghiên cứu và truy xuất thông tin theo thời gian thực.

Hướng dẫn Getting Started chính thức: [Perplexity Quickstart](https://docs.perplexity.ai/getting-started/quickstart)

**Cách lấy API key:**

1. Truy cập [Perplexity](https://perplexity.ai/) và đăng nhập.
2. Vào phần "[API billing](https://www.perplexity.ai/account/api/billing)" để mua tín dụng cho việc sử dụng API.
3. Điều hướng đến phần "[API keys](https://www.perplexity.ai/account/api/keys)" trong cài đặt để tạo API key mới.

### Fireworks AI

Fireworks AI là một nền tảng hiệu suất cao cung cấp quyền truy cập nhanh, tiết kiệm chi phí vào các mô hình ngôn ngữ mã nguồn mở tiên tiến. Nền tảng cung cấp triển khai serverless với các API tương thích OpenAI và hỗ trợ cửa sổ context lên đến 256,000 token.

**Cách lấy API key:**

1. Truy cập [Fireworks AI](https://fireworks.ai/) và tạo tài khoản hoặc đăng nhập.
2. Điều hướng đến [trang API Keys](https://app.fireworks.ai/settings/users/api-keys) trong cài đặt tài khoản của bạn.
3. Nhấp "Create API key" và cung cấp tên mô tả (ví dụ: "SillyTavern").

## Electron Hub

Electron Hub là một nền tảng tương thích OpenAI thống nhất cung cấp quyền truy cập vào các mô hình từ nhiều nhà cung cấp thông qua một API key duy nhất.

**Cách lấy API key:**

1. Tạo tài khoản tại [Electron Hub](https://playground.electronhub.ai/console).
2. Tạo API key từ trang **Console → API Keys**.

## Custom OpenAI-compatible endpoint

!!!warning
Điều quan trọng cần lưu ý là chúng tôi không cung cấp hỗ trợ cho các vấn đề có thể có!
Chúng tôi không đảm bảo khả năng tương thích với mọi endpoint API có thể!
!!!

!!!
Nếu bạn dự định sử dụng tính năng này để sử dụng endpoint local, như TabbyAPI, Oobabooga, Aphrodite hoặc bất kỳ thứ gì giống như vậy, bạn có thể muốn kiểm tra [khả năng tương thích tích hợp cho chúng](/Usage/API_Connections/index.md) thay thế. Tính năng custom endpoint chủ yếu dành cho việc sử dụng với các dịch vụ và chương trình khác mà expose một endpoint Chat Completion API tương thích OpenAI.

Hầu hết các Text Completion API hỗ trợ các tùy chọn tùy chỉnh lớn hơn nhiều so với các tiêu chuẩn của OpenAI cho phép. Các tùy chọn tùy chỉnh lớn hơn này, chẳng hạn như sampler Min-P, có thể đáng để người dùng SillyTavern kiểm tra, điều này có thể cải thiện đáng kể chất lượng của các lần tạo.
!!!

Bạn có thể cấu hình một endpoint thay thế cho backend Chat Completions. Endpoint tùy chỉnh này có thể kết nối với bất kỳ máy chủ nào hỗ trợ schema OpenAI API chung.

Ví dụ về các backend tương thích bao gồm:

* [LM Studio](https://lmstudio.ai/)
* [LiteLLM](https://www.litellm.ai/)
* [LocalAI](https://localai.io/)

### Kết Nối

Để truy cập tính năng này:

1. Chuyển sang loại API 'Chat Completion'
2. Chọn 'Custom (OpenAI-compatible)' cho 'Chat Completion Source'

Nhập URL endpoint tùy chỉnh và API key nếu được yêu cầu. Ví dụ, TabbyAPI yêu cầu API key để xác thực.

!!!tip
**Gợi ý:** Nếu bạn gặp vấn đề kết nối, hãy thử thêm `/v1` vào cuối URL endpoint. KHÔNG thêm hậu tố `/chat/completions`.
!!!

### Chọn Mô Hình

Nếu custom API triển khai endpoint `/v1/models` để cung cấp danh sách các mô hình có sẵn, bạn có thể chọn từ danh sách dropdown. Nếu không, hãy sử dụng trường văn bản để nhập thủ công ID mô hình.

Chọn 'Bypass API status check' để ngăn SillyTavern cảnh báo bạn về endpoint API không hoạt động. Bật tùy chọn này nếu endpoint API của bạn hoạt động đúng nhưng SillyTavern tiếp tục hiển thị cảnh báo.

Nhấp "Test Message" để xác minh kết nối bằng cách gửi một prompt đơn giản đến mô hình.

## Prompt Post-Processing

!!!warning
**Lưu ý:** Tool Calling không được hỗ trợ khi tùy chọn Post-Processing với "no tools" được sử dụng!
!!!

Một số endpoint có thể áp đặt các hạn chế cụ thể về định dạng của các prompt đến, chẳng hạn như yêu cầu chỉ một system message hoặc các role xen kẽ nghiêm ngặt.

SillyTavern cung cấp các bộ chuyển đổi prompt tích hợp để giúp đáp ứng các yêu cầu này (từ ít đến hạn chế nhất):

1. None - không áp dụng xử lý rõ ràng trừ khi được yêu cầu nghiêm ngặt bởi API
2. Merge consecutive messages from the same role
3. Semi-strict - gộp các role và chỉ cho phép một system message tùy chọn
4. Strict - gộp các role, chỉ cho phép một system message tùy chọn và yêu cầu user message phải là đầu tiên
5. Single user message - gộp tất cả tin nhắn từ tất cả các role thành một user message duy nhất

Merge, semi-strict và strict cũng loại bỏ bất kỳ tool call nào khỏi prompt, trừ khi biến thể "with tools" được chọn. Điều này hữu ích cho các API không hỗ trợ tool calling và các prompt hiện có của bạn chứa tool call.

Các tùy chọn ít hạn chế hơn không có hiệu lực đối với các endpoint hạn chế hơn được triển khai trong SillyTavern ngoài "Custom OpenAI-compatible"; Custom có thể báo lỗi khi yêu cầu không hợp lệ.

Trong chế độ strict, nếu không có user message nào tồn tại trước assistant message đầu tiên, thì `promptPlaceholder` từ `config.yaml` sẽ được chèn vào, mặc định là "\[Start a new chat]".
