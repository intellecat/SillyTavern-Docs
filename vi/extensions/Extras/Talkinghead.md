---
route: /vi/extensions/talkinghead/
tags: ['obsolete']
---

# talkinghead

!!!warning
**HỖ TRỢ CHO TALKINGHEAD ĐÃ BỊ HỦY BỎ TRONG SILLYTAVERN 1.12.13. TRANG NÀY ĐƯỢC GIỮ LẠI CHO MỤC ĐÍCH LỊCH SỬ.**
!!!

### Nó là gì?

Một triển khai của Talking Head Anime 3 Demo cho AITuber. Nó có các tính năng sau:

- Tạo các hành động chuyển động ngẫu nhiên giống Live 2D từ một hình ảnh tĩnh duy nhất.
- Đồng bộ môi với đầu ra âm thanh từ bất kỳ đầu ra TTS nào.

Extension này chứa các chương trình demo gốc cho dự án Talking Head(?) Anime from a Single Image 3: Now the Body Too. Như tên gọi, dự án cho phép bạn làm hoạt hình các nhân vật anime, và bạn chỉ cần một hình ảnh duy nhất của nhân vật đó để thực hiện. Có hai chương trình demo:

manual_poser cho phép bạn điều khiển biểu cảm khuôn mặt, xoay đầu, xoay cơ thể và mở rộng ngực do thở của nhân vật thông qua giao diện người dùng đồ họa, vì vậy bạn có thể lưu chúng làm biểu cảm mặc định như Vui, buồn, hạnh phúc, v.v.
ifacialmocap_puppeteer cho phép bạn chuyển chuyển động khuôn mặt của mình sang nhân vật anime.

### Yêu cầu phần cứng

Bạn có thể sử dụng chế độ CPU hoặc GPU (CPU là mặc định). Tuy nhiên, trong chế độ CPU, hãy mong đợi khoảng 1 FPS, và trong chế độ GPU trên RTX3060, tôi đang nhận được khoảng 9-10 FPS.

ifacialmocap_puppeteer yêu cầu thiết bị iOS có khả năng tính toán các tham số blend shape từ nguồn cấp video. Điều này có nghĩa là thiết bị phải có khả năng chạy iOS 11.0 trở lên và phải có camera mặt trước TrueDepth. (Xem trang này để biết thêm thông tin.) Nói cách khác, nếu bạn có iPhone X hoặc tốt hơn, bạn sẽ sẵn sàng.

### Cách sử dụng

Bạn phải khởi chạy extras với các module sau để talkinghead hoạt động: `classify` và `talkinghead`!
classify là bắt buộc để xử lý file talkinghead.png. Ngoài ra, bạn cũng có thể sử dụng `--talkinghead-gpu` để tải các mô hình blend vào bộ nhớ GPU và làm cho hoạt hình nhanh hơn 10 lần. Rất khuyến nghị sử dụng tăng tốc GPU! Theo mặc định, khi chương trình khởi động, nó sẽ tải một hình ảnh mặc định SillyTavern-extras\talkinghead\tha3\images\lambda_00.png. Bạn có thể xác minh nó đang hoạt động bằng cách truy cập http://localhost:5100/api/talkinghead/result_feed hoặc `YOUR EXT URL:PORT/api/talkinghead/result_feed`.

- Sau khi server đã khởi động, hãy vào tab Extension API và kết nối. Sau đó chỉ cần chọn một character card để tải. (`--enable-modules=classify,talkinghead --talkinghead-gpu` khi khởi động server.py)

- Bây giờ chọn Character Expressions, nếu bạn đánh dấu hộp image type talkinghead, script sẽ thay thế biểu cảm nhân vật hiện tại của bạn bằng kết quả của `YOUR EXT URL:PORT/api/talkinghead/result_feed` bỏ đánh dấu hộp SẼ trả lại hình ảnh về biểu cảm gốc, tuy nhiên đôi khi bạn phải gửi tin nhắn mới vào chat để "reload" hình ảnh.

- Nếu bạn không có file talkinghead.png trong thư mục character, nó sẽ chỉ hiển thị hình ảnh mặc định hoặc character card cuối cùng có file talkinghead.png. Hình ảnh nguồn hoạt hình được thay đổi khi character card được thay đổi.

- Bây giờ mở character expressions, cuộn xuống hình ảnh talkinghead và tải lên file hình ảnh đáp ứng các yêu cầu trong phần bên dưới có tên "Ràng buộc trên hình ảnh đầu vào".

- Sau đó đánh dấu và bỏ đánh dấu hộp talkinghead để reload nhân vật. Nếu hình ảnh trông buồn cười, có lẽ là do nó không trong suốt / không có alpha layer. Nếu không, hãy làm theo hướng dẫn và template bên dưới.

### Ràng buộc trên hình ảnh đầu vào
Để hệ thống hoạt động tốt, hình ảnh đầu vào phải tuân theo các ràng buộc sau:

Nó phải có độ phân giải 512 x 512. (Nếu chương trình nhận được hình ảnh đầu vào có kích thước khác, nó sẽ thay đổi kích thước hình ảnh thành độ phân giải này và cũng xuất ra ở độ phân giải này.)
Nó phải có alpha channel.
Nó chỉ phải chứa một nhân vật hình người.
Nhân vật nên đứng thẳng và hướng về phía trước.
Tay của nhân vật nên ở dưới và cách xa đầu.
Đầu của nhân vật phải gần như nằm trong hộp 128 x 128 ở giữa nửa trên của hình ảnh.
Alpha channels của tất cả các pixel không thuộc về nhân vật (tức là các pixel nền) phải là 0.

![Ràng buộc đầu vào](/static/input_spec.png)

### PHẦN NÂNG CAO

### Python Environment

Ngoài tính năng cơ bản (app.py), cả manual_poser và ifacialmocap_puppeteer đều có sẵn dưới dạng ứng dụng desktop. Để chạy chúng, bạn cần thiết lập một môi trường để chạy các chương trình được viết bằng ngôn ngữ Python. Môi trường cần có các gói phần mềm sau:

* Python >= 3.8
* PyTorch >= 1.11.0 với hỗ trợ CUDA
* SciPY >= 1.7.3
* wxPython >= 4.1.1
* Matplotlib >= 3.5.1

Một cách để thực hiện là cài đặt Anaconda và chạy các lệnh sau trong shell của bạn:

> conda create -n talking-head-anime-3-demo python=3.8
> conda activate talking-head-anime-3-demo
> conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
> conda install scipy
> pip install wxpython
> conda install matplotlib

### Các mô hình Blend bổ sung

Chỉ có một mô hình (nhẹ nhất) được bao gồm, nếu bạn muốn các mô hình blend bổ sung, bạn cần tải xuống các file mô hình từ https://www.dropbox.com/s/y7b8jl4n2euv8xe/talking-head-anime-3-models.zip?dl=0 và giải nén nó vào thư mục SillyTavern-extras\talkinghead\tha3\models. Cuối cùng, thư mục data sẽ trông như sau:

+ tha3
  + models
    + separable_float
      - editor.pt
      - eyebrow_decomposer.pt
      - eyebrow_morphing_combiner.pt
      - face_morpher.pt
      - two_algo_face_body_rotator.pt
    + separable_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_float
      - editor.pt
          :
      - two_algo_face_body_rotator.pt
    + standard_half
      - editor.pt
          :
      - two_algo_face_body_rotator.pt

Các file mô hình được phân phối với Creative Commons Attribution 4.0 International License, có nghĩa là bạn có thể sử dụng chúng cho mục đích thương mại. Tuy nhiên, Pramook Khungurn. Talking Head(?) Anime from a Single Image 3: Now the Body Too. <https://github.com/pkhungurn/talking-head-anime-3-demo>, là người tạo ra.

### Chạy ứng dụng Desktop manual_poser
Mở shell. Thay đổi thư mục làm việc của bạn thành thư mục gốc của repository. Sau đó, chạy:

> python tha3/app/manual_poser.py
Lưu ý rằng trước khi chạy lệnh trên, bạn có thể phải kích hoạt môi trường Python chứa các gói cần thiết.

> conda activate extras
nếu bạn chưa kích hoạt môi trường.
