---
route: /extensions/regex/
---

# Regex

## Nó là gì?

Extension Regex cho phép người dùng tự động phát hiện các mẫu cụ thể trong một chuỗi văn bản (được gọi là 'sequences') và áp dụng các thao tác (replacements). Nó có thể là một công cụ mạnh mẽ khi được sử dụng kết hợp với các tính năng SillyTavern khác như [Quick Replies hoặc STscript](/For_Contributors/st-script.md), hoặc đơn giản là một cách để loại bỏ các từ nhất định khỏi chat.

## Liên kết hữu ích

**Tài liệu này sẽ không giải thích quá trình viết một chuỗi RegEx sâu. Có rất nhiều tài nguyên trực tuyến để giúp bạn.**

- [https://regexr.com](https://regexr.com)

- [https://regex101.com](https://regex101.com)

- [https://extendsclass.com/regex-tester.html](https://extendsclass.com/regex-tester.html)

- [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression)

## Điều kiện tiên quyết

Regex là một extension tích hợp của SillyTavern, vì vậy không cần thiết lập bổ sung.

Bạn có thể tìm thấy cài đặt của nó trong panel **<i class="fa-solid fa-cubes"></i> Extensions**.

## Trường hợp sử dụng phổ biến

RegEx thường được sử dụng để áp dụng một chức năng find-replace trên các từ nhất định trong chat, để thêm kiểu markdown cho các từ hoặc loại câu cụ thể, hoặc để trả về một giá trị boolean cho một STscript.

## Script List

![RegEx Extension Script List](/static/extensions/regex-listview.png)

- Các nút ở trên cùng được sử dụng để tạo một script mới.
  - Các script 'Global' sẽ áp dụng cho tất cả các nhân vật và sẽ được lưu vào `settings.json`.
  - Các script 'Scoped' sẽ chỉ áp dụng cho nhân vật hiện tại và sẽ được lưu vào dữ liệu card nhân vật.
- 'Import' cho phép bạn nhập các script RegEx được xuất từ một instance SillyTavern khác.

Dưới đây là danh sách các script của bạn với một số nút hành động.

- Drag handles (ba thanh ngang ở bên trái tên script) cho phép bạn kéo/thả các script theo bất kỳ thứ tự nào bạn muốn.
- Công tắc bật/tắt chính có thể nhanh chóng được chuyển đổi để bật hoặc tắt script mà không thay đổi bất cứ điều gì khác. Các script bị tắt được hiển thị với kiểu ~~strikethrough~~. **Nếu một script bị tắt ở đây, nó sẽ không thể được kích hoạt bởi Quick Reply hoặc STscript.**
- Nút 'Edit' (bút chì) sẽ mở editor script RegEx.
- 'Move to scoped' (mũi tên xuống) sẽ chuyển đổi một script toàn cục thành một script scoped và áp dụng nó cho nhân vật hiện tại. Ngược lại (mũi tên lên), nó sẽ chuyển đổi một script scoped thành toàn cục.
- 'Export' sẽ khiến trình duyệt của bạn tải xuống một file `.json` được xuất của Script, sau đó có thể được chia sẻ và nhập vào một instance SillyTavern khác.
- 'Delete' (thùng rác) xóa script.

## RegEx Editor

![RegEx Editor](/static/extensions/regex-editor.png)

- **Test Mode** : Điều này sẽ mở một chế độ xem so sánh ở đầu editor. Nhập một số văn bản vào hộp 'Input' và kết quả của script RegEx của bạn sẽ được hiển thị trong hộp Output. Đó là một công cụ gỡ lỗi quý giá vì nó sẽ cập nhật hộp Output trong thời gian thực khi bạn thực hiện các thay đổi đối với cài đặt script.

- **Name** : Nhãn cho script được hiển thị trong danh sách script của extension. **Điều này cũng được sử dụng để nhắm mục tiêu script khi kích hoạt nó thông qua slash command hoặc STscript.**

- **Find Regex** : Đây là Regular Expression được sử dụng để phát hiện mẫu văn bản được nhắm mục tiêu của bạn. Đây thường là phần phức tạp nhất của bất kỳ script RegEx nào, và là nơi dễ dàng nhất để mắc lỗi. Tham khảo các liên kết ở đầu trang để biết thông tin cách viết một chuỗi RegEx. Hộp này có thể giải quyết các giá trị của [common SillyTavern macros](/Usage/Characters/macros.md) (như \{\{user\}\}, \{\{char\}\}, v.v.) nếu 'Macros in Find Regex' được đặt để làm như vậy (xem bên dưới).

- **Replace With**: Đây là những gì sẽ thay thế chuỗi được khớp. Trong một ví dụ rất đơn giản, nếu 'Find Regex' của bạn là `apple` và 'Replace With' của bạn là `orange`, thì lần xuất hiện đầu tiên của 'apple' sẽ được tự động thay đổi thành 'orange' trong bất kỳ văn bản nào được áp dụng script.

  - Thêm macro-specific extension \{\{match\}\} vào hộp này sẽ chèn chuỗi được khớp đầy đủ của văn bản. Điều này thường được sử dụng để áp dụng style cho các từ cụ thể. Quay trở lại ví dụ trên, nếu \*\*\{\{match\}\}\*\* được đặt vào hộp 'Replace With' thay vào đó, tất cả các lần xuất hiện của từ 'apple' sẽ được thay thế bằng `**apple**`, sẽ áp dụng kiểu bold markdown cho nó.

  - Các biến như $1, $2, $3, v.v. có thể được sử dụng để chèn những gì được gọi là 'Capture Groups'. Đây là các substrings nằm trong chuỗi văn bản được khớp bởi chuỗi 'Find Regex'. **Lưu ý rằng việc sử dụng các biến này yêu cầu biểu thức khớp chứa các bộ dấu ngoặc để xác định phần nào của chuỗi được khớp tính là một captured group.** Tham khảo các liên kết ở trên để biết tham chiếu về cách thiết lập Capture Groups.

- **Trim Out** : Văn bản đặt trong hộp này sẽ được loại bỏ khỏi chuỗi văn bản được khớp trước khi quá trình 'Replace With' được áp dụng. Ví dụ, nếu match của chúng ta là 'apple', và hộp Trim Out chứa 'le', thì các chữ cái 'le' sẽ bị loại bỏ trước tiên trước khi quá trình 'Replace With' được áp dụng. Vì hộp 'Replace With' của chúng ta chứa \*\*\{\{match\}\}\*\* nó sẽ dẫn đến `**app**` được đặt làm replacement cho 'apple' (trước tiên 'le' bị loại bỏ, và văn bản được khớp còn lại được cho kiểu bold markdown). Có thể áp dụng nhiều trims bằng cách thêm một newline giữa mỗi chuỗi bạn muốn loại bỏ.

- **Affects** : Danh sách các hộp kiểm này xác định các nguồn văn bản mà script RegEx sẽ được áp dụng.
  - 'User Input': script sẽ được chạy đối với nội dung của input được nhập của người dùng sau khi họ nhấn Send.
  - 'AI Response': script sẽ được chạy đối với nội dung của phản hồi AI sau khi nhận được.
  - 'Slash Commands': script sẽ được chạy đối với các giá trị được chèn vào prompt/chat bởi slash commands.
  - 'World Info': script sẽ được chạy trên các nội dung của các mục World Info khi chúng được chèn vào prompt. **Yêu cầu 'Alter Outgoing Prompt' được chọn (hoặc cả hai ephemerality boxes không được chọn).**
  - 'Reasoning': script sẽ được chạy đối với nội dung của đối tượng 'reasoning' được trả về bởi Chat Completion API's như Gemini hoặc Deepseek. Nếu 'Alter Outgoing Prompt' được chọn dưới Ephemerality, script cũng sẽ được áp dụng cho bất kỳ reasoning blocks nào được thêm vào prompt trong các lượt chat tiếp theo.
  - **Nếu tất cả mọi thứ ở đây không được chọn, script sẽ không bao giờ kích hoạt trong quá trình chatting bình thường, nhưng nó vẫn có thể được kích hoạt thông qua slash command hoặc STscript.**

- **Other Options** :
  - 'Disabled' ngăn script chạy. Điều này được sử dụng như một override để ngăn script chạy khi bạn đơn giản là không muốn thay đổi bất kỳ cài đặt script nào và/hoặc không muốn tắt nó hoàn toàn thông qua công tắc trên danh sách script (vì làm như vậy sẽ ngăn slash commands kích hoạt nó).
  - 'Run on Edit' làm cho script cũng chạy sau khi một tin nhắn chat đã được chỉnh sửa. Nếu điều này không được chọn, nội dung của các tin nhắn chat được chỉnh sửa sẽ không kích hoạt script.

- **Macros in Find Regex** : Chọn liệu có thay thế macros (như \{\{user\}\}, \{\{char\}\}, v.v.) có trong hộp chuỗi Find Regex hay không.
  - 'Don't Substitute' sẽ khiến bất kỳ SillyTavern macros nào bị bỏ qua để script RegEx sẽ coi chúng theo nghĩa đen khi tìm kiếm.
  - 'Raw' sẽ gửi giá trị của macro theo đúng nghĩa đen. Điều này có thể thay đổi cách script RegEx của bạn tìm kiếm văn bản nếu giá trị của macro chứa các ký tự đặc biệt nhất định.
  - 'Escaped' sẽ thêm một dấu escape RegEx `\` trước mỗi ký tự để đảm bảo chúng không vô tình thay đổi chuỗi RegEx tổng thể. Điều này có thể hữu ích nếu bạn có các ký tự đặc biệt nhất định trong giá trị của macro.

### Depth Settings

Cài đặt Min/Max Depth cung cấp kiểm soát chính xác đối với các tin nhắn nào trong lịch sử chat mà mẫu regex của bạn sẽ ảnh hưởng:

- **Min Depth**: Chỉ ảnh hưởng đến các tin nhắn ít nhất N levels sâu trong lịch sử chat
  - 0 = tin nhắn cuối cùng
  - 1 = tin nhắn thứ hai đến cuối
  - v.v.
  - Khi để trống (đặt thành 'Unlimited'), hoặc -1, cũng sẽ ảnh hưởng đến tin nhắn để tiếp tục trên Continue action

- **Max Depth**: Chỉ ảnh hưởng đến các tin nhắn không sâu hơn N levels trong lịch sử chat
  - Phải lớn hơn Min Depth để regex áp dụng
  - System prompts và utility prompts không bị ảnh hưởng bởi các cài đặt này

Ví dụ, đặt Min Depth thành 0 và Max Depth thành 2 sẽ chỉ áp dụng regex của bạn cho ba tin nhắn gần đây nhất trong chat.

### Flags

Theo mặc định, mẫu Find Regex phân biệt chữ hoa chữ thường và chỉ áp dụng cho match đầu tiên. Để điều chỉnh hành vi này, cũng như các cờ RegEx khác, bạn có thể thêm chúng như thế này:

```txt
/yourpattern/flags
```

Ví dụ: `/yourpattern/gi` sẽ khớp tất cả các instances của 'yourpattern' trong văn bản, bất kể trường hợp.

Một số cờ phổ biến nhất là:

- `i` : case-insensitive
- `g` : global (áp dụng cho tất cả các matches, không chỉ cái đầu tiên)
- `s` : dotAll (coi input như một dòng duy nhất, vì vậy `.` sẽ khớp với newlines)
- `m` : multi-line (coi input như nhiều dòng, vì vậy `^` và `$` khớp với đầu/cuối của mỗi dòng, không chỉ chuỗi cả)
- `u` : unicode (coi input như unicode, vì vậy `\d`, `\w`, v.v. sẽ khớp với các ký tự unicode)

Để biết thêm thông tin về cờ RegEx, xem trang MDN sau: [Advanced searching with flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions#advanced_searching_with_flags)

### Ephemerality

Theo mặc định (khi không có hộp nào được chọn), một script RegEx sẽ trực tiếp chỉnh sửa các giá trị văn bản được lưu trữ bên trong file JSONL của chat. Điều này đảm bảo cả prompt đi và hiển thị chat sẽ luôn chứa cùng một giá trị. Tuy nhiên, những thay đổi đối với file chat là không thể đảo ngược.

Nếu bạn không muốn điều này xảy ra, bạn có thể bật một trong hai hộp kiểm ở đây để giới hạn các tác động của script RegEx chỉ cho hiển thị hoặc prompt đi.

Nếu chỉ một trong các hộp được chọn, sẽ không có thay đổi nào được thực hiện đối với file chat, nhưng **chỉ item được chọn** sẽ được thay đổi. Điều này có nghĩa là bạn sẽ thấy một điều, nhưng LLM sẽ thấy điều khác. Sử dụng cái này cẩn thận.

Nếu cả hai được chọn, script sẽ hoạt động bình thường theo mọi cách NGOẠI TRỪ nó sẽ không ghi bất kỳ thay đổi nào vào file chat.

## Sử dụng nâng cao

Mặc dù RegEx thường được sử dụng như một công cụ Find/Replace đơn giản, nó cũng có thể được sử dụng theo những cách phức tạp hơn.

Ví dụ, hộp 'Replace With' có thể bao gồm một bộ CSS rules và HTML để thêm một phần tử HTML được định kiểu cụ thể vào chat của bạn bất cứ khi nào một từ nhất định được tìm thấy. Điều này sẽ yêu cầu hộp `Show <tags> in responses` không được chọn trong bảng điều khiển User Settings.

Script cũng có thể được đặt để không bao giờ kích hoạt trong quá trình sử dụng bình thường, nhưng thay vào đó có thể được kích hoạt thông qua slash command như một phần của kiểm tra logic bên trong một STscript. Hộp 'Replace With' sẽ bao gồm một giá trị duy nhất mà script nhận ra để chỉ ra nếu một kiểm tra logic là true hay false. Điều này mở rộng tiện ích của RegEx cho các khả năng đầy đủ của tất cả các slash commands, cho phép các mức kiểm soát và tự động hóa thực sự không giới hạn dựa trên nội dung của chat.
