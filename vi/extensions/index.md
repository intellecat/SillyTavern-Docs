---
label: Extensions
icon: plug
expanded: true
order: 35
route: /extensions/
---

# Extensions

SillyTavern đi kèm với nhiều extensions có thể được bật hoặc tắt trong Extensions panel. Extensions có thể thêm các tính năng mới, thay đổi hành vi của các tính năng hiện có, hoặc cung cấp nội dung bổ sung cho AI sử dụng. Có thể cài đặt nhiều extensions từ menu "Download Extensions & Assets" trong Extensions panel.

## Extensions panel

Để mở hoặc đóng Extensions panel, chọn **<i class="fa-solid fa-cubes fa-fw"></i> Extensions** trong thanh trên cùng.

- **<i class="fa-solid fa-cubes"></i> Manage extensions**: Kích hoạt, vô hiệu hóa và cập nhật extensions
- **Download Extensions & Assets**: Cài đặt [nhiều extensions hơn](#installable-extensions), nhân vật, âm thanh và nền từ kho lưu trữ SillyTavern
- **Notify on extension updates**: Chọn để được thông báo khi có các bản cập nhật có sẵn cho các extensions được cài đặt
- **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**: Nhập một [extension bên thứ ba](#third-party-extensions) từ một URL Git repository

## Extensions tích hợp

Các extensions này được tích hợp trong SillyTavern và không cần cài đặt. Chúng có thể được bật hoặc tắt trong Extensions panel.

:::callout
**[Chat Translation](Translation.md)**

Dịch các tin nhắn chat sang một ngôn ngữ khác
:::

:::callout
**[Image Captioning](captioning.md)**

Tạo văn bản từ hình ảnh để AI của bạn có thể "nhìn thấy" và phản hồi nội dung trực quan trong các cuộc trò chuyện của bạn
:::

:::callout
**[Image Generation](Stable-Diffusion.md)**

Sử dụng local hoặc cloud-based Stable Diffusion, FLUX hoặc DALL-E APIs để tạo hình ảnh
:::

:::callout
**[Expression Images](Expression-Images.md)**

Hình ảnh (hay 'sprites') của nhân vật AI của bạn, được hiển thị bên cạnh hoặc phía sau cửa sổ chat
:::

:::callout
**[Summarize](Summarize.md)**

Tóm tắt tự động của lịch sử chat
:::

:::callout
**[Chat Vectorization](Chat-vectorization.md)**

Tìm các tin nhắn liên quan từ lịch sử chat và thêm chúng vào bối cảnh
:::

:::callout
**[Text To Speech](TTS.md)**

Kể lại giọng nói cho các tin nhắn chat của bạn qua ElevenLabs, Silero, TTS hệ thống của bạn, **[AllTalk](AllTalk.md)**, **[XTTS](XTTS.md)**, và nhiều hơn nữa
:::

:::callout
**[Quick Reply](/For_Contributors/st-script.md#quick-replies-script-library-and-auto-execution)**

Trả lời các tin nhắn chat bằng một cú nhấp chuột, chạy các lệnh và STscripts, và nhiều hơn nữa
:::

:::callout
**Token Counter**

Chuyển đổi văn bản thành tokens và đếm số lượng tokens
:::

---

## Extensions có thể cài đặt

!!!tip
Bạn **phải** có git được cài đặt để tải xuống extensions. Làm theo các hướng dẫn trên [trang cài đặt Git](https://git-scm.com/downloads) nếu bạn chưa cài đặt.
!!!

Bạn có thể duyệt một danh sách của tất cả các extensions có sẵn trực tiếp từ ứng dụng bằng cách đi tới menu **<i class="fa-solid fa-cubes"></i> Extensions** => **Download Extensions & Assets** và nhấp nút **<i class="fa-solid fa-plug-circle-exclamation"></i> Load Asset List**. Để cài đặt một extension, nhấp nút **<i class="fa-solid fa-download"></i> Download**. Để đọc thêm về một extension, nhấp nút **<i class="fa-solid fa-arrow-up-right-from-square"></i> Link** bên cạnh tên của nó để mở trang GitHub của nó.

!!!info Extensions không phải là Extras
Dự án Extras đã bị ngưng dùng vào tháng 4 năm 2024. Bạn không cần phải cài đặt Extras để sử dụng extensions.
!!!

:::callout
**[Blip](Blip.md)**

Hoạt hình text của các tin nhắn nhân vật với tốc độ có thể thay đổi và phát âm thanh cùng hoạt hình.
:::

:::callout
**[Dynamic Audio](Dynamic-Audio.md)**

Thêm nhạc nền và âm thanh xung quanh ngập ngừng vào các chats của bạn.
:::

:::callout
**[EmulatorJS](EmulatorJS.md)**

Chơi các trò chơi bảng điều khiển retro trực tiếp trong các chats SillyTavern.
:::

:::callout
**[Live2d](Live2d.md)**

Thêm hỗ trợ cho các mô hình live2d. Biểu hiện, hoạt hình và tương tác có thể tùy chỉnh.
:::

:::callout
**[Objective](Objective.md)**

Đặt một Mục tiêu cho AI nhắm đến trong quá trình chat.
:::

:::callout
**[RVC](RVC.md)**

Thêm khả năng Realtime Voice Cloning vào mô-đun Text-to-Speech.
:::

:::callout
**[Speech Recognition](Speech-Recognition.md)**

Chuyển đổi giọng nói của bạn thành văn bản bằng trình duyệt hoặc extras.
:::

:::callout
**[VRM](VRM.md)**

Thêm hỗ trợ cho các mô hình VRM. Biểu hiện, hoạt hình và tương tác có thể tùy chỉnh.
:::

:::callout
**[Web Search](WebSearch.md)**

Thêm kết quả tìm kiếm web vào các LLM prompts.
:::

:::callout
**[AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather)**

Cung cấp thông tin thời tiết bằng cách sử dụng AccuWeather API như một slash command hoặc một function tool.
:::

:::callout
**[Chat Top Bar](https://github.com/SillyTavern/Extension-TopInfoBar)**

Thêm một thanh trên cùng vào cửa sổ chat với các phím tắt cho các tác vụ nhanh.
:::

:::callout
**[Chess](https://github.com/SillyTavern/SillyTavern-Chess)**

Chơi trò chơi cờ vua với LLM.
:::

:::callout
**[Code Runner](https://github.com/SillyTavern/Extension-CodeRunner)**

Cho phép chạy JavaScript và STscript code từ các khối code trong chat.
:::

:::callout
**[D&D Dice](https://github.com/SillyTavern/Extension-Dice)**

Một bộ 7 xúc xắc D&D cổ điển cho tất cả các nhu cầu cuộn xúc xắc của bạn.
:::

:::callout
**[Duplicate Finder](https://github.com/SillyTavern/Extension-DupeFinder)**

Thêm khả năng để cụm các nhân vật theo các nhóm tương tự để dễ dàng tìm duplicates.
:::

:::callout
**[Emoji Picker](https://github.com/SillyTavern/Extension-EmojiPicker)**

Thêm một nút để nhanh chóng chèn emojis vào một tin nhắn chat.
:::

:::callout
**[Group Greetings](https://github.com/SillyTavern/Extension-GroupGreetings)**

Cho phép đặt các lời chào thay thế cụ thể cho các chats nhóm.
:::

:::callout
**[Group SendAs](https://github.com/SillyTavern/SillyTavern-GroupSendAs)**

Thêm một nút để nhanh chóng chèn một /sendas command template cho thành viên nhóm được chọn.
:::

:::callout
**[HypeBot](https://github.com/SillyTavern/Extension-HypeBot)**

Hiển thị các gợi ý được cá nhân hóa dựa trên các chats gần đây của bạn bằng cách sử dụng HypeBot engine của NovelAI. Yêu cầu một subscription NovelAI hoạt động.
:::

:::callout
**[Idle](https://github.com/SillyTavern/Extension-Idle)**

Thêm "idle prompting" sau khi người dùng đã không hoạt động trong một khoảng thời gian để tiếp tục cuộc trò chuyện một cách tự nhiên.
:::

:::callout
**[Image Metadata Viewer](https://github.com/SillyTavern/Extension-ImageMetadataViewer)**

Xem siêu dữ liệu của hình ảnh phóng to được gắn vào một chat.
:::

:::callout
**[LaTeX](https://github.com/SillyTavern/Extension-LaTeX)**

Hiển thị các công thức LaTeX và AsciiMath trong các tin nhắn chat.
:::

:::callout
**[Mermaid](https://github.com/SillyTavern/Extension-Mermaid)**

Thêm hiển thị sơ đồ & lưu đồ Mermaid cho các chats SillyTavern.
:::

:::callout
**[Notebook](https://github.com/SillyTavern/Extension-Notebook)**

Thêm một nơi để lưu trữ ghi chú của bạn. Hỗ trợ định dạng văn bản phong phú.
:::

:::callout
**[Parameter Randomizer](https://github.com/SillyTavern/Extension-Randomizer)**

Thêm khả năng ngẫu nhiên hóa các thanh trượt cài đặt API với mỗi thế hệ.
:::

:::callout
**[Prome Visual Novel Extension](https://github.com/Bronya-Rand/Prome-VN-Extension)**

Nâng cao trải nghiệm visual novel hiện tại với nhiều tính năng hơn (Focus Mode, Letterbox Mode, và nhiều hơn nữa)!
:::

:::callout
**[Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)**

Thêm một tùy chọn để kiểm tra và chỉnh sửa các prompts đầu ra trước khi gửi chúng tới máy chủ.
:::

:::callout
**[Push Notifications](https://github.com/SillyTavern/SillyTavern-PushNotifications)**

Cho phép nhận các thông báo push cho các tin nhắn chat đến.
:::

:::callout
**[Quick Persona](https://github.com/SillyTavern/Extension-QuickPersona)**

Thêm một menu dropdown để chọn user personas từ thanh chat.
:::

:::callout
**[RSS](https://github.com/SillyTavern/Extension-RSS)**

Lấy tin tức mới nhất từ các RSS feeds như một slash command hoặc một function tool.
:::

:::callout
**[Screen Share](https://github.com/SillyTavern/Extension-ScreenShare)**

Cung cấp hình ảnh màn hình cho các mô hình đa phương tiện khi bạn gửi một tin nhắn.
:::

:::callout
**[Silence Player](https://github.com/SillyTavern/Extension-Silence)**

Thêm một silence audio player vào menu extensions. Có thể giúp nếu tab trình duyệt đang bị giết ở nền tảng.
:::

:::callout
**[Timelines](https://github.com/SillyTavern/SillyTavern-Timelines)**

Thêm điều hướng timeline vào lịch sử chat.
:::

:::callout
**[Variable Viewer](https://github.com/LenAnderson/SillyTavern-Variable-Viewer)**

Cách dễ dàng để xem và sửa đổi các biến.
:::

:::callout
**[WebLLM](https://github.com/SillyTavern/Extension-WebLLM)**

Cung cấp một giao diện cho các extensions để sử dụng các mô hình ngôn ngữ trực tiếp trong trình duyệt.
:::

## Extensions bên thứ ba

!!!danger
Sử dụng các extensions bên thứ ba có thể có các tác dụng phụ không mong muốn và có thể gây ra rủi ro bảo mật.
Luôn chắc rằng bạn tin tưởng nguồn trước khi nhập một extension qua **<i class="fa-solid fa-cloud-arrow-down"></i> Install extension**.
Chúng tôi không chịu trách nhiệm cho bất kỳ thiệt hại gây ra bởi các extensions bên thứ ba.
!!!

Để cài đặt một extension bên thứ ba, hãy đi tới menu **<i class="fa-solid fa-cubes"></i> Extensions** => **<i class="fa-solid fa-cloud-arrow-down"></i> Install Extension** và dán URL của kho extension. Tùy chọn, chỉ định branch và (trong các tình huống [multi-user](../Administration/multi-user.md)) mục tiêu cài đặt: tất cả người dùng hoặc chỉ người dùng hiện tại. Extension sẽ được tải xuống và tải tự động.
