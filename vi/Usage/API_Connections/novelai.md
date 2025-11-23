---
route: /usage/api-connections/novelai/
---

# NovelAI

NovelAI là một dịch vụ đăng ký trả phí cho phép truy cập không giới hạn hàng tháng vào các mô hình tạo văn bản, tạo hình ảnh và chuyển văn bản thành giọng nói chất lượng cao nội bộ của họ. Đăng ký tài khoản tại đây để bắt đầu: <https://novelai.net/>

Bạn sẽ chỉ nhận được *50 lần tạo* miễn phí để đánh giá mô hình. Khi lỗi **"Not eligible for this model"** xuất hiện, điều này có nghĩa là bạn đã hết thời gian dùng thử và cần đăng ký gói trả phí.

## API Key

Để lấy API key NovelAI của bạn, hãy làm theo các bước sau:

1. Chọn biểu tượng bánh răng ở đầu thanh bên trái.
![Left Sidebar](/static/novel-side.png)

2. Chọn "Account" trong "User Settings".
![User Settings](/static/novel-user.png)

3. Chọn "Get Persistent API Token".
![Account](/static/novel-account.png)

4. Chọn biểu tượng sao chép để sao chép NovelAI API token của bạn vào clipboard.
![Persistent API Token](/static/novel-token.png)

## Mô Hình

Nếu bạn có Opus, thì Erato là mô hình cần sử dụng. Nếu bạn không có Opus, thì Kayra là mô hình tốt nhất có sẵn.

Clio có kích thước context lớn hơn trên các gói Tablet/scroll, nhưng sức mạnh của Kayra thường bù đắp cho sự khác biệt đó.

## Cài Đặt

Các file với cài đặt ở đây (`SillyTavern/data/<user-handle>/NovelAI Settings`).
Bạn cũng có thể thêm thủ công các file cài đặt của riêng mình.

### Response Length

Bạn muốn tạo bao nhiêu văn bản cho mỗi tin nhắn. Lưu ý rằng NovelAI có giới hạn 150 token cho mỗi phản hồi.

### Context Size

Bao nhiêu token của cuộc trò chuyện được giữ trong context tại bất kỳ thời điểm nào. Kích thước context tối đa bạn có thể sử dụng phụ thuộc vào mô hình và gói đăng ký của bạn:

- Kayra (Tablet) - 3072 tokens
- Kayra (Scroll) - 6144 tokens
- Erato (chỉ Opus), Kayra (Opus) và Clio (tất cả các gói) - 8192 tokens

### Preamble

Văn bản được chèn ngay phía trên cuộc trò chuyện để sửa đổi phong cách viết. Định dạng được đề xuất là một danh sách các tag ngắn, như "[ Style: chat, detailed, sensory ]".

## Mô Tả Preset
Đây là, theo Novel AI, những gì các preset mặc định phù hợp.

### Erato

* Golden Arrow - Một preset toàn diện tốt.
* Wilder - Sự đa dạng cao hơn trong lựa chọn từ, nhiều sự khác biệt hơn giữa các lần reroll, dễ mắc lỗi hơn.
* Zany Scribe - Tránh lỗi và lặp lại. Ưu tiên các từ phức tạp hơn.
* Dragonfruit - Ngôn ngữ đa dạng và phức tạp với ít lặp lại. Lỗi và mâu thuẫn thường xuyên hơn.
* Shosetsu - Được thiết kế để viết bằng tiếng Nhật. Cũng hoạt động tốt cho tiếng Anh.

### Kayra

* Asper - Cho viết sáng tạo. Mong đợi những bước ngoặt bất ngờ.
* Carefree - Một preset toàn diện tốt
* Fresh-Coffee - Giữ mọi thứ đúng hướng. Xử lý instruct tốt.
* Pro_Writer - Bắt chước nhịp độ và cảm giác của tiểu thuyết bán chạy nhất
* Stelenes - Có khả năng chọn các lựa chọn thay thế hợp lý hơn. Đa dạng khi thử lại.
* Tea_Time - Nó trở nên tốt khi nó tiếp tục.
* Writers-Daemon - Cực kỳ giàu trí tưởng tượng, đôi khi quá nhiều.

### Clio

* Edgewise - Xử lý tốt nhiều phong cách tạo khác nhau
* Fresh Coffee - Giữ mọi thứ đúng hướng.
* Long-Press - Dành cho văn xuôi sáng tạo.
* Talker Chat - Được thiết kế cho tạo kiểu trò chuyện.
* Vingt-Un - Một preset toàn diện mặc định tốt với xu hướng về văn xuôi.

## Mẹo và FAQ khi sử dụng NovelAI với SillyTavern

Có rất nhiều vấn đề và câu hỏi phổ biến xuất hiện khi chuyển sang NovelAI từ backend API ST khác. Sự khác biệt đến từ mục đích các mô hình được đào tạo. Rất có thể, bạn đã sử dụng mô hình OpenAI hoặc Anthropic (hoặc mô hình local được tạo để giống chúng), được xây dựng xung quanh việc làm theo hướng dẫn của người dùng. Các mô hình của NovelAI được xây dựng hoàn toàn xung quanh hoàn thành văn bản: thay vì lấy đầu vào của bạn làm tin nhắn và xây dựng phản hồi, các mô hình của NAI cố gắng tiếp tục prompt đến. Do sự khác biệt này, rất nhiều mẹo và kiến thức phổ biến hoạt động cho các API khác sẽ không hoạt động cho NAI.

### Điều chỉnh cài đặt cho NovelAI

Trong Advanced Formatting (biểu tượng A):
- Đặt "Context Template" thành "NovelAI"
- Đặt "Tokenizer" thành "Best match"
- Chọn "Always add character's name to prompt"
- Chọn "Collapse Consecutive Newlines"
- Bỏ chọn hộp "Enabled" trong "Instruct Mode"

Trong User Settings (người có bánh răng)
- Bật "Swipes" (Không cụ thể cho NAI, nhưng nó hữu ích nên bạn nên làm)

### Xây dựng/Điều chỉnh character card cho NovelAI

Để tối ưu hóa character card của bạn cho NovelAI, có một vài phương pháp được đề xuất để viết mô tả nhân vật của bạn: văn xuôi và thuộc tính.

Văn xuôi đơn giản đến mức nó không nên hoạt động: "Sylpheed is a young-looking but actually 900 year old nymph. She's short and petite, with long white hair that fades into a green gradient in her braided side ponytail, and emerald green eyes shaped like crosses.[...]" Không, thật sự, chỉ thế thôi. Chỉ cần viết ra, trong các câu bình thường, nhân vật trông như thế nào, hành động như thế nào, v.v., và AI sẽ nhận ra nó.

Nếu bạn không tin vào khả năng viết của mình hoặc muốn một cách có cấu trúc hơn để thực hiện nó, bạn có thể sử dụng phương pháp thuộc tính, có trong dữ liệu đào tạo NovelAI. Điều này hoạt động như một danh sách đơn giản các đặc điểm nhân vật của các loại khác nhau. Đây là danh sách các thuộc tính có thể đã được thử nghiệm hiệu quả với các mô hình của NovelAI:

```
Name:
AKA:
Type: character
Setting:
Nationality:
Species:
Gender:
Age:
Height:
Weight:
Appearance:
Clothing:
Attire:
Personality:
Mind:
Mental:
Likes:
Dislikes:
Sexuality:
Speech:
Voice:
Abilities:
Skills:
Quote:
Affiliation:
Occupation:
Reputation:
Secret:
Family:
Allies:
Enemies:
Background:
Description:
Attributes:
```

"Type: character" ở đó để báo cho AI biết rằng đây là mô tả một nhân vật (trái ngược với địa điểm, đối tượng hoặc loại thứ khác). Phần còn lại của các thuộc tính là tùy chọn, và một số dư thừa (ví dụ, Personality, Mind và Mental đều có nghĩa cơ bản giống nhau), nhưng chúng đã được thử nghiệm và hoạt động tốt với các mô hình của NovelAI. Điền vào những cái nào liên quan đến nhân vật của bạn. Các thuộc tính nên được viết bằng chữ thường và được phân tách bằng dấu phẩy, không cần dấu ngoặc kép quanh các từ. Ví dụ:

```
Skills: lockpicking, stealth, running away very fast
```

Các phương pháp này được đề xuất vì chúng có trong dữ liệu đào tạo của NovelAI, vì vậy chúng hoạt động đặc biệt tốt với mô hình.

#### Ví dụ card

Đây là một vài card ví dụ, được tạo cho NovelAI, thể hiện các cách khác nhau để tạo card cụ thể cho NovelAI. Card đầu tiên, Valka, sử dụng phương pháp thuộc tính cho mô tả nhân vật, trong khi Eris, card thứ hai, sử dụng mô tả văn xuôi, cùng với một lượng lớn dialogue ví dụ.

<div style="display:flex;gap:2em;justify-content:center">

[![Valka](/static/Valka.png)](/static/Valka.png)

[![Eris](/static/Eris.png)](/static/Eris.png)

</div>

#### Những gì không nên làm

Hầu hết các định dạng character card hiện có không phù hợp với NovelAI. Chúng sẽ cho bạn một số kết quả, thậm chí một số kết quả tốt, nhưng chúng có rất nhiều vấn đề. W++ là một trong những kẻ vi phạm lớn nhất, nơi nó không giống bất cứ điều gì mà các mô hình của NovelAI được đào tạo, và việc sử dụng liên tục của nó các dấu ngoặc/dấu ngoặc nhọn/dấu ngoặc kép ăn mất rất nhiều token, làm phồng kích thước của card mà không có lợi ích thực sự.

Trong các định dạng hiện có không được tích hợp vào NovelAI, AliChat là định dạng có khả năng hoạt động nhất, vì nó dựa vào việc sử dụng các tin nhắn ví dụ để truyền đạt cả thông tin về nhân vật và giọng điệu của họ cùng một lúc, trong định dạng của loại tin nhắn mà bạn muốn AI xuất ra.

Đối với hầu hết các định dạng khác, vì chúng thường là cách liệt kê các đặc điểm khác nhau của một nhân vật cụ thể, chúng có thể được chuyển đổi sang phương pháp thuộc tính khá đơn giản.

### Module nào tôi nên sử dụng?

Có lẽ là No Module. Prose Augmenter hữu ích nếu bạn muốn một nhân vật nói theo cách hoa mỹ hơn, nhưng hãy cẩn thận đừng làm quá. Text Adventure có thể hữu ích cho card/câu chuyện kiểu phiêu lưu văn bản.

### Không phải Instruct module?

Bạn có thể gọi Instruct module khi bạn cần nó. Tạo một dòng mới trong tin nhắn của bạn và đặt hướng dẫn của bạn trong dấu ngoặc nhọn như thế này: `{ CharName is offended by that seemingly innocuous statement }` (khoảng trắng là _bắt buộc_ giữa văn bản và dấu ngoặc nhọn). Làm điều đó sẽ tự động chuyển AI sang Instruct module trong một thời gian ngắn. Bạn không muốn sử dụng Instruct module mọi lúc vì nó có xu hướng tạo ra đầu ra ít sáng tạo hơn các module khác, chỉ khi bạn cần hướng dẫn AI mạnh mẽ theo một hướng cụ thể.

### Tại sao phản hồi của tôi liên tục bị cắt?

NovelAI giới hạn độ dài phản hồi ở ~150 token tổng cộng, ngay cả khi bạn đặt thanh trượt cao hơn thế. Khi đạt đến số lượng token trong thanh trượt hoặc 150, tùy theo số nào thấp hơn, nó sẽ tạo thêm tối đa 20 token, tìm kiếm chuỗi dừng hoặc kết thúc câu, vì vậy có giới hạn hiệu quả là 170 token cho một phản hồi, tại thời điểm đó nó sẽ chỉ dừng lại, khiến nó bị cắt.

Nếu nó bị cắt, bạn có thể chọn tùy chọn tiếp tục (trong menu ba dòng ở bên trái hộp văn bản) để nhân vật tiếp tục phản hồi của họ.

Nếu bạn thường xuyên muốn phản hồi dài hơn 170 token, bạn có thể giải quyết giới hạn như thế này:

- Giữ độ dài phản hồi ở 150 token.
- Trong Advanced Formatting, bật Auto-continue.
- Đặt "Target length" thành độ dài mong muốn.

Điều này sẽ nối nhiều lần tạo lại với nhau để cho bạn tin nhắn dài hơn nhưng không đảm bảo rằng phản hồi sẽ đạt 100% độ dài mong muốn nếu mô hình quyết định dừng.

### Làm thế nào để bot viết phản hồi dài hơn?

Đọc phần trên về phản hồi bị cắt. Điều đó sẽ giúp đảm bảo rằng phản hồi không bị cắt sớm bởi việc chạm đến giới hạn độ dài tạo.

Nếu phản hồi của bạn không bị cắt nhưng vẫn quá ngắn, có khả năng bạn đang đối mặt với "garbage in, garbage out" - nếu bạn cho mô hình các ví dụ xấu, nó sẽ tạo ra đầu ra xấu. Nếu character card không có dialogue ví dụ hoặc dialogue ví dụ ngắn và các tin nhắn bạn gửi cho bot ngắn, mô hình sẽ nhận ra điều đó, coi đó là cách được chấp nhận để làm việc và phản hồi sẽ ngắn. Vì vậy, hãy viết dialogue ví dụ dài hơn và tin nhắn dài hơn cho bot. (Bạn luôn có thể sử dụng NovelAI để viết một số dialogue ví dụ cho bạn thay vì tự làm.)

### Làm thế nào để bot ngừng nói thay tôi?

- Kiểm tra rằng tin nhắn đầu tiên và dialogue ví dụ của character card không bao gồm nhân vật thực hiện hành động cho bạn - nếu có, hãy viết lại chúng để loại bỏ việc nó hành động cho bạn
- Đảm bảo rằng "Always add character's name to prompt" được chọn
- Đảm bảo rằng bạn hiện đang sử dụng cùng user persona như phần còn lại của cuộc trò chuyện. Nếu bạn đã thay đổi user persona và không thay đổi lại (hoặc không có persona được khóa cho cuộc trò chuyện đó), các quy tắc thông thường để ngừng tạo cho bạn sẽ thất bại
- Thêm ["\n\{\{user\}\}:"] vào Custom Stopping Strings (không nên cần thiết, nhưng đôi khi giúp ích)

### Tại sao nhân vật của tôi không phản hồi?

Rất nhiều điều có thể gây ra điều này, vì vậy chúng ta cần xem ở một vài nơi:

- Đảm bảo rằng "Always add character's name to prompt" được chọn trong Advanced Formatting
- Kiểm tra để đảm bảo không có lỗi nào đến từ API. Mặc dù bạn có thể sử dụng SillyTavern với bản dùng thử miễn phí NAI, khi nó hết, bạn sẽ chỉ nhận được lỗi
- Kiểm tra những gì bạn có trong "Custom Stopping Strings" - nếu chúng được tạo ra ở đầu phản hồi, nó có thể bị cắt sớm

### Tôi nên sử dụng Author's Note như thế nào?

Nói chung, bạn có lẽ không nên. Nó được chèn rất gần với cuối context, và với các mô hình của NAI, nó thường xuyên vượt qua mọi thứ khác trong context. Nó chủ yếu là một tàn tích từ các mô hình cũ, yếu hơn nơi nó cần thiết hơn.

### Làm thế nào để tạo ngắt cảnh/nhảy thời gian?

Đặt như sau dưới dạng system message hoặc trên các dòng mới ở đầu tin nhắn tiếp theo của bạn:
```
***
[ 2 days later ]
```

Sau đó đặt phần còn lại của tin nhắn của bạn ở dòng tiếp theo. Văn bản trong dấu ngoặc vuông có thể là nhảy thời gian, địa điểm mới hoặc bất cứ điều gì khác. "***" (được đặt tên buồn cười là "dinkus") báo cho AI biết rằng cảnh đã thay đổi, và văn bản trong ngoặc vuông cung cấp thêm ngữ cảnh cho điều đó.

### AI liên tục lặp lại các từ/cụm từ cụ thể, tôi phải làm gì?

Như đã đề cập ở trên, bạn có thể đẩy thanh trượt repetition penalty lên một chút, mặc dù đẩy nó quá xa có thể làm cho đầu ra mất mạch lạc.
Để sửa vấn đề triệt để hơn, hãy quay lại qua context, đặc biệt là các tin nhắn gần đây, và xóa từ/cụm từ lặp lại. Loại bỏ nó khỏi context cho AI ít lý do hơn để bắt đầu nói nó ngay từ đầu.
