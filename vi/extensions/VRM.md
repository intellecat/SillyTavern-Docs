---
route: /extensions/vrm/
---

# VRM

Hướng dẫn này sẽ hướng dẫn bạn qua quá trình thiết lập và tùy chỉnh extension VRM cho trải nghiệm SillyTavern của bạn. Extension này cho phép bạn sử dụng các mô hình VRM động cho nhân vật của bạn, cung cấp một phần tử tương tác và động cho nhân vật ảo của bạn.

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy chắc rằng bạn đã memenuhi các điều kiện tiên quyết sau:

1. **Lựa chọn Branch**: Hãy chắc rằng bạn đang sử dụng branch phiên bản mới nhất của SillyTavern để truy cập các tính năng và cập nhật mới nhất.

2. **Cài đặt Extension**: Cài đặt extension "VRM" từ menu "Download Extensions & Assets" trong Extensions panel (được biểu diễn bằng biểu tượng stacked blocks).

3. **Đặt Folder Mô hình**: Đặt các file VRM model (.vrm) của bạn vào thư mục `/data/<user-handle>/assets/vrm/model` và các file animation của bạn vào thư mục `/data/<user-handle>/assets/vrm/animation`. Các định dạng file animation hiện được hỗ trợ là .fbx và .bvh tương thích với các mô hình VRM. Điều này bao gồm bất kỳ animation nào bạn có thể nhận được từ Mixamo (https://www.mixamo.com/) và bất kỳ animation nào bạn có thể xuất từ các công cụ như XR Animator (https://github.com/ButzYung/SystemAnimatorOnline).

## Cài đặt Extension

Extension VRM cung cấp các cài đặt khác nhau để tùy chỉnh hành vi của mô hình động của bạn. Đây là các cài đặt chính:

![UI global settings](/static/extensions/vrm-global.png)

### Cài đặt Toàn cục

1. **Enabled**:
   - Bật hộp kiểm này để kích hoạt extension, cho phép mô hình VRM của bạn tương tác trong SillyTavern.
   - Bạn có thể tắt extension nếu bạn muốn chỉ sử dụng sprites bình thường.

2. **Look at camera**:
   - Bật hộp kiểm này để làm cho mắt mô hình VRM nhìn vào máy ảnh.

3. **Blink**:
   - Bật hộp kiểm này để làm cho mắt mô hình VRM nhấp nháy ở các khoảng thời gian ngẫu nhiên. Biểu hiện mô hình nên định nghĩa đúng tính chất weight nhấp nháy otherwize mô hình có thể nhấp nháy với các mắt đóng chẳng hạn, nếu điều đó xảy ra thì:
    - sửa chữa mô hình nếu bạn có file .vroid
    - không sử dụng biểu hiện mặt không chính xác đó
    - vô hiệu hóa nhấp nháy hoàn toàn bằng hộp kiểm này

4. **TTS Lip sync**
    - Bật hộp kiểm này để có chuyển động miệng VRM theo dõi âm thanh TTS của bạn khi nó được phát. Chỉ hoạt động với TTS có âm thanh được phát bởi Sillytavern chính nó như XTTS (không ở chế độ streaming). Nếu bị vô hiệu hóa, miệng sẽ được hoạt hình theo độ dài text tin nhắn khi một tin nhắn nhân vật mới được nhận.

5. **Auto-send Interaction**:
   - Bật hộp kiểm này để tự động kích hoạt các tương tác nhân vật khi bạn nhấp vào các vùng có các tin nhắn ánh xạ (tham khảo phần hit areas để biết chi tiết).

### Cài đặt Hiệu suất

1. **Body hitboxes**
    - Bật hộp kiểm này để kích hoạt phát hiện nhấp vào nhiều phần của mô hình VRM tùy thuộc vào mô hình các vùng sau có thể được phát hiện: head/chest/hands/groin/butt/legs/feets. Vị trí Hitboxes được tính toán ở mỗi khung hình và theo các animation của cơ thể, vô hiệu hóa tùy chọn này có thể cải thiện hiệu suất.

2. **Use model cache**
    - Bật hộp kiểm này để giữ trong bộ nhớ mô hình VRM khi chuyển đổi các mô hình, cho phép chuyển về mô hình trước đó nhanh hơn. Hữu ích nếu bạn sử dụng các mô hình khác nhau cho cùng một nhân vật để thay đổi trang phục hoặc hình thức chẳng hạn. Có thể ảnh hưởng đến hiệu suất.

3. **Use animation cache**
    - Bật hộp kiểm này để giữ trong bộ nhớ tất cả các animation được phát trong session. Tất cả animation được gán cho một mô hình cũng sẽ được tải lần đầu tiên mô hình xuất hiện. Sẽ tăng thời gian bạn tải mô hình lần đầu tiên nhưng tạo tất cả chuyển đổi animation tức thì. Có thể ảnh hưởng đến hiệu suất.

### Cài đặt Gỡ lỗi

1. **Show grid**
    - Bật hộp kiểm này để hình dung lưới 3d, hộp kéo mô hình và body hitboxes.

2. **Nút Reload**
    - Nhấp nút này để tải lại cảnh 3d, xóa bộ nhớ cache và tất cả các mô hình VRM. Sử dụng nó nếu một số lỗi xảy ra hoặc nếu bộ nhớ cache bắt đầu ảnh hưởng đến hiệu suất.

### Cài đặt Cảnh

![UI scene settings](/static/extensions/vrm-scene.png)

1. **Light Color**
    - Đặt màu sắc của ánh sáng trong cảnh 3d. Nhấp vào nút reset để đặt lại về màu trắng mặc định. Tùy thuộc vào trình duyệt của bạn, bạn có thể sử dụng một bộ chọn màu, ví dụ bạn có thể chọn màu của hình ảnh nền của bạn để thêm nhiều sự nhập vai hơn.

2. **Light intensity**
    - Đặt cường độ ánh sáng tính bằng phần trăm bằng thanh trượt. Nhấp vào nút reset để đặt lại về giá trị mặc định 100%. Mô hình VRM có thể phản ứng khác nhau với ánh sáng tùy thuộc vào các shader được nướng vào mô hình, chơi với giá trị và xem nó sẽ như thế nào.

![UI model settings](/static/extensions/vrm-model.png)

## Lựa chọn Nhân vật

Các cài đặt này cho phép bạn quản lý các nhân vật và gán các mô hình VRM cho chúng.

1. **Nút Refresh**:
   - Nhấp vào nút refresh để cập nhật danh sách các nhân vật trong chat hiện tại.

2. **Chọn Nhân vật**:
   - Sử dụng danh sách drop-down để chọn một nhân vật để gán một mô hình VRM cho.

3. **Nút Remove**:
   - Nhấp nút này để xóa mô hình được gán cho một nhân vật.

## Lựa chọn Mô hình

1. **Nút Refresh**:
   - Nhấp vào nút refresh nếu mô hình VRM của bạn không xuất hiện trong danh sách.

2. **Chọn Mô hình**:
   - Chọn một mô hình từ danh sách để gán nó cho nhân vật được chọn.
   - Mô hình phải được đặt ở thư mục `/data/<user-handle>/assets/vrm/model`.

3. **Nút Reset**
    - Nhấp nút này để reset cài đặt mô hình về mặc định. Nếu bạn có các file animation tương ứng với giá trị mặc định, chúng sẽ được tự động ánh xạ. Xem ánh xạ đặt tên ở cuối README này.

## Cài đặt Mô hình

1. **Model Scale**:
   - Sử dụng thanh trượt để điều chỉnh kích thước của mô hình, làm cho nó lớn hơn hoặc nhỏ hơn.

2. **Model Center X/Y Offset**:
   - Sử dụng các thanh trượt này để thay đổi vị trí ngang/dọc của mô hình liên quan đến trung tâm cửa sổ.

3. **Model X/Y Rotation**
    - Sử dụng các thanh trượt này để thay đổi xoay ngang/dọc của mô hình liên quan đến mô hình hips.

### Nhận xét
    - Các cài đặt được lưu cho mỗi mô hình không phải cho mỗi nhân vật và thực hiện trong các chats khác nhau.
    - Nếu bạn muốn sử dụng cùng một mô hình cho hai nhân vật khác nhau với các cài đặt khác nhau, hãy tạo một bản sao của file .vrm.
    - Bạn cũng có thể kéo mô hình bằng chuột của bạn, và các cài đặt này sẽ được cập nhật và lưu. Nhấp trái và giữ để kéo mô hình xung quanh màn hình. Nhấp chuột giữa và giữ để xoay mô hình hoặc sử dụng shift-left click. Sử dụng con lăn chuột với con trỏ trên mô hình để phóng to hoặc thu nhỏ nó hoặc sử dụng ctrl+left click.
    - Sử dụng các cài đặt UI này để đưa mô hình của bạn trở lại màn hình nếu bạn nào đó làm nó ra khỏi chế độ xem. Cũng kiểm tra hộp kiểm "Show frame" để rõ ràng thấy nơi bạn có thể nhấp để kéo mô hình.

![UI hitboxes settings](/static/extensions/vrm-hitboxes.png)

## Ánh xạ Hitboxes

    - Tùy thuộc vào định nghĩa xương mô hình, một số vùng hitboxes có thể được tạo, chúng sẽ được liệt kê trong phần này của ui, và bạn có thể gán một biểu hiện/animation/tin nhắn cho mỗi vùng sẽ kích hoạt khi bạn nhấp vào vùng.

![UI classify settings](/static/extensions/vrm-classify.png)

## Ánh xạ Biểu hiện Phân loại

1. **Yêu cầu**
    - Yêu cầu sử dụng extension classify expression; nếu không, nó sẽ quay lại animation mặc định.

2. **Ánh xạ**
    - Cho mỗi cảm xúc được phát hiện bởi extension classify, bạn có thể gán một biểu hiện/motion/tin nhắn. Tin nhắn có thể chứa các lệnh.

## Lệnh

1. **/vrmlightcolor**
    - đặt màu sắc ánh sáng
    - arguments: color
    - ví dụ: "/vrmlightcolor white" hoặc "/vrmlightcolor purple".
2. **/vrmlightintensity**
    - đặt cường độ ánh sáng tính bằng phần trăm
    - arguments: intensity
    - ví dụ: "/vrmlightintensity 0" hoặc "/vrmlightintensity 100
3. **/vrmmodel**
    - gán mô hình vrm cho nhân vật
    - arguments: character, model
    - ví dụ: "/vrmmodel Seraphina.vrm" trong solo chat hoặc "/vrmmodel character=Seraphina model=Seraphina.vrm" trong group chat
4. **/vrmexpression**
    - thay đổi biểu hiện của mô hình
    - arguments: character, expression
    - ví dụ: "/vrmexpression happy" trong solo chat hoặc "/vrmexpression character=Seraphina expression=happy" trong group chat

5. **/vrmmotion**
    - thay đổi animation của mô hình
    - arguments: character, motion, loop, random
    - "/vrmmotion idle" hoặc "/vrmmotion character=Seraphina motion=idle loop=true random=false"

## Ánh xạ mặc định Animations
Nếu các file animation của bạn được đặt tên theo cách sau, chúng sẽ được ánh xạ tự động khi reset cài đặt mô hình. Ví dụ, các file được đặt tên "assets/vrm/animation/neutral.bvh" và "assets/vrm/animation/neutral1.fbx" sẽ được tự động ánh xạ làm một nhóm cho animation default và neutral được phân loại. Tương tự như vậy cho các hitboxes.

    // Fallback
    "default": "assets/vrm/animation/neutral",

    // Classify class
    "admiration": "assets/vrm/animation/admiration",
    "amusement": "assets/vrm/animation/amusement",
    "anger": "assets/vrm/animation/anger",
    "annoyance": "assets/vrm/animation/annoyance",
    "approval": "assets/vrm/animation/approval",
    "caring": "assets/vrm/animation/caring",
    "confusion": "assets/vrm/animation/confusion",
    "curiosity": "assets/vrm/animation/curiosity",
    "desire": "assets/vrm/animation/desire",
    "disappointment": "assets/vrm/animation/disappointment",
    "disapproval": "assets/vrm/animation/disapproval",
    "disgust": "assets/vrm/animation/disgust",
    "embarrassment": "assets/vrm/animation/embarrassment",
    "excitement": "assets/vrm/animation/excitement",
    "fear": "assets/vrm/animation/fear",
    "gratitude": "assets/vrm/animation/gratitude",
    "grief": "assets/vrm/animation/grief",
    "joy": "assets/vrm/animation/joy",
    "love": "assets/vrm/animation/love",
    "nervousness": "assets/vrm/animation/nervousness",
    "neutral": "assets/vrm/animation/neutral",
    "optimism": "assets/vrm/animation/optimism",
    "pride": "assets/vrm/animation/pride",
    "realization": "assets/vrm/animation/realization",
    "relief": "assets/vrm/animation/relief",
    "remorse": "assets/vrm/animation/remorse",
    "sadness": "assets/vrm/animation/sadness",
    "surprise": "assets/vrm/animation/surprise",

    // Hitboxes
    "head": "assets/vrm/animation/hitarea_head",
    "chest": "assets/vrm/animation/hitarea_chest",
    "groin": "assets/vrm/animation/hitarea_groin",
    "butt": "assets/vrm/animation/hitarea_butt",
    "leftHand": "assets/vrm/animation/hitarea_hands",
    "rightHand": "assets/vrm/animation/hitarea_hands",
    "leftLeg": "assets/vrm/animation/hitarea_leg",
    "rightLeg": "assets/vrm/animation/hitarea_leg",
    "rightFoot": "assets/vrm/animation/hitarea_foot",
    "leftFoot": "assets/vrm/animation/hitarea_foot"

Cảm ơn vì đã làm theo hướng dẫn này! Trải nghiệm SillyTavern của bạn hiện được làm giàu với các mô hình 3D động và tương tác.

## Nhận xét
    - Các mô hình VRM được tải bởi extension này là các file .vrm không phải các file .vroid.
    - Các file Animation nên tương thích VRM, bạn có thể sử dụng một công cụ như XR animation (https://github.com/ButzYung/SystemAnimatorOnline) để chuyển đổi file fbx/bvh animation.
    - Bạn có thể tạo các nhóm animation bằng cách có các file có cùng tên kết thúc bằng các số khác nhau, ví dụ: "idle1.bvh", "idle2.bhv", "idle3.bvh" sẽ được coi là một nhóm "idle" và khi được chọn trong một ánh xạ một số sẽ được phát khi kích hoạt, có thể được sử dụng để thêm nhiều loại animations.
    - Bạn có thể nhận được các animation được curation từ repository này: https://github.com/test157t/VRM-Animations-Pack-For-Silly-Tavern
    - Nitral có một số video hướng dẫn về cách sử dụng extension và repo animation: https://www.youtube.com/@nitralai
