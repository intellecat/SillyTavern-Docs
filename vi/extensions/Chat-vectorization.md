---
route: /vi/extensions/chat-vectorization/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        summarization,
        chats,
        messages
    ]
---

# Chat Vectorization

!!!warning Tuyên bố từ chối trách nhiệm
Việc sử dụng tiện ích mở rộng này không đảm bảo trải nghiệm trò chuyện tốt hơn hoặc cải thiện bộ nhớ dưới bất kỳ hình thức nào. Chỉ sử dụng nếu bạn hiểu tất cả các ý nghĩa của việc sử dụng cơ sở dữ liệu vector.
!!!

Chat vectorization tìm kiếm các tin nhắn trong lịch sử trò chuyện hiện tại của bạn có vẻ liên quan đến các tin nhắn gần đây nhất của bạn.
Nó tạm thời xáo trộn các tin nhắn liên quan nhất vào đầu hoặc cuối lịch sử trò chuyện.
Điều này xảy ra khi câu trả lời của mô hình cho tin nhắn cuối cùng của bạn được tạo ra.

Các tin nhắn ở đầu và cuối lịch sử trò chuyện có xu hướng có tác động lớn nhất đến câu trả lời của mô hình.
Do đó, việc xáo trộn các tin nhắn liên quan đến các vị trí này có thể giúp mô hình tập trung vào thông tin liên quan trong câu trả lời của nó.

Đặc biệt, chat vectorization có thể tìm thấy các tin nhắn liên quan quá xa trong lịch sử tin nhắn để vừa vào ngữ cảnh yêu cầu. Việc xáo trộn các tin nhắn này vào ngữ cảnh cung cấp cho mô hình thông tin mà nó sẽ không có.

Chat vectorization là một loại retrieval-augmented generation (RAG). Retrieval-augmented generation tăng chất lượng của các phản hồi được tạo bởi mô hình, bằng cách cung cấp thông tin liên quan bổ sung trong prompt.

* Retrieval: các tin nhắn gần đây nhất được sử dụng để truy xuất các tin nhắn quá khứ liên quan
* Augmented: ngữ cảnh của mô hình được bổ sung bằng cách chèn các tin nhắn quá khứ theo cách hữu ích
* Generation: mô hình được hướng dẫn sử dụng các tin nhắn quá khứ khi tạo phản hồi

!!!info Một số thuật ngữ:
*Vector* là một tập hợp các số có thể đại diện cho các chủ đề, nội dung, phong cách hoặc các đặc điểm khác của một đoạn văn bản.

*Vectorization* là việc tính toán vector đại diện cho một đoạn văn bản. Điều này được thực hiện bởi một mô hình vector hóa.
Giống như các mô hình tạo văn bản tạo văn bản từ văn bản, các mô hình vector hóa tạo vector từ văn bản.

*Vector search* tìm kết quả liên quan bằng cách so sánh các vector thay vì, chẳng hạn, từ khóa. Nếu chúng ta tính toán vector cho một truy vấn tìm kiếm, chúng ta có thể so sánh nó với các vector được lưu trữ cho một tập hợp các đoạn văn bản. Điều này tìm thấy các văn bản trong tập hợp của chúng ta giống nhất với văn bản trong truy vấn tìm kiếm. Trong trường hợp chat vectorization, "truy vấn tìm kiếm" là 2 tin nhắn gần đây nhất, và "các văn bản trong tập hợp của chúng ta" là tất cả các tin nhắn khác trong cuộc trò chuyện.
!!!

## Thiết lập

!!!warning Tương thích với Prompt Caching
Giống như bất kỳ nguồn prompt động nào (World Info, Summarization, v.v.), Chat Vectorization tái cấu trúc tiền tố prompt giữa các lần gọi LLM, có thể dẫn đến bỏ lỡ cache thường xuyên. Khi sử dụng với caching, vectorization thường phản tác dụng, vì các prompt đã sửa đổi hiếm khi trúng cache – làm cho caching trở nên vô dụng. Bạn phải chọn một hoặc cái khác, nhưng không phải cả hai.
!!!

Để bật Chat vectorization, chọn "Extensions" > "Vector Storage" > "Enabled for chat messages".

Cấu hình nguồn vectorization và mô hình vectorization. Chat vectorization sử dụng cùng nguồn vector với Data Bank,
vì vậy bạn có thể đã thiết lập điều này. Các cài đặt cho Nguồn vectorization và Mô hình vectorization được ghi lại trong [Data Bank](/Usage/Characters/data-bank.md).

Chat vectorization sử dụng cùng lưu trữ vector với Data Bank, nhưng điều này không cần được thiết lập hoặc cấu hình.
Cũng có thông tin về Vector Storage trong [Data Bank](/Usage/Characters/data-bank.md).

Chat vectorization không sử dụng Data Bank để lưu trữ các tin nhắn trò chuyện. Các tin nhắn được lưu trữ trong cuộc trò chuyện.

## Chuẩn bị tin nhắn trò chuyện để tìm kiếm (vector storage)

Để các tin nhắn trò chuyện có thể được tìm kiếm, một vector được tính toán cho mỗi tin nhắn và được lưu trữ.

Vectorization xảy ra trong nền, bất cứ khi nào bạn gửi hoặc nhận tin nhắn.

Mỗi tin nhắn được lưu trữ riêng lẻ, để nó có thể được tìm thấy và xáo trộn riêng lẻ trong quá trình tạo.

Các tin nhắn lớn được chia thành "chunks" để mô hình có thể được cung cấp phần liên quan nhất của tin nhắn dài. Kích thước chunk là 400 ký tự.
Bạn có thể thay đổi điều này bằng "Chunk size (chars)".

Các tin nhắn được chia thành các chunk bằng cách tìm ranh giới chunk như ngắt đoạn, ngắt dòng hoặc khoảng trắng giữa các từ. Điều này để tất cả các chunk có ý nghĩa, trong chừng mực có thể. Nếu tin nhắn trò chuyện của bạn có cách khác để đánh dấu điểm chia tự nhiên, như `----`,
bạn có thể thêm điều này vào "Chunk boundary". Cài đặt cho "Chunk boundary" được chia sẻ với Data Bank.

### Các điều khiển lưu trữ vector

Để tính toán vector cho tất cả tin nhắn trong cuộc trò chuyện hiện tại, mà không cần đợi chúng được xử lý trong nền, hãy chọn "Vectorize All" từ cài đặt.

Để xem có bao nhiêu tin nhắn trong cuộc trò chuyện hiện tại đã được vector hóa, hãy chọn "View Stats". Điều này hiển thị tổng số vector được lưu trữ.
Nó cũng chỉ ra các tin nhắn trò chuyện cụ thể đã được vector hóa, bằng cách đánh dấu chúng bằng một quả bóng màu xanh lá cây.

Để xóa tất cả các vector cho các tin nhắn trong cuộc trò chuyện hiện tại, hãy chọn "Purge Vectors".

!!!
Các điều khiển cho "Vectorize All" và "Purge Vectors" **trong Chat vectorization** chỉ ảnh hưởng đến các vector được lưu trữ cho cuộc trò chuyện hiện tại.
Tuy nhiên, có các nút giống hệt trong File vectorization ảnh hưởng đến các vector cho các tệp trong Data Bank. Đảm bảo rằng bạn đang xóa các vector mà bạn định xóa.
!!!

## Tìm các tin nhắn liên quan để xáo trộn (vector retrieval)

Để tìm các tin nhắn liên quan nhất trong lịch sử trò chuyện, các tin nhắn gần đây nhất được chuyển đổi (vector hóa) thành vector truy vấn. Theo mặc định, 2 tin nhắn gần đây nhất được sử dụng. Để thay đổi điều này, hãy thay đổi giá trị của "Query messages". Giá trị này cũng được sử dụng khi tìm nội dung liên quan từ Data Bank.

Các tin nhắn quá khứ phải có điểm liên quan ít nhất 25% để được bao gồm. Bạn có thể thay đổi điều này bằng "Score threshold". Cài đặt cho ngưỡng điểm được chia sẻ với Data Bank.

3 tin nhắn liên quan nhất từ lịch sử trò chuyện được xáo trộn. Bạn có thể thay đổi điều này bằng "Insert#".

Để tránh làm phiền các sự kiện gần đây nhất trong cuộc trò chuyện, 5 tin nhắn gần đây nhất không được xáo trộn. Để thay đổi điều này, hãy thay đổi giá trị của "Retain#".

## Xáo trộn tin nhắn (augmented generation)

Các tin nhắn được xáo trộn đến một trong 3 vị trí:

* Đầu cuộc trò chuyện, sau Main Prompt / Story String (mặc định)
* Đầu cuộc trò chuyện và *trước* Main Prompt / Story String
* Cuối cuộc trò chuyện, trước 2 tin nhắn cuối cùng ("In-chat @ Depth 2"). Vì bạn vừa gửi tin nhắn, vị trí này thường ngay trước câu trả lời trước đó từ mô hình.

Bạn có thể thay đổi điều này bằng "Injection Position" và "Depth".

Các tin nhắn được bao gồm theo thứ tự liên quan, với các tin nhắn liên quan hơn được hiển thị sau các tin nhắn ít liên quan hơn.

Tên của người hoặc nhân vật đã gửi mỗi tin nhắn được bao gồm.

Các tin nhắn được hiển thị cho mô hình dưới dạng "sự kiện quá khứ". Điều này hỗ trợ mô hình hiểu rằng các tin nhắn chứa thông tin từ một điểm khác trong lịch sử trò chuyện so với điểm chúng được chèn vào. Bạn có thể thay đổi điều này bằng "Injection Template".

Bạn có thể xem prompt cuối cùng cho mô hình bằng popup Prompt Itemization, terminal logs hoặc browser console logs. Browser console logs hữu ích để hiểu tất cả các bước trong Chat vectorization đang làm gì.

## Vector summarization

!!!warning Cảnh báo
Tính năng Vector summarization là thử nghiệm.

**Vector summarization không tạo tóm tắt cuộc trò chuyện của bạn. Nó không biến các tin nhắn được truy xuất thành tóm tắt. Nó không làm cho lịch sử trò chuyện của bạn ngắn hơn. Nó không phải là "như [Summarize](/extensions/Summarize.md) nhưng tốt hơn".**
!!!

Vector summarization nhằm mục đích làm cho tìm kiếm vector của các tin nhắn trò chuyện hiệu quả hơn. Nó thực hiện điều này bằng cách giới thiệu một bước tóm tắt trước khi vector hóa. Bước tóm tắt trích xuất các phần quan trọng nhất của tin nhắn, để vector kết quả là chỉ báo tốt hơn về những gì tin nhắn liên quan đến.

Vector summarization có thể làm cho tìm kiếm vector kém hiệu quả hơn.

Để tóm tắt các tin nhắn trong lịch sử trò chuyện và tạo vector cho mỗi tin nhắn đã tóm tắt, hãy chọn "Summarize chat messages for vector generation".

Tin nhắn đã tóm tắt không thay thế tin nhắn gốc trong cuộc trò chuyện. Nếu tìm kiếm vector khớp với vector của tin nhắn đã tóm tắt, tin nhắn gốc được truy xuất từ lịch sử trò chuyện và xáo trộn vào ngữ cảnh. Các phiên bản đã tóm tắt của các tin nhắn được giữ lại trong Vector Storage, có thể hữu ích cho việc gỡ lỗi.

Để tóm tắt nội dung của các tin nhắn được sử dụng để tìm kiếm lịch sử trò chuyện (2 tin nhắn cuối cùng theo mặc định), hãy chọn "Summarize chat messages when sending".

Mỗi lần một tin nhắn được tóm tắt để vector hóa, một yêu cầu riêng được thực hiện cho mô hình tóm tắt. Bạn có thể chọn nguồn tóm tắt nào được sử dụng bằng "Summarize with". Chọn "Main API" sẽ tạo tóm tắt bằng cùng mô hình và cài đặt kết nối mà bạn sử dụng để tạo hoàn thành trò chuyện hoặc văn bản.

Yêu cầu bao gồm nội dung tin nhắn thô và hướng dẫn về cách mô hình nên tạo tóm tắt. Bạn có thể thay đổi hướng dẫn bằng "Summary Prompt".
