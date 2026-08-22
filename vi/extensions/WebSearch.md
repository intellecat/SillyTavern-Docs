---
route: /vi/extensions/websearch/
---

# Tìm kiếm Web

Thêm kết quả tìm kiếm web vào các LLM prompts.

!!! Note
Một số nguồn [Chat Completion](/Usage/API_Connections/openai.md) cung cấp chức năng tìm kiếm web tích hợp. Trong trường hợp này, extension này sẽ phần lớn là dư thừa. Kiểm tra panel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** cho toggle "Enable web search". Ví dụ, điều này có sẵn cho các backend Claude, Google AI Studio / Vertex AI, OpenRouter, Chutes và các backend khác.
!!!

## Các nguồn có sẵn

### Plugin Selenium

Yêu cầu một plugin server chính thức được cài đặt và bật.

Xem [SillyTavern-WebSearch-Selenium](https://github.com/SillyTavern/SillyTavern-WebSearch-Selenium) để biết thêm chi tiết.

Hỗ trợ các engine Google và DuckDuckGo.

### Extras API

Yêu cầu một module `websearch` và trình duyệt Chrome/Firefox được cài đặt trên máy chủ lưu trữ.

Hỗ trợ các engine Google và DuckDuckGo.

### SerpApi

Yêu cầu một khóa API.

Lấy khóa ở đây: <https://serpapi.com/dashboard>

### SearXNG

Yêu cầu một URL instance SearXNG (có thể private hoặc public). Sử dụng định dạng HTML cho kết quả tìm kiếm.

Chuỗi tùy chọn SearXNG: nhận được từ SearXNG - preferences - COOKIES - Copy preferences hash

Tìm hiểu thêm: <https://docs.searxng.org/>

### Tavily AI

Yêu cầu một khóa API.

Lấy khóa ở đây: <https://app.tavily.com/>

### KoboldCpp

URL KoboldCpp phải được cung cấp trong cài đặt Text Completion API. Phiên bản KoboldCpp phải >= 1.81.1 và WebSearch module phải được bật khi khởi động: bật Network => Enable WebSearch trong GUI launcher hoặc thêm `--websearch` vào command line.

Xem: <https://github.com/LostRuins/koboldcpp/releases/tag/v1.81.1>

### Serper

Yêu cầu một khóa API.

Lấy khóa ở đây: <https://serper.dev/>

### Z.AI

Yêu cầu một khóa API, đặt nó trong cài đặt Chat Completion API trước. Không tương thích với gói đăng ký Coding API!

Lấy khóa ở đây: <https://z.ai/manage-apikey/apikey-list/>

Tài liệu: <https://docs.z.ai/api-reference/tools/web-search>

## Cách sử dụng

1. Hãy chắc rằng bạn sử dụng phiên bản mới nhất của SillyTavern.
2. Cài đặt extension qua menu "Download Extensions & Assets" trong SillyTavern.
3. Mở cài đặt "Web Search" extension, đặt khóa API của bạn hoặc kết nối với Extras, và bật extension.
4. Các kết quả tìm kiếm web sẽ được thêm vào prompt một cách hữu cơ khi bạn chat. **Chỉ các tin nhắn người dùng kích hoạt tìm kiếm.**
5. Để bao gồm kết quả tìm kiếm một cách hữu cơ hơn, bao bọc các truy vấn tìm kiếm bằng dấu ngoặc kép đơn: ```Tell me about the `latest Ryan Gosling movie`.``` sẽ tạo ra truy vấn tìm kiếm `latest Ryan Gosling movie`.
6. Tùy chọn, cấu hình cài đặt theo ý thích của bạn.

## Cài đặt

### Chung

1. Enabled - chuyển đổi extension bật và tắt.
2. Sources = đặt nguồn kết quả tìm kiếm.
3. Cache Lifetime - khoảng bao lâu (tính bằng giây) kết quả tìm kiếm được lưu trữ trong bộ nhớ cache cho prompt của bạn. Mặc định = một tuần.

### Cài đặt Prompt

1. Prompt Budget - đặt dung lượng tối đa của văn bản được chèn (tính bằng ký tự văn bản, KHÔNG phải tokens). Quy tắc ngón tay cái: 1 token ~ 3-4 ký tự, điều chỉnh theo giới hạn bối cảnh của mô hình của bạn. Mặc định = 1500 ký tự.
2. Insertion Template - cách kết quả được chèn vào prompt. Hỗ trợ macro thông thường + macro đặc biệt: \{\{query\}\} cho truy vấn tìm kiếm và \{\{text\}\} cho kết quả tìm kiếm.
3. Injection Position - nơi kết quả đi trong prompt. Các tùy chọn giống như đối với Author's Note: là injection in-chat hoặc trước/sau system prompt.

### Kích hoạt Tìm kiếm

1. Use function tool - sử dụng [function calling](/For_Contributors/Function-Calling.md) để kích hoạt tìm kiếm hoặc cạo các trang web. Phải sử dụng một API Chat Completion được hỗ trợ và được bật trong cài đặt AI Response. **Vô hiệu hóa tất cả các phương pháp kích hoạt khác khi engaged.**
2. Use Backticks - bật kích hoạt tìm kiếm bằng các từ được bao quanh bởi dấu ngoặc kép đơn.
3. Use Trigger Phrases - bật kích hoạt tìm kiếm bằng các cụm từ trigger.
4. Regular expressions - cung cấp một regex có style JS để khớp với tin nhắn người dùng. Nếu regex khớp, tìm kiếm với một truy vấn nhất định sẽ được kích hoạt. Truy vấn tìm kiếm hỗ trợ `{{macros}}` và cú pháp $1 để tham chiếu nhóm được khớp. Ví dụ: regex `/what is happening in (.*)/i` cho truy vấn tìm kiếm `news in $1` sẽ khớp với một tin nhắn chứa `what is happening in New York` và kích hoạt tìm kiếm với truy vấn `news in New York`.
5. Trigger Phrases - thêm các cụm từ sẽ kích hoạt tìm kiếm, một cái một. Nó có thể ở bất cứ nơi nào trong tin nhắn, và truy vấn bắt đầu từ từ trigger và kéo dài đến tổng cộng "Max Words". Để loại trừ một tin nhắn cụ thể khỏi xử lý, nó phải bắt đầu bằng một khoảng thời gian, ví dụ: `.What do you think?`. Ưu tiên của trigger: đầu tiên theo thứ tự trong textbox, sau đó là ưu tiên đầu tiên trong tin nhắn người dùng.
6. Max Words - bao nhiêu từ được bao gồm trong truy vấn tìm kiếm (bao gồm cụm từ trigger). Google có giới hạn khoảng 32 từ trên mỗi prompt. Mặc định = 10 từ.

### Cạo Trang

1. Visit Links - văn bản sẽ được trích xuất từ các trang kết quả tìm kiếm được truy cập và lưu vào tệp attachment.
2. Visit Count - bao nhiêu liên kết sẽ được truy cập và phân tích cú pháp cho văn bản.
3. Visit Domain Blacklist - các tên miền trang web được loại trừ khỏi việc truy cập. Mỗi cái một.
4. File Header - template header file, được chèn ở đầu file văn bản, có một macro \{\{query\}\} bổ sung.
5. Block Header - template link block, được chèn với nội dung được phân tích cú pháp của mỗi liên kết. Sử dụng macro \{\{link\}\} cho URL trang và \{\{text\}\} cho nội dung trang.
6. Save Target - nơi để lưu kết quả cạo. Các tùy chọn có thể có: trigger message attachments, hoặc chat attachments của Data Bank, hoặc chỉ hình ảnh (nếu nguồn hỗ trợ).
7. Include Images - gắn các hình ảnh liên quan vào chat. Yêu cầu một nguồn hỗ trợ hình ảnh (xem bên dưới).

## Thêm thông tin

Các kết quả tìm kiếm từ truy vấn mới nhất sẽ vẫn được bao gồm trong prompt cho đến khi tìm kiếm hợp lệ tiếp theo được tìm thấy.
Nếu bạn muốn hỏi các câu hỏi bổ sung mà không vô tình kích hoạt tìm kiếm, hãy bắt đầu tin nhắn của bạn bằng một khoảng thời gian.

!!!info
Web Search function tool luôn ghi đè các trigger khác nếu được bật và khả dụng.
!!!

Ưu tiên của trigger (nếu nhiều được bật):

1. Backticks.
2. Regular expressions.
3. Trigger phrases.

Để loại bỏ tất cả các truy vấn trước đó khỏi xử lý, hãy bắt đầu tin nhắn người dùng bằng dấu chấm than, ví dụ: tin nhắn người dùng `!Now let's talk about...` sẽ loại bỏ cái này và mọi tin nhắn ở trên nó.

Extension này cũng cung cấp một slash command `/websearch` để sử dụng trong STscript. Thêm thông tin ở đây: [STscript Language Reference](/For_Contributors/st-script.md#extension-commands)

```stscript
/websearch (links=on|off snippets=on|off [query]) – thực hiện một truy vấn tìm kiếm web. Sử dụng named arguments để chỉ định những gì để trả lại - page snippets (mặc định: on), full parsed pages (mặc định: off) hoặc cả hai.

Ví dụ: /websearch links=off snippets=on how to make a sandwich
```

### Điều gì có thể được bao gồm trong kết quả tìm kiếm?

**Thesaurus:**

- Answer box: Câu trả lời trực tiếp cho câu hỏi.
- Knowledge graph: Kiến thức bách khoa học về chủ đề.
- Page snippets: Các trích dẫn liên quan từ các trang web.
- Relevant questions: Câu hỏi và câu trả lời cho các chủ đề tương tự.
- Images: Các hình ảnh liên quan.

#### SerpApi

1. Answer box.
2. Knowledge graph.
3. Page snippets (max 10).
4. Relevant questions (max 10).
5. Images (max 10).

#### Plugin Selenium và Extras API

1. Google - answer box, knowledge graph, page snippets.
2. DuckDuckGo - page snippets.

**Plugin Selenium** có thể cung cấp thêm hình ảnh.

#### SearXNG

1. Infobox.
2. Page snippets.
3. Images.

#### Tavily AI

1. Answer.
2. Page contents.
3. Images (up to 5).

#### KoboldCpp

1. Page titles.
2. Page snippets.

#### Serper

1. Answer box.
2. Knowledge graph.
3. Page snippets.
4. Relevant questions.
5. Images.

#### Z.AI

1. Page titles.
2. Page snippets.
