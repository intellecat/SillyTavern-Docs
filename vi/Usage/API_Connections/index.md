---
order: 150
icon: repo-forked
expanded: false
route: /usage/api-connections/
---

# Kết Nối API

SillyTavern có thể kết nối với nhiều loại LLM API khác nhau.
Dưới đây là mô tả về điểm mạnh, điểm yếu và các trường hợp sử dụng tương ứng của chúng.

## ELI5: Chat Completions vs Text Completions

Khi bạn lần đầu tiên điều hướng đến trang "API Connections" trong ST, bạn sẽ nhận thấy một tùy chọn dropdown để chọn giữa các tùy chọn sử dụng cách đặt tên như "Chat Completion" và "Text Completion". Việc hiểu điều này nghĩa là gì sẽ hữu ích.

Nó không phải là gì: Dễ nghĩ "Text Completion" là các mô hình local và "Chat Completion" là các LLM dựa trên cloud, nhưng không phải như vậy. Cũng không phải là ví dụ "Novel AI" hay "Kobold" thực sự là một loại mô hình riêng biệt, mặc dù chúng là các tùy chọn riêng biệt trong dropdown API trong ST. Bạn có thể ép các mô hình vào các cấu trúc API khác nhau với backend phù hợp, nhưng đó không phải là trọng tâm của phần này.

Khi bạn gửi tin nhắn bằng ST, cuộc trò chuyện của bạn, mô tả nhân vật và các prompt khác như lorebook hoặc ghi chú của tác giả được xây dựng thành một "prompt" duy nhất để gửi đến mô hình. "Loại" API cho mô hình bạn đang sử dụng quyết định chính xác cách prompt này sẽ được xây dựng (điều mà ST tự động xử lý cho bạn ở nền - bạn có thể mở terminal ST của mình và xem chính xác prompt được gửi đến AI trông như thế nào).

### Chat Completions

Một mô hình Chat Completion, như tên gọi của nó, sẽ cấu trúc prompt của bạn thành một chuỗi tin nhắn giữa User (bạn) và Assistant (AI) hoặc System (trung lập). Các mô hình được đào tạo cho Chat Completion giúp tạo ra cảm giác của một "Cuộc trò chuyện", với AI "phản hồi" tin nhắn cuối cùng. Khi bạn sử dụng trang web ChatGPT, bạn đang xử lý với Chat Completions API ở nền.

### Text Completions (a.k.a just "Completions")

Mặt khác, một Text Completion, và một lần nữa như tên gọi của nó, sẽ chuyển đổi prompt của bạn thành một chuỗi dài và mô hình sẽ chỉ cố gắng tiếp tục điều này (như, theo đúng nghĩa đen hãy tưởng tượng tất cả văn bản của bạn, hàng trăm tin nhắn của bạn, tất cả định dạng của bạn, newline, v.v. được nén thành một câu rất dài).

Nếu các tin nhắn của bạn trong ST được định dạng dưới dạng một chuỗi tin nhắn giữa YourPersona: và Character:, mô hình Text Completion sẽ cố gắng tiếp tục mẫu này và ST sẽ hiển thị nó dưới dạng tin nhắn trò chuyện mới cho bạn, nhưng thực sự mô hình chỉ cố gắng tiếp tục văn bản. Nếu bạn cung cấp đầu vào "The Sun rises in the", một mô hình text completion có khả năng sẽ hoàn thành tin nhắn đó cho bạn với "East".

Hầu hết các mô hình Text Completion có "Instruct Template" được đề xuất (thường được đề cập trong tài liệu hoặc trang tải xuống của mô hình) giúp chúng "phản hồi" tin nhắn và hướng dẫn, giống như mô hình Chat Completion. ST thường có hầu hết (nếu không phải tất cả) các Instruct Template có sẵn để bạn chọn từ trang "Advanced Formatting".

## API Local

- Các LLM API này có thể được chạy trên PC của bạn.
- Chúng miễn phí sử dụng và không có bộ lọc nội dung.
- Quá trình cài đặt có thể phức tạp (**Nhóm phát triển SillyTavern không cung cấp hỗ trợ cho việc này**).
- Yêu cầu tải xuống riêng các mô hình LLM từ [HuggingFace](https://huggingface.co/models?other=LLM) có thể từ 5-50GB mỗi mô hình.
- Hầu hết các mô hình không mạnh bằng các cloud LLM API.

### KoboldCpp

- API dễ sử dụng với khả năng offload CPU (hữu ích cho người dùng có VRAM thấp) và streaming
- Chạy từ một file nhị phân duy nhất trên Windows, Mac và Linux
- Hỗ trợ các mô hình GGUF
- Chậm hơn các loader chỉ GPU như AutoGPTQ và Exllama/v2
- [GitHub](https://github.com/LostRuins/koboldcpp), [Hướng dẫn thiết lập](/Usage/API_Connections/koboldcpp.md)

### llama.cpp

- Nguồn gốc từ đó KoboldCpp và Ollama được fork
- Cung cấp các file nhị phân được biên dịch sẵn và tùy chọn biên dịch từ nguồn
- Hỗ trợ các mô hình GGUF
- Giao diện CLI nhẹ cho llama-server
- [GitHub](https://github.com/ggml-org/llama.cpp)

### Ollama

- Dễ thiết lập và sử dụng nhất trong tất cả các API dựa trên llama.cpp
- Một [catalog](https://ollama.com/library) gọn gàng các mô hình có sẵn để tải xuống một cú nhấp chuột
- Hỗ trợ các mô hình GGUF được gói trong định dạng riêng của Ollama
- [GitHub](https://github.com/ollama/ollama), [Website](https://ollama.com/)

### Oobabooga TextGeneration WebUI

- Giao diện người dùng Gradio tất cả trong một với streaming
- Hỗ trợ rộng rãi nhất cho các mô hình được lượng tử hóa (AWQ, Exl2, GGML, GGUF, GPTQ) và FP16
- Có sẵn bộ cài đặt một cú nhấp chuột
- Cập nhật thường xuyên, đôi khi có thể phá vỡ khả năng tương thích với SillyTavern
- [GitHub](https://github.com/oobabooga/text-generation-webui#one-click-installers)

**Cách đúng để kết nối SillyTavern với OpenAI API mới của Ooba:**

1. Đảm bảo bạn đang ở bản cập nhật mới nhất của TextGen của Oobabooga (tính đến ngày 14 tháng 11 năm 2023).
2. Chỉnh sửa file CMD_FLAGS.txt và thêm cờ `--api` vào đó. Sau đó khởi động lại máy chủ của Ooba.
3. Kết nối ST với `http://localhost:5000/` (theo mặc định) mà không chọn hộp 'Legacy API'. Bạn có thể xóa hậu tố `/v1` khỏi URL mà console của Ooba cung cấp cho bạn.

*Bạn có thể thay đổi cổng lưu trữ API bằng cờ `--api-port 5001`, trong đó 5001 là cổng tùy chỉnh của bạn.*

### TabbyAPI

- API nhẹ dựa trên [Exllamav2](https://github.com/turboderp/exllamav2) với streaming
- Hỗ trợ các mô hình Exl2, GPTQ và FP16
- [Extension chính thức](https://github.com/theroyallab/ST-tabbyAPI-loader) cho phép tải/dỡ mô hình trực tiếp từ SillyTavern
- Không được đề xuất cho người dùng có VRAM thấp (không có CPU offloading)
- [GitHub](https://github.com/theroyallab/tabbyAPI), [Hướng dẫn thiết lập](/Usage/API_Connections/tabbyapi.md)

### KoboldAI Classic (không được dùng nữa, bị bỏ rơi)

- Chạy trên PC của bạn, 100% riêng tư, có nhiều mô hình có sẵn
- Cung cấp quyền kiểm soát trực tiếp nhất đối với cài đặt tạo của AI
- Yêu cầu lượng lớn VRAM trong GPU của bạn (6-24GB, tùy thuộc vào mô hình LLM)
- Mô hình giới hạn ở context 2k
- Không có streaming
- Các phiên bản KoboldAI phổ biến:
  - [Henky's United](https://github.com/henk717/KoboldAI)
  - [0cc4m's 4bit-supporting United](https://github.com/0cc4m/KoboldAI)

## Cloud LLM APIs

- Các LLM API này được chạy như các dịch vụ cloud và không yêu cầu tài nguyên trên PC của bạn
- Chúng mạnh hơn/thông minh hơn hầu hết các LLM local
- Tuy nhiên, tất cả chúng đều có bộ lọc nội dung ở các mức độ khác nhau, và hầu hết yêu cầu thanh toán

### AI Horde

- SillyTavern có thể truy cập API này ngay lập tức mà không cần cài đặt bổ sung
- Sử dụng GPU của các tình nguyện viên cá nhân (Horde Workers) để xử lý phản hồi cho đầu vào trò chuyện của bạn
- Phụ thuộc vào Worker về thời gian chờ tạo, cài đặt AI và các mô hình có sẵn
- [Website](https://aihorde.net/), [Hướng dẫn thiết lập](./horde.md)

### OpenAI (ChatGPT)

- Dễ thiết lập và lấy API key
- Yêu cầu thanh toán trước cho tín dụng và tính phí theo prompt
- Rất logic. Phong cách sáng tạo có thể lặp đi lặp lại và dự đoán được
- Hầu hết các mô hình mới hơn (gpt-4-turbo, gpt-4o) hỗ trợ đa phương thức
- [Website](https://platform.openai.com/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#openai)

### Claude (by Anthropic)

- Được đề xuất cho người dùng muốn cuộc trò chuyện AI của họ có phong cách viết sáng tạo, độc đáo
- Yêu cầu thanh toán trước cho tín dụng và tính phí theo prompt
- Các mô hình mới nhất (Claude 3) hỗ trợ đa phương thức
- Yêu cầu phong cách prompting cụ thể và sử dụng [prefills](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prefill-claudes-response) để điều khiển phản hồi
- [Website](https://console.anthropic.com/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#claude)

### Google AI Studio và Vertex AI

- Có gói miễn phí với giới hạn tốc độ (Gemini Flash), có thể yêu cầu thông tin thanh toán
- [AI Studio](https://aistudio.google.com/) thường có các mô hình và tính năng mới nhất
- [Vertex AI](https://console.cloud.google.com/vertex-ai/studio) phức tạp hơn để thiết lập, nhưng ổn định hơn
- [Hướng dẫn thiết lập](/Usage/API_Connections/google.md)

### Mistral (by Mistral AI)

- Các mô hình hiệu quả với các kích thước và trường hợp sử dụng khác nhau. Bạn có thể tạo tài khoản và API key trên [platform của họ](https://console.mistral.ai/api-keys/).
- Từ kích thước context 32k đến 128k cho sử dụng chung, và 32k đến 256k cho coding.
- Gói miễn phí với giới hạn tốc độ.
- Kiểm duyệt hợp lý, với các nguyên tắc chính của Mistral là trung lập và trao quyền cho người dùng, thông tin thêm [tại đây](https://mistral.ai/terms/).
- [Website](https://console.mistral.ai/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#mistral-ai)

### OpenRouter

- Cung cấp API thống nhất để truy cập tất cả các LLM chính trên thị trường
- Hệ thống tín dụng trả theo token, cũng như các mô hình miễn phí với yêu cầu hàng ngày giới hạn
- Không có kiểm duyệt bắt buộc, trừ khi được yêu cầu bởi nhà cung cấp LLM
- [Website](https://openrouter.ai), [Hướng dẫn thiết lập](/Usage/API_Connections/OpenRouter.md)

### DeepSeek

- Cung cấp quyền truy cập vào các phiên bản mới nhất của các mô hình DeepSeek V3 (`deepseek-chat`) và DeepSeek R1 (`deepseek-reasoner`) rất phổ biến
- Yêu cầu thanh toán cho tín dụng ($2 tối thiểu), nhưng các mô hình khá rẻ so với chất lượng của chúng
- Không có kiểm duyệt trên API, nhưng các mô hình có thể từ chối một số prompt nhất định
- [Website](https://platform.deepseek.com/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#deepseek)

### AI21

- Cung cấp quyền truy cập vào các mô hình mở của Jamba Family
- Có bản dùng thử miễn phí ($10 trong ba tháng), sau đó yêu cầu thanh toán hàng tháng theo token
- [Website](https://ai21.com/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#ai21)

### Cohere

- Cung cấp quyền truy cập vào các mô hình mới nhất từ Cohere (command-r, command-a, c4ai-aya, v.v.)
- Có gói miễn phí (Trial Keys) với đủ giới hạn tốc độ cho sử dụng thông thường
- [Website](https://cohere.com/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#cohere)

### Perplexity

- Cung cấp quyền truy cập vào các mô hình Perplexity Sonar hỗ trợ online độc đáo qua API của họ
- Yêu cầu cấu hình billing và mua tín dụng
- [Website](https://perplexity.ai/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#perplexity)

### Mancer AI

- Dịch vụ lưu trữ các mô hình không bị hạn chế của nhiều họ khác nhau
- Sử dụng 'tín dụng' để trả tiền cho token trên các mô hình khác nhau
- Không ghi lại prompt theo mặc định, nhưng bạn có thể bật nó để nhận giảm giá tín dụng cho token.
- Sử dụng API tương tự như `Oobabooga TextGeneration WebUI`, xem [Mancer docs](https://mancer.tech/docs/clients/#sampling-parameters) để biết chi tiết.
- [Website](https://mancer.tech/), [Hướng dẫn thiết lập](/Usage/API_Connections/mancer.md)

### DreamGen

- Các mô hình không bị kiểm duyệt được điều chỉnh cho viết sáng tạo có thể điều khiển
- Tín dụng miễn phí hàng tháng, cũng như đăng ký trả phí
- Các mô hình từ 7B đến 70B
- [Hướng dẫn thiết lập](DreamGen.md)

### Pollinations

- Không yêu cầu thiết lập, có thể sử dụng ngay lập tức
- Cung cấp quyền truy cập vào nhiều mô hình miễn phí
- Đầu ra đôi khi có thể bao gồm quảng cáo với liên kết đến các dịch vụ bên thứ ba

### NovelAI

- Không có bộ lọc nội dung, mô hình mới nhất dựa trên Llama 3
- Yêu cầu đăng ký trả phí, gói xác định độ dài context tối đa
- [Website](https://novelai.net/), [Hướng dẫn thiết lập](/Usage/API_Connections/novelai.md)

### Electron Hub

- Một API key mở khóa các mô hình từ nhiều nhà cung cấp (OpenAI, Anthropic, DeepSeek, v.v.) để tạo văn bản và hình ảnh
- $0.25 tín dụng miễn phí mỗi ngày, có gói trả phí
- [Website](https://www.electronhub.ai/), [Hướng dẫn thiết lập](/Usage/API_Connections/openai.md#electron-hub)

### AI/ML API

- API thống nhất cho hơn 300 mô hình bao gồm Claude, GPT-4o, Gemini, LLaMA 3, Mistral và các mô hình khác
- Có gói miễn phí với giới hạn tốc độ, gói đăng ký và tùy chọn trả theo mức sử dụng
- [Website](https://aimlapi.com), [Docs](https://docs.aimlapi.com), [Models](https://aimlapi.com/models)
