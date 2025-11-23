---
order: 110
icon: smiley
route: /usage/core-concepts/personas/
templating: false
---

# Personas

## Persona là gì?

Persona trong SillyTavern là danh tính bạn sử dụng để tham gia vào các cuộc trò chuyện — về cơ bản là sự kết hợp giữa tên hiển thị, avatar và văn bản mô tả tùy chọn của bạn. Personas cho phép bạn dễ dàng chuyển đổi vai trò hoặc "nhân vật" bạn đang nói, mà không cần phải cập nhật thủ công tên người dùng/avatar mỗi lần.

!!!
**Lưu ý:** Avatar/tên người dùng cũ không được gắn với persona đã bị xóa. Dữ liệu hiện có sẽ được chuyển đổi sang personas. Nếu không có tên được chỉ định, persona sẽ được đặt tên "[Unnamed Persona]".
!!!

## Làm thế nào để tạo Persona?

1. Mở bảng điều khiển **Persona Management** (nút <i class="fa-solid fa-face-smile"></i> trong menu trên cùng).
2. Tạo một persona trống với nút **Create** và đặt tên cho nó.
3. Trong danh sách persona, chọn persona mới được tạo.
4. Ở bên phải, bạn có thể điền mô tả của mình và đặt avatar qua nút "Change Persona Image". Cả hai đều là tùy chọn.
5. Bây giờ persona của bạn đã sẵn sàng để sử dụng trong các cuộc trò chuyện.

### Chuyển đổi Character thành Persona

Personas cũng có thể được tạo bằng cách chuyển đổi bất kỳ nhân vật hiện có nào. Chỉ cần mở nhân vật, chọn "More..." và nhấp vào "Convert to Persona". Một persona có cùng tên và mô tả sẽ được tạo. Các trường khác của character card như Scenario hoặc Personality sẽ không được sử dụng. Nhân vật sẽ không bị xóa.

!!! Note
Vì macro `{{user}}` và `{{char}}` có ý nghĩa ngược lại khi được sử dụng trong mô tả Persona và Character, bạn sẽ được nhắc hoán đổi chúng nếu mô tả đã chuyển đổi chứa một trong số chúng.
!!!

## Mô tả Persona

Mỗi persona có thể lưu trữ một mô tả văn bản tùy chỉnh — các đặc điểm tinh thần và thể chất, tuổi tác, nghề nghiệp, hoặc bất kỳ chi tiết cá nhân nào. Chúng cũng có thể bao gồm các macro mẫu như `{{char}}` hoặc `{{user}}` (xem [Macros](/Usage/Characters/macros.md)).

Vị trí mô tả persona của bạn được chèn vào prompt AI phụ thuộc vào cài đặt **Position** trong bảng điều khiển Persona Management:

- **None (disabled)**
- **In Story String / Prompt Manager** (mặc định)
- **Top of Author's Note** / **Bottom of Author's Note** (sẽ chỉ được thêm khi có Author's Note)
- **In Chat @ Depth** (điều này sẽ mở các tùy chọn cấu hình để đặt độ sâu và vai trò)

Vị trí được lưu **cho từng persona**.

## Tiêu đề Persona

Tiêu đề là một trường văn bản tùy chọn có thể được sử dụng để lưu trữ thông tin bổ sung về persona và không được sử dụng trong prompt, nhưng được hiển thị trong bảng điều khiển Persona Management.

Để đặt tiêu đề, nhấp vào nút **<i class="fa-solid fa-pencil"></i> Rename Persona** trong bảng điều khiển Persona Management và nhập tiêu đề trong trường "Persona Title", hoặc chỉ định nó trong quá trình tạo persona. Đặt giá trị rỗng khi tiêu đề đã tồn tại sẽ xóa nó.

## Kết nối Persona / Khóa

Kết nối persona đảm bảo rằng một persona nhất định được tự động chọn trong các tình huống nhất định. Nếu không có persona nào được kết nối, persona hiện được chọn sẽ tiếp tục được chọn.

Có ba loại khóa:

1. **<i class="fa-solid fa-unlock"></i> Chat lock** – Persona được khóa vào cuộc trò chuyện hiện tại.
2. **<i class="fa-solid fa-unlock"></i> Character lock** – Persona được khóa vào một nhân vật cụ thể.
3. **<i class="fa-solid fa-crown"></i> Default persona** – Một persona được sử dụng bất cứ khi nào không có khóa nào khác được áp dụng.

### 1. Khóa vào Chat

Nếu một persona được khóa vào một cuộc trò chuyện, mở cuộc trò chuyện đó trong tương lai sẽ tự động chuyển persona đang hoạt động của bạn sang persona đã khóa.

- **Để khóa**: Chọn persona mong muốn, sau đó nhấp vào nút **<i class="fa-solid fa-unlock"></i> Chat** trong phần "Connections" (hoặc sử dụng `/persona-lock type=chat on`).
- **Để mở khóa**: Nhấp vào nút lần nữa (hoặc sử dụng `/persona-lock type=chat off`).

### 2. Khóa vào Character

Bạn cũng có thể liên kết một persona với một nhân vật cụ thể. Mở bất kỳ cuộc trò chuyện nào với nhân vật đó sẽ tự động chọn persona đã khóa của bạn.

- **Để khóa**: Chọn persona mong muốn, sau đó nhấp vào nút **<i class="fa-solid fa-unlock"></i> Character** trong phần "Connections" (hoặc sử dụng `/persona-lock type=character on`).
- **Để mở khóa**: Nhấp vào nút lần nữa (hoặc sử dụng `/persona-lock type=character off`).

Bảng điều khiển Persona Management cũng hiển thị nhân vật nào được liên kết với persona đó (hiển thị dưới dạng avatar nhỏ). Nhấp vào chúng sẽ điều hướng trực tiếp đến cuộc trò chuyện của nhân vật đó.

#### Khóa nhiều Personas vào cùng một Character

Nếu một persona khác đã được liên kết với nhân vật đó, nó sẽ tự động bị hủy liên kết theo mặc định.

Để có nhiều persona được liên kết cùng một lúc, có thể sử dụng cài đặt toàn cầu **Allow multiple persona connections per character**.
Nếu nhiều persona được liên kết với cùng một nhân vật, bạn sẽ thấy một popup hỏi persona nào sẽ sử dụng mỗi khi bạn mở hoặc bắt đầu một cuộc trò chuyện mới với nhân vật đó (trừ khi một persona được gắn với cuộc trò chuyện).

### 3. Default Persona

**Default persona** của bạn được sử dụng bất cứ khi nào không có khóa liên quan nào khác. Default persona có thể nhận biết bằng viền màu vàng xung quanh avatar của nó.

- **Để đặt/hủy default**: Chọn persona mong muốn, sau đó nhấp vào nút **<i class="fa-solid fa-crown"></i> Default** trong phần "Connections" (hoặc sử dụng `/persona-lock type=default`).

Chỉ có thể chọn một persona làm default persona.

### Temporary Persona

Nếu bất kỳ tùy chọn kết nối nào trong ba tùy chọn kết nối một persona với nhân vật/cuộc trò chuyện hiện tại, bạn vẫn có thể chọn sử dụng một persona khác. Persona này sẽ được đánh dấu trong bảng persona là "Temporary Persona". Bất kỳ lần tải lại nào của cửa sổ trình duyệt hoặc chuyển sang một cuộc trò chuyện khác và quay lại sẽ đặt lại nó về persona đã liên kết.

Bạn có thể *chuyển đổi* thủ công Temporary Persona để được kết nối liên tục bằng cách liên kết nó với cuộc trò chuyện.

## Cài đặt Persona toàn cầu

Tất cả các cài đặt trong **Current Persona** được lưu cho từng persona. Một vài cài đặt toàn cầu cũng tồn tại, chúng có thể được tìm thấy trong **Global Persona Settings** trong bảng điều khiển Persona Management.

1. **Show notifications on switching personas**
   - Bật thông báo liên quan đến persona (ví dụ: "Persona Auto Selected", "Temporary Persona").

2. **Allow multiple persona connections per character**
   - Khi **bật**, bạn có thể liên kết nhiều persona với một nhân vật duy nhất. Mở cuộc trò chuyện của nhân vật đó sẽ nhắc bạn chọn persona nào sử dụng. Nếu tắt, chỉ có thể kết nối một persona với một nhân vật tại một thời điểm.

3. **Auto-lock a chosen persona to the chat**
   - Khi **bật**, bất cứ khi nào bạn chọn một persona (thủ công hoặc bằng tự động chọn) hoặc tạo một cuộc trò chuyện mới, nó sẽ khóa persona đó vào cuộc trò chuyện.
   Kết hợp với "Allow multiple" cung cấp tùy chọn để có lựa chọn persona cho mỗi nhân vật, nhưng giữ nó gắn kết một khi đã chọn cho một cuộc trò chuyện.

## Lệnh Slash cho Personas

### `/persona-lock type=<type?>`

- `chat` khóa persona hiện tại vào cuộc trò chuyện đang hoạt động của bạn.
- `character` khóa persona hiện tại vào nhân vật đang sử dụng.
- `none` (hoặc không có đối số) mở khóa/xóa khóa persona cho ngữ cảnh hiện tại.
- Nếu được sử dụng mà không có đối số, nó trả về trạng thái khóa hiện tại (hoặc lỗi nếu không có).
- Trạng thái khóa có thể được chọn qua `on`, `off` hoặc `toggle`. Mặc định là toggle.

### `/persona <name>`

- Nhanh chóng chuyển đổi persona đang hoạt động của bạn theo tên mà không cần mở bảng điều khiển Persona Management.
- Ví dụ: `/persona Blaze`.
- Sử dụng `mode=temp` cho phép tạm thời đặt tên của persona **hiện tại**, mặc dù một persona có cùng tên có thể đã tồn tại (giữ nguyên avatar và mô tả hiện tại của bạn).

### `/persona-sync`

- Gán lại tất cả tin nhắn người dùng trong cuộc trò chuyện đang hoạt động cho persona **hiện tại** và tên của nó.

> **Lưu ý:** Các lệnh cũ `/lock` và `/unlock` vẫn còn để tương thích ngược nhưng có thể bị xóa trong tương lai. Sử dụng `/persona-lock` thay thế.

## Mẹo Pro

1. **Chuyển đổi persona giữa cuộc trò chuyện** không gán lại các tin nhắn người dùng trước đó của bạn cho persona mới; chúng vẫn được gán cho bất kỳ persona nào bạn đang sử dụng tại thời điểm đó.
2. **Gán lại hàng loạt**: Nếu bạn cần tất cả các tin nhắn trước đó khớp với một persona mới, nhấn nút **sync** hoặc sử dụng `/persona-sync`.
3. **Thay thế hình ảnh persona** mà không mất mô tả hoặc khóa bằng cách chọn persona của bạn và nhấp vào nút **<i class="fa-solid fa-images"></i> Change Persona Image**.
4. **Popup liên kết nhân vật**: Nếu nhiều persona được liên kết với cùng một nhân vật, bạn sẽ nhận được popup để chọn persona nào mỗi khi bạn mở cuộc trò chuyện. Đây là một cách tiện lợi để có một lựa chọn nhỏ các persona để chọn cho các nhân vật cụ thể.
5. **Sao lưu**: Bạn có thể sao lưu toàn bộ danh sách persona của mình (tên, kết nối nhân vật, mô tả) bằng nút **Backup** trong Persona Management, và khôi phục nó sau nếu cần.

!!!tip Lưu ý về sao lưu

- Hình ảnh và kết nối cuộc trò chuyện không được lưu cùng với personas và sẽ không được sao lưu qua cách này.
- Các bản sao lưu này không được thiết kế để chia sẻ, vì chúng chứa các liên kết nội bộ.

!!!
