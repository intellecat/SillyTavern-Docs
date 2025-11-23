---
route: /extensions/summarize/
---

# Tóm tắt

## Nó là gì?

Extension này cho phép bạn tạo, lưu trữ và sử dụng các tóm tắt được tạo tự động dựa trên các sự kiện xảy ra trong các chats của bạn. Tóm tắt có thể giúp liệt kê các chi tiết chung về những gì đang xảy ra trong câu chuyện, điều này *có thể* được diễn giải như một bộ nhớ dài hạn, nhưng hãy xem câu nói đó bằng một hạt muối. Vì các bản tóm tắt được tạo bởi các mô hình ngôn ngữ, các đầu ra có thể mất một số chi tiết quan trọng hoặc chứa các hallucinations, vì vậy bạn luôn được khuyên giữ track trạng thái tóm tắt và sửa nó theo cách thủ công nếu cần.

## Cấu hình chung

Extension tóm tắt được cài đặt trong SillyTavern theo mặc định, do đó nó sẽ xuất hiện trong danh sách Extensions panel (icon stacked cubes) của ST như thế này:

![Summarize Config Panel](/static/extensions/summarize.png)

- **Current summary** - hiển thị và cung cấp khả năng sửa đổi tóm tắt hiện tại. Tóm tắt được cập nhật và nhúng vào siêu dữ liệu của file chat cho tin nhắn là tin nhắn cuối cùng trong bối cảnh khi tóm tắt được tạo. Xóa hoặc chỉnh sửa một tin nhắn từ chat có một tóm tắt được gắn vào nó sẽ hoàn nguyên trạng thái cho tóm tắt hợp lệ cuối cùng.
- **Restore Previous** - xóa tóm tắt hiện tại, khôi phục nó về trạng thái trước đó. Điều này hữu ích nếu summarizer làm một công việc tồi tại bất kỳ thời điểm nào.
- **Pause** - chọn cái này để ngăn tóm tắt được cập nhật tự động. Điều này hữu ích nếu bạn muốn cung cấp một tóm tắt tùy chỉnh của riêng bạn hoặc để vô hiệu hóa hiệu quả tóm tắt bằng cách xóa hộp và dừng cập nhật.
- **Popup window** - cho phép tách tóm tắt thành một bảng điều khiển UI có thể di chuyển trên thanh bên. Hữu ích cho bố cục desktop để dễ dàng truy cập cài đặt tóm tắt mà không phải điều hướng qua menu extensions.
- **Injection Template** - định nghĩa cách tóm tắt sẽ được bao bọc khi được chèn vào các prompts chat thông thường. Một macro đặc biệt \{\{summary\}\} nên được sử dụng để biểu thị vị trí chính xác của trạng thái tóm tắt hiện tại trong văn bản injection prompt.
- **Injection Position** - đặt vị trí của prompt injection. Các tùy chọn giống như đối với Author's Notes: trước hoặc sau prompt chính, hoặc in-chat ở độ sâu được chỉ định.

## Các nguồn tóm tắt được hỗ trợ

### Main API

Tóm tắt sẽ được cung cấp bởi AI backend, mô hình và cài đặt được chọn hiện tại của bạn. Phương pháp này không yêu cầu cài đặt bổ sung, chỉ cần một kết nối API hoạt động.

Tùy chọn này có các sub-modes sau khác nhau tùy thuộc vào cách xây dựng prompt tóm tắt:

1. Raw, blocking. Tóm tắt sẽ được tạo bằng cách sử dụng không có gì ngoài prompt tóm tắt và lịch sử chat. Các prompts tiếp theo cũng sẽ bao gồm tóm tắt trước đó với các tin nhắn được gửi sau khi tóm tắt được tạo (xem ví dụ). Chế độ này có thể (và sẽ) tạo các prompts có rất nhiều tính biến thể giữa chúng, vì vậy không được khuyến nghị sử dụng nó với các backends có thời gian xử lý prompt chậm, như llama.cpp và các công cụ phái sinh của nó.
2. Raw, non-blocking. Giống như trên, nhưng tạo chat sẽ không bị chặn trong quá trình tạo tóm tắt. Không phải mọi backend đều hỗ trợ các yêu cầu đồng thời, vì vậy chuyển sang chế độ blocking nếu tóm tắt không thành công.
3. Classic, blocking. Prompt tóm tắt sẽ được gửi ở cuối prompt tạo thông thường của bạn, như một hướng dẫn hệ thống trung lập, không bỏ qua card nhân vật, prompt chính, ví dụ đối thoại và các phần khác của prompts chat. Điều này thường dẫn đến các prompts hoạt động tốt với việc tái sử dụng processed prompts, vì vậy được khuyến nghị sử dụng với llama.cpp và những người anh em của nó.

#### Summary Settings explained

1. **Summary Prompt** - định nghĩa prompt sẽ được sử dụng để tạo tóm tắt. Có thể bao gồm bất kỳ macros nào được biết đến, cũng như một macro đặc biệt \{\{words\}\} (xem bên dưới).
2. **Target summary length (words)** - định nghĩa giá trị của macro \{\{words\}\} có thể được chèn vào Summary Prompt. Cài đặt này hoàn toàn tùy chọn và không có hiệu ứng nào nếu macro không được sử dụng.
3. **API response length (tokens)** - cho phép đặt một độ dài phản hồi API ghi đè cho tạo tóm tắt khác từ giá trị được đặt toàn cục.
4. **Max messages per request _(raw modes only)_** - đặt để giới hạn số lượng tối đa của tin nhắn sẽ được bao gồm trong một prompt tóm tắt. `0` có nghĩa là không có giới hạn rõ ràng, nhưng số lượng tin nhắn kết quả cần tóm tắt vẫn sẽ phụ thuộc vào kích thước bối cảnh tối đa, được tính bằng công thức: `max summary buffer = context size - summarization prompt - previous summary - response length`. Sử dụng cái này khi bạn muốn có các tóm tắt tập trung hơn trên các mô hình có kích thước bối cảnh lớn.
5. **No WI/AN** - bỏ qua World Info và Author's Note khỏi văn bản cần tóm tắt. Chỉ có tác dụng khi sử dụng prompt builder Classic. Raw prompt builder luôn bỏ qua WI/AN.
6. **Update every X messages** - đặt khoảng thời gian tóm tắt được tạo. `0` có nghĩa là tóm tắt tự động bị vô hiệu hóa, nhưng bạn vẫn có thể kích hoạt nó theo cách thủ công bằng cách nhấp nút "Summarize now". Điều này nên được điều chỉnh dựa trên tốc độ buffer prompt điền đầy hoàn toàn. Lý tưởng nhất, bạn muốn có tóm tắt đầu tiên được tạo khi các tin nhắn bắt đầu bị loại bỏ khỏi prompt.
7. **Update every X words** - giống như trên, nhưng sử dụng words (không phải tokens!) thay vào đó, về mặt lý thuyết có thể là một phép đo chính xác hơn do tính không thể dự đoán của nội dung của các tin nhắn chat thông thường, nhưng kết quả của bạn có thể khác.

Nếu cả hai thanh trượt "Update every" được đặt thành giá trị khác không, thì cả hai sẽ kích hoạt các bản cập nhật tóm tắt ở các khoảng thời gian tương ứng của chúng, tùy thuộc vào những gì xảy ra trước. Rất được khuyến nghị để cập nhật các giá trị này tương ứng khi bạn chuyển sang một mô hình khác có kích thước bối cảnh khác, nếu không, tạo tóm tắt có thể kích hoạt quá thường xuyên, hoặc không bao giờ.

Nếu bạn không chắc chắn về cài đặt khoảng thời gian, bạn có thể nhấp nút "magic wand" ở trên thanh trượt "Update every" để cố gắng đoán các giá trị tối ưu dựa trên một số heuristics đơn giản. Mô tả ngắn gọn của thuật toán như sau:

1. Tính toán số lượng token và từ cho tất cả các tin nhắn chat
2. Xác định độ dài tóm tắt mục tiêu dựa trên các từ prompt mong muốn
3. Tính toán số lượng tối đa của tin nhắn có thể phù hợp với prompt dựa trên độ dài tin nhắn trung bình
4. Nếu "Max messages" được đặt, điều chỉnh trung bình để tính đến các tin nhắn không phù hợp với giới hạn tóm tắt
5. Làm tròn xuống các tin nhắn trung bình được điều chỉnh cho một bội số của 5

#### Example prompts

**Raw prompt**
```
System:
[Summarization prompt]

Previous summary.

User:
Message foo.

Char:
Message bar.
```

**Classic prompt**
```
[Main prompt]

[Character card]

[Example dialogues]

User:
Message foo.

Char:
Message bar.

System:
[Summarization prompt]
```

### Extras API

Máy chủ Extras với module `summarize` có thể chạy một mô hình tóm tắt phụ trợ (BART).

Nó có một kích thước bối cảnh rất nhỏ (~1024 tokens), vì vậy khả năng xử lý các tóm tắt lớn của nó khá hạn chế.

Để cấu hình nguồn tóm tắt Extras, hãy thực hiện những điều sau:

1. Cài đặt hoặc Cập nhật [Extras](https://github.com/SillyTavern/SillyTavern-extras) lên phiên bản mới nhất.
2. Chạy Extras với module `summarize` được bật: `python server.py --enable-modules=summarize`

#### Changing Summary Model

Theo mặc định, Summarize sử dụng mô hình [Qiliang/bart-large-cnn-samsum-ChatGPT_v3](https://huggingface.co/Qiliang/bart-large-cnn-samsum-ChatGPT_v3) cho mục đích tóm tắt.

Điều này có thể được thay đổi bằng cách sử dụng lệnh dòng argument `--summarization-model=(###Hugging-Face-Model-URL-Here###)`

Một mô hình tóm tắt thay thế đã biết là `Qiliang/bart-large-cnn-samsum-ElectrifAi_v10`.
