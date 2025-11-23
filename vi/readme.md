---
route: /
---

# SillyTavern là gì?

![SillyTavern - LLM Frontend for Power Users](/static/banner.png)

SillyTavern (hoặc viết tắt là ST) là giao diện người dùng được cài đặt cục bộ cho phép bạn tương tác với các mô hình LLM tạo văn bản, công cụ tạo hình ảnh và mô hình giọng nói TTS. Mục tiêu của chúng tôi là trao quyền cho người dùng với càng nhiều tiện ích và khả năng kiểm soát các lời nhắc LLM càng tốt, đồng thời xem đường cong học tập dốc như một phần của niềm vui.

SillyTavern là một dự án đam mê được mang đến cho bạn bởi cộng đồng những người đam mê LLM tận tâm và sẽ luôn miễn phí và mã nguồn mở. Bắt đầu vào tháng 2 năm 2023 như một nhánh rẽ của TavernAI 1.2.8, SillyTavern hiện có hơn 200 người đóng góp và 2 năm phát triển độc lập, và tiếp tục phục vụ như một phần mềm hàng đầu cho những người đam mê AI am hiểu.

## Ảnh chụp màn hình

|   [![API Connection](/static/screenshot1.jpg)](/static/screenshot1.jpg)    |  [![Chat UI](/static/screenshot2.jpg)](/static/screenshot2.jpg)   |
|:--------------------------------------------------------------------------:|:-----------------------------------------------------------------:|
| [![Advanced Formatting](/static/screenshot3.jpg)](/static/screenshot3.jpg) | [![World Info](/static/screenshot4.jpg)](/static/screenshot4.jpg) |

## Yêu cầu cài đặt

Yêu cầu phần cứng là tối thiểu: nó sẽ chạy trên bất kỳ thứ gì có thể chạy NodeJS 18 trở lên. Nếu bạn định thực hiện suy luận LLM trên máy cục bộ của mình, chúng tôi khuyên dùng card đồ họa NVIDIA dòng 3000 với ít nhất 6GB VRAM.

Làm theo hướng dẫn cài đặt cho nền tảng của bạn:

* [Windows](/Installation/Windows.md)
* [Linux và Mac](/Installation/LinuxMacOS.md)
* [Android](/Installation/Android.md)
* [Docker](/Installation/Docker.md)

## Các nhánh

SillyTavern đang được phát triển sử dụng hệ thống hai nhánh để đảm bảo trải nghiệm mượt mà cho tất cả người dùng.

* `release` -🌟 **Được khuyến nghị cho hầu hết người dùng.** Đây là nhánh ổn định và được khuyến nghị nhất, chỉ được cập nhật khi các bản phát hành chính thức được đẩy lên. Phù hợp cho phần lớn người dùng. Thường được cập nhật mỗi tháng một lần.
* `staging` - ⚠️ **Không được khuyến nghị cho người dùng thông thường.** Nhánh này có các tính năng mới nhất, nhưng hãy cẩn thận vì nó có thể bị lỗi bất cứ lúc nào. Chỉ dành cho người dùng chuyên sâu và những người đam mê. Cập nhật nhiều lần mỗi ngày.

## Tôi cần gì ngoài SillyTavern?

Vì SillyTavern chỉ là một giao diện, bạn sẽ cần quyền truy cập vào backend LLM để cung cấp khả năng suy luận. Bạn có thể sử dụng AI Horde để trò chuyện ngay lập tức. Ngoài ra, chúng tôi hỗ trợ nhiều backend LLM cục bộ và trên đám mây khác: API tương thích OpenAI, KoboldAI, Tabby và nhiều hơn nữa. Bạn có thể đọc thêm về các API được hỗ trợ trong phần [Kết nối API](/Usage/API_Connections/index.md).

## Thẻ nhân vật

SillyTavern được xây dựng xung quanh khái niệm "thẻ nhân vật". Thẻ nhân vật là một tập hợp các lời nhắc thiết lập hành vi của LLM và được yêu cầu để có các cuộc trò chuyện liên tục trong SillyTavern. Chúng hoạt động tương tự như GPTs của ChatGPT hoặc bots của Poe. Nội dung của thẻ nhân vật có thể là bất kỳ thứ gì: một kịch bản trừu tượng, một trợ lý được điều chỉnh cho một nhiệm vụ cụ thể, một nhân vật nổi tiếng hoặc một nhân vật hư cấu.

Để có một cuộc trò chuyện nhanh mà không cần chọn thẻ nhân vật hoặc chỉ để kiểm tra kết nối LLM, chỉ cần nhập lời nhắc của bạn vào thanh nhập trên [Màn hình chào mừng](/Usage/welcome-assistants.md) sau khi mở SillyTavern. Điều này sẽ tạo một thẻ nhân vật "Trợ lý" trống mà bạn có thể tùy chỉnh sau.

Để có ý tưởng chung về cách xác định thẻ nhân vật, hãy xem nhân vật mặc định (Seraphina) hoặc tải xuống các thẻ do cộng đồng tạo được chọn từ menu "Download Extensions & Assets".

Bạn cũng có thể tạo thẻ nhân vật của riêng mình từ đầu. Tham khảo hướng dẫn [Thiết kế nhân vật](/Usage/Characters/characterdesign.md) để biết thêm thông tin.

## Tính năng chính

* [Cài đặt tạo văn bản nâng cao](/Usage/Prompts/advancedformatting.md) với nhiều cài đặt sẵn do cộng đồng tạo
* [Hỗ trợ World Info](Usage/worldinfo.md): tạo lore phong phú hoặc tiết kiệm token trên thẻ nhân vật của bạn
* [Trò chuyện nhóm](/Usage/Characters/groupchats.md): phòng nhiều bot cho các nhân vật trò chuyện với bạn và/hoặc với nhau
* [Tùy chọn tùy chỉnh giao diện phong phú](/Usage/User_Settings/uicustomization.md): màu chủ đề, hình nền, CSS tùy chỉnh và nhiều hơn nữa
* [Persona người dùng](/Usage/personas.md): cho AI biết một chút về bạn để đắm chìm hơn
* [Hỗ trợ RAG tích hợp sẵn](/Usage/Characters/data-bank.md): thêm tài liệu vào các cuộc trò chuyện của bạn để AI tham khảo
* Hệ thống con [lệnh trò chuyện](/Usage/Chatting/slashcommands.md) mở rộng và [công cụ kịch bản](/For_Contributors/st-script.md) riêng

## Tiện ích mở rộng

SillyTavern hỗ trợ khả năng mở rộng.

* [Biểu cảm cảm xúc của nhân vật (sprites)](/extensions/Expression-Images.md)
* [Tự động tóm tắt lịch sử trò chuyện](/extensions/Summarize.md)
* Tự động dịch giao diện và [trò chuyện](extensions/Translation.md)
* [Tạo hình ảnh Stable Diffusion/FLUX/DALL-E](/extensions/Stable-Diffusion.md)
* [Chuyển văn bản thành giọng nói cho tin nhắn phản hồi AI (qua ElevenLabs, Silero hoặc System TTS của hệ điều hành)](/extensions/TTS.md)
* [Khả năng tìm kiếm web để thêm ngữ cảnh thế giới thực bổ sung vào lời nhắc của bạn](/extensions/WebSearch.md)
* Nhiều tiện ích khác có sẵn để tải xuống từ menu "Download Extensions & Assets".

## Làm thế nào tôi có thể liên hệ trực tiếp với các nhà phát triển?

* Discord: cohee, rossascends, wolfsblvt
* Reddit: [/u/RossAscends](https://www.reddit.com/user/RossAscends/), [/u/sillylossy](https://www.reddit.com/user/sillylossy/), [u/Wolfsblvt](https://www.reddit.com/user/Wolfsblvt/)
* [Đăng vấn đề GitHub](https://github.com/SillyTavern/SillyTavern/issues)

## Tôi thích dự án của bạn! Làm thế nào để đóng góp?

* Chúng tôi hoan nghênh pull requests! Làm theo [Hướng dẫn đóng góp](https://github.com/SillyTavern/SillyTavern/blob/release/CONTRIBUTING.md) để bắt đầu.
* Chúng tôi cũng hoan nghênh các báo cáo lỗi hữu ích và có kiến thức sử dụng các mẫu được cung cấp trong GitHub của chúng tôi.
* Chúng tôi không chấp nhận quyên góp tiền cho chính dự án.

## Quyên góp cá nhân

Sự hỗ trợ của bạn cho từng cá nhân đóng góp được đánh giá cao, nhưng nó sẽ không ảnh hưởng đến hướng phát triển tổng thể của SillyTavern.

* RossAscends có [Patreon](https://www.patreon.com/RossAscends) & [Kofi](https://ko-fi.com/rossascends) cá nhân

## Giấy phép

SillyTavern là một dự án miễn phí và mã nguồn mở được phát hành theo [Giấy phép AGPL-3.0](https://github.com/SillyTavern/SillyTavern/blob/release/LICENSE).
