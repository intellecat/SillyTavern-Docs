---
icon: gear
label: Cài đặt cục bộ
route: /vi/extensions/extras/installation/
---

# Cài đặt Extras

Trang này chứa hướng dẫn cài đặt SillyTavern Extras trên thiết bị cục bộ của bạn.

!!! Đã ngừng phát triển
Dự án Extras đã ngừng phát triển vào tháng 4 năm 2024 và sẽ không nhận được bất kỳ cập nhật hoặc module mới nào. Phần lớn các module hiện đã có sẵn trong ứng dụng SillyTavern chính. Bạn vẫn có thể cài đặt và sử dụng nhưng đừng mong đợi nhận được hỗ trợ ngay lập tức nếu gặp bất kỳ vấn đề nào.
!!!

Cài đặt cục bộ Extras có thể khó khăn hoặc không khả thi trên hệ điều hành của bạn (đặc biệt là Termux).

## Sử dụng [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)

* Dễ dàng thiết lập
* Miễn phí sử dụng
* Không yêu cầu tín dụng GPU Colab (sử dụng tùy chọn `use_cpu`)
* Xem [Trang Hướng dẫn Colab](/extensions/Extras/Installation.md#running-extras-in-colab) để biết chi tiết.

### Chạy Extras trong Colab

* Mở [Official Extras Colab](https://colab.research.google.com/github/SillyTavern/SillyTavern/blob/release/colab/GPU.ipynb)
* Chọn các tùy chọn "Extra" mong muốn
* chọn `use_cpu` để chạy Extras mà không cần tín dụng GPU
  * điều này sẽ làm Stable Diffusion chậm hơn, nhưng mọi thứ khác sẽ chạy bình thường
* Không bắt buộc, nhưng được khuyến nghị: chọn tùy chọn `secure` để tạo API key bảo vệ instance chia sẻ của bạn.
* Nhấp vào nút Start ở bên trái (trông giống như nút 'play' hình tam giác)
* Chờ cho đến khi nó tải xong mọi thứ
* Tìm liên kết `trycloudflare.com` ở cuối output. Bỏ qua liên kết localhost, nó sẽ không hoạt động (chúng tôi đã thử!).
* Nó sẽ bắt đầu với dòng chữ `Running on`
* Sao chép liên kết API URL được liệt kê dưới dòng đó. (**ĐỪNG sao chép URL 'localhost', hãy sử dụng URL còn lại**)
* Khởi động SillyTavern với hỗ trợ extensions: (đặt `enableExtensions` thành `true` trong `config.yaml` của bạn nếu cần)
* Điều hướng đến menu Extensions của SillyTavern (nhấp vào biểu tượng 'stacked blocks' ở đầu trang).
* Dán API URL vào ô ở trên cùng. (**KHÔNG phải ô API Key**)
* Nếu bạn KHÔNG kích hoạt tùy chọn `secure`, hãy đảm bảo ô API Key hoàn toàn trống khi sử dụng official colab.
* Nếu bạn đã kích hoạt tùy chọn `secure`, hãy dán API key được tạo vào ô API Key.
* API key sẽ xuất hiện trong console output của colab, ví dụ: `Your API key is fee2f3f559`
* Nhấp "Connect"

---

## Phương pháp cài đặt cục bộ

### MiniConda (khuyến nghị)

Phương pháp này được khuyến nghị vì Conda tạo một 'virtual environment' cho các gói yêu cầu của Extras tồn tại bên trong, do đó chúng không ảnh hưởng đến thiết lập Python toàn hệ thống của bạn.

1. Cài đặt [Miniconda](https://docs.conda.io/en/latest/miniconda.html)

    _(Quan trọng!) Đọc [cách sử dụng Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/getting-started.html)_

2. Cài đặt [git](https://git-scm.com/downloads)

    _(Những người đã cài đặt SillyTavern bằng git từ đầu có thể bỏ qua bước này!)_

    Sau khi bạn đã cài đặt cả hai...

    Gõ/dán các lệnh dưới đây `MỘT LỆNH MỘT LẦN` TRONG `CỬA SỔ CONDA COMMAND PROMPT` và nhấn `Enter` sau mỗi lệnh.

3. Tạo một Conda environment mới (gọi nó là `extras`):

    `conda create -n extras`

4. Kích hoạt environment mới

    `conda activate extras` (bạn sẽ thấy `(extras)` xuất hiện ở bên trái dấu nhắc lệnh của bạn)

5. Cài đặt các gói hệ thống cần thiết (sẽ mất một chút thời gian)

    `conda install python=3.11 git`

6. Clone GitHub repo của Extras

    `git clone https://github.com/SillyTavern/SillyTavern-extras`

7. Điều hướng đến repo Extras đã clone của bạn

    `cd SillyTavern-extras`

8. Cài đặt các yêu cầu của Extras bằng cách sử dụng **một** trong các lệnh sau (sẽ mất thời gian, một lần nữa):

   * `pip install -r requirements.txt` - cho các tính năng cơ bản
   * `pip install -r requirements-rvc.txt` - cho real-time voice cloning
   * `pip install -r requirements-coqui.txt` - cho Coqui TTS (không khuyến nghị)

    Xem trang [Vấn đề thường gặp](/extensions/Extras/Installation.md#extras-install-common-problems) nếu bạn gặp lỗi ở bước này!

9. Xem bên dưới 'Chạy Extras sau khi cài đặt'

---

### Cài đặt toàn hệ thống

Cách này dễ dàng hơn, nhưng sẽ ảnh hưởng đến cài đặt Python toàn hệ thống của bạn.

Điều này có thể gây xung đột nếu bạn làm việc với nhiều chương trình Python có yêu cầu khác nhau.

Nếu đây là lần đầu tiên bạn tiếp xúc với bất cứ thứ gì liên quan đến Python, điều đó không nên là vấn đề.

1. Cài đặt Python 3.11: <https://www.python.org/downloads/release/python-3115/>
2. Cài đặt git: <https://git-scm.com/downloads>
3. Mở cửa sổ dấu nhắc lệnh và đi đến thư mục mà bạn có quyền truy cập đầy đủ.
4. Clone repo: `git clone https://github.com/SillyTavern/SillyTavern-extras`, nhấn Enter.
5. Sau khi clone hoàn tất, gõ `cd SillyTavern-extras`, nhấn Enter.
6. Gõ `python -m pip install -r requirements.txt`
7. Xem bên dưới 'Chạy Extras sau khi cài đặt'

---

## Chạy Extras sau khi cài đặt

### Xác nhận extensions đã được kích hoạt

1. Mở file có tên `config.yaml` trong trình soạn thảo văn bản. File nằm trong thư mục cài đặt cơ bản của ST.
2. Tìm dòng có nội dung `enableExtensions`.
3. Đảm bảo dòng đó có `true`, chứ không phải `false`.

### Quyết định module nào sẽ sử dụng

(Chỉ cần thực hiện một lần)

* Extras luôn được khởi động bằng dòng lệnh Python.
* `python server.py` là tối thiểu, nhưng nó không kích hoạt bất kỳ module hữu ích nào.
* để kích hoạt các module, bạn phải sử dụng bộ sửa đổi `--enable-modules=`, với danh sách các tên module được phân tách bằng dấu phẩy

Ví dụ: `python server.py --enable-modules=caption,summarize,classify`

Điều này sẽ kích hoạt Image Captioning, Chat Summary và live updating Character Expressions.

Dưới đây là bảng mô tả từng module.

| Tên          | Mô tả                                                               |
|--------------|---------------------------------------------------------------------|
| `caption`    | Image captioning                                                    |
| `summarize`  | Text summarization                                                  |
| `classify`   | Text sentiment classification                                       |
| `sd`         | Stable Diffusion image generation                                   |
| `silero-tts` | [Silero TTS server](https://github.com/ouoertheo/silero-api-server) |
| `edge-tts`   | [Microsoft Edge TTS client](https://github.com/rany2/edge-tts)      |
| `chromadb`   | Vector storage server                                               |
| `coqui-tts`  | Coqui TTS                                                           |
| `rvc`        | Real-time voice cloning                                             |

* Quyết định module nào bạn muốn thêm vào dòng lệnh Python của mình.
* Chúng sẽ được sử dụng trong bước tiếp theo.

**LƯU Ý: Không được có `khoảng trắng nào trong danh sách module của lệnh Python của bạn!`**

### Khởi động Extras Server

Trong khi vẫn ở cửa sổ dấu nhắc lệnh bên trong thư mục cài đặt Extras...

1. Đảm bảo conda environment của bạn đang hoạt động (nếu bạn sử dụng phương pháp cài đặt Conda)
2. Gõ `activate extras` nếu environment chưa hoạt động.
3. Gõ `python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE`
4. Extras server sẽ tải.
5. Sau một lúc, nó sẽ hiển thị cho bạn một URL ở cuối. Đối với cài đặt cục bộ, mặc định là `http://localhost:5100`.
6. Sao chép API URL.

### Kết nối ST với Extras server

1. Khởi động server SillyTavern của bạn và xem giao diện SillyTavern trong trình duyệt.
2. Mở panel Extensions (thông qua biểu tượng 'Stacked Blocks' ở đầu trang)
3. Dán API URL vào ô nhập.
4. Nhấp `Connect`.

Để chạy lại Extras, chỉ cần kích hoạt environment và chạy các lệnh này trong dấu nhắc lệnh.

`conda activate extras`, Nhấn Enter.
`python server.py`, Nhấn Enter.

Đảm bảo thêm các tùy chọn bổ sung cho server.py (xem bên dưới) mà thiết lập của bạn yêu cầu.

## Tạo file .bat để khởi động dễ dàng

Điều này là tùy chọn và chỉ áp dụng cho Windows, nhưng có thể thực hiện tương tự trên MacOS.

1. Xem Desktop Windows của bạn
2. Nhấp chuột phải, chọn `New`, và sau đó nhấp `Text Document`
3. Một file mới sẽ xuất hiện trên Desktop của bạn, yêu cầu tên.
4. Đặt tên file là `STExtras.txt`
5. Mở file mới tạo trong trình soạn thảo văn bản.
6. Dán đoạn code sau vào:

    ```
    cd C:\_your_\_full_\_Extras_\_folder_\_path_\
    call conda activate extras
    python server.py --enable-modules=YOUR,SELECTED,MODULE,LIST,HERE,WITH,NO,SPACES
    call conda deactivate
    pause
    ```

7. Thay thế đường dẫn thư mục giữ chỗ bằng đường dẫn thư mục cài đặt Extras thực tế của bạn.
8. Thay thế dòng lệnh python bằng dòng lệnh thực tế của bạn
9. Lưu file với tên mới `STExtras.bat` (Sử dụng `File` >> `Save As` trong hầu hết các trình soạn thảo văn bản)

Bây giờ bạn có thể chỉ cần nhấp đúp vào file .bat này để dễ dàng khởi động Extras.

Nếu bạn muốn thay đổi danh sách module (hoặc bất kỳ bộ sửa đổi dòng lệnh nào khác cho extras server), chỉ cần chỉnh sửa lệnh python bên trong file .bat.

## Vấn đề thường gặp khi cài đặt Extras

Phần này liệt kê các câu hỏi và vấn đề thường gặp trong khi cài đặt SillyTavern Extras.

### Lỗi: Could not import the 'talkinghead' module on Linux

Nó yêu cầu cài đặt một gói bổ sung vì nó không được cài đặt tự động do không tương thích với Colab. Chạy lệnh này sau khi bạn cài đặt các yêu cầu khác:

`pip install wxpython`

### Extras server không thể kết nối với AUTOMATIC1111's Stable Diffusion Web UI

> Could not connect to remote SD backend at <http://127.0.0.1:7860>! Disabling SD module...

**Đảm bảo webui-user.bat mà bạn khởi động Stable Diffusion chứa tùy chọn dòng lệnh --api trong biến COMMANDLINE_ARGS.**

Tìm và thay thế dòng đó trong "webui-user.bat" của bạn: `set COMMANDLINE_ARGS=--api`

![Cách nó nên trông](/static/extensions/sd-user.png)

Nếu chế độ API bị tắt cho SD Web UI, Extras server sẽ không thể kết nối và bạn sẽ không thể tạo hình ảnh!

#### Vẫn không hoạt động?

Đảm bảo rằng bạn khởi động mọi thứ theo đúng thứ tự, chờ mỗi chương trình tải xong trước khi tiến hành bước tiếp theo:

1. Stable Diffusion Web UI
2. SillyTavern Extras
3. SillyTavern

Extras server không thể kết nối lại với Stable Diffusion API nếu nó được tải sau.

### Lỗi xây dựng wheel hnswlib khi cài đặt ChromaDB

> ERROR: Could not build wheels for hnswlib, which is required to install pyproject.toml-based projects

Trước khi cài đặt module ChromaDB, bạn phải thực hiện `một trong những điều sau`:

* Cài đặt Visual C++ build tools: <https://visualstudio.microsoft.com/visual-cpp-build-tools/>
* Cài đặt gói `hnswlib` với conda: `conda install -c conda-forge hnswlib`

---

### Lỗi khi cài đặt yêu cầu Python trên Mac

> ERROR: No matching distribution found for torch==2.0.0+cu117

Mac không hỗ trợ CUDA, vì vậy các gói torch nên được cài đặt không có hỗ trợ CUDA.

Cài đặt các yêu cầu bằng file `requirements-silicon.txt` thay thế.

---

### Thiếu modules?

* Bạn phải chỉ định danh sách tên module trong dòng lệnh Python của mình, với bộ sửa đổi `--enable-modules`.
* Xem phần [Modules](/extensions/Extras/Installation.md#decide-which-module-to-use).

---

### Ô API Key dùng để làm gì?

* Ô API Key trong panel Extensions của SillyTavern chỉ được sử dụng khi bạn có:
  * tạo một file văn bản có tên `api_key.txt` trong thư mục cài đặt Extras của bạn, chứa 'mật khẩu' Extras mà bạn chọn.
  * khởi động extras với đối số dòng lệnh `--secure`.
* Điều này làm cho Extras API 'được khóa bằng mật khẩu', vì vậy chỉ những người dùng có key đó trong ô API Key mới có thể truy cập nó.
* Điều này chủ yếu hữu ích cho những người muốn triển khai công khai Extras của riêng họ (colab, v.v.).
* Người dùng chạy Extras trên PC của riêng họ để sử dụng cá nhân không nên gõ gì vào ô API Key.

### Còn về mobile/Android/Termux thì sao? 🤔

* Có một số người trong cộng đồng thành công khi chạy Extras trên điện thoại của họ thông qua Ubuntu trên Termux.
* Tuy nhiên, Extras không được tạo ra với hỗ trợ di động trong tâm trí.
* Không có hỗ trợ nào được cung cấp cho những người chạy Extras trên thiết bị Android của họ.
* Hãy hướng tất cả câu hỏi của bạn đến người tạo hướng dẫn được liên kết dưới đây thay thế.

#### ❗ Điều này KHÔNG ĐƯỢC HỖ TRỢ

<https://rentry.org/STAI-Termux#downloading-and-running-tai-extras>
