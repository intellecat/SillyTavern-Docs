---
order: 10
label: Windows
route: /vi/installation/windows/
---
# Cài đặt Windows

!!!warning
KHÔNG CÀI ĐẶT VÀO BẤT KỲ THƯ MỤC NÀO DO WINDOWS KIỂM SOÁT (Program Files, System32, v.v.).

KHÔNG CHẠY START.BAT VỚI QUYỀN ADMIN

CÀI ĐẶT TRÊN WINDOWS 7 LÀ KHÔNG THỂ VÌ NÓ KHÔNG THỂ CHẠY NODEJS 20
!!!

## Cài đặt qua Git

1. Cài đặt [NodeJS](https://nodejs.org/en) (phiên bản LTS mới nhất được khuyến nghị)
2. Cài đặt [Git for Windows](https://gitforwindows.org/)
3. Mở Windows Explorer (`Win+E`)
4. Duyệt đến hoặc Tạo một thư mục không được kiểm soát hoặc giám sát bởi Windows. (ví dụ: C:\MySpecialFolder\)
5. Mở Command Prompt bên trong thư mục đó bằng cách nhấp vào 'Address Bar' ở trên cùng, gõ `cmd`, và nhấn Enter.
6. Khi hộp đen (Command Prompt) xuất hiện, gõ MỘT trong các lệnh sau vào đó và nhấn Enter:

   - cho Nhánh Release: `git clone https://github.com/SillyTavern/SillyTavern -b release`
   - cho Nhánh Staging: `git clone https://github.com/SillyTavern/SillyTavern -b staging`

7. Khi mọi thứ được clone xong, nhấp đúp vào `Start.bat` để NodeJS cài đặt các yêu cầu của nó.
8. Sau đó máy chủ sẽ khởi động, và SillyTavern sẽ xuất hiện trong trình duyệt của bạn.

## Cài đặt qua SillyTavern Launcher

1.  Trên bàn phím của bạn: nhấn **`WINDOWS + R`** để mở hộp thoại Run. Sau đó, chạy lệnh sau để cài đặt git:
    ```shell
    cmd /c winget install -e --id Git.Git
    ```
2. Trên bàn phím của bạn: nhấn **`WINDOWS + E`** để mở File Explorer, sau đó điều hướng đến thư mục nơi bạn muốn cài đặt launcher. Khi ở trong thư mục mong muốn, gõ `cmd` vào thanh địa chỉ và nhấn enter. Sau đó, chạy lệnh sau:
   ```shell
    git clone https://github.com/SillyTavern/SillyTavern-Launcher.git && cd SillyTavern-Launcher && start installer.bat
    ```

## Cài đặt qua GitHub Desktop
(Điều này cho phép sử dụng git **chỉ** trong GitHub Desktop, nếu bạn muốn sử dụng `git` trên dòng lệnh, bạn cũng cần cài đặt [Git for Windows](https://gitforwindows.org/))

1. Cài đặt [NodeJS](https://nodejs.org/en) (phiên bản LTS mới nhất được khuyến nghị)
2. Cài đặt [GitHub Desktop](https://central.github.com/deployments/desktop/desktop/latest/win32)
3. Sau khi cài đặt GitHub Desktop, nhấp vào `Clone a repository from the internet....` (Lưu ý: Bạn **KHÔNG cần** tạo tài khoản GitHub cho bước này)

    ![image](/static/windows-1.png)

4. Trên menu, nhấp vào tab URL, nhập URL này `https://github.com/SillyTavern/SillyTavern`, và nhấp Clone. Bạn có thể thay đổi Local path để thay đổi nơi SillyTavern sẽ được tải xuống.

    ![image](/static/windows-2.png)

5. Để mở SillyTavern, sử dụng Windows Explorer để duyệt vào thư mục nơi bạn đã clone repository. Theo mặc định, repository sẽ được clone tại đây: `C:\Users\[Your Windows Username]\Documents\GitHub\SillyTavern`

6. Nhấp đúp vào tệp `start.bat`. (Lưu ý: phần `.bat` của tên tệp có thể bị ẩn bởi hệ điều hành của bạn, trong trường hợp đó, nó sẽ trông giống như một tệp có tên "`Start`". Đây là thứ bạn nhấp đúp để chạy SillyTavern)

    ![image](/static/windows-3.png)

7. Sau khi nhấp đúp, một cửa sổ console lệnh đen lớn sẽ mở ra và SillyTavern sẽ bắt đầu cài đặt những gì nó cần để hoạt động.

8. Sau quá trình cài đặt, nếu mọi thứ hoạt động, cửa sổ console lệnh sẽ trông như thế này và một tab SillyTavern sẽ mở trong trình duyệt của bạn:

    ![image](/static/windows-4.png)

9. Kết nối với bất kỳ [API được hỗ trợ](/Usage/API_Connections/index.md) nào và bắt đầu trò chuyện!
