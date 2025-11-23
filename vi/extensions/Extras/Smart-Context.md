---
route: /extensions/smart-context/
---

# Smart Context

## **EXTENSION NÀY KHÔNG CÒN ĐƯỢC DUY TRÌ VÀ KHÔNG ĐƯỢC KHUYẾN NGHỊ SỬ DỤNG. HÃY XEM XÉT [CHAT VECTORIZATION](/extensions/Chat-vectorization.md) LÀ MỘT GIẢI PHÁP THAY THẾ KHẢ THI.**

!!!warning Tuyên bố miễn trừ trách nhiệm
Việc sử dụng extension này không đảm bảo trải nghiệm chat tốt hơn hoặc cải thiện bộ nhớ dưới bất kỳ hình thức nào. Chỉ sử dụng nếu bạn hiểu tất cả các tác động của việc sử dụng vector database.
!!!

### Nó là gì?

Smart Context là một extension của SillyTavern sử dụng thư viện [ChromaDB](https://www.trychroma.com) để cung cấp cho các nhân vật AI của bạn quyền truy cập vào thông tin tồn tại bên ngoài giới hạn ngữ cảnh lịch sử chat thông thường.

### Tại sao nó hữu ích?

Nếu bạn có một cuộc trò chuyện rất dài, phần lớn nội dung nằm ngoài cửa sổ ngữ cảnh thông thường và do đó không có sẵn cho AI khi viết phản hồi.

Smart Context tự động lấy toàn bộ lịch sử của file chat và đưa nó vào vector database. Cơ sở dữ liệu này sau đó được tìm kiếm mỗi khi bạn nhập thứ gì đó mới vào chat, và nếu tìm thấy tin nhắn có từ khóa khớp, các tin nhắn chat đó sẽ được đặt vào ngữ cảnh để AI có thể nhìn thấy chúng khi viết phản hồi tiếp theo.

***

### Hướng dẫn thiết lập

1. Cập nhật SillyTavern lên ít nhất phiên bản 1.10.6.
2. Cài đặt extension "Smart Context" từ menu "Download Extensions & Assets" trong panel Extensions (biểu tượng stacked blocks).
3. Cài đặt hoặc Cập nhật [Extras](https://github.com/SillyTavern/SillyTavern-extras) lên phiên bản mới nhất. Ngoài ra, sử dụng [Colab notebook](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb).
4. *Chỉ cài đặt cục bộ:* Cài đặt requirements-complete.txt cho Extras (ngay cả khi bạn đã thực hiện một lần trước đó trong lần cài đặt trước).
5. Chạy Extras với module chromadb được kích hoạt: `python server.py --enable-modules=chromadb`

#### Gặp lỗi khi cài đặt ChromaDB?

```
ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects
```

Cài đặt gói chromadb yêu cầu một trong các điều sau:

- Đã cài đặt Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
- Cài đặt hnswlib từ conda: `conda install -c conda-forge hnswlib`

***

### Cấu hình

Sau khi Smart Context được kích hoạt, bạn nên cấu hình nó trong giao diện SillyTavern.
Cấu hình Smart Context có thể được thực hiện từ trong menu Extensions ![STExtensionMenuIcon](/static/extensions/menu-icon.png)

![Smart Context Config Panel](/static/extensions/smart-context.png)

Có 4 khái niệm chính cần lưu ý:

- Chat History Preservation
- Memory Injection Amount
- Individual Memory Length
- Injection Strategy

***

#### SmartContext chỉ bắt đầu sau khi có 10 tin nhắn trong lịch sử chat

- Khi bắt đầu một cuộc trò chuyện mới, ChromaDB không hoạt động.
- Khi cuộc trò chuyện đã tích lũy 10 tin nhắn, nó sẽ bắt đầu ghi lại tất cả tin nhắn vào cơ sở dữ liệu và gọi lại tin nhắn khi cần.

#### Chat History Preservation ('kept mesages')

Theo mặc định, ChromaDB sẽ giữ càng nhiều tin nhắn lịch sử chat tự nhiên gần đây như được chỉ định trong thanh trượt.
Bất kỳ tin nhắn nào vượt quá số lượng này sẽ bị xóa khỏi lời nhắc được gửi của bạn, và nếu 'memories' tồn tại trong cơ sở dữ liệu, chúng sẽ được thêm vào thay thế các tin nhắn lịch sử chat cũ hơn (xem Chiến lược bên dưới).

***

#### Memory Injection Amount

Số lượng 'memories' tối đa mà Smart Context sẽ chèn vào ngữ cảnh.
Không phải mọi lần chèn đều nhận được số lượng đầy đủ này.
Nếu bạn gửi input liên quan đến 'dogs' và chỉ có một tin nhắn khác trong DB liên quan đến dogs, thì chỉ có 1 mục sẽ được chèn.

***

#### Individual Memory Length

Đây là độ dài tối đa được phép cho mỗi 'memory' được chèn.
Đơn vị là **KÝ TỰ** (không phải tokens).
Nếu đặt quá nhỏ, memory có thể bị cắt giữa chừng.

Ví dụ:

`Ross: I like dogs with long fur and fluffy tails. I dislike dogs with short fur and short tails.`

'Memory' trong cơ sở dữ liệu này dài 103 ký tự, vì vậy bạn cần đặt thanh trượt ít nhất `103` để kéo nó hoàn toàn vào ngữ cảnh.

Nếu thanh trượt nhỏ hơn 103, tin nhắn sẽ bị cắt và chèn như vậy.

***

### Chiến lược chèn

#### Replace oldest history

Chiến lược này giữ X tin nhắn gần đây, xóa tất cả tin nhắn trước đó, và thay thế chúng bằng 'memories'.

Ưu điểm

- ít có khả năng làm tràn giới hạn ngữ cảnh của bạn
- memories tồn tại gần đầu ngữ cảnh sẽ có ít tác động ngay lập tức đến phản hồi trong khi vẫn cung cấp 'thông tin nền'.

Nhược điểm

- tin nhắn cũ được chèn trực tiếp vào lịch sử chat không có dấu phân cách đặc biệt, và thường không có mối liên quan tự nhiên ngay lập tức với các tin nhắn lịch sử chat tự nhiên được bảo tồn. Điều này có thể gây nhầm lẫn cho các mô hình AI kém thông minh hơn.

#### Add to Bottom

Chiến lược này giữ lịch sử chat ở trạng thái tự nhiên và thêm 'memories' **sau** nó bên trong một [bracket header] được định dạng.
Điều này có nghĩa là thanh trượt 'kept messages' bị vô hiệu hóa.

Ưu điểm

- không làm ngắn hoặc thay đổi lịch sử chat tự nhiên hiện tại
- 'memories' tồn tại sau chat và có tác động mạnh hơn đến phản hồi AI tiếp theo

Nhược điểm

- vì không có mục chat nào bị xóa/thay thế, có khả năng cao hơn là bạn sẽ làm tràn giới hạn ngữ cảnh.
- vì memories tồn tại rất gần cuối lời nhắc, chúng có thể có QUẤY NHIỀU tác động đến phản hồi của AI.

#### Custom Depth

Chiến lược này giữ lịch sử chat ở trạng thái tự nhiên và thêm 'memories' ở độ sâu bạn xác định trong template bạn chỉ định.
Điều này có nghĩa là thanh trượt 'kept messages' bị vô hiệu hóa.
Tin nhắn chèn tùy chỉnh nên bao gồm từ template `{{memories}}` là nơi tất cả các memories được truy vấn sẽ được đặt.

Ưu điểm

- linh hoạt để thử nghiệm vị trí memory
- giới thiệu tùy chỉnh về memory trong ngữ cảnh

Nhược điểm

- vì không có mục chat nào bị xóa/thay thế, có khả năng cao hơn là bạn sẽ làm tràn giới hạn ngữ cảnh.


#### Use % Strategy

Lưu ý: Điều này không tương thích với chiến lược 'Add to Bottom', vốn không xóa bất kỳ tin nhắn nào.

Trong khi sử dụng chiến lược 'Replace Oldest History', việc đánh dấu hộp này sẽ kích hoạt thanh trượt để chọn phần trăm lịch sử chat trong ngữ cảnh để thay thế bằng SmartContext memories. Nó cũng sẽ vô hiệu hóa hai thanh trượt để chọn số lượng tin nhắn theo cách thủ công.

Chiến lược này tự động tính toán phần trăm lịch sử chat được thay thế bằng SmartContext memories, thay vì một số lượng tin nhắn cố định.

Ưu điểm

- dễ dàng hơn việc tự tính toán số lượng tin nhắn theo cách thủ công
- điều chỉnh với kích thước ngữ cảnh có sẵn, áp dụng cùng một phần trăm cho không gian lời nhắc nhỏ và lớn

Nhược điểm

- tính toán cho lượng lịch sử cần xóa có thể hơi không chính xác vì chúng dựa trên tokens ước tính cho mỗi tin nhắn
- nó làm tròn số lượng tin nhắn cần xóa đến số gần nhất chia hết cho 5 (0, 5, 10, 15, 20, v.v.), do đó nó không tinh vi như lựa chọn số thủ công.

***

### Chiến lược gọi lại Memory

#### Recall only from this chat

Đây là hành vi mặc định của smart-context và kéo 'memories' chỉ từ bộ sưu tập ChromaDB cho cuộc trò chuyện cụ thể này.

#### Recall from all character chats

Đây là một hành vi thử nghiệm của smart-context kéo 'memories' từ tất cả các bộ sưu tập ChromaDB cho nhân vật được chọn.
Giả thuyết rằng điều này nên cho phép phát triển một bộ nhớ mạnh mẽ hơn trải dài nhiều tương tác.
Được khuyến nghị sử dụng với chiến lược 'Add to Bottom' hoặc 'Custom Depth' và 'kept messages' được đặt ở số thấp để ChromaDB sẽ kéo từ memory sớm hơn.

### Sử dụng Smart Context

Sau khi được kích hoạt và cấu hình, Smart Context xảy ra tự động.

ChromaDB tạo một cơ sở dữ liệu mới cho mỗi cuộc trò chuyện được mở bên trong SillyTavern.
Cơ sở dữ liệu này tự động được điền với toàn bộ lịch sử chat.

Bạn cũng có thể chèn thủ công các file văn bản vào cơ sở dữ liệu.

Các file văn bản này không nhất thiết phải là chats. Chúng có thể là bất cứ thứ gì (mục wikipedia, fanfic, v.v.).

#### Xóa cơ sở dữ liệu

Bạn có thể sử dụng nút 'Purge DB' để xóa cơ sở dữ liệu cho cuộc trò chuyện hiện tại.

Điều này có thể hữu ích nếu bạn thấy các memories không chính xác đã được lưu trữ (chẳng hạn như tin nhắn chat bạn đã xóa hoặc chỉnh sửa).

***

### FAQ

#### Điều gì xảy ra với cơ sở dữ liệu khi tôi kết thúc trò chuyện? Tôi có thể lưu chúng không?

Đối với Extras servers được cài đặt cục bộ, Smart Context lưu các cơ sở dữ liệu. Không cần lưu chúng theo cách thủ công trong các trường hợp sử dụng thông thường.

Đối với người dùng colab, các cơ sở dữ liệu bị xóa khi extras server tắt. Sử dụng nút export để lưu cơ sở dữ liệu dưới dạng file JSON, và import nó vào lần sau khi bạn muốn sử dụng.

**Thông thường không cần lưu các cơ sở dữ liệu Smart Context.**

Hiện tại chúng tôi có tính năng Import/Export, cho phép bạn lưu DB của chat và sử dụng lại sau này.

#### Tôi có thể tạo một cơ sở dữ liệu lớn cho tất cả các cuộc trò chuyện của mình để tham khảo không?

Đây sẽ không phải là cách sử dụng tốt khả năng của Smart Context.
Chúng tôi khuyên bạn nên sử dụng World Info cho mục đích này.
