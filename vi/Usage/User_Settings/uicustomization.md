---
order: 20
route: /vi/usage/core-concepts/uicustomization/
---

# Tùy chỉnh Giao diện

## Chủ đề Giao diện

### Quản lý Chủ đề

Các tệp chủ đề cho phép bạn lưu, chia sẻ và tái sử dụng các tùy chỉnh giao diện của mình. Bạn có thể duy trì nhiều chủ đề cho các tâm trạng hoặc mục đích khác nhau, và chuyển đổi giữa chúng ngay lập tức.

* Import/Export tệp chủ đề
* Xóa các chủ đề hiện có
* Lưu thay đổi vào chủ đề hiện tại
* Lưu dưới dạng chủ đề mới

Tất cả các cài đặt trong phần này được lưu vào chủ đề hiện tại. Nếu bạn chuyển chủ đề, các cài đặt sẽ được thay thế bằng cài đặt của chủ đề mới.

### Cài đặt Hiển thị

Các tùy chọn hiển thị này ảnh hưởng đến cách nhân vật và tin nhắn được trình bày trong giao diện chat.

#### Kiểu Ảnh đại diện

Chọn giữa Circle, Square, Rectangle, hoặc Rounded Square. Cài đặt này áp dụng cho cả ảnh đại diện người dùng và AI.

#### Kiểu Chat

| Kiểu         | Mô tả                                                                                                                                                          | [Lệnh Slash](/For_Contributors/st-script.md#ui-styling) |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| **Flat**     | Kiểu "chat log" sạch sẽ và liên tục, một khung vẽ phẳng để các tương tác AI của bạn trở nên sống động.                                                         | `/flat`<br>`/default`                                    |
| **Bubbles**  | Kiểu "instant messenger" với bong bóng riêng biệt cho mỗi tin nhắn, góc bo tròn dễ chịu và hiệu ứng 3D tinh tế.                                                | `/bubble`<br>`/bubbles`                                  |
| **Document** | Giao diện nhỏ gọn, giống tài liệu với bố cục tập trung vào văn bản. Ẩn ảnh đại diện, dấu thời gian và nút điều khiển tin nhắn cho các tin nhắn đã qua. | `/single`<br>`/story`                                    |

### Thông báo

Đặt vị trí nơi các cửa sổ bật lên thông báo (toast messages) sẽ xuất hiện trên màn hình.

* Top Left
* Top Center (mặc định)
* Top Right
* Bottom Left
* Bottom Center
* Bottom Right

### Kiểu Media

Kiểu hiển thị mặc định cho các media đính kèm (hình ảnh, âm thanh, video) trong tin nhắn chat. Các tiện ích mở rộng thêm media vào tin nhắn chat có thể ghi đè cài đặt này. Cũng có thể thay đổi thủ công cho từng tin nhắn bằng hành động "Toggle media display style" trong menu ngữ cảnh tin nhắn.

* **List**: Hiển thị tất cả media đính kèm cùng lúc theo bố cục dạng lưới.
* **Gallery**: Hiển thị media đính kèm theo kiểu gallery dạng carousel.

!!!
Cài đặt này cũng ảnh hưởng đến cách các media đính kèm inline được gửi đến các nguồn Chat Completion được hỗ trợ: list gửi tất cả các media đính kèm cùng lúc, trong khi gallery gửi media đính kèm đã chọn.
!!!

### Màu Chủ đề

Tùy chỉnh bảng màu của mọi thành phần giao diện để tạo chủ đề hoàn hảo của bạn. Màu sắc có thể được chọn bằng bộ chọn màu và bao gồm các tùy chọn độ trong suốt khi áp dụng.

* Main Text
* Italics Text
* Underlined Text
* Quote Text
* Text Shadow
* Chat Background
* UI Background
* UI Border
* User Message
* AI Message

### Cài đặt Bố cục & Hình ảnh

Tinh chỉnh cách trình bày hình ảnh của giao diện với các thanh trượt này.

* **Chat Width**: Điều chỉnh độ rộng cửa sổ chat (25-100% màn hình)
* **Font Scale**: Tùy chỉnh kích thước văn bản (0.5-1.5x)
* **Blur Strength**: Kiểm soát độ mờ panel giao diện (0-30)
* **Shadow Width**: Điều chỉnh cường độ bóng văn bản (0-5)

### Công tắc Chủ đề

Các công tắc này kiểm soát nhiều tính năng và hành vi giao diện. Một số tùy chọn có thể cải thiện hiệu suất trên các thiết bị cấu hình thấp, trong khi những tùy chọn khác thêm thông tin hữu ích hoặc chức năng vào giao diện chat.

* **Reduced Motion**: Vô hiệu hóa hoạt ảnh và chuyển tiếp
* **No Blur Effect**: Xóa hiệu ứng mờ nền để có hiệu suất tốt hơn
* **No Text Shadows**: Vô hiệu hóa hiệu ứng bóng văn bản
* **[Visual Novel mode](Visual-Novel.md)**: Chat nhỏ gọn với sprite nền
* **Expand Message Actions**: Luôn hiển thị menu ngữ cảnh tin nhắn đầy đủ
* **Zen Sliders**: Điều khiển tham số đơn giản hóa
* **Mad Lab Mode**: Phạm vi tham số không giới hạn
* **Message Timer**: Hiển thị thời gian tạo phản hồi AI
* **Chat Timestamps**: Hiển thị dấu thời gian tin nhắn
* **Model Icons**: Hiển thị biểu tượng mô hình AI cho tin nhắn
* **Message IDs**: Hiển thị số thứ tự tin nhắn
* **Hide Chat Avatars**: Xóa ảnh đại diện khỏi chat
* **Message Token Count**: Hiển thị số lượng token mỗi tin nhắn
* **Compact Input Area**: Đầu vào một hàng (chỉ Di động)
* **Swipe # for All Messages**: Hiển thị số swipe trên tất cả tin nhắn
* **Characters Hotswap**: Nút chọn nhanh cho nhân vật yêu thích
* **Avatar Hover Magnification**: Hiệu ứng zoom khi di chuột qua ảnh đại diện
* **Tags as Folders**: Tổ chức nhân vật bằng thẻ làm thư mục
* **Click to Edit**: Nhấp vào tin nhắn để nhanh chóng mở trình chỉnh sửa tin nhắn

### Custom CSS

Cho phép bạn áp dụng các kiểu CSS tùy chỉnh để tùy chỉnh thêm giao diện của giao diện chat.

Sử dụng <i class="fa-fw fa-solid fa-maximize" title="Expand icon"></i> **Expand** để mở rộng cửa sổ trình chỉnh sửa để có khả năng hiển thị và chỉnh sửa tốt hơn.

Nếu bạn chuyển chủ đề, CSS tùy chỉnh của bạn sẽ được thay thế bằng CSS tùy chỉnh của chủ đề mới. Đảm bảo bạn lưu CSS tùy chỉnh của mình vào một chủ đề nếu bạn muốn giữ nó khi chuyển chủ đề.

Nếu bạn sử dụng nhiều CSS tùy chỉnh, hoặc muốn sử dụng cùng một CSS tùy chỉnh với nhiều chủ đề, tiện ích mở rộng không chính thức [CSS Snippets extension](https://github.com/LenAnderson/SillyTavern-CssSnippets) có thể giúp bạn quản lý và tổ chức CSS tùy chỉnh của mình.

---

## Âm thanh Tin nhắn

Để phát âm thanh tùy chỉnh của riêng bạn khi nhận được tin nhắn mới từ bot, hãy thay thế tệp MP3 sau trong thư mục SillyTavern của bạn:

`public/sounds/message.mp3`

Phát ở âm lượng 80%.

Nếu tùy chọn "[Background Sound Only](index.md#miscellaneous)" được bật, âm thanh chỉ phát nếu cửa sổ SillyTavern **không được focus**.

## Hiển thị Công thức

Để bật hiển thị công thức toán học, hãy sử dụng [LaTeX extension](https://github.com/SillyTavern/Extension-LaTeX). Để có tiện ích mở rộng, bạn cần cài đặt nó qua menu "Download Extensions & Assets" trong SillyTavern.

Nhập công thức của bạn trong các khối code với các định danh ngôn ngữ `latex` hoặc `asciimath` cho LaTeX và AsciiMath tương ứng. Tiện ích mở rộng sử dụng [KaTeX](https://katex.org/) để hiển thị.

<pre><code>```latex
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
```

```asciimath
int_{-oo}^{oo} e^{-x^2} dx = sqrt{pi}
```</code></pre>

!!!info Thông báo ngừng hỗ trợ
Cú pháp wrapper kế thừa `$` và `$$` không còn được hỗ trợ. Vui lòng sử dụng các script regex sau để polyfill cú pháp cũ:

* [$$ - LaTeX](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$$_-_latex.json)
* [$ - AsciiMath](https://github.com/SillyTavern/Extension-LaTeX/raw/refs/heads/main/assets/$_-_asciimath.json)
!!!
