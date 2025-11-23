---
order: 70
route: /usage/prompts/tokenizer/
---

# Tokenizer

Tokenizer là một công cụ chia một đoạn văn bản thành các đơn vị nhỏ hơn gọi là tokens. Các tokens này có thể là từng từ riêng lẻ hoặc thậm chí các phần của từ, chẳng hạn như tiền tố, hậu tố, hoặc dấu câu. Một quy tắc chung là một token thường tương ứng với 3~4 ký tự văn bản.

SillyTavern cung cấp một tùy chọn "Best match" cố gắng khớp tokenizer bằng cách sử dụng các quy tắc sau tùy thuộc vào nhà cung cấp API được sử dụng.

Text Completion APIs **(có thể ghi đè)**:

1. NovelAI Clio: NerdStash tokenizer.
2. NovelAI Kayra: NerdStash v2 tokenizer.
3. Text Completion: API tokenizer (nếu được hỗ trợ) hoặc Llama tokenizer.
4. KoboldAI Classic / AI Horde: Llama tokenizer.
5. KoboldCpp: model API tokenizer.

Nếu bạn nhận được kết quả không chính xác hoặc muốn thử nghiệm, bạn có thể đặt một _tokenizer ghi đè_ cho SillyTavern sử dụng trong khi tạo yêu cầu đến AI backend:

1. None. Mỗi token được ước tính là ~3.3 ký tự, làm tròn lên đến số nguyên gần nhất. **Thử cái này nếu prompts của bạn bị cắt ở độ dài context cao.** Cách tiếp cận này được sử dụng bởi KoboldAI Lite.
2. Llama tokenizer. Được sử dụng bởi họ Llama 1/2 models: Vicuna, Hermes, Airoboros, v.v. **Chọn nếu bạn sử dụng model Llama 1/2.**
3. Llama 3 tokenizer. Được sử dụng bởi các model Llama 3/3.1. **Chọn nếu bạn sử dụng model Llama 3/3.1.**
4. NerdStash tokenizer. Được sử dụng bởi model Clio của NovelAI. **Chọn nếu bạn sử dụng model Clio.**
5. NerdStash v2 tokenizer. Được sử dụng bởi model Kayra của NovelAI. **Chọn nếu bạn sử dụng model Kayra.**
6. Mistral V1 tokenizer. Được sử dụng bởi họ Mistral models cũ hơn và các finetunes của chúng. **Chọn nếu bạn sử dụng model Mistral cũ hơn.**
7. Mistral Nemo tokenizer. Được sử dụng bởi họ Mistral Nemo models và các finetunes của chúng. **Chọn nếu bạn sử dụng model Mistral Nemo/Pixtral.**
8. Yi tokenizer. Được sử dụng bởi các model Yi. **Chọn nếu bạn sử dụng model Yi.**
9. Gemma tokenizer. Được sử dụng bởi các model Gemini/Gemma. **Chọn nếu bạn sử dụng model Gemma.**
10. DeepSeek tokenizer. Được sử dụng bởi các model DeepSeek (như R1). **Chọn nếu bạn sử dụng model DeepSeek.**
11. API tokenizer. Truy vấn generation API để lấy số lượng token trực tiếp từ model. Các backends đã biết hỗ trợ: Text Generation WebUI (ooba), koboldcpp, TabbyAPI, Aphrodite API. **Chọn nếu bạn sử dụng backend được hỗ trợ.**

Chat Completion APIs **(không thể ghi đè)**:

1. OpenAI: tokenizer phụ thuộc model qua [tiktoken](https://github.com/openai/tiktoken).
2. Claude: tokenizer phụ thuộc model qua [WebTokenizers](https://github.com/mlc-ai/tokenizers-cpp).
3. OpenRouter: Llama, Mistral, Gemma, Yi tokenizers cho các model tương ứng của chúng.
4. Google AI Studio: Gemma tokenizer.
5. AI21 API: Jamba tokenizer (yêu cầu tải xuống một lần).
6. Cohere API: Command-R hoặc Command-A tokenizer (yêu cầu tải xuống một lần).
7. MistralAI API: Mistral V1 hoặc V3 tokenizer (yêu cầu tải xuống một lần).
8. DeepSeek API: DeepSeek tokenizer (yêu cầu tải xuống một lần).
9. Fallback tokenizer: GPT-3.5 turbo tokenizer.

#### Tokenizers bổ sung

Các tokenizers này không được bao gồm trong cài đặt mặc định do kích thước của chúng. Một lần tải xuống được yêu cầu khi chúng được sử dụng lần đầu tiên.

1. Qwen2 tokenizer.
2. Command-R / Command-A tokenizers. Được sử dụng bởi nguồn Cohere trong Chat Completion.
3. Mistral V3 (Nemo) tokenizer. Được sử dụng bởi nguồn MistralAI trong Chat Completion (các model Nemo và Pixtral).
4. DeepSeek (deepseek-chat) tokenizer. Được sử dụng bởi nguồn DeepSeek trong Chat Completion.

Nếu bạn không muốn sử dụng tải xuống internet, tùy chọn opt-out tồn tại trong config.yaml: `enableDownloadableTokenizers`. Đặt thành `false` để vô hiệu hóa tải xuống.

Bạn cũng có thể tải xuống tokenizers thủ công từ repository [SillyTavern-Tokenizers](https://github.com/SillyTavern/SillyTavern-Tokenizers). Tải xuống các tệp JSON và đặt chúng trong thư mục con `_cache` của data root của bạn, đường dẫn là `./data/_cache` theo mặc định. Tạo thư mục `_cache` nếu nó không tồn tại. Sau đó, khởi động lại server SillyTavern để khởi tạo lại tokenizers.

Nếu model tokenizer bắt buộc không được cached và tải xuống bị vô hiệu hóa, một fallback tokenizer (Llama 3) sẽ được sử dụng để đếm.

### Token Padding

!!! Áp dụng cho: Text Completion APIs
SillyTavern sẽ luôn sử dụng tokenizer khớp cho các Chat Completion models, vì vậy không cần token padding.
!!!

Trừ khi SillyTavern sử dụng một tokenizer được cung cấp bởi remote backend API chạy model, tất cả số lượng token được giả định trong quá trình tạo prompt được ước tính dựa trên loại [tokenizer](#tokenizer) được chọn.

Vì kết quả của tokenization có thể không chính xác trên các kích thước context gần với giá trị tối đa được định nghĩa bởi model, một số phần của prompt có thể bị cắt bớt hoặc bỏ qua, điều này có thể ảnh hưởng tiêu cực đến sự mạch lạc của các định nghĩa nhân vật.

Để ngăn chặn điều này, SillyTavern phân bổ một phần kích thước context làm padding để tránh thêm nhiều chat items hơn model có thể chứa. Nếu bạn thấy rằng một số phần của prompt bị cắt bớt ngay cả với tokenizer khớp nhất được chọn, hãy điều chỉnh padding để description không bị truncated.

Bạn có thể nhập giá trị âm cho reverse padding, cho phép phân bổ nhiều hơn số lượng token tối đa đã đặt.
