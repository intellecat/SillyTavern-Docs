---
order: 160
icon: sliders
route: /usage/common-settings/
---

# Cài đặt chung

Các cài đặt này kiểm soát quá trình lấy mẫu khi tạo văn bản bằng mô hình ngôn ngữ. Ý nghĩa của các cài đặt này là phổ quát cho tất cả các backend được hỗ trợ.

## Cài đặt Context

### Response (tokens)

Số lượng token tối đa mà API sẽ tạo để phản hồi.

- Độ dài phản hồi càng cao, thời gian tạo phản hồi càng lâu.
- Nếu được API hỗ trợ, bạn có thể bật `Streaming` để hiển thị phản hồi từng phần khi nó đang được tạo.
- Khi `Streaming` tắt, phản hồi sẽ được hiển thị tất cả cùng một lúc khi chúng hoàn thành.

### Context (tokens)

Số lượng token tối đa mà SillyTavern sẽ gửi đến API dưới dạng prompt, trừ đi độ dài phản hồi.

- Context bao gồm thông tin nhân vật, system prompts, lịch sử trò chuyện, v.v.
- Một đường chấm giữa các tin nhắn biểu thị phạm vi context cho cuộc trò chuyện. Các tin nhắn phía trên đường đó không được gửi đến AI.
- Để xem thành phần của context sau khi tạo tin nhắn, nhấp vào tùy chọn tin nhắn `Prompt Itemization` (mở rộng menu `...` và nhấp vào biểu tượng hình vuông có đường kẻ).

## Tham số Sampler

### Temperature

Temperature kiểm soát tính ngẫu nhiên trong việc chọn token:

- Temperature thấp (<1.0) dẫn đến văn bản có thể dự đoán được hơn, ưu tiên các token có xác suất cao hơn
- Temperature cao (>1.0) tăng tính sáng tạo và đa dạng trong đầu ra bằng cách cho các token có xác suất thấp hơn cơ hội tốt hơn.

Đặt thành 1 cho xác suất gốc.

### Repetition Penalty

Cố gắng hạn chế sự lặp lại bằng cách phạt các token dựa trên tần suất chúng xuất hiện trong context.

Đặt giá trị thành 1 để vô hiệu hóa hiệu ứng của nó.

#### Repetition Penalty Range

Có bao nhiêu token từ token được tạo cuối cùng sẽ được xem xét cho repetition penalty. Điều này có thể phá vỡ phản hồi nếu đặt quá cao, vì các từ phổ biến như "the, a, and," v.v. sẽ bị phạt nhiều nhất.

Đặt giá trị thành 0 để vô hiệu hóa hiệu ứng của nó.

#### Repetition Penalty Slope

Nếu cả cái này và `Repetition Penalty Range` đều trên 0, repetition penalty sẽ có hiệu ứng lớn hơn ở cuối prompt. Giá trị càng cao, hiệu ứng càng mạnh.

Đặt giá trị thành 0 để vô hiệu hóa hiệu ứng của nó.

### Top K

Top K đặt số lượng tối đa các token hàng đầu có thể được chọn. Ví dụ, nếu Top K là 20, điều này có nghĩa chỉ 20 token có thứ hạng cao nhất sẽ được giữ lại (bất kể xác suất của chúng đa dạng hay hạn chế).

Đặt thành 0 (hoặc -1, tùy thuộc vào backend của bạn) để vô hiệu hóa.

### Top P

Top P (còn gọi là nucleus sampling) cộng tất cả các token hàng đầu cần thiết để cộng lại thành tỷ lệ phần trăm mục tiêu. Nếu 2 token hàng đầu đều là 25%, và Top P là 0.50, chỉ 2 token hàng đầu được xem xét.

Đặt giá trị thành 1 để vô hiệu hóa hiệu ứng của nó.

### Typical P

Typical P Sampling ưu tiên các token dựa trên độ lệch của chúng so với entropy trung bình của tập hợp. Nó duy trì các token có xác suất tích lũy gần với ngưỡng được xác định trước (ví dụ: 0.5), nhấn mạnh những token có nội dung thông tin trung bình.

Đặt giá trị thành 1 để vô hiệu hóa hiệu ứng của nó.

### Min P

Giới hạn pool token bằng cách cắt bỏ các token có xác suất thấp so với token hàng đầu. Tạo ra các phản hồi mạch lạc hơn nhưng cũng có thể làm tệ hơn sự lặp lại nếu đặt quá cao.

- Hoạt động tốt nhất ở các giá trị thấp như `0.1-0.01`, nhưng có thể đặt cao hơn với `Temperature` cao. Ví dụ: `Temperature: 5, Min P: 0.5`

Đặt giá trị thành 0 để vô hiệu hóa hiệu ứng của nó.

### Top A

Top A đặt ngưỡng cho việc chọn token dựa trên bình phương xác suất token cao nhất. Ví dụ, nếu giá trị Top-A là 0.2 và xác suất của token hàng đầu là 50%, các token có xác suất dưới 5% (0.2 * 0.5^2) bị loại trừ.

Đặt giá trị thành 0 để vô hiệu hóa hiệu ứng của nó.

### Tail Free Sampling

Tail-Free Sampling (TFS) tìm kiếm đuôi của các token có xác suất thấp trong phân phối, bằng cách phân tích tốc độ thay đổi xác suất token bằng đạo hàm. Nó giữ lại các token lên đến một ngưỡng (ví dụ: 0.3) dựa trên đạo hàm bậc hai được chuẩn hóa. Càng gần 0, càng nhiều token bị loại bỏ.

Đặt giá trị thành 1 để vô hiệu hóa hiệu ứng của nó.

### Smoothing Factor

Tăng khả năng của các token có xác suất cao trong khi giảm khả năng của các token có xác suất thấp bằng cách sử dụng phép biến đổi bậc hai. Nhằm mục đích tạo ra các phản hồi sáng tạo hơn bất kể `Temperature`.

- Hoạt động tốt nhất mà không cần các sampler cắt ngắn như `Top K`, `Top P`, `Min P`, v.v.

Đặt giá trị thành 0 để vô hiệu hóa hiệu ứng của nó.

### Dynamic Temperature

Mở rộng temperature động dựa trên khả năng của token hàng đầu. Nhằm mục đích tạo ra đầu ra sáng tạo hơn mà không hy sinh tính mạch lạc.

- Chấp nhận phạm vi temperature từ tối thiểu đến tối đa. Ví dụ: `Minimum Temp: 0.75` và `Maximum Temp: 1.25`
- `Exponent` áp dụng một đường cong hàm mũ dựa trên token hàng đầu.

Bỏ chọn để vô hiệu hóa hiệu ứng của nó.

### Epsilon Cutoff

Epsilon cutoff đặt ngưỡng xác suất dưới đó các token bị loại khỏi việc lấy mẫu. Đơn vị 1e-4; một giá trị hợp lý là 3.

Đặt thành 0 để vô hiệu hóa.

### Eta Cutoff

Eta cutoff là tham số chính của kỹ thuật Eta Sampling đặc biệt. Đơn vị 1e-4; một giá trị hợp lý là 3. Xem bài báo [Truncation Sampling as Language Model Desmoothing by Hewitt et al. (2022)](https://arxiv.org/abs/2210.15191) để biết chi tiết.

Đặt thành 0 để vô hiệu hóa.

### DRY Repetition Penalty

DRY phạt các token sẽ mở rộng phần cuối của đầu vào thành một chuỗi đã xảy ra trước đó trong đầu vào. Nếu bạn muốn cho phép lặp lại một số chuỗi nhất định nguyên văn (ví dụ: tên), bạn có thể thêm chúng vào danh sách sequence breakers. Xem Pull Request [tại đây](https://github.com/oobabooga/text-generation-webui/pull/5677).

Đặt multiplier thành 0 để vô hiệu hóa.

### Exclude Top Choices (XTC)

Thuật toán lấy mẫu XTC loại bỏ các token có khả năng cao nhất khỏi xem xét thay vì cắt bỏ các token ít có khả năng nhất. Nó loại bỏ tất cả ngoại trừ token ít có khả năng nhất đáp ứng một ngưỡng nhất định, với một xác suất nhất định. Điều này đảm bảo rằng ít nhất một lựa chọn "khả thi" vẫn còn, giữ lại tính mạch lạc. Xem Pull Request [tại đây](https://github.com/oobabooga/text-generation-webui/pull/6335).

Đặt probability thành 0 để vô hiệu hóa.

### Mirostat

Mirostat khớp perplexity đầu ra với perplexity đầu vào, do đó tránh bẫy lặp lại (nơi, khi suy luận tự hồi quy tạo ra văn bản, perplexity của đầu ra có xu hướng về không) và bẫy nhầm lẫn (nơi perplexity phân kỳ). Để biết chi tiết, xem bài báo [Mirostat: A Neural Text Decoding Algorithm that Directly Controls Perplexity by Basu et al. (2020)](https://arxiv.org/abs/2007.14966).

Mode chọn phiên bản Mirostat.

- 0 = vô hiệu hóa,
- 1 = Mirostat 1.0 (chỉ llama.cpp),
- 2 = Mirostat 2.0.

### Beam Search

Một thuật toán tham lam, brute-force được sử dụng trong lấy mẫu LLM để tìm chuỗi từ hoặc token có khả năng nhất. Nó mở rộng nhiều chuỗi ứng viên cùng một lúc, duy trì một số lượng cố định (beam width) các chuỗi hàng đầu ở mỗi bước.

### Top nsigma

Một phương pháp lấy mẫu lọc logit dựa trên các thuộc tính thống kê của chúng. Nó giữ các token trong n độ lệch chuẩn của giá trị logit tối đa, cung cấp một giải pháp thay thế đơn giản hơn cho lấy mẫu top-p/top-k trong khi duy trì tính ổn định lấy mẫu qua các temperature khác nhau.
