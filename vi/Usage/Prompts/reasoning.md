---
order: 40
tags: ['>=1.12.12']
route: /usage/prompts/reasoning/
---

# Reasoning

Trong các language models, reasoning (còn được gọi là model thinking) đề cập đến một kỹ thuật chain-of-thought (CoT) phản ánh quá trình giải quyết vấn đề của con người thông qua phân tích từng bước. SillyTavern cung cấp một số tính năng làm cho việc sử dụng các reasoning models hiệu quả và nhất quán hơn trên các backend được hỗ trợ.

## Các vấn đề phổ biến

1. Khi sử dụng reasoning models, quá trình reasoning nội bộ của model tiêu thụ một phần giới hạn token response của bạn, ngay cả khi reasoning này không được hiển thị trong output cuối cùng (ví dụ: o3-mini hoặc Gemini Thinking). Nếu bạn nhận thấy các response của bạn đang quay lại không đầy đủ hoặc trống, bạn nên thử điều chỉnh cài đặt Max Response Length được tìm thấy trong panel **<i class="fa-solid fa-sliders"></i> AI Response Configuration**. Đối với reasoning models, điển hình là sử dụng giới hạn token cao hơn đáng kể - từ 1024 đến 4096 tokens - so với các conversational models tiêu chuẩn.

## Cấu hình

!!!
Hầu hết các cài đặt liên quan đến reasoning có thể được cấu hình trong phần "Reasoning" của panel **<i class="fa-solid fa-font"></i> Advanced Formatting**.
!!!

Các reasoning blocks xuất hiện trong chat dưới dạng các phần tin nhắn có thể thu gọn. Chúng có thể được thêm thủ công, tự động bởi backend, hoặc thông qua phân tích response (xem bên dưới).

Theo mặc định, reasoning blocks được thu gọn để tiết kiệm không gian. Nhấp vào một block để mở rộng và xem nội dung của nó. Bạn có thể đặt các blocks tự động mở rộng bằng cách bật **Auto-Expand** trong cài đặt reasoning.

Khi một reasoning block được mở rộng, bạn có thể sao chép hoặc chỉnh sửa nội dung của nó bằng các nút **<i class="fa-solid fa-copy"></i> Copy** và **<i class="fa-solid fa-pencil"></i> Edit**.

Một số models hỗ trợ reasoning, nhưng sẽ không gửi lại suy nghĩ của chúng. Vẫn có thể hiển thị reasoning block với thời gian reasoning cho những models đó bằng cách chuyển đổi cài đặt **Show Hidden**.

## Thêm Reasoning

### Thủ công

Thêm một reasoning block vào bất kỳ tin nhắn nào thông qua menu **<i class="fa-solid fa-pencil"></i> Message Edit**. Nhấp vào **<i class="fa-solid fa-lightbulb"></i>** trong khi chỉnh sửa để thêm một phần reasoning. Các extensions của bên thứ ba cũng có thể thêm reasoning bằng cách viết vào trường `extra.reasoning` của đối tượng message trước khi thêm nó vào chat.

### Với một Command

Sử dụng lệnh STscript `/reasoning-set` để thêm reasoning vào một tin nhắn. Lệnh nhận `at` (message ID, mặc định là tin nhắn cuối cùng) và văn bản reasoning làm arguments.

```stscript
/reasoning-set at=0 This is the reasoning for the first message.
```

### Bởi Backend

Nếu LLM backend và model bạn chọn hỗ trợ reasoning output, việc bật "Request model reasoning" trong panel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** sẽ thêm một reasoning block chứa quá trình thinking của model.

Các nguồn được hỗ trợ:

- Claude
- DeepSeek
- Google AI Studio
- Google Vertex AI
- OpenRouter
- xAI (Grok)
- AI/ML API

"Request model reasoning" không xác định xem một model có thực hiện reasoning hay không. Claude và Google (2.5 Flash) cho phép chế độ thinking được chuyển đổi; xem [Reasoning Effort](#reasoning-effort).

### Bằng Parsing

Bật "Auto-Parse" trong panel **<i class="fa-solid fa-font"></i> Advanced Formatting** để tự động phân tích reasoning từ output của model.

Response phải chứa một phần reasoning được bọc trong các chuỗi Prefix và Suffix đã cấu hình. Các chuỗi được cung cấp theo mặc định tương ứng với định dạng reasoning DeepSeek R1.

Ví dụ với prefix `<think>` và suffix `</think>`:

```txt
<think>
This is the reasoning.
</think>

This is the main content.
```

## Prompting với Reasoning

Theo mặc định, nội dung reasoning blocks được nhận dạng không được gửi lại cho model. Để bao gồm reasoning trong prompts, bật "Add to Prompts" trong panel **<i class="fa-solid fa-font"></i> Advanced Formatting**. Nội dung reasoning sẽ được bọc trong các chuỗi Prefix và Suffix đã cấu hình và được phân tách bởi một Separator khỏi context chính. Cài đặt số Max Additions kiểm soát có bao nhiêu reasoning blocks có thể được bao gồm, đếm từ cuối prompt.

!!!
Hầu hết các nhà cung cấp model không khuyến nghị gửi CoT lại cho model trong các cuộc trò chuyện nhiều lượt.
!!!

### Tiếp tục từ Reasoning

Một trường hợp đặc biệt khi reasoning có thể được gửi lại cho model mà không cần bật toggle "Add to Prompts" là khi generation được tiếp tục (ví dụ: bằng cách nhấn "Continue" từ menu **<i class="fa-solid fa-bars"></i> Options**), nhưng tin nhắn đang được tiếp tục chỉ chứa reasoning mà không có nội dung thực tế. Điều này cho model cơ hội để hoàn thành một reasoning chưa hoàn chỉnh và bắt đầu tạo nội dung chính. Prompt sẽ được gửi như sau:

```txt
<think>
Incomplete reasoning...
```

## Regex Scripts

Các regular expression scripts từ [Regex extension](/extensions/Regex.md) có thể được áp dụng cho nội dung của reasoning blocks. Đánh dấu "Reasoning" trong phần "Affects" của trình chỉnh sửa script để nhắm mục tiêu các reasoning blocks cụ thể.

Các tùy chọn ephemerality khác nhau ảnh hưởng đến reasoning blocks theo các cách sau:

1. Không có ephemerality: nội dung reasoning được thay đổi vĩnh viễn.
2. Run on edit: regex script sẽ được đánh giá lại khi reasoning block được chỉnh sửa.
3. Alter chat display: regex được áp dụng cho văn bản hiển thị của reasoning block, không phải nội dung cơ bản.
4. Alter outgoing prompts: regex chỉ được áp dụng cho reasoning blocks trước khi chúng được gửi đến model.

## Reasoning Effort

Reasoning Effort là một cài đặt Chat Completion trong panel **<i class="fa-solid fa-sliders"></i> AI Response Configuration** ảnh hưởng đến có bao nhiêu tokens có thể được sử dụng cho reasoning. Hiệu ứng của mỗi tùy chọn phụ thuộc vào nguồn được kết nối. Đối với các nguồn dưới đây, Auto đơn giản có nghĩa là tham số liên quan không được bao gồm trong request.

| Option  | Claude (≤ 21333 nếu không streaming) | OpenAI (keyword)     | OpenRouter (keyword)             | xAI (Grok) (keyword) | Perplexity (keyword) |
| ------- | ------------------------------------ | -------------------- | -------------------------------- | -------------------- | -------------------- |
| Models  | Opus 4, Sonnet 4/3.7                 | o4-mini, o3\*, o1\*  | các models áp dụng               | grok-3-mini          | sonar-deep-research  |
| Auto    | không chỉ định, **không thinking**   | không chỉ định       | không chỉ định, hiệu ứng phụ thuộc | không chỉ định      | không chỉ định       |
| Minimum | ngân sách 1024 tokens                | "low"                | "low", hoặc 20% max response     | "low"                | "low"                |
| Low     | 15% max response, tối thiểu 1024     | "low"                | "low", hoặc 20% max response     | "low"                | "low"                |
| Medium  | 25% max response, tối thiểu 1024     | "medium"             | "medium", hoặc 50% max response  | "low"                | "medium"             |
| High    | 50% max response, tối thiểu 1024     | "high"               | "high", hoặc 80% max response    | "high"               | "high"               |
| Maximum | 95% max response, tối thiểu 1024     | "high"               | "high", hoặc 80% max response    | "high"               | "high"               |

- Đối với Claude, budget được giới hạn ở 21333 nếu streaming bị vô hiệu hóa. Nếu budget được tính toán sẽ ít hơn 1024, thì max response được thay đổi thành 2048.
- Đối với OpenRouter, Perplexity và AI/ML API, chỉ một keyword kiểu OpenAI được gửi.

Google AI Studio và Vertex AI như sau:

| Model          | Auto (dynamic thinking) | Minimum            | Low                              | Medium     | High       | Maximum               |
| -------------- | ----------------------- | ------------------ | -------------------------------- | ---------- | ---------- | --------------------- |
| 2.5 Pro        | thinkingBudget = -1     | 128                | 15% max response, tối thiểu 128  | 25% of max | 50% of max | thấp hơn max hoặc 32768 |
| 2.5 Flash      | thinkingBudget = -1     | 0, **không thinking** | 15% max response              | 25% of max | 50% of max | thấp hơn max hoặc 24576 |
| 2.5 Flash Lite | thinkingBudget = -1     | 0, **không thinking** | 15% max response, tối thiểu 512 | 25% of max | 50% of max | thấp hơn max hoặc 24576 |

- Đối với Gemini 2.5 Pro và 2.5 Flash/Lite, budget được giới hạn ở 32768 hoặc 24576 tokens tương ứng, bất kể cài đặt streaming.
