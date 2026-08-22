---
order: tts-xtts
route: /vi/extensions/xtts/
---

# XTTS với voice cloning

Lời chào! Vì vậy, bạn đã choáng ngợp trước những bài đăng trên Reddit cho thấy công nghệ AI text-to-speech đã tiến xa đến đâu?

Cảm thấy phấn khích khi cho robotic waifu/husbando của bạn một bộ điều chỉnh giọng nói mới lấp lánh?

Đừng sợ, công nghệ đột phá đáng kinh ngạc này đã có sẵn tại SillyTavern cục bộ của bạn, bạn chỉ cần một...

## Điều kiện tiên quyết

1. Phiên bản mới nhất của SillyTavern.
2. [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/miniconda-install.html) được cài đặt.
3. (Windows) [Visual C++ Build Tools](https://visualstudio.microsoft.com/visual-cpp-build-tools/) được cài đặt.
4. Các file WAV với các clip giọng nói để sao chép từ (~10 giây mỗi file). Yêu cầu file: PCM, Mono, 22050Hz, 16-bit (chuyển đổi qua Audacity).
5. Tạo một thư mục với các subfolder "speakers" và "output". Đặt các file WAV vào "speakers".

Ví dụ về cấu trúc thư mục:
```
C:\xtts
  - speakers
    - alice.wav
    - bob.wav
  - output
```

## Cài đặt

[daswer123](https://github.com/daswer123) đã tạo một API server chạy mô hình XTTSv2 trên máy tính của bạn và kết nối với extension TTS của SillyTavern.

Nó hoàn toàn độc lập với Extras API và sẽ sử dụng một môi trường riêng biệt.

**Rất quan trọng:** Đừng cài đặt các yêu cầu sau vào môi trường Extras hoặc Python hệ thống của bạn.
Nó sẽ phá vỡ các package khác, downgrade không cần thiết, v.v.

Hướng dẫn sau được cung cấp bằng cách sử dụng Miniconda, nhưng bạn cũng có thể thực hiện nó với venv (không được bao gồm ở đây).
Mở Anaconda command prompt và làm theo các hướng dẫn từng dòng.

### Nhận server lên và chạy

1. Điều hướng đến thư mục bạn đã tạo ở bước 4 của điều kiện tiên quyết.
    ```
    cd C:\xtts
    ```
2. Tạo một conda env mới. Từ bây giờ, chúng ta sẽ gọi nó `xtts`.
    ```
    conda create -n xtts
    ```
3. Kích hoạt một env mới được tạo.
    ```
    conda activate xtts
    ```
4. Cài đặt Python 3.10 vào env của bạn. Xác nhận bằng "y" khi được nhắc.
    ```
    conda install python=3.10
    ```
5. Cài đặt XTTS server với các yêu cầu của nó.
    ```
    pip install xtts-api-server pydub
    ```
6. Cài đặt PyTorch. Điều này có thể mất một thời gian. Dòng sau cài đặt PyTorch với hỗ trợ GPU acceleration (CUDA).
Nếu bạn muốn chỉ sử dụng CPU inference, hãy bỏ phần cuối cùng bắt đầu bằng `--index-url`.
    ```
    pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
    ```
7. Khởi động XTTS server trên host và port mặc định: <http://localhost:8020>
    ```
    python -m xtts_api_server
    ```
8. Trong lần khởi động đầu tiên, mô hình sẽ được tải xuống (khoảng ~2 GB).
Đừng quên đọc thông báo pháp lý từ Coqui AI rất cẩn thận. Lol, tôi đang đùa, chỉ cần nhấn "y" một lần nữa.

### Kết nối tới SillyTavern

1. Mở bảng điều khiển extensions, mở rộng menu TTS, và chọn "XTTSv2" trong danh sách nhà cung cấp.
2. Chọn ngôn ngữ text-to-speech của bạn trong dropdown Language (Tôi sẽ buồn nếu nó không phải là Tiếng Ba Lan).
3. Xác minh rằng endpoint nhà cung cấp trỏ tới <http://localhost:8020> và "Available voices" hiển thị danh sách các mẫu giọng nói của bạn.
4. Chọn bất kỳ nhân vật nào và đặt ánh xạ giữa mẫu giọng nói và nhân vật.
Nếu danh sách nhân vật trống, hãy nhấn "Reload" một vài lần.
5. Cấu hình các cài đặt TTS còn lại theo tùy chọn của bạn.

### Bây giờ bạn đã sẵn sàng!

Nhấp vào biểu tượng bullhorn trong menu tác vụ bối cảnh cho bất kỳ tin nhắn nào và nghe giọng nói được sao chép đẹp phát ra từ loa của bạn. Tạo mất một thời gian và nó không phải là real-time ngay cả trên các GPU RTX high-end.

### Streaming?

Có thể sử dụng HTTP streaming với phiên bản XTTS server mới nhất để nhận các chunks của âm thanh được tạo ngay khi nó khả dụng!

#### Điều này không hoạt động với RVC!

Âm thanh vẫn sẽ được tạo (giả sử bạn đang sử dụng phiên bản RVC extension mới nhất) và được chuyển đổi, *nhưng không được stream* vì RVC yêu cầu có file âm thanh đầy đủ trước khi bắt đầu chuyển đổi. Streaming RVC vẫn đang được điều tra...

#### Làm thế nào để có được hỗ trợ streaming?

1. Cập nhật SillyTavern lên phiên bản mới nhất.
2. Cập nhật XTTS server lên phiên bản mới nhất.

    ```bash
    conda activate xtts
    pip install xtts-api-server --upgrade
    ```

3. Khởi động và kết nối XTTS tới ST như thường lệ.
4. Bật cài đặt "Streaming" XTTS extension trong SillyTavern.

### Âm thanh bị cắt?

Thử tăng cài đặt "chunk size".

Để tham khảo: với chunk size 200, RTX 3090 có thể sản xuất âm thanh liên tục với chi phí tăng độ trễ âm thanh.

### Làm thế nào để khởi động lại TTS server?

Chỉ cần làm các bước 1, 3 và 7 từ hướng dẫn cài đặt.

### Android??

Không chắc, nó không thể chạy các ứng dụng yêu cầu PyTorch nếu không có một số phép thuật huyền bí mà chúng tôi không hỗ trợ. Bạn có thể tự thử với rủi ro của riêng mình, nhưng sẽ không được hỗ trợ nếu bạn gặp bất kỳ sự cố nào.

Giải pháp tốt nhất của bạn là host TTS API trên PC của bạn trên mạng cục bộ, chỉ cần đừng quên chỉ định host và port để lắng nghe - xem [README](https://github.com/daswer123/xtts-api-server/blob/main/README.md).
