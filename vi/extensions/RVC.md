---
route: /extensions/rvc/
---

# Retrieval-based Voice Conversion (RVC)

Hướng dẫn này sẽ hướng dẫn bạn cách sử dụng RVC, một kỹ thuật cho phép chuyển giao các đặc tính giọng nói từ một clip âm thanh này sang clip âm thanh khác, giúp giọng nói có thể nói ở các tones và style khác nhau.

Bạn đã bao giờ thích những video "Presidents Play X" nổi tiếng chưa? Chúng được tạo bằng RVC. Với extension RVC, bạn có thể làm cho các nhân vật SillyTavern nói bằng bất kỳ giọng nào bạn muốn, cho dù là anime, phim ảnh hay thậm chí là giọng nói độc đáo của chính bạn.

RVC KHÔNG phải là TTS: nó giống như speech-to-speech hơn. Nó nhận một clip âm thanh làm đầu vào. Ở phía sau, điều mà RVC làm là làm việc song song với extension TTS của SillyTavern: nó chờ TTS tạo ra một file âm thanh (mà TTS sẽ làm bất kể bạn có sử dụng RVC hay không), sau đó RVC sẽ thực hiện một lần pass thứ hai lấy file âm thanh TTS và biến đổi nó thành giọng nói được sao chép từ cấu hình RVC của bạn.

## RVC Setup

SillyTavern's RVC hỗ trợ nhiều nguồn API thực hiện chuyển đổi âm thanh:

* [rvc-python](https://github.com/daswer123/rvc-python)
* [SillyTavern Extras](https://github.com/SillyTavern/SillyTavern-Extras) (không được dùng nữa)

### Các điều kiện tiên quyết chung

Trước khi bắt đầu, hãy chắc rằng bạn đã memenuhi các điều kiện tiên quyết sau.

#### ffmpeg

Hãy chắc rằng bạn có binary `ffmpeg` trong biến môi trường PATH của bạn. Công cụ này được sử dụng để chuyển đổi âm thanh đến.

**Windows**:

* Sử dụng Toolbox trong script SillyTavern Launcher để cài đặt ffmpeg tự động: <https://github.com/SillyTavern/SillyTavern-Launcher>
* Hoặc tải bản dựng tại đây: <https://www.gyan.dev/ffmpeg/builds/>
* Cách sửa đổi biến PATH: <https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/>
* Để kiểm tra xem bạn đã làm đúng chưa, hãy mở command prompt và chạy ```ffmpeg```. Nó sẽ in phiên bản ffmpeg và thông tin.

**Linux**:

Cài đặt ffmpeg bằng trình quản lý package của bạn.

```shell
# Debian/Ubuntu
sudo apt install ffmpeg
# Arch Linux
sudo pacman -S ffmpeg
# Fedora
sudo dnf install ffmpeg
```

**macOS**:

Cài đặt ffmpeg bằng [Homebrew](https://brew.sh/):

```shell
brew install ffmpeg
```

#### Hãy chắc rằng TTS được bật và hoạt động

RVC phụ thuộc vào TTS, bạn cần bật một extension TTS. TTS của bạn phải hoạt động đúng và kể lại các cuộc trò chuyện của bạn trước khi bạn cố gắng thêm RVC vào!

Xin lưu ý rằng:

* Engin TTS hệ thống không hỗ trợ chuyển đổi giọng nói.
* TTS truyền phát sẽ chờ luồng âm thanh kết thúc trước khi chuyển đổi.

#### Cài đặt extension

Cài đặt extension "RVC" từ menu "Download Extensions & Assets" trong Extensions panel (icon stacked blocks).

#### Bật RVC trong SillyTavern*

Trong SillyTavern, điều hướng đến **Extensions** > **RVC** và bật nó.

#### Chọn nguồn

Trong cài đặt extension, chọn một RVC source để sử dụng. Sau đó tiến hành các hướng dẫn cài đặt cụ thể của nguồn.

### rvc-python Setup

#### 1. Cài đặt package

Làm theo hướng dẫn cài đặt từ trang GitHub: [rvc-python Installation](https://github.com/daswer123/rvc-python?tab=readme-ov-file#installation). Nên làm theo hướng dẫn cài đặt CUDA nếu bạn có GPU Nvidia.

Nếu bạn gặp sự cố khi cài đặt trên Windows (ví dụ: bước building fairseq bị lỗi), hãy chắc rằng phần mềm sau được cài đặt trên PC của bạn:

* [Windows 10 SDK](https://developer.microsoft.com/en-us/windows/downloads/windows-sdk/)
* [Visual Studio Build Tools 2022](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2022)

#### 2. Chuẩn bị các model

Tạo một thư mục để lưu trữ RVC models. Theo mặc định, nó được đặt tên là `rvc_models` và được chọn từ thư mục hiện tại của bạn khi khởi động server. Mỗi model là một subfolder (tên của nó sẽ hiển thị trong UI) nên chứa các file `.pth` (bắt buộc) và `.index` (tùy chọn).

Đọc thêm: [rvc-python Model Management](https://github.com/daswer123/rvc-python?tab=readme-ov-file#model-management)

#### 3. Khởi động API server

Khởi động API server bằng cách chạy lệnh sau:

```shell
python -m rvc_python api -p 5050 -l -md models_path
```

**Arguments:**

* `5050` - đặt một listening port cho server. Thay đổi nếu bạn muốn host trên một port khác.
* `models_path` - đặt một đường dẫn cho models. Xóa nếu bạn muốn sử dụng thư mục mặc định `rvc_models`.
* `-l` - đặt server để lắng nghe trên tất cả các network interfaces. Xóa để chỉ lắng nghe trên localhost.

#### 4. Kết nối tới server

* Trong cài đặt RVC extension, đặt một **rvc-python API URL** thích hợp. Theo mặc định, nó sẽ là `http://localhost:5050`.
* Chọn checkbox **Use CUDA** nếu bạn đã cài đặt rvc-python để hỗ trợ CUDA acceleration.
* Nhấn "Refresh" để tải danh sách các giọng nói khả dụng.

#### 5. Cấu hình voice map

**Voice map định nghĩa cài đặt chuyển đổi giọng nói cho mỗi nhân vật hoặc user persona.**

* Để thiết lập voice map, chọn tên nhân vật hoặc persona của bạn từ dropdown "Character", sau đó chọn RVC "Voice", sau đó nhấp Apply.
* Tùy chọn, bạn cũng có thể cấu hình các cài đặt liên quan khác như pitch correction hoặc filtering.
* Nếu bạn đã làm mọi thứ đúng cách, vùng gỡ lỗi Voice Map sẽ hiển thị một cái gì đó như 'Betty:MyVoice(rvpme)'.

### SillyTavern Extras Setup

#### 1. Chuẩn bị RVC Model Files

* Trong một file browser, điều hướng tới: `\SillyTavern-extras\data\models\rvc`.
* Tạo một subfolder như 'Betty' và đặt các file `.pth` và `.index` vào trong nó. (Gợi ý: bạn có thể tải xuống các file giọng nói từ https://voice-models.com, hãy chắc rằng tên giọng nói nói rằng đó là RVPME.)

#### 2. Cài đặt Requirements

Cài đặt các yêu cầu cần thiết bằng cách sử dụng lệnh:

```shell
pip install -r requirements-rvc.txt`
```

#### 3. Chạy SillyTavern-extras với RVC được bật

Khởi động SillyTavern-extras với module RVC được bật. Ví dụ này giả sử bạn đã sử dụng Edge TTS được cài đặt sẵn với SillyTavern-extras:

```shell
python server.py --enable-modules=rvc,edge-tts
```

Tùy chọn, bạn có thể muốn chạy RVC trên GPU của bạn nếu bạn có một GPU khả năng, bằng cách thêm ```--cuda``` vào lệnh khởi động. Dựa trên một bài kiểm tra nhanh, việc sử dụng VRAM là 3.4GB cho kể lại 50 tokens (~36 từ), và 7.6GB cho 200 tokens (~150 từ).

#### 4. Thiết lập Voice Mapping

Tạo một Voice map cho RVC. Đặt Character của bạn thành tên nhân vật SillyTavern mong muốn của bạn, và đặt Voice thành folder RVC bạn đã tạo ở bước 1, sau đó nhấp Apply. Nếu bạn đã làm mọi thứ đúng cách, Voice Map sẽ hiển thị một cái gì đó như 'Betty:MyVoice(rvpme)'.

#### 5. Chọn Pitch Extraction

* Chọn "rmvpe" làm phương pháp pitch extraction.
* Nếu bạn gặp sự cố với "rmvpe" hãy thử các phương pháp khác (ví dụ: "harvest" hoặc "torchcrepe").

#### 6. (Tùy chọn) Cấu hình RVC để lưu các thế hệ của bạn vào file

Nếu bạn muốn lưu âm thanh RVC được tạo ra cho mục đích kiểm tra hoặc khắc phục sự cố, hãy thêm ```--rvc-save-file``` vào lệnh khởi động của bạn. Điều này sẽ lưu thế hệ cuối cùng dưới `SillyTavern-extras/data/tmp/rvc_output.wav`:

```shell
python server.py --enable-modules=rvc,edge-tts --rvc-save-file
```

#### Expression-Based Dynamic Voice

##### 1. Cấu hình RVC Models

Trong folder RVC model của bạn, có các file `.pth` và `.index` riêng biệt cho mỗi biểu hiện được phân loại (ví dụ: anger, fear, joy, love, sadness, surprise).

##### 2. Bật Modules

Bật cả module RVC và classify:

```shell
python server.py --enable-modules=rvc,classify
```

##### 3. Sử dụng RVC Module

Phần còn lại của cài đặt tương tự như sử dụng module RVC một mình (như giải thích ở trên).

## Huấn luyện RVC Model của riêng bạn

### Sử dụng RVC Easy Menu bởi Deffcolony (Chỉ Windows)

Tự động cài đặt và khởi chạy Mangio-RVC: https://github.com/deffcolony/rvc-easy-menu

#### 1. Clone Repository

Clone repository vào vị trí bạn mong muốn:

```shell
git clone https://github.com/deffcolony/rvc-easy-menu.git
```

#### 2. Khởi chạy RVC-Launcher.bat

* Mở file `RVC-Launcher.bat`.
* Chọn tùy chọn 1 để cài đặt RVC.

#### 3. Hoàn thành Cài đặt

Khi được nhắc, cài đặt các package và dependiencies được yêu cầu.

#### 4. Mở WebUI cho Voice Training

Sau khi cài đặt, chọn tùy chọn 2 để mở WebUI cho voice training.

### Mangio-RVC: Training a Voice Model

***Dataset Preparation***:

**1. Chuẩn bị Audio**:

* Đặt audio bạn muốn huấn luyện vào folder `datasets`.
* Đảm bảo audio không có tiếng ồn nền – chỉ cần giọng nói thô.
* Audio dài hơn sẽ tạo ra chất lượng đầu ra tốt hơn.

***WebUI Training***:

**1. Truy cập Training Tab**:

* Nhấp vào training tab trong WebUI.

**2. Cấu hình Experiment**:

* Nhập tên experiment (ví dụ: `my-epic-voice-model`).
* Đặt phiên bản thành v2.

**3. Xử lý Dữ liệu và Trích xuất Tính năng**:

* Nhấp "Process data" và "Feature extraction".
* Đặt "Save frequency" thành 50.

**4. Training Parameters**:

* Đặt "Total training epochs" thành 300.
* Nhấp "Train feature index" và "Train model".
