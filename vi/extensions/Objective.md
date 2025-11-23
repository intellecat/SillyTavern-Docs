---
route: /extensions/objective/
---

# Objective

### Đó là gì?

Tiện ích mở rộng Objective cho phép người dùng chỉ định một Mục tiêu để AI phấn đấu trong cuộc trò chuyện. Mục tiêu này được chia nhỏ thành các nhiệm vụ từng bước. Nhiệm vụ có thể được phân nhánh, nơi các nhiệm vụ con có thể được tạo tự động hoặc thủ công. Điều này cung cấp khả năng tạo cây nhiệm vụ phức tạp. Trạng thái hoàn thành của mỗi nhiệm vụ trong danh sách sẽ được kiểm tra theo khoảng thời gian nhất định.

Điều này khác với việc thêm hướng dẫn tĩnh thông qua prompting ở chỗ nó thêm các chỉ thị tuần tự và theo nhịp độ để AI tuân theo mà không cần sự can thiệp của người dùng. Nó mang lại trải nghiệm chân thực hơn về việc AI tự động phấn đấu đạt được mục tiêu.

### Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các điều kiện tiên quyết sau:

- Đảm bảo bạn đang sử dụng phiên bản mới nhất của SillyTavern.
- Cài đặt tiện ích mở rộng "Objective" từ menu "Download Extensions & Assets" trong bảng Extensions (biểu tượng khối xếp chồng).

### Trường hợp sử dụng phổ biến

Trí tưởng tượng của bạn là giới hạn, bạn có thể đưa ra bất kỳ mục tiêu nào cho AI mà bạn muốn và nó sẽ lên kế hoạch về cách đạt được nó. Bạn có thể yêu cầu nó lên kế hoạch cách tiêu diệt một con quỷ, cướp một ngôi đền, tổ chức một bữa tiệc xa hoa, hoặc thậm chí chiếm lấy thế giới.

![Objective Settings Panel](/static/extensions/objective-panel.png)

### Cấu hình

- Tiện ích mở rộng được tìm thấy trong menu Extensions dưới Objective.

- Nhập một mục tiêu vào hộp văn bản trên cùng, sau đó nhấp vào `Auto-Generate Tasks`. Điều này gửi một yêu cầu đến API được kết nối và yêu cầu nó cung cấp danh sách các nhiệm vụ phù hợp với mục tiêu bạn đã nhập.

*Lưu ý: Nhấp Auto-Generate Tasks sẽ xóa tất cả các nhiệm vụ hiện có cho Mục tiêu hiện được chọn trước khi thêm nhiệm vụ mới.*

- Khi nhận được phản hồi từ AI, một danh sách các nhiệm vụ sẽ được tạo tự động trong khoảng trống bên dưới hộp nhập Mục tiêu. Nhiệm vụ có thể được chỉnh sửa sau khi tạo.

- Ở dưới cùng của bảng là hai hộp: `Position in Chat` và `Task Check Frequency`
  - `Position in Chat` - Đây là 'độ sâu' trong phần trò chuyện của prompt mà bạn muốn nhiệm vụ hiện tại được chèn vào. Số càng thấp, AI càng chú ý đến nhiệm vụ. Đặt thành 0 sẽ làm cho nhiệm vụ trở thành điều chính trong tâm trí AI. Đặt ở giá trị cao sẽ đặt nhiệm vụ ở nền và cho phép AI tập trung vào cuộc trò chuyện trong tay, nhưng đặt quá cao có thể khiến AI không bao giờ 'hoàn thành' nhiệm vụ.
  - `Task Check Frequency` - Đây là tần suất bạn muốn AI kiểm tra xem nhiệm vụ đã hoàn thành chưa. Nếu đặt thành `3`, AI sẽ được hỏi xem nhiệm vụ hiện tại đã hoàn thành chưa mỗi tin nhắn thứ 3.

-  Mục tiêu, nhiệm vụ và mô tả của chúng được lưu theo thời gian thực vào phiên trò chuyện hiện tại. Prompt tùy chỉnh được lưu toàn cục.

### Prompt tùy chỉnh
Bạn có thể tùy chỉnh các prompt được gửi đến LLM để tạo nhiệm vụ, kiểm tra hoàn thành nhiệm vụ và cho việc chèn prompt. Chỉnh sửa prompt sẽ lưu chúng cho phiên hiện tại. Prompt tùy chỉnh có thể được lưu và tải để duy trì.

- Nhấp Edit Prompts để mở cửa sổ chỉnh sửa prompt. Bạn có thể chỉnh sửa prompt của mình như mong muốn.
- Để lưu prompt, nhập tên và nhấp Save Prompt.
- Để tải prompt, chọn prompt từ danh sách thả xuống.
- Để xóa prompt đã lưu, chọn nó từ danh sách thả xuống và nhấp Delete Prompt

**CẢNH BÁO: Kiểm tra nhiệm vụ xảy ra trong một yêu cầu API riêng biệt. Đặt Task Check Frequency thành 1 sẽ nhân đôi số lần gọi API của bạn đến dịch vụ LLM. Hãy cẩn thận với điều này nếu bạn đang sử dụng dịch vụ trả phí.**

### Sử dụng

Theo mặc định, tiện ích mở rộng Objective sẽ theo dõi tất cả các nhiệm vụ và trạng thái hoàn thành tương ứng của chúng tự động.

Người dùng cũng có thể tạo, cập nhật, xóa và hoàn thành nhiệm vụ thủ công bất cứ lúc nào.


#### Lựa chọn nhiệm vụ hiện tại

Nhiệm vụ hiện tại sẽ luôn là nhiệm vụ chưa hoàn thành đầu tiên được liệt kê. Bất kỳ cập nhật thủ công nào đối với nhiệm vụ sẽ kích hoạt kiểm tra xem nhiệm vụ hiện tại nên là gì. Vì vậy, nếu bạn thêm một nhiệm vụ phía trên một loạt các nhiệm vụ đã hoàn thành, nó sẽ được đặt làm nhiệm vụ hiện tại. Sau khi hoàn thành, các nhiệm vụ đã hoàn thành trước đó sẽ bị bỏ qua và nhiệm vụ chưa hoàn thành tiếp theo sẽ được chọn làm 'Hiện tại'.

Khi sử dụng nhiệm vụ cha/con trong cây nhiệm vụ, các nhiệm vụ được chọn theo chiều sâu trước, nghĩa là tất cả các nhiệm vụ con sẽ được chọn theo thứ tự trước, sau đó tiếp tục xuống danh sách các nhiệm vụ cho Mục tiêu/Nhiệm vụ hiện tại.

#### Nhiệm vụ nhánh

Nhấp nút Branch Task để đặt nhiệm vụ hiện tại làm Mục tiêu nơi bạn có thể tự động tạo hoặc tạo thủ công các nhiệm vụ làm nhiệm vụ con. Bạn có thể tiếp tục biến bất kỳ nhiệm vụ con nào thành Mục tiêu và tiếp tục tạo theo ý thích của bạn.

Đánh dấu nhiệm vụ cha là hoàn thành sẽ khiến tiện ích mở rộng bỏ qua tất cả các nhiệm vụ con. Khi tất cả các nhiệm vụ con hoàn thành, nhiệm vụ cha sẽ được đánh dấu là hoàn thành

#### Hoàn thành nhiệm vụ thủ công

Bạn có thể chuyển đổi trạng thái hoàn thành của nhiệm vụ thủ công bằng cách `nhấp vào hộp kiểm` bên cạnh nó. Điều này sẽ đặt nhiệm vụ chưa hoàn thành tiếp theo được chọn.

#### Kiểm tra nhiệm vụ thủ công

Nếu bạn muốn kích hoạt thủ công AI để kiểm tra hoàn thành nhiệm vụ, hãy nhấp vào nút Extras Extension (biểu tượng `đũa phép` ở bên phải thanh nhập trò chuyện) và chọn `Manual Task Check`.

![Manual Task Check](/static/extensions/task-check.png)

#### Thêm nhiệm vụ thủ công

Khi không có nhiệm vụ nào, nút `Add Task` hiển thị, cho phép bạn tạo thủ công nhiệm vụ đầu tiên.

Nếu các nhiệm vụ khác đã có, hãy nhấp nút `+` ở bên phải của bất kỳ nhiệm vụ nào để chèn một nhiệm vụ mới sau nó.

#### Xóa nhiệm vụ

Nhấp `x` màu đỏ để xóa nhiệm vụ hiện có. Nhiệm vụ chưa hoàn thành tiếp theo sẽ tự động được chọn làm nhiệm vụ hiện tại.

Xóa một nhiệm vụ có nhiệm vụ con sẽ xóa tất cả các nhiệm vụ con và con cháu của chúng.

#### Ẩn nhiệm vụ

Nếu bạn muốn không biết các nhiệm vụ mà AI đang cố gắng hoàn thành, hãy chọn hộp `Hide Tasks` để ẩn danh sách nhiệm vụ và làm cho ý định của AI trở thành một bí ẩn. Để bí ẩn 100%, hãy làm điều này trước khi nhấp `Auto-Generate Tasks`!
