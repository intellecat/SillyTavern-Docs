---
route: /usage/api-connections/tabbyapi/
---

# TabbyAPI
Một ứng dụng dựa trên FastAPI cho phép tạo văn bản bằng LLM sử dụng backend Exllamav2, với hỗ trợ cho các mô hình Exl2, GPTQ và FP16.

* [GitHub](https://github.com/theroyallab/tabbyAPI)

### Hướng Dẫn Nhanh
1. Làm theo [hướng dẫn cài đặt](https://github.com/theroyallab/tabbyAPI/wiki/01.-Getting-Started) trên GitHub TabbyAPI chính thức.
2. [Tạo config.yml của bạn](https://github.com/theroyallab/tabbyAPI/wiki/02.-Server-options) để thiết lập đường dẫn mô hình, mô hình mặc định, độ dài sequence, v.v. Bạn có thể bỏ qua hầu hết (nếu không phải tất cả) các cài đặt này nếu bạn muốn.
3. Khởi chạy TabbyAPI. Nếu hoạt động, bạn sẽ thấy điều gì đó như thế này:

    ![TabbyAPI terminal](/static/tabby-terminal.png)

4. Trong Text Completion API trong SillyTavern, chọn TabbyAPI.
5. Sao chép API key của bạn từ terminal TabbyAPI vào `Tabby API key` và đảm bảo `API URL` của bạn là đúng (nó nên là `http://127.0.0.1:5000` theo mặc định).

Nếu bạn đã làm mọi thứ đúng, bạn sẽ thấy điều gì đó như thế này trong SillyTavern:

![TabbyAPI SillyTavern](/static/tabby-config.png)

Bây giờ bạn có thể trò chuyện bằng TabbyAPI!

### TabbyAPI Loader
Các nhà phát triển của TabbyAPI đã tạo một extension chính thức để tải/dỡ mô hình trực tiếp từ SillyTavern. Cài đặt rất đơn giản:
1. Trong SillyTavern, nhấp vào tab Extensions và điều hướng đến Download Extensions & Assets.
2. Sao chép `https://raw.githubusercontent.com/theroyallab/ST-repo/main/index.json` vào Assets URL và nhấp nút plug ở bên phải.
3. Bạn sẽ thấy điều gì đó như thế này. Nhấp nút download bên cạnh Tabby Loader.

    ![Tabby Loader](/static/tabby-assets.png)

4. Nếu cài đặt thành công, bạn sẽ thấy thông báo màu xanh lá cây bật lên ở đầu màn hình của bạn. Trong tab extensions, điều hướng đến TabbyAPI Loader và sao chép admin key của bạn từ terminal TabbyAPI vào Admin Key.
5. Nhấp nút refresh bên cạnh Model Select. Khi bạn nhấp vào hộp văn bản ngay bên dưới nó, bạn sẽ thấy tất cả các mô hình trong thư mục mô hình của bạn.

![Tabby Loader Extension](/static/tabby-loader.png)

Bây giờ bạn có thể tải và dỡ các mô hình của mình trực tiếp từ SillyTavern!

### Hỗ Trợ
Vẫn cần trợ giúp? Truy cập [TabbyAPI GitHub](https://github.com/theroyallab/tabbyAPI) để có liên kết đến máy chủ Discord chính thức của nhà phát triển và [đọc wiki](https://github.com/theroyallab/tabbyAPI/wiki/1.-Getting-Started).
