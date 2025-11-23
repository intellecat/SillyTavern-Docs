---
order: 180
icon: question
route: /usage/faq/
---

# FAQ

## Giải thích SillyTavern là gì

Các mô hình ngôn ngữ AI hiện đại như ChatGPT đã trở nên mạnh mẽ đến mức một số trong số chúng giờ đây có thể mô phỏng một cách thuyết phục một nhân vật mà bạn tạo ra, và bạn có thể trò chuyện, viết tiểu thuyết cùng, v.v. Ví dụ, bạn có thể yêu cầu AI giả vờ là một giáo viên Go tên là Jubei từ thời trung cổ Nhật Bản, và nó sẽ hành động và phản hồi phù hợp. Bạn có thể có một cuộc trò chuyện dài với Jubei, đi đến quán rượu cùng nhau, quyết định đánh nhau với samurai, bất cứ điều gì bạn có thể tưởng tượng, và AI sẽ chơi cùng và viết/phản ứng xung quanh nội dung này, đóng vai là người phản biện và dungeon master của bạn. Trí tưởng tượng của bạn là giới hạn. Bạn có thể bảo AI giả vờ là Wonder Woman. Bạn cũng có thể chỉ định một kịch bản ("Wonder Woman và tôi đang cướp ngân hàng"), một phong cách viết ("Wonder Woman nói tiếng lóng"), hoặc bất cứ điều gì khác bạn có thể nghĩ ra.

SillyTavern là một ứng dụng để hỗ trợ các cách sử dụng này:

* Đây là một giao diện người dùng xử lý giao tiếp với các mô hình ngôn ngữ AI.
* Nó cho phép bạn tạo character card mới (prompts), và chuyển đổi giữa chúng một cách dễ dàng.
* Nó cho phép bạn nhập nhân vật do người khác tạo.
* Nó sẽ giữ lịch sử trò chuyện của bạn với một nhân vật, cho phép bạn tiếp tục bất cứ lúc nào, bắt đầu cuộc trò chuyện mới, xem lại các cuộc trò chuyện cũ, v.v.
* Trong nền, nó thực hiện các việc cần thiết để chuẩn bị prompt AI cho bạn. Cụ thể, nó sẫn gửi một system prompt (hướng dẫn cho AI) để chuẩn bị AI tuân theo các quy tắc nhất định nhằm cải thiện độ chính xác của phản hồi.

## Tổng quan về các tùy chọn mô hình AI

SillyTavern có thể tương tác với hai loại AI:

1. [Dịch vụ web](/Usage/API_Connections/openai.md) (Dựa trên Cloud, thường phải trả phí, độc quyền, đóng)
2. [Self-hosted](/Usage/API_Connections/self-hosted.md) (cục bộ, miễn phí, mã nguồn mở)

### AI dịch vụ web trả phí

Các mô hình web trả phí là hộp đen. Bạn trả tiền cho một công ty để sử dụng dịch vụ AI của họ. Bạn đặt thông tin tài khoản của mình vào SillyTavern và nó sẽ kết nối với nhà cung cấp của bạn để sử dụng AI thay mặt bạn.

Ưu điểm:

* Rất dễ dàng để bắt đầu.
* Chất lượng viết AI cao nhất.

Nhược điểm:

* Chúng tốn tiền để sử dụng.
* Mọi thứ đều được ghi lại trên máy chủ của họ. Vấn đề về quyền riêng tư.
* Chúng thường bị kiểm duyệt và sẽ từ chối trò chuyện với bạn về một số chủ đề nhất định.

### AI Self-hosted

Các mô hình self-hosted là các mô hình miễn phí bạn có thể chạy trên PC của mình nhưng yêu cầu một PC mạnh và nhiều công việc hơn để thiết lập.

Ưu điểm:

* Khi bạn thiết lập xong, chúng có thể được sử dụng miễn phí ngay cả khi không có kết nối Internet.
* Hoàn toàn riêng tư. Mọi thứ bạn viết đều ở trên PC của riêng bạn.
* Có nhiều loại mô hình đa dạng. Là một công nghệ dựa trên cộng đồng, bạn có thể tìm thấy các mô hình phù hợp với các nhiệm vụ hoặc hành vi nhất định mà bạn muốn.

Nhược điểm:

* Chúng không có khả năng như các mô hình <abbr title="State of the art">SOTA</abbr> (tức là chúng viết hội thoại tệ hơn, ít sáng tạo hơn, v.v.).
* Chạy các mô hình cục bộ yêu cầu GPU với ít nhất 6GB VRAM.

Nếu bạn quan tâm đến việc sử dụng chúng, hãy tham khảo hướng dẫn chuyên dụng tại đây: [Cách sử dụng mô hình Self-Hosted](/Usage/API_Connections/self-hosted.md).

## Tôi có thể sử dụng SillyTavern trên điện thoại hoặc máy tính bảng không?

iPhone và iPad không có khả năng chạy toàn bộ ứng dụng SillyTavern, nhưng vì nó chỉ là một giao diện web, bạn có thể chạy nó trên một máy tính khác trên Wi-Fi gia đình của bạn, và sau đó truy cập nó trong trình duyệt di động của bạn. Tham khảo [Remote Connections](/Administration/remote-connections.md) để biết thêm thông tin.

Đối với người dùng Android, ngoài cách trên, bạn có thể chạy toàn bộ SillyTavern trực tiếp trên điện thoại của mình, mà không cần PC, sử dụng ứng dụng Termux. Tham khảo [Installation (Android)](/Installation/Android.md). (LƯU Ý: Cài đặt Termux không được hỗ trợ chính thức, và chúng tôi không thể đảm bảo nó sẽ hoạt động.)

## Tôi đã thử nhập character card PNG nhưng gặp lỗi rằng nó không hợp lệ. Tại sao?

Hai khả năng:

1. Card không có định nghĩa được nhúng bên trong nó và chỉ là một tệp hình ảnh bình thường. Một số chương trình hoặc trình quản lý tệp sẽ loại bỏ các định nghĩa được nhúng khỏi card khi bạn lưu chúng. Đảm bảo rằng bạn đang sử dụng tệp PNG gốc như nó được đăng bởi người chia sẻ.
2. Tệp PNG thực sự là tệp WEBP với tên tệp `.png`. Bạn có thể thử đổi tên card thành `.webp` trước khi nhập, hoặc tìm phiên bản PNG thích hợp của hình ảnh.

## Làm thế nào để tạo nhân vật AI của riêng tôi?

1. Nhấp vào nút Character Management
2. Nhấp vào Create New Character
3. Trong Character Name, đặt tên, như Amanda
4. Tùy chọn, nhấp vào nút Select Avatar để chọn hình ảnh chân dung cho nhân vật này
5. Trong Description, mô tả nhân vật và bao gồm bất kỳ thông tin nào bạn muốn mà bạn cảm thấy liên quan đến cuộc trò chuyện. Ví dụ: ```Amanda là một sinh viên đi du lịch trong năm nghỉ học của mình. Cô ấy cao 6 feet, và là một cầu thủ bóng chuyền. Cô ấy có vóc dáng thể thao. Cô ấy có mái tóc nâu dài. Cô ấy yêu thích thời kỳ Victorian England, và xem TV và đọc tiểu thuyết liên quan đến thời kỳ đó.```
Ví dụ, nếu bạn muốn Amanda thân thiện, thì bạn sẽ thêm: ```Amanda cực kỳ vui vẻ và hướng ngoại.```
6. Trong First Message, viết lời chào của nhân vật khi bạn bắt đầu một cuộc trò chuyện mới. Ví dụ: ```*Amanda vẫy tay chào bạn* Này! Bạn cũng là một du khách ba lô à?```
7. Nhấp vào nút Create Character

Bây giờ bạn đã có một nhân vật cơ bản để trò chuyện. Chọn Amanda từ danh sách nhân vật, và một cuộc trò chuyện mới sẽ bắt đầu.

Lưu ý rằng bạn có thể sử dụng Description và/hoặc First Message để tạo một kịch bản cụ thể hơn, và/hoặc bao gồm chính bạn trong mô tả. Ví dụ:

```txt
Description:
Amanda là một sinh viên đi du lịch trong năm nghỉ học của mình. Cô ấy cao 6 feet, và là một cầu thủ bóng chuyền. Cô ấy có vóc dáng thể thao. Cô ấy có mái tóc nâu dài. Cô ấy yêu thích thời kỳ Victorian England, và xem TV và đọc tiểu thuyết liên quan đến thời kỳ đó. Cô ấy đang giữ một bí mật nặng trĩu trên tâm hồn của mình. Cô ấy đang chờ đúng người để gỡ bỏ gánh nặng, nhưng điều này có thể dẫn đến một trò chơi mèo vờn chuột chống lại một hội kín quyền lực. Cô ấy vừa mới đến Calcutta.

Bạn là Rajesh Nahasmapetilon, một siêu sao bóng chuyền nổi tiếng thế giới của Ấn Độ. Bạn đang đi dạo ở Calcutta. Amanda nhìn thấy bạn và la lên vì phấn khích.

First Message:
*Amanda chạy đến bạn, rạng rỡ.* Rajesh! Tôi không thể tin được! Tôi là một fan hâm mộ lớn. Tôi có poster của bạn trong phòng ngủ của tôi.
```

Bất kỳ thông tin liên quan nào bạn bao gồm đều có thể được sử dụng. Nó được sử dụng tốt như thế nào phụ thuộc vào mức độ mạnh của mô hình AI.

LƯU Ý: bạn có thể quay lại và chỉnh sửa bất kỳ thông tin nào trong số này sau khi nhân vật được tạo, ngoại trừ tên.

## Khóa API của tôi được lưu ở đâu? Tại sao tôi không thể thấy chúng?

SillyTavern lưu khóa API của bạn vào tệp `secrets.json` trong thư mục dữ liệu người dùng (`/data/default-user/secrets.json` là đường dẫn mặc định).

Theo mặc định, khóa API sẽ không hiển thị từ giao diện sau khi bạn đã lưu chúng và làm mới trang.

Để bật xem khóa của bạn:

1. Đặt giá trị của `allowKeysExposure` thành `true` trong tệp `config.yaml`.
2. Khởi động lại máy chủ SillyTavern.
3. Nhấp vào liên kết 'View hidden API keys' ở cuối bên phải của API Connection Panel.

## Mẹo về hiệu suất

### Tại sao giao diện người dùng lại chậm/giật?

* Thử bật chế độ No Blur Effect (Fast UI) trên bảng điều khiển User settings.
* Bật Reduced motion trong cài đặt giao diện UI theme để loại bỏ các animation trang trí.
* Đảm bảo trình duyệt của bạn đang sử dụng Hardware Acceleration.
* Nếu sử dụng response streaming, đặt streaming FPS về giá trị thấp hơn (khuyến nghị 10-15 FPS).

### Tôi đang gặp độ trễ đầu vào. Tôi có thể làm gì?

Suy giảm hiệu suất, đặc biệt là độ trễ đầu vào, thường được cho là do các extension trình duyệt. Các extension có vấn đề đã biết bao gồm:

* iCloud Password Manager
* DeepL Translation
* Công cụ sửa lỗi ngữ pháp dựa trên AI
* Các extension chặn quảng cáo khác nhau

Nếu bạn gặp vấn đề về hiệu suất và không thể xác định nguyên nhân, hoặc nghi ngờ vấn đề với chính SillyTavern, vui lòng:

1. [Ghi lại profile hiệu suất](https://developer.chrome.com/docs/devtools/performance/reference)
2. Xuất profile dưới dạng tệp JSON
3. Gửi nó cho nhóm phát triển để phân tích

Chúng tôi khuyên bạn nên kiểm tra trước với tất cả các extension trình duyệt và extension SillyTavern của bên thứ ba bị tắt để cô lập nguồn gốc của suy giảm hiệu suất.

### Khi tôi nhập rất nhiều nhân vật, ứng dụng trở nên chậm. Tại sao?

Thật không may, SillyTavern không được thiết kế để xử lý thư viện nhân vật khổng lồ. Càng có nhiều, càng mất nhiều thời gian để tải danh sách nhân vật. Dữ liệu chứng cứ cho thấy rằng suy giảm hiệu suất bắt đầu trở nên đáng chú ý khi bạn có hơn 1000 nhân vật.

Tuy nhiên, có một số điều bạn có thể làm để giảm thiểu vấn đề:

**1. Sử dụng lazy loading.**

Bật lazy loading của nhân vật bằng cách đặt giá trị `performance.lazyLoadCharacters` thành true trong tệp `config.yaml`. Sau lần khởi động lại máy chủ tiếp theo, danh sách nhân vật sẽ chỉ tải dữ liệu đầy đủ của các nhân vật bạn tương tác. Xin lưu ý rằng một số extension của bên thứ ba có thể không hoạt động chính xác với cài đặt này được bật nếu chúng không được cập nhật để hỗ trợ nó (liên hệ với nhà phát triển extension để biết thêm thông tin).

**2. Sử dụng memory cache.**

Tăng dung lượng cache bộ nhớ nếu bạn có một chút RAM dư. Điều này sẽ cho phép máy chủ giữ nhiều nhân vật hơn trong bộ nhớ, giảm thời gian cần để tải chúng. Bạn có thể làm điều này bằng cách điều chỉnh giá trị của `performance.memoryCacheCapacity` thành số cao hơn trong tệp `config.yaml`. Giá trị mặc định là `100mb`. Quy tắc đại khái: tăng giá trị lên 100mb cho mỗi 3000 nhân vật bạn có.

**Hạn chế:**

1. Tìm kiếm nhân vật nâng cao (fuzzy) sẽ không hoạt động khi lazy loading được bật. Chỉ tên nhân vật sẽ được tìm kiếm.
2. Memory cache bị vô hiệu hóa trên các thiết bị Android do lượng bộ nhớ có sẵn hạn chế.

## Làm thế nào để AI viết nhiều hơn?

Đôi khi AI sẽ chỉ phản hồi bằng một câu duy nhất khi bạn muốn nó nói nhiều hơn.
Đây thường là vấn đề với các mô hình chạy cục bộ.

Nếu bạn chỉ muốn bot tiếp tục viết từ nơi nó dừng lại ở cuối phản hồi gần đây nhất của nó, bạn có thể gửi một tin nhắn người dùng trống bằng cách không nhập gì vào Input Bar và nhấp Send. Điều này sẽ buộc bot tiếp tục câu chuyện.

Chiến lược để sửa lỗi này:

* Tăng giá trị của cài đặt `Response Length`
* Thiết kế một `First Message` tốt cho Character, hiển thị họ nói theo cách dài dòng. Các mô hình AI có thể cải thiện rất nhiều khi được hướng dẫn về phong cách viết bạn mong đợi.
* Thêm một cụm từ trong Description Box của nhân vật như "thích nói nhiều" hoặc "người nói rất dài dòng"
* Làm điều tương tự cho `Author's Note` của bạn, hoặc `Post-History Instruction Prompt`
* Như một phương sách cuối cùng, bạn có thể thử bật `Auto-Continue` (trong bảng điều khiển User Settings), nhưng sẽ làm cho các phản hồi ra chậm hơn vì nó làm cho AI tạo ra các phản hồi nhỏ liên tiếp, và sau đó kết hợp tất cả chúng lại thành một phản hồi lớn. Nó cũng có thể không tương thích với một số tùy chọn API.

## Làm thế nào để AI viết ít hơn?

Đây chủ yếu chỉ là vấn đề đối với các mô hình như ChatGPT hoặc Claude. Các chiến lược tương tự có thể được áp dụng nhưng ngược lại.

* Giảm giá trị của cài đặt `Response Length`
* Cho nhân vật một cụm từ như 'nói ngắn gọn', hoặc 'không nói nhiều' trong Description của họ.
* Cho nhân vật một First Message ngắn gọn để đặt tông và kỳ vọng cho cuộc trò chuyện.
* Đảm bảo `Auto-Continue` bị tắt.

## Làm thế nào để AI ngừng viết hành động của nhân vật của tôi và tự mình điều khiển cốt truyện?

Điều này nên được xử lý trong `Author's Note` với sự kết hợp của các cụm từ như:

* Phản hồi của \{\{char\}\} chỉ nên thụ động và phản ứng với hành động của \{\{user\}\}.
* Phản hồi tiếp theo của bạn chỉ nên từ POV của \{\{char\}\}.
* Bạn không bao giờ được phép ra lệnh hành động hoặc lời nói cho \{\{user\}\}
