---
order: 30
route: /vi/usage/core-concepts/data-bank/
tags:
    [
        vector storage,
        RAG,
        retrieval-augmented generation,
        vectors,
        documents,
        files,
        attachments,
    ]
---

# Data Bank (RAG)

Retrieval-augmented generation (RAG) là một kỹ thuật cung cấp các nguồn kiến thức bên ngoài cho LLM. Nó giúp cải thiện độ chính xác của các câu trả lời AI bằng cách truy cập thông tin ngoài dữ liệu huấn luyện của model.

SillyTavern cung cấp một bộ công cụ để xây dựng cơ sở kiến thức đa mục đích từ nhiều nguồn khác nhau, cũng như sử dụng dữ liệu đã thu thập trong các prompt của LLM.

## Truy cập Data Bank

Extension Chat Attachments tích hợp sẵn (được bao gồm theo mặc định trong các phiên bản phát hành >= 1.12.0) thêm một tùy chọn mới trong menu "Magic Wand" - Data Bank. Đây là trung tâm của bạn để quản lý các tài liệu có sẵn cho RAG trong SillyTavern.

## Về Documents

Data Bank lưu trữ các tệp đính kèm, còn được gọi là documents. Các documents được chia thành ba phạm vi khả dụng.

1. Global attachments - có sẵn trong mọi chat, dù là solo hay group.
2. Character attachments - chỉ có sẵn cho nhân vật hiện đang được chọn, bao gồm khi họ đang trả lời trong một group. _Attachments được lưu cục bộ và không được xuất cùng với character card!_
3. Chat attachments - chỉ có sẵn trong chat hiện đang mở. Mọi nhân vật trong chat đều có thể lấy từ nó.

!!!info Lưu ý
Mặc dù không chính thức là một phần của data bank, bạn có thể đính kèm tệp ngay cả vào các tin nhắn riêng lẻ. Sử dụng tùy chọn Attach File từ menu "Wand", hoặc biểu tượng kẹp giấy trong hàng hành động tin nhắn.
!!!

Document có thể là gì? Thực tế là bất kỳ thứ gì có thể biểu diễn được dưới dạng văn bản thuần túy!

Ví dụ bao gồm, nhưng không giới hạn ở:

- Tệp cục bộ (sách, bài báo khoa học, v.v.)
- Trang web (Wikipedia, bài viết, tin tức)
- Bản ghi video

Các extension và plugin khác nhau cũng có thể cung cấp các cách mới để thu thập và xử lý dữ liệu, thêm về điều đó bên dưới.

## Nguồn dữ liệu

Để thêm một document vào bất kỳ phạm vi nào, hãy nhấp vào "Add" và chọn một trong các nguồn có sẵn.

### Notepad

Tạo tệp văn bản từ đầu hoặc chỉnh sửa một attachment hiện có.

### File

Tải lên một tệp từ ổ cứng của máy tính. SillyTavern cung cấp các bộ chuyển đổi tích hợp cho các định dạng tệp phổ biến:

- PDF (chỉ văn bản)
- HTML
- Markdown
- ePUB
- TXT

Bạn cũng có thể đính kèm bất kỳ tệp văn bản nào với các phần mở rộng không chuẩn, chẳng hạn như JSON, YAML, mã nguồn, v.v. Nếu không có chuyển đổi nào được biết đến từ loại tệp đã chọn và tệp không thể được phân tích cú pháp dưới dạng tài liệu văn bản thuần túy, việc tải lên tệp sẽ bị từ chối, có nghĩa là các tệp binary thô không được phép.

!!!info Lưu ý
Nhập tài liệu Microsoft Office (DOCX, PPTX, XLSX) và LibreOffice (ODT, ODP, ODS) yêu cầu [Server Plugin](https://github.com/SillyTavern/SillyTavern-Office-Parser) được cài đặt và tải. Xem trang README của plugin để biết hướng dẫn cài đặt.
!!!

### Web

Lấy văn bản từ một trang web bằng URL của nó. Tài liệu HTML sau đó được xử lý thông qua thư viện [Readability](https://github.com/mozilla/readability) để chỉ trích xuất văn bản có thể sử dụng.

Một số máy chủ web có thể từ chối yêu cầu fetch, được bảo vệ bởi Cloudflare hoặc phụ thuộc nhiều vào JavaScript để hoạt động. Nếu bạn gặp sự cố với bất kỳ trang web cụ thể nào, hãy tải xuống trang theo cách thủ công thông qua trình duyệt web và đính kèm nó bằng cách sử dụng file uploader.

### YouTube

Tải xuống bản ghi của video YouTube bằng ID hoặc URL của nó, được tải lên bởi người tạo hoặc được tự động tạo bởi Google. Một số video có thể tắt bản ghi, ngoài ra việc phân tích cú pháp các video hạn chế độ tuổi không khả dụng vì nó yêu cầu đăng nhập.

Bản ghi được tải bằng ngôn ngữ mặc định của video. Tùy chọn, bạn có thể chỉ định mã ngôn ngữ hai chữ cái để thử và tải bản ghi bằng một ngôn ngữ cụ thể. Tính năng này không phải lúc nào cũng có sẵn và có thể thất bại, vì vậy hãy sử dụng nó một cách thận trọng.

### Web Search

!!!info Lưu ý
Nguồn này yêu cầu phải có extension [Web Search](/extensions/WebSearch.md) được cài đặt và cấu hình đúng cách. Xem trang được liên kết để biết thêm chi tiết.
!!!

Thực hiện tìm kiếm web và tải xuống văn bản từ các trang kết quả tìm kiếm. Điều này tương tự như nguồn Web nhưng hoàn toàn tự động. Một công cụ tìm kiếm được chọn sẽ được kế thừa từ cài đặt extension, vì vậy hãy thiết lập nó trước.

Để bắt đầu, chỉ định truy vấn tìm kiếm, số lượng liên kết tối đa được truy cập và loại đầu ra: một tệp kết hợp (được định dạng theo quy tắc extension) hoặc một tệp riêng cho mỗi trang. Bạn có thể chọn lưu các đoạn trích trang cũng như.

### Fandom

!!!info Lưu ý
Nguồn này yêu cầu phải có [Server Plugin](https://github.com/SillyTavern/SillyTavern-Fandom-Scraper) được cài đặt và tải. Xem trang README của plugin để biết hướng dẫn cài đặt.
!!!

Lấy các bài viết từ wiki [Fandom](https://www.fandom.com/) bằng ID hoặc URL của nó. Vì một số wiki rất lớn, có thể có lợi khi giới hạn phạm vi bằng cách sử dụng biểu thức chính quy bộ lọc, nó sẽ được kiểm tra với tiêu đề của bài viết. Nếu không có bộ lọc nào được cung cấp, thì tất cả các trang đều có thể được xuất. Bạn có thể lưu chúng dưới dạng các tệp riêng lẻ cho mỗi trang hoặc kết hợp thành một tài liệu duy nhất.

### Bronie Parser Extension (Bên thứ ba)

!!!warning Lưu ý
Nguồn này đến từ bên thứ ba và **không liên kết** với nhóm SillyTavern. Nguồn này yêu cầu bạn phải cài đặt [Bronie Parser Extension](https://github.com/Bronya-Rand/Bronie-Parser-Extension) của Bronya Rand cũng như các Server Plugins yêu cầu parser hoạt động.
!!!

Bronie Parser Extension của Bronya Rand cho phép sử dụng các scrapers bên thứ ba, chẳng hạn như [HoYoLab](https://wiki.hoyolab.com) của miHoYo/HoYoverse vào SillyTavern, tương tự như các nguồn dữ liệu khác.

Hiện tại, Bronie Parser Extension của Bronya Rand hỗ trợ những điều sau:

- HoYoLab của miHoYo/HoYoverse (cho Genshin Impact/Honkai: Star Rail) qua [HoYoWiki-Scraper-TS](https://github.com/Bronya-Rand/HoYoWiki-Scraper-TS)

Để bắt đầu, cài đặt Bronie Parser Extension của Bronya Rand bằng cách làm theo [hướng dẫn cài đặt](https://github.com/Bronya-Rand/Bronie-Parser-Extension?tab=readme-ov-file#installation) và cài đặt một Server Plugin được hỗ trợ vào SillyTavern. Khởi động lại SillyTavern và đi đến menu _Data Bank_. Nhấp vào `+ Add` và bạn sẽ thấy rằng các scrapers bạn vừa cài đặt được thêm vào danh sách các nguồn có thể lấy thông tin từ đó.

## Vector Storage

Vì vậy, bạn đã tự xây dựng cho mình một thư viện thông tin tốt và toàn diện về chủ đề cụ thể của bạn. Tiếp theo là gì?

Để sử dụng các documents cho RAG, bạn cần sử dụng một extension tương thích sẽ chèn dữ liệu liên quan vào prompt của LLM.

Vector Storage, được đi kèm với SillyTavern, là một triển khai tham chiếu của một extension như vậy. Nó sử dụng embeddings (còn được gọi là vectors) để tìm kiếm các documents liên quan đến các cuộc chat đang diễn ra của bạn.

!!!info Sự thật thú vị

1. Embeddings là các mảng số biểu diễn trừu tượng một đoạn văn bản, được tạo ra bởi các model ngôn ngữ chuyên biệt. Các văn bản tương tự hơn có khoảng cách ngắn hơn giữa các vector tương ứng của chúng.
2. Extension Vector Storage sử dụng thư viện [Vectra](https://github.com/Stevenic/vectra) để theo dõi các file embeddings. Chúng được lưu trữ trong các tệp JSON trong thư mục `/vectors` của thư mục dữ liệu người dùng của bạn. Mỗi document được biểu diễn nội bộ bằng tệp index/collection riêng của nó.
   !!!

Vì chức năng Vectors bị tắt theo mặc định, bạn cần mở bảng extensions (biểu tượng "Stacked Cubes" trên thanh trên cùng), sau đó điều hướng đến phần "Vector Storage" và đánh dấu vào hộp kiểm "Enabled for files" trong "File vectorization settings".

Bản thân Vector Storage không tạo ra bất kỳ vectors nào, bạn cần sử dụng một nhà cung cấp embedding tương thích.

## Nhà cung cấp Vector

!!!warning Cảnh báo
Embeddings chỉ có thể sử dụng được khi chúng được truy xuất bằng cùng một model đã tạo ra chúng. Khi thay đổi model hoặc nguồn embedding, các vectors cần được tính toán lại.
!!!

### Cục bộ

Các nguồn này miễn phí và không giới hạn và sử dụng CPU/GPU của bạn để tính toán embeddings.

1. Local (Transformers) - chạy trên máy chủ Node. SillyTavern sẽ tự động tải xuống một model tương thích ở định dạng ONNX từ HuggingFace. Model mặc định: [jina-embeddings-v2-base-en](https://huggingface.co/Cohee/jina-embeddings-v2-base-en).
2. WebLLM - yêu cầu một extension được cài đặt và một trình duyệt web [hỗ trợ WebGPU](https://caniuse.com/webgpu). Chạy trực tiếp trong trình duyệt của bạn, có thể sử dụng tăng tốc phần cứng. Tự động tải xuống các model được hỗ trợ từ HuggingFace. Cài đặt extension từ đây: <https://github.com/SillyTavern/Extension-WebLLM>.
3. Ollama - lấy từ <https://ollama.com/>. Đặt URL API trong menu kết nối API (trong Text Completion, mặc định: `http://localhost:11434`). Phải tải xuống một model tương thích trước, sau đó đặt tên của nó trong cài đặt extension. Model ví dụ: [mxbai-embed-large](https://ollama.com/library/mxbai-embed-large). Tùy chọn, kiểm tra một tùy chọn để giữ model được tải trong bộ nhớ.
4. llama.cpp server - lấy từ [ggerganov/llama.cpp](https://github.com/ggerganov/llama.cpp) và chạy tệp thực thi máy chủ với cờ `--embedding`. Tải các model embedding GGUF tương thích từ HuggingFace, ví dụ: [nomic-ai/nomic-embed-text-v1.5-GGUF](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5-GGUF).
5. vLLM - lấy từ [vllm-project/vllm](https://github.com/vllm-project/vllm). Đặt URL API và API key trong menu kết nối API trước.
6. Extras (không được dùng nữa) - chạy trong [Extras API](https://github.com/SillyTavern/SillyTavern-extras) sử dụng SentenceTransformers loader. Model mặc định: [all-mpnet-base-v2](https://huggingface.co/sentence-transformers/all-mpnet-base-v2). Nguồn này không được duy trì và cuối cùng sẽ bị loại bỏ trong tương lai.

### Nguồn API

Tất cả các nguồn này yêu cầu API key của dịch vụ tương ứng và thường có chi phí sử dụng, nhưng nhìn chung việc tính toán embeddings khá rẻ.

1. OpenAI
2. Cohere
3. Google AI Studio
4. Google Vertex AI
5. TogetherAI
6. MistralAI
7. NomicAI
8. OpenRouter
9. Electron Hub
10. Chutes
11. NanoGPT
12. SiliconFlow
13. Cloudflare Workers AI

## Cài đặt Vectorization

Sau khi bạn đã chọn nhà cung cấp embedding của mình, đừng quên cấu hình các cài đặt khác sẽ xác định các quy tắc để xử lý và truy xuất documents.

!!!info Lưu ý
Tách, vectorization và truy xuất thông tin từ các attachments mất một chút thời gian. Mặc dù việc thu nạp ban đầu của tệp có thể mất một lúc, các truy vấn tìm kiếm RAG thường đủ nhanh để không tạo ra độ trễ đáng kể.
!!!

### Message attachments

Các cài đặt này kiểm soát các tệp được đính kèm trực tiếp vào các tin nhắn.

Các quy tắc sau áp dụng:

1. Chỉ các tin nhắn phù hợp với cửa sổ context của LLM mới có thể truy xuất các attachments của chúng.
2. Khi extension vector storage bị tắt, các file attachments và tin nhắn đi kèm của chúng được chèn đầy đủ vào prompt.
3. Khi file vectorization được bật, thì tệp sẽ được chia thành các chunks và chỉ các phần liên quan nhất sẽ được chèn, tiết kiệm không gian context và cho phép model tập trung.

- Size threshold (KB) - đặt ngưỡng chia chunk. Chỉ các tệp lớn hơn kích thước được chỉ định mới được chia.
- Chunk size (chars) - đặt kích thước mục tiêu của một chunk riêng lẻ (bằng ký tự văn bản, không phải token của model!).
- Chunk overlap (%) - đặt phần trăm kích thước chunk sẽ được chia sẻ giữa các chunks liền kề. Điều này cho phép chuyển tiếp mượt mà hơn giữa các chunks, nhưng cũng có thể tạo ra một số sự dư thừa.
- Retrieve chunks - đặt số lượng tối đa các chunks tệp liên quan nhất được truy xuất. Chúng sẽ được chèn theo thứ tự ban đầu của chúng.

### Tệp Data Bank

Các cài đặt này kiểm soát cách xử lý các documents Data Bank.

Các quy tắc sau áp dụng:

1. Khi file vectorization bị tắt, Data Bank không được sử dụng.
2. Ngược lại, tất cả các documents có sẵn từ phạm vi hiện tại (xem ở trên) được xem xét cho truy vấn. Chỉ các chunks liên quan nhất trên tất cả các tệp được truy xuất. Nhiều chunks của cùng một tệp được chèn theo thứ tự ban đầu của chúng.
3. Các chunks được chèn sẽ dành trước một phần của context trước khi khớp các tin nhắn chat.

- Size threshold (KB) - đặt ngưỡng chia chunk. Chỉ các tệp lớn hơn kích thước được chỉ định mới được chia.
- Chunk size (chars) - đặt kích thước mục tiêu của một chunk riêng lẻ (bằng ký tự văn bản, không phải token của model!).
- Chunk overlap (%) - đặt phần trăm kích thước chunk sẽ được chia sẻ giữa các chunks liền kề. Điều này cho phép chuyển tiếp mượt mà hơn giữa các chunks, nhưng cũng có thể tạo ra một số sự dư thừa.
- Retrieve chunks - đặt số lượng tối đa các chunks tệp được truy xuất. Khoản trợ cấp này được chia sẻ giữa tất cả các tệp.
- Injection Template - xác định cách thông tin được truy xuất sẽ được chèn vào prompt. Bạn có thể sử dụng macro đặc biệt \{\{text\}\} để chỉ định vị trí của văn bản được truy xuất, cũng như bất kỳ macro nào khác.
- Injection Position - đặt vị trí chèn prompt injection. Các quy tắc tương tự như Author's Note và World Info được áp dụng.

### Cài đặt được chia sẻ

- Query messages - có bao nhiêu tin nhắn chat mới nhất sẽ được sử dụng để truy vấn các chunks document.
- Score threshold - điều chỉnh để cho phép loại bỏ việc truy xuất các chunks dựa trên điểm liên quan của chúng (0 - không khớp, 1 - khớp hoàn hảo). Giá trị cao hơn cho phép truy xuất chính xác hơn và ngăn thông tin hoàn toàn ngẫu nhiên xâm nhập vào context. Các giá trị hợp lý nằm trong khoảng từ 0.2 (lỏng lẻo hơn) đến 0.5 (tập trung hơn).
- Chunk boundary - một chuỗi tùy chỉnh sẽ được ưu tiên khi chia các tệp thành các chunks. Nếu không được chỉ định, mặc định là chia theo (theo thứ tự) ngắt dòng đôi, ngắt dòng đơn và khoảng trắng giữa các từ.
- Only chunk on custom boundary - nếu được bật, việc chia chunk sẽ chỉ xảy ra trên ranh giới chunk được chỉ định. Ngược lại, việc chia chunk cũng sẽ xảy ra trên các ranh giới mặc định.
- Translate files into English before processing - nếu được bật, sẽ sử dụng API dịch được cấu hình trong extension [Chat Translation](/extensions/Translation.md) để dịch các tệp sang tiếng Anh trước khi xử lý chúng. Điều này hữu ích khi sử dụng các model embedding chỉ hỗ trợ văn bản tiếng Anh.
- Include in World Info Scanning - kiểm tra nếu bạn muốn nội dung được chèn kích hoạt các mục lore book.
- Vectorize All - buộc thu nạp embeddings cho tất cả các tệp chưa được xử lý.
- Purge Vectors - xóa các file embeddings, cho phép tính toán lại các vectors của chúng.

!!!info Lưu ý
Đối với cài đặt "Chat vectorization", xem [Chat Vectorization](/extensions/Chat-vectorization.md).
!!!

## Kết luận

Xin chúc mừng! Trải nghiệm chat của bạn giờ đây đã được nâng cao với sức mạnh của RAG. Khả năng của nó chỉ bị giới hạn bởi trí tưởng tượng của bạn. Như mọi khi, đừng ngại thử nghiệm!
