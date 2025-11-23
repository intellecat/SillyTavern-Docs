---
order: 130
icon: globe
route: /usage/core-concepts/worldinfo/
templating: false
---

# World Info

**World Info (còn được gọi là Lorebooks hoặc Memory Books) là một công cụ mạnh mẽ có sẵn trong ST để chèn prompt động vào cuộc trò chuyện của bạn nhằm giúp hướng dẫn phản hồi của AI.**

Thông thường, World Info (viết tắt là WI) được sử dụng để tăng cường hiểu biết của AI về các chi tiết trong thế giới hư cấu của bạn, tuy nhiên bạn có thể sử dụng entry World Info để chèn BẤT CỨ ĐIỀU GÌ mà bạn muốn chèn vào prompt.

Nó hoạt động như một từ điển động chỉ chèn thông tin liên quan từ các entry World Info khi các từ khóa liên kết với các entry có mặt trong văn bản tin nhắn.

Engine SillyTavern kích hoạt và tích hợp liền mạch lore phù hợp vào prompt, cung cấp thông tin nền cho AI.

*Điều quan trọng cần lưu ý là trong khi World Info giúp hướng dẫn AI hướng đến nội dung mong muốn, nó không đảm bảo sự xuất hiện của nó trong các tin nhắn đầu ra được tạo. Điều đó phụ thuộc vào mô hình của bạn sử dụng thông tin bổ sung tốt như thế nào!*

## Mẹo Pro

* Engine World Info là một công cụ quản lý prompt rất mạnh mẽ. Đừng chỉ tập trung vào việc thêm lore nhân vật, hãy thoải mái thử nghiệm.
* Từ khóa kích hoạt, tiêu đề và thông tin khác không có trong trường **Content** không được chèn vào context, vì vậy mỗi entry World Info nên có mô tả toàn diện, độc lập.
* Để tạo ra lore thế giới phong phú và chi tiết, các entry có thể được liên kết với nhau và tham chiếu lẫn nhau bằng cách sử dụng kích hoạt đệ quy. Xem thêm về [Recursion](#recursive-scanning) bên dưới.
* SillyTavern cung cấp ngân sách context linh hoạt cho thông tin nền được chèn vào. Để tiết kiệm token prompt, nên giữ nội dung entry ngắn gọn.

## Đọc thêm

* [World Info Encyclopedia](https://rentry.co/world-info-encyclopedia): Hướng dẫn chuyên sâu đầy đủ về World Info và Lorebooks. Bởi kingbri, Alicat, Trappu.

## Character Lore

Tùy chọn, các tệp World Info có thể được gán cho một nhân vật để phục vụ như các nguồn lore chuyên dụng trên tất cả các cuộc trò chuyện với nhân vật đó (bao gồm cả nhóm).

Một World Info chính có thể được gắn vào nhân vật. Để làm điều đó, điều hướng đến bảng điều khiển Character Management và nhấp vào nút globe, sau đó chọn World Info từ danh sách thả xuống và nhấp "Ok". Khi xuất nhân vật, tệp này cũng sẽ được nhúng vào dữ liệu character card.

Để hủy gắn, thay đổi hoặc gán thêm tệp World Info làm character lore, shift-click nút globe hoặc nhấp "More..." sau đó "Link World Info". Lưu ý rằng chỉ tệp World Info chính được xuất cùng với nhân vật.

### Chiến lược chèn Character Lore

Khi tạo phản hồi AI, các entry từ World Info nhân vật sẽ được kết hợp với các entry từ bộ chọn World Info toàn cầu bằng một trong các chiến lược sau:

#### Sorted Evenly (mặc định)

Tất cả các entry sẽ được sắp xếp theo Insertion Order của chúng như thể chúng là một phần của một tệp lớn, bỏ qua nguồn.

#### Character Lore First

Các entry từ Character World Info sẽ được bao gồm trước theo Insertion Order của chúng, sau đó là các entry từ Global World Info.

#### Global Lore First

Các entry từ Global World Info sẽ được bao gồm trước theo Insertion Order của chúng, sau đó là các entry từ Character World Info.

### World Info Entry

#### Key

Danh sách các từ khóa kích hoạt một entry World Info. Các key không phân biệt chữ hoa chữ thường theo mặc định (điều này có thể [cấu hình](#case-sensitive-keys)).

##### Regular Expression (Regex) làm Keys

Các key cho phép cách tiếp cận linh hoạt hơn để khớp bằng cách hỗ trợ regex. Điều này giúp có thể khớp nội dung động hơn với các từ hoặc ký tự tùy chọn, khoảng trắng và tất cả các tiện ích khác mà regex cung cấp.
Nếu một key được định nghĩa là regex hợp lệ (kiểu regex Javascript, với `/` làm dấu phân cách. Tất cả các flag đều được phép), nó sẽ được xử lý như vậy khi kiểm tra xem một entry có nên được kích hoạt hay không. Nhiều regex có thể được nhập dưới dạng các key riêng biệt và sẽ hoạt động cùng nhau. Bên trong một regex, dấu phẩy là có thể. Các key plaintext không hỗ trợ dấu phẩy, vì chúng được xử lý như dấu phân cách key.

Một ví dụ về trường hợp sử dụng cho khớp regex nâng cao:
Một entry/instruction nên được chèn vào khi char đang thực hiện hành động liên quan đến thời tiết

```js
/(?:{{char}}|he|she) (?:is talking about|is noticing|is checking whether|observes) (?:the )?(rainy weather|heavy wind|it is going to rain|cloudy sky)/i
```

Để biết thêm thông tin về cú pháp và khả năng Regex: [Regular expressions - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions)

###### Advanced Regex Per-Message Matching

ST thêm tiền tố cho mỗi tin nhắn trò chuyện trong buffer quét WI với `character name:` và sau v1.12.6, nối thêm chúng bằng ký tự giá trị 1 (`\x01`).
Điều này có nghĩa là bạn có thể khớp đầu vào hoặc đầu ra cụ thể từ một nhân vật nhất định bằng regex gắn với ký tự phân tách đó.

Ví dụ, để chỉ khớp người dùng nói "hello", bạn có thể sử dụng regex sau:

```js
/\x01{{user}}:[^\x01]*?hello/
```

##### Key Input

Có hai chế độ để nhập từ khóa, mỗi chế độ có giao diện người dùng hơi khác nhau. Trong ⌨️ *plaintext mode* (mặc định), các key có thể được nhập dưới dạng danh sách phân tách bằng dấu phẩy trong một trường văn bản duy nhất. Regex cũng có thể được bao gồm, nhưng chúng không có bất kỳ làm nổi bật đặc biệt nào. Trong ✨ *fancy mode*, các key xuất hiện dưới dạng các phần tử riêng biệt và regex sẽ được làm nổi bật như vậy. Control hỗ trợ chỉnh sửa và xóa keys. Chế độ có thể được chuyển đổi qua nút inline bên trong input control.

#### Optional Filter

Danh sách các từ khóa bổ sung phân tách bằng dấu phẩy kết hợp với primary key.
Nếu không có đối số nào được cung cấp, flag này bị bỏ qua.
Hỗ trợ logic cho AND ANY, NOT ANY, hoặc NOT ALL

1. AND ANY = Kích hoạt entry chỉ khi primary key và Bất kỳ một trong các optional filter key có trong context được quét.
2. AND ALL = Kích hoạt entry chỉ khi primary key và TẤT CẢ các optional filter key đều có mặt.
3. NOT ANY = Kích hoạt entry chỉ khi primary key và Không có optional filter key nào trong context được quét.
4. NOT ALL = Ngăn kích hoạt entry mặc dù có primary key trigger, nếu tất cả các optional filter đều trong context được quét.

Các key này cũng hỗ trợ [regex](#regular-expression-regex-as-keys).

#### Entry Content

Văn bản được chèn vào prompt khi entry được kích hoạt.

#### Insertion Order

Giá trị số. Xác định mức độ ưu tiên của entry nếu nhiều entry được kích hoạt cùng một lúc. Các entry có số order lớn hơn sẽ được chèn gần cuối context hơn vì chúng sẽ có tác động nhiều hơn đến đầu ra. Ví dụ, một entry có số Order 100 sẽ xuất hiện trong context trước một entry có số Order 250.

#### Insertion Position

* **Before Char Defs:** Entry World Info được chèn trước mô tả và kịch bản của nhân vật. Có tác động trung bình đến cuộc trò chuyện.
* **After Char Defs:** Entry World Info được chèn sau mô tả và kịch bản của nhân vật. Có tác động lớn hơn đến cuộc trò chuyện.
* **Before Example Messages:** Entry World Info được phân tích như một khối hội thoại mẫu và được chèn trước các ví dụ do character card cung cấp.
* **After Example Messages:** Entry World Info được phân tích như một khối hội thoại mẫu và được chèn sau các ví dụ do character card cung cấp.
* **Top of AN:** Entry World Info được chèn ở đầu nội dung Author's Note. Có tác động biến đổi tùy thuộc vào vị trí Author's Note.
* **Bottom of AN:** Entry World Info được chèn ở cuối nội dung Author's Note. Có tác động biến đổi tùy thuộc vào vị trí Author's Note.
* **@ D:** Entry World Info được chèn ở một độ sâu cụ thể trong cuộc trò chuyện (Depth 0 là đáy của prompt).
  * ⚙️ - như một tin nhắn vai trò system
  * 👤 - như một tin nhắn vai trò user
  * 🤖 - như một tin nhắn vai trò assistant
* **Outlet:** Entry World Info không được chèn tự động. Thay vào đó, nội dung của nó được lưu trữ dưới một outlet được đặt tên để bạn có thể quyết định chính xác nơi nó xuất hiện trong prompt bằng cách gọi nó với [macro `{{outlet::Name}}`](#outlet-name).

Các entry Example Message sẽ được định dạng theo cài đặt xây dựng prompt: Instruct Mode hoặc Chat Completion prompt manager. Chúng cũng tuân theo các quy tắc Example Messages Behavior: được đẩy dần ra khi context đầy, luôn giữ lại, hoặc bị vô hiệu hóa hoàn toàn.

Nếu Author's Note của bạn bị vô hiệu hóa (Insertion Frequency = 0), các entry World Info ở vị trí A/N sẽ bị bỏ qua!

#### Outlet Name

Khi vị trí chèn **Outlet** được chọn, một trường **Outlet Name** bổ sung sẽ có sẵn cho entry. Tên bạn cung cấp ở đây nhóm các entry lại với nhau và xác định token bạn sẽ sử dụng để kéo chúng vào prompt theo cách thủ công.

Sử dụng macro `{{outlet::YourName}}` trong [Prompt Manager](./Prompts/prompt-manager.md) hoặc [Advanced Formatting](./Prompts/advancedformatting.md) các trường prompt. Khi prompt được xây dựng, macro được thay thế bằng nội dung kết hợp của mọi entry World Info có cùng outlet name, phân tách bằng dòng mới, được sắp xếp theo giá trị [Insertion Order](#insertion-order) của chúng.

Nếu một outlet entry thiếu tên, nó sẽ bị bỏ qua trong quá trình tạo, vì vậy hãy đảm bảo điền vào trường. Outlet name hỗ trợ tự động hoàn thành dựa trên các tên bạn đã sử dụng để dễ dàng sử dụng lại các nhãn nhất quán.

##### Hạn chế và lưu ý

* Đặt outlet macro bên trong các entry World Info không được hỗ trợ và sẽ không hoạt động. Điều này xung đột với thứ tự đánh giá của World Info và có thể dẫn đến vòng lặp vô hạn.
* Lồng outlet không được hỗ trợ. Bạn không thể đặt outlet macro bên trong nội dung outlet khác. Giống như trên, điều này có thể dẫn đến vòng lặp vô hạn.
* Các trường character card (Description, Personality, Scenario, v.v.) không thể mở rộng outlet. Các trường đó được phân tích sớm để chúng có thể hoạt động như [nguồn khớp bổ sung](#additional-matching-sources) cho các trigger World Info, có nghĩa là outlet không khả dụng khi văn bản của chúng được xử lý. Sử dụng một trường nhận biết macro khác nếu bạn cần đặt nội dung outlet trong phần thân prompt thay thế.
* Trình soạn thảo Author's Note cũng không thể giải quyết outlet. Để đặt nội dung outlet xung quanh Author's Note, hãy gán các entry cho vị trí chèn **Top of AN** hoặc **Bottom of AN** thay vì dựa vào macro.
* Outlet name phân biệt chữ hoa chữ thường. Macro `{{outlet::}}` phải sử dụng chính xác cách viết hoa giống như **Outlet Name** của entry, nếu không sẽ không có nội dung nào được trả về.
* Khoảng trắng đầu hoặc cuối trong outlet name bị bỏ qua khi bạn gọi macro, vì vậy các tên được lưu với khoảng trắng thừa sẽ không khớp. Tránh thêm khoảng trắng vào tên để chúng có thể được giải quyết chính xác.
* Các outlet macro không có nội dung nào được gán sẽ được thay thế bằng chuỗi rỗng.

#### Entry Title / Memo

Một trường văn bản để thuận tiện cho bạn gắn nhãn các entry của bạn, không được sử dụng bởi AI hoặc bất kỳ logic trigger nào.

Nếu trống, có thể được điền lại bằng key đầu tiên của các entry bằng cách nhấp vào nút "Fill empty memos".

#### Strategy

1. 🔵 (Blue Circle) = Entry sẽ luôn có mặt trong prompt.
2. 🟢 (Green Circle) = Entry sẽ chỉ được kích hoạt khi có từ khóa.
3. 🔗 (Chain Link) = Entry được phép được chèn bằng độ tương đồng embedding.

Mỗi Entry cũng có một toggle cho phép bạn bật hoặc tắt entry.

#### Probability (Trigger %)

Giá trị này hoạt động như một bộ lọc bổ sung thêm cơ hội để entry KHÔNG được chèn khi nó được kích hoạt bằng bất kỳ phương tiện nào (constant, primary key, recursion).

1. Probability = 100 có nghĩa là entry sẽ được chèn vào mỗi lần kích hoạt.
2. Probability = 50 có nghĩa là entry sẽ được chèn với cơ hội 1:1.
3. Probability = 0 có nghĩa là entry sẽ KHÔNG được chèn (về cơ bản là vô hiệu hóa nó).

Sử dụng điều này để tạo các sự kiện ngẫu nhiên trong cuộc trò chuyện của bạn. Ví dụ, mỗi tin nhắn có thể có 1% cơ hội đánh thức Elder God nếu tên của nó được đề cập trong tin nhắn.

#### Inclusion Group

Inclusion group kiểm soát cách các entry được chọn khi nhiều entry có cùng nhãn group được kích hoạt đồng thời. Nếu nhiều entry có cùng nhãn group được kích hoạt, chỉ một sẽ được chèn vào prompt.

Theo mặc định, entry được chọn được chọn ngẫu nhiên dựa trên Group Weight của chúng (mặc định là 100 điểm) — số càng cao, xác suất lựa chọn càng cao. Điều này cho phép lựa chọn ngẫu nhiên giữa các entry đã kích hoạt, thêm yếu tố bất ngờ và đa dạng vào các tương tác.

Một entry duy nhất có thể là một phần của nhiều inclusion group nếu chúng được định nghĩa dưới dạng danh sách phân tách bằng dấu phẩy. Cùng logic như giải thích ở trên sẽ được áp dụng. Nếu entry đó được kích hoạt, nó sẽ *vô hiệu hóa* tất cả các entry khác là một phần của bất kỳ group nào của nó. Do đó, nếu bất kỳ group nào được kích hoạt, entry này sẽ không được kích hoạt.

#### Prioritize Inclusion

Để cung cấp nhiều quyền kiểm soát hơn đối với entry nào được kích hoạt qua [Inclusion Group](/Usage/worldinfo.md#inclusion-group), bạn có thể sử dụng cài đặt 'Prioritize Inclusion'. Tùy chọn này cho phép bạn chỉ định một cách xác định entry nào sẽ chọn thay vì ngẫu nhiên cuộn cơ hội Group Weight.

Nếu nhiều entry có cùng nhãn group và cài đặt này được bật được kích hoạt, entry có giá trị 'Order' cao nhất sẽ được chọn. Điều này hữu ích để tạo các chuỗi dự phòng qua inclusion group. Ví dụ để ưu tiên các entry độ sâu thấp với sự nhấn mạnh nhiều hơn, hoặc để chọn một hướng dẫn cụ thể về thiết lập cảnh hơn một hướng dẫn khác nếu cả hai đều hợp lệ.

#### Use Group Scoring

Khi cài đặt này được bật toàn cầu hoặc cho mỗi entry, số lượng key entry đã kích hoạt xác định người chiến thắng group. Chỉ tập hợp con của một group có số lượng key khớp cao nhất sẽ còn lại để được kích hoạt bởi Group Weight hoặc Inclusion Priority - phần còn lại sẽ bị vô hiệu hóa và loại khỏi group.

Sử dụng điều này để cung cấp tính cụ thể hơn cho các entry riêng lẻ trong các group lớn. Ví dụ, chúng có thể có một key chung và một key cụ thể. Một entry ngẫu nhiên sẽ được chèn khi không có key cụ thể nào được cung cấp, và ngược lại.

Logic tính điểm cho primary key là 1 match = 1 điểm.

Đối với secondary key, tương tác phụ thuộc vào Selective Logic đã chọn:

1. AND ANY: 1 secondary match = 1 điểm.
2. AND ALL: 1 điểm cho mọi secondary key nếu tất cả chúng đều khớp.
3. NOT ANY và NOT ALL: không thay đổi.

Ví dụ:

* Entry 1. Keys: song, sing, Black Cat. Group: songs
* Entry 2. Keys: song, sing, Ghosts. Group: songs

Đầu vào `sing me a song` có thể kích hoạt cả hai entry (cả hai đã kích hoạt 2 key), nhưng `sing me a song about Ghosts` sẽ chỉ kích hoạt Entry 2 (đã kích hoạt 3 key).

#### Automation ID

Cho phép tích hợp các entry World Info với [STscripts](/For_Contributors/st-script.md) từ extension Quick Replies. Nếu cả lệnh quick reply và entry WI đều có cùng Automation ID, lệnh sẽ được thực thi tự động khi entry có ID khớp được kích hoạt.

Automation được thực thi theo thứ tự chúng được kích hoạt, tuân thủ chiến lược sắp xếp đã chỉ định của bạn, kết hợp [Character Lore Insertion Strategy](#character-lore-insertion-strategy) với sắp xếp 'Priority'. Điều này dẫn đến các entry [Blue Circle](#strategy) được xử lý trước, theo sau là các entry khác theo 'Order' đã chỉ định của chúng. Các entry được kích hoạt đệ quy sẽ được xử lý sau theo cùng thứ tự.

Lệnh script sẽ chỉ chạy một lần nếu nhiều entry có cùng Automation ID được kích hoạt.

#### Character Filter

Danh sách tên nhân vật mà entry này có thể được kích hoạt. Nếu danh sách này không trống, entry sẽ chỉ được kích hoạt cho các nhân vật có tên trong danh sách. Khi một tag được chọn, entry sẽ chỉ được kích hoạt cho các nhân vật có tag cụ thể đó.

Chế độ "Exclude" đảo ngược bộ lọc, có nghĩa là entry sẽ được kích hoạt cho tất cả các nhân vật ngoại trừ những nhân vật được thêm vào danh sách hoặc có (các) tag đã chọn.

#### Triggers

Các loại tạo mà entry World Info này có thể được kích hoạt. Nếu không có gì được chọn, entry có thể được kích hoạt cho tất cả các loại tạo. Nếu một hoặc nhiều được chọn, entry sẽ chỉ được kích hoạt cho các loại tạo cụ thể đó:

* **Normal:** Yêu cầu tạo tin nhắn thông thường.
* **Continue:** Khi nút Continue được nhấn.
* **Impersonate:** Khi nút Impersonate được nhấn.
* **Swipe:** Khi việc tạo được kích hoạt bằng cách vuốt.
* **Regenerate:** Khi nút Regenerate được nhấn trong cuộc trò chuyện đơn.
* **Quiet:** Yêu cầu tạo nền, thường được kích hoạt bởi [extensions](/extensions/index.md) hoặc lệnh [STscript](/For_Contributors/st-script.md).

!!!
Trigger "Regenerate" không khả dụng trong cuộc trò chuyện nhóm vì nó sử dụng logic tạo lại khác: tất cả các tin nhắn từ phản hồi cuối cùng bị xóa và các tin nhắn được xếp hàng bằng loại tạo "Normal" theo [Chiến lược phản hồi nhóm](/Usage/Characters/groupchats.md#reply-order-strategies) đã chọn.
!!!

#### Additional matching sources

Theo mặc định, các entry World Info chỉ được khớp với nội dung từ cuộc trò chuyện hiện tại. Các tùy chọn này cho phép bạn khớp entry với thông tin nhân vật khác nhau không xuất hiện trong cuộc trò chuyện, hoặc thậm chí thông tin persona. Điều này hữu ích khi bạn muốn có nhiều entry được sử dụng giữa một số nhân vật nhưng không muốn phải quản lý danh sách tag lớn, hoặc không muốn phải cập nhật danh sách bộ lọc nhân vật mỗi khi bạn tạo một nhân vật mới. Điều này cũng cho phép bạn khớp các entry dựa trên persona bạn đang kích hoạt.

* **Character Description**: Khớp với mô tả nhân vật.
* **Character Personality**: Khớp với tóm tắt tính cách nhân vật, được tìm thấy trong Advanced Definitions.
* **Scenario**: Khớp với kịch bản nhân vật được chỉ định, được tìm thấy trong Advanced Definitions.
* **Persona Description**: Khớp với mô tả của persona hiện được chọn.
* **Character's Note**: Khớp với ghi chú của nhân vật, có thể được tìm thấy trong Advanced Definitions.
* **Creator's Notes**: Khớp với ghi chú của người tạo nhân vật, có thể được tìm thấy trong Advanced Definitions. Ghi chú của người tạo thường không được bao gồm trong prompt.

## Vector Storage Matching

Extension Vector Storage cung cấp một giải pháp thay thế cho khớp từ khóa bằng cách sử dụng độ tương đồng giữa các tin nhắn trò chuyện gần đây và nội dung entry World Info.

Để bật và sử dụng điều này, các điều kiện tiên quyết sau cần được đáp ứng:

1. Extension Vector Storage được bật và được cấu hình để sử dụng một trong các nguồn embedding có sẵn.
2. Hộp kiểm "Enable for World Info" được đánh dấu trong cài đặt extension Vector Storage.
3. Hoặc các entry World Info được phép khớp keyless có trạng thái "Vectorized" (🔗) hoặc tùy chọn "Enabled for all entries" được chọn trong cài đặt Vector Storage.

Lựa chọn mô hình vectorization trong extension và ý nghĩa lý thuyết đằng sau thuật ngữ "embeddings" sẽ không được đề cập ở đây. Kiểm tra hướng dẫn [Data Bank](/Usage/Characters/data-bank.md#vector-storage) nếu bạn cần thêm thông tin về chủ đề này.

Vector Storage matching tuân theo tập hợp quy tắc này:

* Số lượng entry tối đa được phép khớp với Vector Storage có thể được điều chỉnh với cài đặt "Max Entries". Số này chỉ đặt giới hạn và không ảnh hưởng đến ngân sách token được đặt trong cài đặt kích hoạt cho World Info. Tất cả các quy tắc ngân sách vẫn được áp dụng.
* Tính năng này chỉ thay thế kiểm tra từ khóa. Tất cả các kiểm tra bổ sung phải được đáp ứng để entry được chèn: trigger%, character filters, inclusion groups, v.v.
* Cài đặt "Scan Depth" từ Activation Settings hoặc ghi đè entry không được sử dụng. Giá trị "Query messages" của Vector Storage được sử dụng thay thế để lấy văn bản để khớp. Điều này cho phép cấu hình như "Scan Depth" đặt thành 0, vì vậy không có khớp từ khóa thông thường nào được thực hiện, nhưng các entry vẫn có thể được kích hoạt bởi vector.
* Trạng thái "Vectorized" chỉ là một dấu hiệu bổ sung. Entry vẫn sẽ hoạt động như một bản ghi bình thường, được bật, không phải constant sẽ được kích hoạt bởi từ khóa nếu chúng được đặt. Xóa từ khóa nếu bạn muốn chúng chỉ được kích hoạt bởi vector.

!!!info Lưu ý
Vì chất lượng truy xuất phụ thuộc hoàn toàn vào đầu ra của mô hình embedding, không thể dự đoán chính xác các entry nào sẽ được chèn. Nếu bạn muốn kết quả xác định và có thể dự đoán được, hãy sử dụng khớp từ khóa.
!!!

## Timed Effects

Thông thường, đánh giá World Info là stateless, có nghĩa là kết quả của đánh giá là như nhau, chỉ phụ thuộc vào ngữ cảnh trò chuyện hiện tại. Tuy nhiên, với sự giới thiệu của Timed Effects, bạn có thể tạo các entry có độ trễ kích hoạt, vẫn hoạt động sau khi được kích hoạt, hoặc không thể được kích hoạt sau khi kích hoạt.

### Quy tắc Timed Effects

1. Khung thời gian cho các hiệu ứng được đo bằng tin nhắn (không phải cặp tin nhắn/trao đổi), với 0 có nghĩa là không có hiệu ứng.
2. Hiệu ứng chỉ áp dụng trong cuộc trò chuyện mà entry được kích hoạt. Các nhánh kế thừa trạng thái của cuộc trò chuyện cha.
3. Các timed effect đang hoạt động bị xóa nếu cuộc trò chuyện không tiến triển, ví dụ: nếu tin nhắn cuối cùng bị vuốt hoặc bị xóa.
4. Thực hiện bất kỳ thay đổi nào đối với entry hiện đang có timed effect sẽ khiến hiệu ứng bị xóa bắt buộc.
5. Kích hoạt liên tiếp các từ khóa không làm mới thời lượng hiệu ứng nếu nó đã hoạt động.

### Các loại Timed Effects

1. Sticky - entry vẫn hoạt động trong N tin nhắn sau khi được kích hoạt. Các entry Stickied bỏ qua kiểm tra xác suất trên các lần quét tiếp theo cho đến khi chúng hết hạn.
2. Cooldown - entry không thể được kích hoạt trong N tin nhắn sau khi được kích hoạt. Có thể được sử dụng cùng với sticky: entry đi vào cooldown khi thời lượng sticky kết thúc.
3. Delay - entry không thể được kích hoạt trừ khi có ít nhất N tin nhắn trong cuộc trò chuyện tại thời điểm đánh giá.
    * Delay = 0 -> Entry có thể được kích hoạt bất cứ lúc nào.
    * Delay = 1 -> Entry không thể được kích hoạt nếu cuộc trò chuyện trống (không có lời chào).
    * Delay = 2 -> Entry không thể được kích hoạt nếu có không hoặc chỉ một tin nhắn trong cuộc trò chuyện, v.v.

### Ví dụ về Timed Effects

Cấu hình entry: sticky = 3, cooldown = 2, delay = 2.

```txt
Message 0: delay
Message 1: entry activated
Message 2: sticky
Message 3: sticky
Message 4: sticky
Message 5: cooldown
Message 6: cooldown
Message 7: entry can be activated again
```

## Activation Settings

Menu có thể thu gọn ở đầu màn hình World Info.

### Scan Depth

> Có thể được ghi đè ở cấp độ entry.

Xác định có bao nhiêu tin nhắn trong lịch sử trò chuyện nên được quét cho các key World Info.

* Nếu đặt thành 0, thì chỉ các entry đệ quy và Author's Note được đánh giá.
* Nếu đặt thành 1, thì SillyTavern chỉ quét tin nhắn cuối cùng.
* 2 = hai tin nhắn cuối cùng, v.v.

### Include Names

Xác định xem tên của những người tham gia trò chuyện có nên được bao gồm trong buffer văn bản được quét dưới dạng tiền tố tin nhắn hay không. Điều này cho phép kích hoạt các entry sử dụng tên làm từ khóa mà không cần đề cập trực tiếp tên trong tin nhắn.

Xem ví dụ về văn bản được quét bên dưới, giả sử những người tham gia trò chuyện có tên là Alice và Bob.

Enabled (mặc định):

```txt
Alice: Hello! Good to see you.
Bob: How is the weather today?
```

Disabled:

```txt
Hello! Good to see you.
How is the weather today?
```

### Context % / Budget

**Xác định có bao nhiêu token có thể được sử dụng bởi các entry World Info cùng một lúc.**
Bạn có thể xác định một ngưỡng tương đối so với cài đặt max-context của API (Context %) hoặc một ngưỡng token khách quan (Budget)

Nếu ngân sách cạn kiệt, thì không có entry nào được kích hoạt thêm ngay cả khi các key có trong prompt.

Các entry constant sẽ được chèn trước. Sau đó là các entry có số order lớn hơn.

Các entry được chèn bằng cách đề cập trực tiếp key của chúng có mức độ ưu tiên cao hơn những entry được đề cập trong nội dung của các entry khác.

### Min Activations

**Cài đặt này loại trừ lẫn nhau với Max Recursion Steps.**

Minimum Activations: Nếu đặt thành giá trị khác không, điều này sẽ bỏ qua giới hạn của "scan-depth", tìm kiếm tất cả nhật ký trò chuyện ngược từ tin nhắn mới nhất cho các từ khóa cho đến khi có nhiều entry như được chỉ định trong min activations đã được kích hoạt. Điều này vẫn sẽ bị giới hạn bởi cài đặt Max Depth hoặc giới hạn Budget tổng thể của bạn.

*Các lượt quét bổ sung được kích hoạt bởi Min Activations sẽ không kiểm tra các entry được thêm bởi đệ quy trong các bước trước đó. Chỉ tin nhắn trò chuyện và extension prompt có thể kích hoạt các kích hoạt bổ sung này. Tuy nhiên, các entry được kích hoạt bởi Min Activations có thể kích hoạt các entry khác như bình thường.*

### Max Depth

Độ sâu tối đa để quét khi sử dụng cài đặt Min Activations.

### Recursive scanning

Quét đệ quy cho phép các entry kích hoạt các entry khác hoặc được kích hoạt bởi các entry khác, cho phép các tương tác và phụ thuộc phức tạp giữa các entry World Info khác nhau. Tính năng này có thể nâng cao đáng kể bản chất động của các kịch bản sáng tạo của bạn.
Việc quét đệ quy có được bật hay không có thể được kiểm soát bằng cài đặt toàn cầu **Recursive Scan**.
Có ba tùy chọn có sẵn để kiểm soát đệ quy cho mỗi entry:

* **Non-recursable**: Khi hộp kiểm này được chọn, entry sẽ không được kích hoạt bởi các entry khác. Điều này hữu ích cho thông tin tĩnh không nên thay đổi hoặc bị ảnh hưởng bởi các entry world info khác.

* **Prevent further recursion**: Chọn tùy chọn này đảm bảo rằng một khi entry này được kích hoạt, nó sẽ không kích hoạt bất kỳ entry nào khác. Điều này hữu ích để tránh các chuỗi kích hoạt không mong muốn.

* **Delay until recursion**: Entry này sẽ chỉ được kích hoạt trong các kiểm tra đệ quy, có nghĩa là nó sẽ không được kích hoạt trong lần chuyển ban đầu nhưng có thể được kích hoạt bởi các entry khác có đệ quy được bật. Bây giờ, với **Recursion Level** được thêm vào cho những delay đó, các entry được nhóm theo cấp độ. Ban đầu, chỉ cấp độ đầu tiên (số nhỏ nhất) sẽ khớp. Một khi không tìm thấy khớp nào, cấp độ tiếp theo trở nên đủ điều kiện để khớp, lặp lại quá trình cho đến khi tất cả các cấp độ được kiểm tra. Điều này cho phép kiểm soát nhiều hơn về cách thức và thời điểm các lớp thông tin sâu hơn được tiết lộ trong đệ quy, đặc biệt khi kết hợp với các tiêu chí như NOT ANY hoặc NOT ALL kết hợp các key match.

**Các entry có thể kích hoạt các entry khác bằng cách đề cập đến từ khóa của chúng trong văn bản nội dung.**

Ví dụ, nếu World Info của bạn chứa hai entry:

```txt
Entry #1
Keyword: Bessie
Content: Bessie is a cow and is friends with Rufus.
```

```txt
Entry #2
Keyword: Rufus
Content: Rufus is a dog.
```

**CẢ HAI** sẽ được kéo vào context nếu văn bản tin nhắn chỉ đề cập **Bessie**.

### Max Recursion Steps

**Cài đặt này loại trừ lẫn nhau với Min Activations.**

Khi đặt thành không, lồng đệ quy chỉ bị giới hạn bởi ngân sách prompt của bạn. Khi đặt thành giá trị khác không, giới hạn tổng số lần quét đến "cấp độ lồng" tối đa mong muốn.

Ví dụ về giá trị:

* 1 hiệu quả vô hiệu hóa đệ quy vì kiểm tra dừng sau bước đầu tiên.
* 2 chỉ có thể kích hoạt các entry đệ quy một lần.
* 3 có thể kích hoạt đệ quy hai lần...

### Case-sensitive keys

> Có thể được ghi đè ở cấp độ entry.

**Để được kéo vào context, các key entry cần khớp với chữ hoa chữ thường như chúng được xác định trong entry World Info.**

Điều này hữu ích khi các key của bạn là các từ phổ biến hoặc các phần của các từ phổ biến.

Ví dụ, khi cài đặt này hoạt động, các key 'rose' và 'Rose' sẽ được xử lý khác nhau, tùy thuộc vào đầu vào.

### Match whole words

> Có thể được ghi đè ở cấp độ entry.

Các entry có key chỉ chứa một từ sẽ chỉ được khớp nếu toàn bộ từ có trong văn bản tìm kiếm. Được bật theo mặc định.

Ví dụ, nếu cài đặt được bật và key entry là "king", thì văn bản như "long live the king" sẽ được khớp, nhưng "it's not to my liking" sẽ không.

**Quan trọng:** cài đặt này có thể có tác động bất lợi khi được sử dụng với các ngôn ngữ không sử dụng khoảng trắng để phân tách các từ (ví dụ: tiếng Nhật hoặc tiếng Trung). Nếu bạn viết các entry bằng các ngôn ngữ này, nên giữ nó tắt.

### Alert on overflow

Hiển thị cảnh báo nếu World Info đã kích hoạt vượt quá ngân sách token được phân bổ.
