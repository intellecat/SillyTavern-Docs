---
order: 60
route: /vi/usage/core-concepts/tags/
---

# Tags

Các character cards và groups có thể được gán không hoặc nhiều tags. Chúng hữu ích để tổ chức các bộ sưu tập nhanh chóng phát triển theo chủ đề, chất lượng, nguồn gốc hoặc bất cứ điều gì bạn thích.

## Gắn tag

Có một số cách để thêm hoặc xóa tags vào character card:

- Nhập các tags nhúng trong quá trình nhập.
- Mở một card từ bảng Character Management. Từ đó bạn sẽ có thể gán tags cho character card.
- Gắn tag hàng loạt.

Để thực hiện gắn tag hàng loạt, hãy nhấp vào nút "Bulk edit characters" (biểu tượng bút chì), chọn các cards bạn muốn gắn tag, nhấp chuột phải vào bất kỳ card nào trong số chúng, sau đó nhấp vào "Tag" trong menu ngữ cảnh.

!!!info Lưu ý
Xin lưu ý rằng các groups không thể được gắn tag hàng loạt.
!!!

Từ màn hình này, bạn sẽ có thể:

- Thêm hoặc xóa tags bằng combo box.
- Xóa tất cả tags khỏi các cards đã chọn ("All").
- Xóa giao của tags giữa tất cả các cards đã chọn khỏi các cards đó ("Mutual").
- Nhập (tạo cục bộ) tất cả tags được lưu trữ trong character card, trong trường hợp bạn đã nhập nó ("Import All").
- Nhập (tạo cục bộ) các tags được lưu trữ trong character card cũng tồn tại cục bộ với tên khớp ("Import Existing").

## Quản lý

Để xem và quản lý tất cả các tags hiện có, hãy mở bảng Character Management sau đó nhấp vào nút "Manage tags" (biểu tượng bánh răng).

Bạn có thể sao lưu và khôi phục tất cả thông tin ở đây (danh sách tag, gán tag cho cards, màu sắc, cài đặt folder, v.v.) bằng các nút ở góc trên bên phải.

Bạn có thể sử dụng các nút grip ở bên trái để sắp xếp lại các tags khi chúng xuất hiện trong bộ lọc tag trong Character Management.

!!!warning Cảnh báo
Tệp JSON sao lưu tags không dành cho việc chia sẻ với người khác vì nó chứa thông tin cụ thể cho instance của bạn, chẳng hạn như tên entity nội bộ!
!!!

## Nhập tags khi nhập character cards

Khi nhập các character cards bên ngoài từ các hình ảnh đã tải xuống (hoặc từ nút "Import content from external URL"), bạn sẽ được nhắc tùy chọn nhập các tags mà nó chứa. Chúng không bắt buộc để card hoạt động; tags chỉ đơn giản là tổ chức.

Các tags nhúng trong card được lưu trữ trong phần "Creator's Metadata" của menu "Advanced Definitions" của trình chỉnh sửa nhân vật. Nếu bạn muốn đề xuất một số tags cho người dùng khác sẽ nhập nhân vật đó, hãy điền vào trường "Tags to Embed" bằng danh sách các tags được phân tách bằng dấu phẩy.

!!!info Lưu ý
Popup này sẽ chỉ xuất hiện nếu tùy chọn User Settings "Import Card Tags" được đặt thành "Ask".
!!!

Trong popup "Import tags for CHARACTER NAME" mở ra, bạn sẽ thấy danh sách các tags Existing (mà bạn đã có cục bộ với tên khớp) và tags New (mà bạn không có cục bộ).

Bạn có thể:

- Cắt bớt danh sách theo nhu cầu và sau đó nhấn "Import" - các tags Existing còn lại sẽ được thêm vào character card đã nhập, và các tags New còn lại sẽ được tạo cục bộ và sau đó được thêm vào card.
- Hoặc chỉ cần nhấn "Import none" để bỏ qua các tags có trong character card và chỉ nhập card.
- Hoặc "Import All" như một phím tắt để nhập tất cả các tags được tìm thấy trong character card (LƯU Ý: bao gồm bất kỳ tags nào bạn đã cắt từ danh sách ở trên; sử dụng nút "Import" nếu bạn đã làm).
- Hoặc "Import Existing" như một phím tắt để chỉ nhập các tags đã tồn tại cục bộ với tên khớp.

## Lọc character cards

Sau khi bạn tạo tags, bạn sẽ thấy chúng trên một hàng trong bảng Character Management. Bạn có thể nhấp vào chúng để chuyển đổi trạng thái lọc tag; theo thứ tự:

- Một lần nhấp sẽ hiển thị các cards được gắn tag với tag này.
- Nhấp lần nữa để chỉ hiển thị các cards KHÔNG được gắn tag với tag này.
- Nhấp lần nữa để đặt lại lọc theo tag này.

Bạn có thể lọc theo bất kỳ số lượng tags nào cùng một lúc.

## Tags as Folders

!!!info Lưu ý
Để sử dụng chức năng này, nó phải được bật trước trong User Settings, trong cột UI Theme. Trạng thái của chuyển đổi này cũng lưu với UI theme.
!!!

Từ nút "Manage tags" (biểu tượng bánh răng), mỗi mục tag có một nút chuyển đổi đa trạng thái để chuyển đổi giữa các chế độ tags-as-folder này (được gọi là "bogus folder" trong code):

- một lần nhấp để biến tag này thành "open folder". Nó sẽ xuất hiện như một mục ảo trong danh sách card; nhấp vào nó sẽ chỉ hiển thị các cards có tag đó
- nhấp lần nữa để biến tag này thành "closed folder". Như trên, nhưng các cards được gắn tag với tag này sẽ không xuất hiện theo mặc định - bạn sẽ cần nhấp vào folder để xem chúng.
- nhấp lần nữa để đặt lại trạng thái tag-as-folder cho tag này.
