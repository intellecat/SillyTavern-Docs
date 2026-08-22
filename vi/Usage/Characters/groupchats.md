---
order: 70
route: /vi/usage/core-concepts/groupchats/
---

# Group Chats

## Chiến lược thứ tự trả lời

Quyết định cách các nhân vật trong group chats được chọn để trả lời.

### Manual

Bạn có thể chọn nhân vật để trả lời theo cách thủ công từ menu hoặc bằng lệnh `/trigger`. Thành viên group được chọn sẽ là người duy nhất trả lời. Các tin nhắn của người dùng sẽ không tự động kích hoạt bất kỳ phản hồi nào. Kích hoạt generation với input người dùng trống sẽ kích hoạt một thành viên group chưa bị tắt tiếng ngẫu nhiên để trả lời.

### Natural Order

Cố gắng mô phỏng luồng của một cuộc trò chuyện thực của con người. Thuật toán như sau:

1. Các đề cập đến tên thành viên group được trích xuất từ tin nhắn cuối cùng trong chat.

    Chỉ các từ nguyên vẹn được nhận dạng là đề cập! Nếu tên nhân vật của bạn là "Misaka Mikoto", họ sẽ chỉ kích hoạt trên "Misaka" hoặc "Mikoto", nhưng không bao giờ với "Misa", "Railgun", v.v.

    Trừ khi cài đặt "Allow Self Responses" được bật, các nhân vật sẽ không trả lời các đề cập đến tên của họ trong tin nhắn của chính họ!

2. Các nhân vật được kích hoạt bởi yếu tố "Talkativeness".

    Talkativeness xác định tần suất nhân vật nói nếu họ không được đề cập. Điều chỉnh giá trị này trên màn hình "Advanced Definitions" trong trình chỉnh sửa nhân vật. Giá trị thanh trượt trên thang tuyến tính từ **0% / Shy** (nhân vật không bao giờ nói trừ khi được đề cập) đến **100% / Chatty** (nhân vật luôn trả lời). Giá trị mặc định cho các nhân vật mới là 50% cơ hội.

3. Một nhân vật ngẫu nhiên được chọn.

    Nếu không có nhân vật nào được kích hoạt ở các bước trước, một người nói được chọn ngẫu nhiên, bỏ qua tất cả các điều kiện khác.

### List Order

Các nhân vật được chọn dựa trên thứ tự họ được trình bày trong danh sách thành viên group. Không có quy tắc nào khác được áp dụng.

### Pooled Order

Kích hoạt một nhân vật ngẫu nhiên chưa nói kể từ tin nhắn người dùng cuối cùng. Nếu tất cả các nhân vật đã nói, chọn một người ngẫu nhiên cho đến tin nhắn người dùng tiếp theo.

## Chế độ xử lý generation của Group chat

Cài đặt này quyết định cách xử lý thông tin nhân vật của các thành viên group chat. Bất kể lựa chọn nào, lịch sử group chat luôn được chia sẻ giữa tất cả các thành viên.

### Swap character cards

Chế độ mặc định. Mỗi khi tin nhắn được tạo, chỉ thông tin character card của người nói đang hoạt động được đưa vào context.

### Join character cards

Thông tin của tất cả các thành viên group được kết hợp thành một prompt chung theo thứ tự danh sách của họ. Điều này có thể giúp trong các trường hợp khi thay đổi các khối lớn của context là không mong muốn, ví dụ: với llama.cpp prompt caching.

Chế độ này có hai chế độ phụ (bạn phải chọn một):

* Include muted - các nhân vật bị tắt tiếng sẽ luôn được đưa vào joint prompt.
* Exclude muted - các nhân vật bị tắt tiếng sẽ không được đưa vào nếu họ không phải là người nói hiện tại.

Các trường sau đang được kết hợp:

1. Description
2. Scenario, nếu không được ghi đè cho chat
3. Personality
4. Message examples
5. Character notes / Depth prompts

**Quan trọng!** Xin lưu ý rằng do cách character card điển hình được cấu trúc, việc sử dụng chế độ này có thể dẫn đến hành vi bất ngờ, bao gồm nhưng không giới hạn ở: các nhân vật bị nhầm lẫn về bản thân, có tính cách hợp nhất, đặc điểm không chắc chắn, v.v.

### Join Prefix and Suffix

Khi 'Join character cards' được chọn, tất cả các trường tương ứng của các nhân vật đang được kết hợp lại với nhau. Điều này có nghĩa là trong prompt kết quả, tất cả các mô tả nhân vật sẽ được kết hợp thành một khối văn bản lớn. Nếu bạn muốn các trường đó được phân tách, bạn có thể xác định prefix và/hoặc suffix.

Các tùy chọn này hỗ trợ macros bình thường và cũng sẽ thay thế \{\{char\}\} bằng tên của nhân vật liên quan và \<FIELDNAME\> bằng tên của phần (ví dụ: description, personality, scenario, v.v.)

## Các tùy chọn menu Group Chat khác

### Mute Character

Biểu tượng bong bóng chat bị gạch ngang bên cạnh avatar nhân vật trong menu group chat có thể tắt hoặc bật phản hồi từ một nhân vật cụ thể trong chat.

### Force Talk

Biểu tượng bong bóng chat bên cạnh avatar nhân vật trong menu group chat sẽ kích hoạt phản hồi chỉ từ một nhân vật cụ thể, bỏ qua chiến lược thứ tự trả lời. Nó sẽ hoạt động ngay cả khi thành viên group bị tắt tiếng.

### Auto-mode

Trong khi auto-mode được bật, group chat sẽ tuân theo thứ tự trả lời và kích hoạt generation tin nhắn mà không cần tương tác của người dùng. Lượt auto-mode tiếp theo được kích hoạt sau khi chờ 5 giây khi nhân vật được chọn cuối cùng gửi tin nhắn của mình. Khi người dùng bắt đầu gõ vào vùng văn bản gửi tin nhắn, auto-mode sẽ bị tắt, nhưng các generations đã xếp hàng không tự động dừng lại.

### Allow Self Responses

Sẽ cho phép các phản hồi liên tiếp từ nhân vật đã gửi tin nhắn mới nhất của mỗi lượt nếu họ tình cờ được kích hoạt do tự đề cập đến bản thân khi Natural Order được chọn. Không có hiệu lực trên List order.

### Group Chat Scenario Override

Tất cả các thành viên group sẽ sử dụng văn bản scenario đã nhập thay vì những gì được chỉ định trong character cards của họ. Các chat phân nhánh kế thừa scenario override từ parent của chúng và có thể được thay đổi riêng lẻ sau đó.

### Peek Character Definitions

Nhấp vào biểu tượng character card bên cạnh avatar trong menu group chat sẽ nhanh chóng điều hướng đến màn hình định nghĩa nhân vật thông thường. Bất kỳ thay đổi nào được thực hiện ở đây sẽ được lưu vào card chính nó.

Để quay lại group chat, nhấp vào liên kết tiêu đề Group Name.

### Quản lý thành viên

Bất kỳ nhân vật hiện có nào của bạn đều có thể được thêm, xóa, tắt tiếng hoặc sắp xếp lại trong group chat. Theo mặc định, một thành viên mới được thêm vào đầu danh sách thành viên group và sau đó có thể được sắp xếp lại bằng các biểu tượng mũi tên.

### Group Chat pop-out

Group chat menu pop-out có thể được kích hoạt bằng cách nhấp vào biểu tượng bên cạnh trường "Current Members". Điều này tạo ra một pop-out của menu group chat. Bằng cách bật MovingUI từ user settings, menu này có thể được thay đổi kích thước và kéo đến bất kỳ vị trí nào trong giao diện và hoạt động giống như menu group chat thông thường.
