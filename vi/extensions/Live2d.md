---
route: /vi/extensions/live2d/
---

# Live2D

Hướng dẫn này sẽ hướng dẫn bạn qua quá trình thiết lập và tùy chỉnh tiện ích mở rộng Live2D cho trải nghiệm SillyTavern của bạn. Tiện ích mở rộng này cho phép bạn sử dụng các mô hình Live2D động cho nhân vật của mình, cung cấp một yếu tố động và tương tác cho nhân vật ảo của bạn.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các điều kiện tiên quyết sau:

1. **Lựa chọn nhánh**: Đảm bảo bạn đang sử dụng phiên bản mới nhất của SillyTavern để truy cập các tính năng và cập nhật mới nhất.

2. **Cài đặt tiện ích mở rộng**: Cài đặt tiện ích mở rộng "Live2D" từ menu "Download Extensions & Assets" trong bảng Extensions (được đại diện bởi biểu tượng khối xếp chồng).

3. **Đặt thư mục mô hình**: Đặt các thư mục mô hình Live2D của bạn vào thư mục `/data/<user-handle>/assets/live2d`. Một thư mục tài nguyên `live2d` được tổ chức đúng cách có thể trông như thế này:

    ![Asset folder example](/static/extensions/live2d-folder.png)

    - Một thư mục mô hình Live2D nên bao gồm tất cả các thành phần cần thiết cho mô hình Live2D, chẳng hạn như biểu cảm, chuyển động, kết cấu, âm thanh và tệp cài đặt. Đáng chú ý là tệp `***.model.json` phải ở gốc của thư mục mô hình Live2D để mô hình được phát hiện bởi tiện ích mở rộng. Trong ví dụ này, thư mục mô hình live2d `shizuku` có thể trông như thế này:

    ![Live2d model folder example](/static/extensions/live2d-model.png)

    - Lưu ý: Mô hình cũng có thể được đặt trong các thư mục dành riêng cho nhân vật, chẳng hạn như `/data/<user-handle>/characters/Shizuku/live2d/`. Tuy nhiên, các mô hình trong thư mục nhân vật sẽ chỉ có thể truy cập cho nhân vật cụ thể đó.

## Cài đặt tiện ích mở rộng

Tiện ích mở rộng Live2D cung cấp nhiều cài đặt khác nhau để tùy chỉnh hành vi của mô hình động của bạn. Dưới đây là các cài đặt chính:

![UI global settings](/static/extensions/live2d-global.png)

### Cài đặt chung

1. **Enabled**:
   - Bật hộp kiểm này để kích hoạt tiện ích mở rộng, cho phép mô hình Live2D của bạn tương tác trong SillyTavern.
   - Bạn có thể tắt tiện ích mở rộng nếu bạn muốn chỉ sử dụng sprites thông thường.
   - Bạn có thể tắt tiện ích mở rộng khi bạn muốn di chuyển sprites thông thường trong cuộc trò chuyện nhóm và bật lại khi bạn sẵn sàng sử dụng mô hình Live2D.

2. **Follow Cursor**:
   - Bật hộp kiểm này để làm cho mô hình Live2D theo con trỏ của bạn, với điều kiện mô hình hỗ trợ tính năng này.

3. **Auto-send Interaction**:
   - Bật hộp kiểm này để tự động kích hoạt tương tác nhân vật khi bạn nhấp vào các vùng với tin nhắn được ánh xạ (tham khảo phần vùng hit để biết chi tiết).


## Cài đặt Debug

Các cài đặt này giúp bạn kiểm soát hành vi và khả năng hiển thị của mô hình Live2D của bạn cho mục đích gỡ lỗi.

1. **Reset Model Before Animation**:
   - Bật hộp kiểm này để tải lại mô hình trước bất kỳ hoạt ảnh nào. Điều này buộc hoạt ảnh bắt đầu và cho phép bạn spam nhấp nếu cần thiết. Một số mô hình có thể yêu cầu điều này để đảm bảo rằng các hoạt ảnh bắt đầu từ trạng thái tương thích.

2. **Show Model Frames**:
   - Bật hộp kiểm này để hiển thị khung mô hình, giúp dễ dàng xác định nơi để nhấp để kéo mô hình xung quanh. Nó cũng hiển thị vùng hit, nếu có. Di chuột qua vùng hit sẽ hiển thị tên của nó.

3. **Reload button**
    - Nhấp nút này để tải lại mọi mô hình live2d. Sử dụng nó trong trường hợp có gì đó lỗi.

## Lựa chọn nhân vật

Các cài đặt này cho phép bạn quản lý nhân vật và gán mô hình Live2D cho họ.

1. **Refresh Button**:
   - Nhấp nút refresh để cập nhật danh sách các nhân vật trong cuộc trò chuyện hiện tại.

2. **Select Character**:
   - Sử dụng danh sách thả xuống để chọn một nhân vật để gán mô hình Live2D.

3. **Remove Button**:
   - Nhấp nút này để xóa tất cả các mô hình được gán cho một nhân vật. Một lời nhắc xác nhận sẽ xuất hiện để xác nhận việc xóa.

## Lựa chọn mô hình

![UI model list](/static/extensions/live2d-list.png)

1. **Refresh Button**:
   - Nhấp nút refresh nếu mô hình Live2D của bạn không xuất hiện trong danh sách.

2. **Select Model**:
   - Chọn một mô hình từ danh sách để gán nó cho nhân vật đã chọn.
   - Mô hình có thể nằm trong thư mục asset hoặc thư mục nhân vật hiện tại.
   - Danh sách hiển thị tên thư mục mô hình, nguồn gốc của nó (asset hoặc nhân vật), và tên của tệp cài đặt mô hình được phát hiện.
   - Lưu ý rằng một số thư mục mô hình có thể chứa các phiên bản khác nhau của cùng một mô hình. Bạn có thể thử các tệp mô hình khác nhau để xem cái nào hoạt động tốt nhất.
   - Chọn none sẽ sử dụng sprites thông thường nếu có
   - Cài đặt được lưu cho mỗi nhân vật và mô hình

## Cài đặt mô hình

![UI model settings](/static/extensions/live2d-settings.png)

1. **Model Scale**:
   - Sử dụng thanh trượt để điều chỉnh kích thước của mô hình, làm cho nó lớn hơn hoặc nhỏ hơn.

2. **Model Center X Offset**:
   - Sử dụng thanh trượt để thay đổi vị trí ngang của mô hình so với trung tâm cửa sổ.

3. **Model Center Y Offset**:
   - Sử dụng thanh trượt để điều chỉnh vị trí dọc của mô hình so với trung tâm cửa sổ.


### Ghi chú
- Các cài đặt được lưu và chuyển sang các cuộc trò chuyện khác nhau.
- Bạn cũng có thể kéo mô hình bằng chuột của mình, và các cài đặt đó sẽ được cập nhật và lưu.
- Sử dụng các cài đặt UI này để đưa mô hình của bạn trở lại màn hình nếu bạn vô tình làm cho nó ra khỏi tầm nhìn. Ngoài ra, hãy chọn hộp kiểm "Show frame" để thấy rõ ràng nơi bạn có thể nhấp để kéo mô hình.

## Model Talk

![UI model talk](/static/extensions/live2d-talk.png)

1. **Param mouth open Y id**
    - Chọn từ danh sách ID của tham số tương ứng với giá trị Y miệng của mô hình. Không phải tất cả các mô hình đều có một, và tên có thể khác nhau từ mô hình này sang mô hình khác. Thường là một cái gì đó như "PARAM_MOUTH_OPEN_Y" hoặc "ParamMouthOpenY". Kiểm tra mô hình khi chọn một phần tử từ danh sách; nó sẽ cố gắng chạy hoạt ảnh nói. Nếu miệng di chuyển, bạn đã tìm thấy!

2. **Mouth movement speed**
    - Điều chỉnh thanh trượt để thay đổi tốc độ chuyển động của hoạt ảnh miệng.

3. **Time per character**
    - Đặt thời lượng thời gian của mỗi ký tự. Thời lượng của hoạt ảnh nói sẽ là thời gian này nhân với số lượng ký tự của tin nhắn.

### Ghi chú
- Hoạt ảnh miệng này không hoạt động trên mọi mô hình và mọi hoạt ảnh. Ngay cả khi mô hình của bạn có các hoạt ảnh mà miệng di chuyển, nó không có nghĩa là hoạt ảnh miệng có thể được kiểm soát bởi tiện ích mở rộng này. Nếu không có gì hiển thị trong danh sách tham số, mô hình của bạn có thể được tạo với phiên bản Live2D quá cũ để truy cập các tham số đúng cách.

## Model Animations

![UI model animations](/static/extensions/live2d-animations.png)

1. **Starter animation**
    - Chọn một biểu cảm và chuyển động từ danh sách sẽ phát khi bắt đầu cuộc trò chuyện với nhân vật. Bạn cũng có thể thêm độ trễ trong đó mô hình sẽ vô hình nếu bạn cần ẩn nhân vật trong một khoảng thời gian để đạt được hiệu ứng hoàn hảo.

2. **Default animation**
    - Chọn một biểu cảm và chuyển động từ danh sách sẽ phát khi nhân vật gửi tin nhắn. Sử dụng hoạt ảnh dự phòng khi sử dụng tiện ích mở rộng classify expression.

### Ghi chú
- Hoạt ảnh sẽ phát khi bạn chọn một trong các danh sách.
- Sử dụng nút replay để phát lại hoạt ảnh đã chọn.
- Một số mô hình có các biểu cảm được định nghĩa là chuyển động.
- Nếu không có gì hiển thị trong danh sách, có khả năng tệp cài đặt mô hình của bạn không có biểu cảm/chuyển động được định nghĩa.

## Ánh xạ vùng hit

![UI model mapping](/static/extensions/live2d-mapping.png)

1. **Default click animation**
    - Chọn một biểu cảm và chuyển động từ danh sách sẽ phát khi bạn nhấp vào mô hình. Bạn cũng có thể đặt một tin nhắn sẽ được gửi dưới dạng tin nhắn người dùng.

2. **Hit areas**
    - Nếu mô hình có vùng hit, chúng sẽ được liệt kê, và bạn có thể gán một hoạt ảnh/tin nhắn cho mỗi vùng.


### Ghi chú
- Một số mô hình không có vùng hit, nhưng nhấp mặc định được phát hiện cho tất cả.
- Nhấp mặc định sẽ kích hoạt nếu bạn nhấp vào vùng hit không có gì được ánh xạ hoặc nếu bạn nhấp bên ngoài bất kỳ vùng hit nào.
- Các vùng hit có ưu tiên được định nghĩa trong mô hình; ví dụ, "mouth" nằm bên trong "head". Nếu nó không hoạt động đúng, có thể do tệp mô hình.
- Đối với một số mô hình, các hoạt ảnh cần được hoàn thành trước khi bắt đầu một hoạt ảnh khác. Sử dụng hộp kiểm debug nếu bạn muốn buộc refresh và spam các hoạt ảnh.

## Ánh xạ biểu cảm được phân loại

![UI model classify](/static/extensions/live2d-classify.png)

1. **Yêu cầu**
    - Yêu cầu sử dụng tiện ích mở rộng classify expression; nếu không, nó sẽ dự phòng sang hoạt ảnh mặc định.

2. **Ánh xạ**
    - Đối với mỗi cảm xúc được phát hiện bởi tiện ích mở rộng classify, bạn có thể gán một hoạt ảnh biểu cảm/chuyển động.

### Ghi chú
- Nếu hoạt ảnh trước đó chưa kết thúc khi một tin nhắn mới được nhận, có thể hoạt ảnh mới sẽ không phát. Hành vi này phụ thuộc vào mô hình Live2D. Sử dụng hộp kiểm debug nếu bạn muốn buộc hoạt ảnh phát.

Cảm ơn bạn đã làm theo hướng dẫn này! Trải nghiệm SillyTavern của bạn giờ đây được làm phong phú với các mô hình Live2D động và tương tác.
