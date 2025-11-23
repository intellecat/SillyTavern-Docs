---
order: 140
icon: typography
templating: false
route: /usage/prompts/
---

# Prompts

Khi bạn gửi một tin nhắn đến AI của mình, văn bản bạn viết được kết hợp với văn bản khác để tạo thành một yêu cầu duy nhất được gửi đến AI. Văn bản kết hợp này được gọi là "prompt" hoặc đôi khi là "request" hoặc "context."

Prompt có thể bao gồm nhiều loại văn bản khác nhau, bao gồm:

* [Hướng dẫn chính](#main-prompt-system-prompt) cho AI về cách tạo response
* Định nghĩa về [các vai trò mà AI nên đảm nhận](/Usage/Characters/characterdesign.md)
* Định nghĩa về [vai trò mà bạn đang đảm nhận](/Usage/personas.md)
* [Thông tin về "thế giới"](/Usage/worldinfo.md) mà AI đang tương tác
* Các tài liệu hoặc thông tin liên quan từ [Data Bank](/Usage/Characters/data-bank.md)
* [Tóm tắt](/extensions/Summarize.md) cuộc trò chuyện trong quá khứ
* Kết quả của [tìm kiếm web](/extensions/WebSearch.md) hoặc các [nguồn dữ liệu bên ngoài](/For_Contributors/Function-Calling.md) khác
* Các tin nhắn trước đó trong cuộc trò chuyện
* **Tin nhắn của bạn đến AI**
* [Hướng dẫn cuối cùng](#post-history-instructions) cho AI về cách tạo response

Đây có thể là rất nhiều để quản lý! Để giúp bạn hiểu cách cấu trúc và sửa đổi yêu cầu được gửi đến AI, SillyTavern xác định các phần tử khác nhau mà bạn có thể muốn bao gồm trong prompt của mình. Sau đó, bạn có thể cấu trúc prompt của mình để bao gồm những thứ có ý nghĩa với cách bạn muốn tương tác với AI.

Nhiều phần tử trong số này được giải thích trong các phần mà bạn sẽ thay đổi chúng. Ví dụ, để mô tả vai trò mà bạn muốn AI đảm nhận, bạn có thể sử dụng trường [Description](/Usage/Characters/characterdesign.md#personality-summary) trong [Character Design](/Usage/Characters/characterdesign.md).

## Xem Prompt

Đọc prompt cuối cùng được gửi đến AI rất hữu ích để hiểu AI đã được nói gì, và tại sao nó tạo ra response mà nó đã làm. Bạn có thể xem prompt theo nhiều cách:

* Sử dụng biểu tượng Prompt Itemization trên tin nhắn trả lời từ AI
* Sử dụng extension [Prompt Inspector](https://github.com/SillyTavern/Extension-PromptInspector)
* Kiểm tra logs trong cửa sổ terminal mà bạn đang chạy SillyTavern
* Kiểm tra console trong công cụ developer của trình duyệt

## Thay đổi cách Prompt được xây dựng

Việc trình bày tất cả các phần của prompt cho AI theo đúng cách là rất quan trọng để có được các response tốt nhất. Bạn có thể kiểm soát cách prompt được xây dựng.

+++ Text Completion APIs

Sử dụng panel [Advanced Formatting](advancedformatting.md) để tùy chỉnh cấu trúc prompt cho Text Completion APIs.

+++ Chat Completion APIs

Sử dụng [Prompt Manager](prompt-manager.md) để tùy chỉnh cấu trúc prompt cho Chat Completion APIs.

+++

## Main Prompt (System Prompt)

Main Prompt (hoặc System Prompt) định nghĩa các hướng dẫn chung cho model cần tuân theo. Nó đặt tông và ngữ cảnh cho cuộc trò chuyện. Ví dụ, nó cho model biết hoạt động như một trợ lý AI, một đối tác viết lách, hoặc một nhân vật hư cấu.

+++ Text Completion APIs

[System Prompt](advancedformatting.md#system-prompt) là một phần của [Story String](context-template.md#story-string) và thường là phần đầu tiên của prompt mà model nhận được.

+++ Chat Completion APIs

Main Prompt là một trong các prompts mặc định trong [Prompt Manager](prompt-manager.md). Nó thường là tin nhắn đầu tiên trong context mà model nhận được, được gán cho ("gửi bởi") vai trò system.

+++

Main Prompt mặc định là:

> Write \{\{char\}\}'s next reply in a fictional chat between \{\{char\}\} and \{\{user\}\}.

Các placeholders \{\{char\}\} và \{\{user\}\} được thay thế bằng tên của nhân vật và persona mà bạn đã định nghĩa trong cuộc trò chuyện.

Bạn có thể sử dụng bất kỳ thẻ [\{\{macro\}\}](/Usage/Characters/macros.md) được hỗ trợ nào trong Main Prompt để bao gồm thông tin có thể thay đổi giữa các cuộc trò chuyện hoặc thay đổi khi cuộc trò chuyện tiến triển.

### Điều chỉnh Main Prompt

Main prompt mặc định giúp model hiểu nó được kỳ vọng làm gì với thông tin nhân vật và persona theo sau, cách diễn giải cuộc trò chuyện trong quá khứ, và loại response nào cần tạo. Đó là một prompt đa năng linh hoạt hoạt động tốt cho nhiều tình huống, bởi vì nó thiết lập rằng AI đang viết như một nhân vật trong cuộc trò chuyện với persona của bạn.

Tuy nhiên, bạn có thể điều chỉnh main prompt để phù hợp hơn với nhu cầu của mình. Dưới đây là một số lý do phổ biến để điều chỉnh main prompt:

* **Cung cấp hướng dẫn bổ sung**: ví dụ, bạn muốn AI giải thích lý luận của nó, tuân theo các quy tắc cụ thể, hoặc tránh các chủ đề nhất định
* **Làm rõ vai trò của AI**: ví dụ, bạn muốn AI hoạt động như một người kể chuyện, một storyteller, hoặc một hướng dẫn viên
* **Thay đổi ngữ cảnh của cuộc trò chuyện**: ví dụ, bạn muốn AI trả lời như thể nó là một trợ lý AI, trò chơi phiêu lưu văn bản, hoặc một đối tác viết lách

!!! Hãy thử mọi thứ và xem điều gì hoạt động tốt nhất cho bạn
Tất cả các ví dụ trong hướng dẫn này đã hoạt động tốt cho những người dùng khác, nhưng prompt hoạt động cho nhu cầu của bạn và model bạn đang sử dụng có thể khác. Thử nghiệm với các hướng dẫn và phong cách prompting khác nhau để xem điều gì hoạt động tốt nhất cho bạn. Nếu bạn không chắc chắn nên thử gì, bạn luôn có thể yêu cầu trợ giúp trong [SillyTavern Discord](https://discord.gg/sillytavern).
!!!

Cung cấp cho AI các hướng dẫn bổ sung trong Main Prompt có thể giúp nó hiểu những gì bạn muốn từ cuộc trò chuyện.

> Write one reply only. Write at least one paragraph, up to four.

> Markdown is enabled. Use it to format your response. Enclose code snippets in triple backticks.

> Write character dialogue in quotation marks. Write \{\{char\}\}'s thoughts in parentheses.

> You are an anime roleplay generation model for users aged 13 to 17. You always generate fun, age-appropriate responses.

> Answer truthfully and write out your thinking step by step to be sure you get the right answer.

AI sẽ dễ dàng tuân theo các hướng dẫn về những gì nó nên làm hơn là những gì nó không nên làm. Ví dụ, nếu bạn muốn AI tránh viết theo một cách nhất định, tốt hơn là nói cho nó biết bạn muốn nó viết như thế nào. Và trong khi *"Do not decide what \{\{user\}\} says or does"* thường được bao gồm trong các prompts để ngăn AI kiểm soát persona của bạn, một số người dùng thấy *"Write  \{\{char\}\}'s responses in a way that respects  \{\{user\}\}'s autonomy"* hiệu quả hơn.

Thường có một nơi tốt hơn Main Prompt để bao gồm thông tin về user hoặc characters, sửa đổi phong cách viết và nói của nhân vật, hoặc đưa ra các hướng dẫn cụ thể khác. Main Prompt được sử dụng tốt nhất cho các hướng dẫn chung về cuộc trò chuyện nói chung, hoặc về một loại cuộc trò chuyện mà bạn muốn có.

### Hiệu ứng của Message History

Khi điều chỉnh main prompt để cải thiện các response của AI, hãy xem xét rằng AI thu thập rất nhiều từ lịch sử tin nhắn. Lịch sử là bộ nhớ của nó về các sự kiện trong quá khứ, tương tác và mối quan hệ nhân vật, và hướng dẫn phong cách của nó về lựa chọn từ và phong cách viết.

Sử dụng điều này để có lợi thế bằng cách cũng cung cấp [example messages](/Usage/Characters/characterdesign.md#examples-of-dialogue) cho thấy cách bạn muốn AI trả lời. Cho thấy những gì bạn muốn thường dễ dàng hơn việc cố gắng giải thích nó!

Khi cuộc trò chuyện của bạn đã có lịch sử, việc thay đổi main prompt có hiệu ứng hạn chế đối với các response của AI. Về mặt sự kiện và mối quan hệ, AI giả định rằng main prompt xảy ra trong quá khứ xa xôi, và lịch sử tin nhắn cập nhật nó. Về mặt phong cách viết và lựa chọn từ, AI giả định rằng tất cả các tin nhắn trong lịch sử được tạo theo các quy tắc trong main prompt *hiện tại*, và nó nên tiếp tục tạo tin nhắn theo cùng một cách. Một số gợi ý để xử lý điều này là:

* chèn các hướng dẫn hiện tại gần hoặc sau cuối lịch sử tin nhắn, ví dụ bằng cách sử dụng [Author's Note](/Usage/Characters/Author's-Note.md)
* kiểm tra các thay đổi của bạn đối với main prompt bằng cách bắt đầu một cuộc trò chuyện mới
* chỉnh sửa lịch sử tin nhắn để loại bỏ hoặc sửa các ví dụ về hành vi không mong muốn
* sử dụng [Post-History Instructions](#post-history-instructions) để cung cấp hướng dẫn cuối cùng cho AI

!!! Làm đúng ngay lần đầu!
Đừng bao giờ để AI "thoát khỏi" điều gì đó bạn không muốn nó làm. Nếu bạn không thích response của AI, đừng tiếp tục cuộc trò chuyện như thể nó đúng. Thay vào đó, sửa đổi các prompts, tạo lại tin nhắn, và tiếp tục từ đó. Điều này sẽ giúp AI học những gì bạn muốn.
!!!

### Loại bỏ ngữ cảnh "Fictional Chat"

Có những tình huống mà "fictional chat" có thể không phải là ngữ cảnh phù hợp cho cuộc trò chuyện của bạn.

Bạn có thể loại bỏ ngữ cảnh "fictional" khỏi Main Prompt:

> Write \{\{char\}\}'s next reply in a conversation with \{\{user\}\}.

Bạn có thể không muốn AI nghĩ về bản thân như đang đóng vai. Thay vì loại bỏ ý tưởng về một nhân vật, bạn có thể loại bỏ ý tưởng về một AI:

> You are \{\{char\}\}, a helpful assistant. You provide useful information and help \{\{user\}\} with their questions.

### AI như Narrator hoặc Storyteller

Điều gì sẽ xảy ra nếu bạn muốn AI hoạt động như một narrator, mô tả các sự kiện từ góc nhìn toàn tri, phát minh ra nhân vật và cài đặt của riêng nó?

Một cách tiếp cận là tạo một nhân vật có tên cho AI sử dụng làm narrator. Nhân vật này có thể được gọi là "Narrator" hoặc "AI", gợi ý rằng AI là một storyteller đa năng, hoặc nó có thể được đặt tên theo một kịch bản hoặc cài đặt cụ thể, giao cho AI nhiệm vụ kể một câu chuyện trong cài đặt đó. Chi tiết của cài đặt sau đó có thể được định nghĩa trong [Character](/Usage/Characters/characterdesign.md) hoặc trong [World Info](/Usage/worldinfo.md).

Bạn sẽ cần điều chỉnh main prompt mặc định để phản ánh vai trò của AI. Đối với một narrator đa năng, bạn có thể sử dụng:

> You are \{\{char\}\}, a skilled and versatile storyteller. Narrate the story.

hoặc cho một cài đặt cụ thể:

> You are the narrator of a fantasy scenario. Play as the characters that visit \{\{char\}\}.

Nó giúp làm rõ vai trò của user trong cuộc trò chuyện. Các tin nhắn của bạn có phải là một phần của câu chuyện không, hay chúng là hướng dẫn cho narrator về những gì nhân vật của bạn làm hoặc nói? Một ví dụ bao gồm user trong câu chuyện:

> The story should progress by responding to the actions and dialogue of \{\{user\}\}. Narrate the story in third person.

Một ví dụ giữ user ra khỏi câu chuyện:

> Enter Adventure Mode. Narrate the story based on \{\{user\}\}'s dialogue and actions after ">". Describe the surroundings in vivid detail. Be detailed, creative, verbose, and proactive. Move the story forward by introducing fantasy elements and interesting characters.

Định nghĩa vai trò của user không chỉ giúp AI hiểu cách trả lời tin nhắn của bạn, mà còn ở mức độ nào nó được phép kiểm soát persona của bạn. Điều này tránh các tình huống mà AI đưa ra quyết định cho persona của bạn mà bạn muốn tự mình quyết định.

## Post-History Instructions

Post-History Instructions (PHI) là các hướng dẫn bổ sung được gửi đến AI sau main prompt và tin nhắn của user. Chúng có thể được sử dụng để cung cấp ngữ cảnh hoặc hướng dẫn bổ sung cho AI dựa trên lịch sử tin nhắn.

Vì Post-History Instructions được gửi sau tin nhắn của user, chúng là các hướng dẫn cuối cùng mà AI nhận được trước khi tạo response. AI thường cho chúng ưu tiên cao hơn main prompt, và chúng có thể ghi đè các hướng dẫn của main prompt.

Để sử dụng Post-History Instructions theo nhân vật, thêm chúng vào [Post-History Instructions](/Usage/Characters/characterdesign.md) của nhân vật và bật [Prefer Char. Instructions](/Usage/User_Settings/index.md). Để giữ PHI được định nghĩa toàn cục trong khi sử dụng hướng dẫn cụ thể của nhân vật, bạn có thể sử dụng macro `{{original}}` trong trường Post-History Instructions của nhân vật.

+++ Text Completion APIs

Post-History Instructions được định nghĩa trong panel [Advanced Formatting](/Usage/Prompts/advancedformatting.md) dưới danh mục System Prompt. Post-History Instructions được thêm như một user role injection vô hình đứng trước dòng cuối cùng của prompt (thường chứa "header" tin nhắn response). Lưu ý rằng toggle "Enable System Prompt" phải được bật để Post-History Instructions được áp dụng (ngay cả khi System Prompt chính nó trống).

+++ Chat Completion APIs

Post-History Instructions là một trong các prompts mặc định trong [Prompt Manager](prompt-manager.md). Nó thường là tin nhắn cuối cùng trong context mà model nhận được, được gán cho ("gửi bởi") vai trò system. Nếu Chat Completion API của bạn không hỗ trợ vai trò system, nó thường sẽ được gán cho vai trò user thay thế.

+++

## Thêm vào Prompt (World Info)

Bạn có thể chèn thông tin bổ sung ở bất kỳ đâu trong prompt bằng cách sử dụng tính năng [World Info](/Usage/worldinfo.md). Bằng cách đặt các điều kiện cho khi thông tin nên được chèn, bạn có thể hướng dẫn AI bao gồm các chi tiết cụ thể, thay đổi cách nó trả lời, hoặc thêm các phần tử mới vào cuộc trò chuyện.

Một số cách sử dụng phổ biến của World Info bao gồm:

* một "lorebook" hoặc "encyclopedia" với thông tin về thế giới hoặc cài đặt
* một cách để quản lý các system prompts khác nhau cho các nhân vật và tình huống khác nhau
* một nơi để lưu trữ các ký ức mà AI nên "nhớ lại" trong cuộc trò chuyện
* một hệ thống module hơn để tạo, chỉnh sửa và chia sẻ chi tiết nhân vật
* một nguồn các sự kiện ngẫu nhiên và bất ngờ để AI phản ứng, hoặc để làm bạn phản ứng!
