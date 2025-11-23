---
order: 10
tags:
    [
        visual novel,
        vn,
    ]
route: /usage/user-settings/visual-novel/
---

# Chế độ Visual Novel (VN)

Chế độ Visual Novel là một bố cục màn hình đặc biệt trong SillyTavern cho phép bạn trò chuyện với các nhân vật có sprite (hoặc ảnh thẻ nhân vật) giống như trong các visual novel như Doki Doki Literature Club, The Fruits of Grisaia, Fate: Stay/night và các trò chơi VN nổi tiếng khác.

## Bật/Tắt Chế độ Visual Novel

### Bật Chế độ Visual Novel

Chế độ Visual Novel được tích hợp sẵn trong SillyTavern và có thể được bật bằng cách vào *User Settings* (biểu tượng User Settings) và chọn **Visual Novel Mode** bên dưới *No Text Shadows*.

![User Settings](/static/vn/vn-mode-toggle.png)

### Tắt Chế độ Visual Novel

Tắt Chế độ Visual Novel giống như các bước bật chế độ. Bỏ chọn Visual Novel Mode và bạn sẽ quay lại màn hình chat bình thường.

!!!warning Lưu ý về Chế độ VN với Tiện ích VN
Một số tiện ích mở rộng (như Prome VN Extension) sẽ tự động bật 'Visual Novel Mode' nếu bạn sử dụng chế độ VN riêng của chúng. Bật/Tắt VN Mode từ menu *User Settings* cũng sẽ ảnh hưởng đến các tiện ích này.
!!!

## Giao diện Visual Novel

![VN Display](/static/vn/vn-display.png)

Trong Chế độ Visual Novel, giao diện người dùng được điều chỉnh một chút để phù hợp với sprite nhân vật (hoặc ảnh thẻ nhân vật) được hiển thị ở giữa. Tuy nhiên, trong cuộc trò chuyện nhóm với nhiều nhân vật, các sprite nhân vật sẽ tự động phân bổ vị trí, điều chỉnh để phù hợp với nhau như được hiển thị bên dưới.

![Group VN Display](/static/vn/group-vn-display.png)

### Chế độ VN với MovingUI

!!!info
Để bật MovingUI, vào *User Settings* và chọn **MovingUI**. Lưu ý rằng tính năng này **chỉ** hoạt động trên máy tính để bàn.
!!!

Nếu **MovingUI** được bật trong *User Settings*, các sprite (hoặc ảnh thẻ nhân vật) có thể được di chuyển xung quanh nếu bạn muốn sắp xếp lại hoặc đặt chúng vào một vị trí cụ thể hơn trên màn hình.

!!!warning Lưu ý về Kích thước Sprite
Nếu kích thước sprite nhân vật của bạn tương đối lớn, sẽ khó khăn khi cố gắng di chuyển một số sprite với MovingUI vì nút để kéo sprite có thể bị che khuất bởi sprite hiện có. Bạn có thể sẽ phải di chuyển chúng nhiều hơn bình thường, đặc biệt nếu có nhiều nhân vật trên màn hình để có vị trí tốt hơn.
!!!

![Group VN Display (MovingUI)](/static/vn/vn-group-display-movingui.png)

## Lấy Sprite Nhân vật

Bạn có thể lấy sprite nhân vật bằng cách duyệt internet để tìm sprite có sẵn, ví dụ như từ một nhân vật đã có trong Visual Novel hoặc trò chơi có tính năng Visual Novel như DDLC hoặc CounterSide. Nếu nhân vật bạn muốn chưa có sprite sẵn, bạn có một số lựa chọn khác.

1. Tìm kiếm trong bài đăng nhân vật để tìm gói ZIP sprite hoặc liên kết đến gói sprite.
    !!!info
    Một số người tạo bot có thể phát hành bot của họ kèm theo gói sprite (trong cùng một bài đăng hoặc trong kênh sprite). Tìm kiếm trong các bài đăng đó xem có ai đã tạo sprite cho nhân vật bạn muốn chưa.
    !!!
2. Tự tạo bằng cách sử dụng LoRA và Stable Diffusion.
    !!!warning
    Tạo sprite từ đầu rất tốn thời gian (đặc biệt nếu không có LoRA cho nhân vật của bạn và/hoặc cho mô hình Stable Diffusion bạn muốn sử dụng) và sẽ yêu cầu phần cứng tốt để tạo chúng, đặc biệt nếu bạn dự định tạo 28 biểu cảm sprite thay vì 6 và nếu bạn đang sử dụng SDXL và/hoặc nâng cấp độ phân giải sprite lên cao hơn.
    !!!
3. Sử dụng ảnh thẻ nhân vật. Nó có thể không giống sprite, nhưng ít nhất bạn có thứ gì đó để nhìn trên màn hình. Tuy nhiên, không thể sử dụng nhiều thẻ nhân vật trong chế độ VN.
    !!! Ảnh Thẻ Nhân vật với Prome Visual Novel Extension
    Với Prome Visual Novel Extension 1.0.6+, có một tính năng gọi là `Emulate Character Card as Sprite` cho phép bạn có cuộc trò chuyện nhóm với cả nhân vật có sprite và không có sprite bằng cách sử dụng thẻ nhân vật của họ làm sprite trong chat.

    ![Character Card Group Chat](/static/vn/extensions/prome/card-emulation.png)
    !!!

## Tiện ích mở rộng VN

### Prome Visual Novel Extension

Prome Visual Novel Extension là một tiện ích mở rộng bên thứ ba được chứng thực từ Bronya Rand và Prometheus, nâng cao trải nghiệm visual novel trong SillyTavern với các tính năng như Letterbox Mode làm cho giao diện visual novel "điện ảnh" hơn, Focus Mode với Darken Character Sprites, Traditional VN Mode chỉ hiển thị tin nhắn cuối cùng trong chat và nhiều tính năng khác đang được phát triển!

|                              Letterbox Mode                              |                          Traditional VN Mode                           |
|:------------------------------------------------------------------------:|:----------------------------------------------------------------------:|
| ![Horizontal Letterbox Mode](/static/vn/extensions/prome/horizontal.png) | ![Traditional VN Mode](/static/vn/extensions/prome/single-message.png) |

|                 Hide Sheld (Message Box)                  |                      Focus Mode (w/ Darken Sprites)                      |
|:---------------------------------------------------------:|:------------------------------------------------------------------------:|
| ![Sheld Hide](/static/vn/extensions/prome/sheld_hide.png) | ![Focus Mode w/ Darken Sprites](/static/vn/extensions/prome/defocus.png) |

Để cài đặt Prome Visual Novel Extension, bạn có thể cài đặt bằng cách vào `Download Extensions & Assets` và tìm *Prome Visual Novel Extension*, hoặc làm theo hướng dẫn cài đặt trên trang Github [Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension?tab=readme-ov-file#installation-and-usage). Điều chỉnh cài đặt của Prome có thể được tìm thấy trong *Extensions* -> **Prome (Visual Novel Extension)** hoặc qua menu 🪄 (Wand).
