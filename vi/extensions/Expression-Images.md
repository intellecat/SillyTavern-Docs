---
route: /extensions/expression-images/
---

# Character Expressions

## Đó là gì?

Expression images là các hình ảnh (còn gọi là 'sprites') của nhân vật AI của bạn được hiển thị bên cạnh (hoặc phía sau) cửa sổ trò chuyện.

Expression images có thể tự động thay đổi dựa trên phân loại, điều chỉnh theo cảm xúc được thể hiện trong phản hồi trò chuyện gần đây nhất của AI.

## Thêm hình ảnh biểu cảm nhân vật

1. Mở bảng Extensions và mở rộng phần 'Character Expressions'. Nếu bạn đang mở cuộc trò chuyện nhân vật, bạn sẽ thấy một lưới các hình ảnh placeholder.
![Expression Drawer](/static/extensions/expression-drawer.png)
2. Nhấp nút 'Upload image' ở góc trên bên trái của mỗi hình ảnh trong lưới, và chọn hình ảnh bạn muốn áp dụng cho cảm xúc đó. Điều này sẽ lưu hình ảnh với tên tệp chính xác bên trong thư mục `/data/<user-handle>/characters/(character_name_here)/`.
3. Lặp lại điều này cho tất cả các biểu cảm bạn muốn gán hình ảnh.

### Nhập tệp ZIP hình ảnh biểu cảm

Sử dụng nút '<i class="fa-solid fa-file-zipper"></i> Upload sprite pack (ZIP)', bạn có thể nhập tệp zip chứa một bộ sưu tập hình ảnh biểu cảm, và những hình ảnh đó sẽ tự động được thêm vào thư mục chính xác cho **nhân vật hiện được chọn** của bạn. Tệp ZIP phải chứa tất cả hình ảnh trong cấu trúc phẳng (không có thư mục con) và các tệp được đặt tên chính xác. Việc nhập zip sẽ không tự động đổi tên bất kỳ hình ảnh nào để chúng khớp với các cảm xúc.

## Thay đổi biểu cảm thủ công

1. Nhấp vào bất kỳ hình ảnh biểu cảm đã tải lên (sprites) nào để hiển thị chúng gần giao diện trò chuyện (với chế độ UI mặc định) hoặc ở giữa màn hình (trong chế độ Visual Novel).
2. Sử dụng lệnh slash `/expression-set (name)` hoặc Quick Reply phù hợp để đặt sprite mà không cần mở menu tiện ích mở rộng.

## Thay đổi biểu cảm tự động

Để tự động đặt biểu cảm khi nhân vật trả lời, bạn có nhiều tùy chọn.
Biểu cảm thay đổi theo tin nhắn hoặc theo khoảng thời gian đều đặn khi streaming tin nhắn được bật.

### Module classify hoạt động như thế nào?

Module `classify` sử dụng một mô hình 'phân tích cảm xúc' nhỏ chạy cùng với máy chủ SillyTavern. Mô hình này lấy đầu ra mới từ AI và phát hiện loại cảm xúc hoặc emotion nào mà văn bản đang thể hiện. Mặc dù nhiều cảm xúc có thể được thể hiện trong một tin nhắn duy nhất, mô hình chỉ chọn một cảm xúc có khả năng nhất và trả về cho SillyTavern. Tiện ích mở rộng frontend sau đó hiển thị hình ảnh được liên kết với cảm xúc đó.

### Hướng dẫn thiết lập (Local)

1. Mở bảng extensions và mở rộng menu tiện ích mở rộng "Character Expressions".
2. Chọn "Local" trong dropdown nguồn phân loại.
3. Điều này sẽ bắt đầu tải xuống một lần mô hình phân loại từ HuggingFace Hub (khoảng ~100 Mb).
4. Tạo bất kỳ tin nhắn nào để xác minh rằng phân loại hoạt động và sprite xuất hiện. Bạn cũng có thể kiểm tra console máy chủ để xem log debug.

Phân loại local mặc định là 28 nhãn hình ảnh có thể: [Cohee/distilbert-base-uncased-go-emotions-onnx](https://huggingface.co/Cohee/distilbert-base-uncased-go-emotions-onnx)

Để sử dụng mô hình phân loại 6 tùy chọn, hãy thay đổi giá trị của biến `extensions.models.classification` trong tệp `config.yaml` thành: [Cohee/bert-base-uncased-emotion-onnx](https://huggingface.co/Cohee/bert-base-uncased-emotion-onnx)

### Hướng dẫn thiết lập (với LLM)

1. Kết nối với bất kỳ API được hỗ trợ và cấu hình đúng nào qua **<i class="fa-solid fa-plug"></i> API Connections**.
2. Nhập hình ảnh biểu cảm theo cách giống như đã đề cập ở trên.
3. Chọn "Main API" trong dropdown nguồn phân loại.
4. Tùy chọn, cấu hình prompt hướng dẫn phân loại.
5. Tạo bất kỳ tin nhắn nào để xác minh rằng phân loại hoạt động và sprite xuất hiện. Bạn cũng có thể kiểm tra console máy chủ để xem log debug.

#### Chiến lược xây dựng Prompt

Nguồn LLM chính cho phép chọn cách xây dựng prompt phân loại:

* **Limited Context**: Chỉ tin nhắn cuối cùng và prompt hướng dẫn hệ thống được gửi.
* **Full Context**: Toàn bộ lịch sử trò chuyện, bao gồm thẻ nhân vật được gửi.

### Hướng dẫn thiết lập (WebLLM)

1. Cài đặt [tiện ích mở rộng WebLLM](https://github.com/SillyTavern/Extension-WebLLM) chính thức.
2. Nhập hình ảnh biểu cảm theo cách giống như đã đề cập ở trên.
3. Chọn "WebLLM" trong dropdown nguồn phân loại.
4. Tùy chọn, cấu hình prompt hướng dẫn phân loại.
5. Tạo bất kỳ tin nhắn nào để xác minh rằng phân loại hoạt động và sprite xuất hiện. Bạn cũng có thể kiểm tra console máy chủ để xem log debug.

### Hướng dẫn thiết lập (với Extras)

> [!WARNING]
> Extras đã không còn được dùng và có thể bị xóa trong các bản cập nhật tương lai.

1. Cài đặt và chạy Extras với module `classify` được bật: `python server.py --enable-modules=classify`
2. Nhập hình ảnh biểu cảm theo cách giống như đã đề cập ở trên.
3. Chọn "Extras" trong dropdown nguồn phân loại.
4. Hình ảnh biểu cảm phù hợp sẽ tự động hiển thị bất cứ khi nào AI gửi cho bạn phản hồi.

Extras API sử dụng mô hình phân loại với 6 tùy chọn theo mặc định: [nateraw/bert-base-uncased-emotion](https://huggingface.co/nateraw/bert-base-uncased-emotion)

Cũng có một mô hình với 28 tùy chọn: [joeddav/distilbert-base-uncased-go-emotions-student](https://huggingface.co/joeddav/distilbert-base-uncased-go-emotions-student)

Để sử dụng mô hình này, bạn cần thay đổi dòng lệnh Extras của mình để bao gồm đối số sau (với khoảng trắng trước và sau): `--classification-model=joeddav/distilbert-base-uncased-go-emotions-student`

## Biểu cảm tùy chỉnh

Làm thế nào để có thêm tùy chọn biểu cảm hơn những gì được cung cấp theo mặc định? Bạn có thể thiết lập **Custom Expressions** trong cài đặt tiện ích mở rộng. Bạn có thể gán bất kỳ tên nào cho Custom Expressions. Chúng sẽ xuất hiện trong danh sách hình ảnh biểu cảm và có thể được gán hình ảnh như các biểu cảm khác. Chúng sẽ có chỉ báo cho thấy đó là tùy chỉnh.

> [!TIP]
> Cả Local và Extras chỉ hỗ trợ danh sách hạn chế các biểu cảm.
>
> Nếu bạn muốn Custom Expressions được hiển thị, bạn cần phải huấn luyện một mô hình phân loại với các nhãn được hỗ trợ (nằm ngoài phạm vi của hướng dẫn này), hoặc bạn có thể sử dụng LLM hoặc WebLLM làm nguồn phân loại, cả hai sẽ tự động sử dụng tất cả các biểu cảm hiện có - cả mặc định và bất kỳ biểu cảm tùy chỉnh nào.

## Định dạng hình ảnh nào được hỗ trợ cho Expressions?

Bất kỳ định dạng hình ảnh nào đều được cho phép, bao gồm webp và gif động.

Định dạng phổ biến nhất là tệp PNG với nền trong suốt.

## Sử dụng biểu cảm mặc định

Nếu bạn không có hình ảnh biểu cảm cho tất cả các biểu cảm của nhân vật, hoặc không có hình ảnh nào cả, có nhiều tùy chọn về những gì sẽ hiển thị theo mặc định.
Tất cả những tùy chọn này có thể được chọn qua dropdown dưới 'Default / Fallback Expression'.

1. **Chọn một Fallback Expression**: Nếu một biểu cảm được chọn mà bạn không có hình ảnh cho nó, biểu cảm fallback sẽ được hiển thị thay thế. Chỉ cần chọn một trong các biểu cảm có sẵn từ dropdown.
2. **[No Fallback]**: Khi không có hình ảnh tồn tại, không hiển thị gì.
3. **[Default emojis]**: Bạn có thể sử dụng các biểu cảm mặc định tích hợp sẵn trong SillyTavern. Đây là những hình ảnh kiểu emoji đơn giản.

## Sử dụng nhiều hình ảnh cho mỗi biểu cảm

Có thể thêm nhiều hình ảnh cho mỗi biểu cảm để cho phép đa dạng hơn trong các biểu cảm được hiển thị.
Để bật điều này, chỉ cần bật **Allow multiple sprites per expression**.
Bây giờ bạn có thể tải lên nhiều hơn một hình ảnh, và bất kỳ hình ảnh bổ sung nào sẽ được hiển thị với một đánh dấu nhỏ.

Các hình ảnh riêng lẻ có thể được chọn thủ công bằng cách chọn chúng bằng một cú nhấp, hoặc qua `/expression-set type=sprite`, điều này sẽ liệt kê các hình ảnh sprite có sẵn, thay vì các biểu cảm.

Bất cứ khi nào một biểu cảm với nhiều hình ảnh được tự động chọn, một trong các hình ảnh hiện có sẽ được chọn ngẫu nhiên.
Nếu bạn muốn buộc một hình ảnh mới của biểu cảm đó được chọn khi cùng một biểu cảm được sử dụng nhiều lần, bạn có thể bật **Re-roll if same sprite is used again**.

### Quy ước đặt tên cho nhiều hình ảnh cho mỗi biểu cảm

Trong trường hợp có nhiều hình ảnh cho mỗi biểu cảm, các tệp cần được đặt tên theo một cách cụ thể.
Các tệp cần bắt đầu bằng tên của biểu cảm, và sau đó theo sau là một hậu tố, được phân tách bằng dấu chấm hoặc dấu gạch ngang. Ví dụ: `joy.png`, `joy-1.png`, `joy.expressive.png`
Tên tệp phải tuân theo định dạng này cho cả việc tải lên trực tiếp và nhập ZIP.

## Ghi đè thư mục Sprite

> [!NOTE]
> Tên hiển thị (không phải tên tệp thẻ nhân vật) quyết định bộ hình ảnh nào được sử dụng

Nếu bạn có nhiều hơn một nhân vật với cùng tên hiển thị, cả hai sẽ sử dụng cùng một bộ hình ảnh biểu cảm.

Nếu bạn muốn một bộ hình ảnh khác nhau được sử dụng cho mỗi phiên bản của nhân vật cùng tên, bạn có thể sử dụng ghi đè thư mục sprites.
Ghi đè thư mục cũng có thể được sử dụng để xác định các bộ sprite khác nhau (trang phục, v.v.) của cùng một nhân vật.

### Cách đặt ghi đè

1. Tạo một thư mục trong `/data/<user-handle>/characters` với bất kỳ tên nào và đặt hình ảnh vào đó, ví dụ: `/data/<user-handle>/characters/Boris`.
2. Mở cuộc trò chuyện với nhân vật mà bạn muốn ghi đè sprites.
3. Nhập tên của thư mục ghi đè vào ô nhập "Sprite Folder Override" và nhấp "Submit".
4. Danh sách Sprites sẽ tải lại và chỉ báo "Sprite set" sẽ hiển thị thư mục ghi đè.
5. Ngoài ra, bạn có thể sử dụng lệnh slash `/costume` để đạt được kết quả tương tự: `/costume Boris`.
6. Bằng cách thêm dấu gạch chéo ngược vào đầu tên thư mục ghi đè, nó sẽ được giải quyết thành một thư mục con trong thư mục sprites nhân vật hiện tại, ví dụ: `/costume \tracksuit` cho nhân vật có tên Boris sẽ được giải quyết thành thư mục `/data/<user-handle>/characters/Boris/tracksuit`.
