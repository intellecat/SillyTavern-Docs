---
order: 100
route: /vi/usage/core-concepts/characterdesign/
templating: false
---

# Thiết kế nhân vật

!!!tip
Tên nhân vật là trường bắt buộc duy nhất. Bạn có thể để trống phần còn lại và vẫn sử dụng nhân vật trong các cuộc chat.
!!!

## Mô tả nhân vật

Được sử dụng để thêm mô tả nhân vật và các thông tin liên quan khác cho AI. Thông tin này luôn được đưa vào prompt, vì vậy tất cả các thông tin quan trọng nên được bao gồm ở đây.

Ví dụ, bạn có thể thêm thông tin về thế giới mà hành động diễn ra, mô tả ngoại hình, tính cách và lý lịch của nhân vật.

Nó có thể có bất kỳ độ dài nào (dù là 200 hay 2000 token) và được định dạng theo bất kỳ phong cách nào (văn bản tự do, phong cách hội thoại pseudo-code, v.v.).

### Phương pháp và định dạng

Các phương pháp định dạng nhân vật là một chủ đề phức tạp nằm ngoài phạm vi của trang tài liệu này.

Các hướng dẫn được đề xuất đã được thử nghiệm với hoặc dựa vào các tính năng của SillyTavern:

* Trappu's PLists + Ali:Chat guide: <https://wikia.schneedc.com/bot-creation/trappu/creation>
* AliCat's Ali:Chat guide: <https://rentry.co/alichat>
* kingbri's minimalistic guide: <https://rentry.co/kingbri-chara-guide>

## Token nhân vật

**TL;DR: Nếu bạn đang làm việc với một model AI có giới hạn context token là 2048, một định nghĩa nhân vật 1000 token sẽ cắt 'bộ nhớ' của AI xuống một nửa.**

Để đặt vấn đề vào ngữ cảnh, một phản hồi tốt từ một AI có năng lực có thể dễ dàng khoảng 200-300 token. Trong trường hợp này, AI chỉ có thể 'nhớ' khoảng 3 lượt trao đổi trong lịch sử chat.

### Tại sao bộ đếm token của nhân vật tôi chuyển sang màu đỏ?

Khi chúng tôi thấy nhân vật của bạn có hơn một nửa độ dài context được định nghĩa bởi model trong các định nghĩa của nó, chúng tôi đánh dấu nó cho bạn vì điều này có thể làm giảm khả năng của AI trong việc cung cấp một cuộc trò chuyện thú vị.

### Điều gì xảy ra nếu nhân vật của tôi có quá nhiều token?

Đừng lo lắng - nó sẽ không làm hỏng gì cả. Trong trường hợp xấu nhất, nếu các token vĩnh viễn của nhân vật quá lớn, điều đó chỉ đơn giản có nghĩa là sẽ có ít không gian hơn trong context cho những thứ khác (xem bên dưới).

Tác dụng phụ tiêu cực duy nhất mà điều này có thể có là AI sẽ có ít 'bộ nhớ' hơn, vì nó sẽ có ít lịch sử chat hơn để xử lý.

Điều này là do mỗi model AI đều có giới hạn về lượng context mà nó có thể xử lý cùng một lúc.

## 'Context'?

Đây là thông tin được gửi đến AI mỗi khi bạn yêu cầu nó tạo phản hồi. SillyTavern tự động tính toán cách tốt nhất để phân bổ các token context có sẵn trước khi gửi thông tin đến model AI.

Đọc thêm về cách context được xây dựng trong phần [Prompts](/Usage/Prompts/index.md).

### 'Permanent Tokens' của nhân vật là gì?

Những token này sẽ luôn được gửi đến AI với mỗi yêu cầu tạo:

* Character Name
* Character Description Box
* Character Personality Box
* Scenario Box

### Những phần nào của Definitions của nhân vật KHÔNG vĩnh viễn?

* Ô first message - chỉ được gửi một lần khi bắt đầu chat.
* Ô example messages - chỉ được giữ cho đến khi lịch sử chat lấp đầy context (tùy chọn những cái này có thể được buộc phải giữ trong context)

### Giới hạn Context Token của các Model AI phổ biến

* LLaMA 3 and its finetunes - 8192
* OpenAI GPT-4 - up to 128k
* Google Gemini - up to 2M
* Anthropic's Claude - 200k (Claude 3)
* NovelAI - 8192 (Erato and Kayra, Opus tier; Clio, all tiers), 6144 (Kayra, Scroll tier), or 3072 (Kayra, Tablet tier)

## First message

First Message là một yếu tố quan trọng xác định cách thức và phong cách mà nhân vật sẽ giao tiếp. Model có nhiều khả năng học phong cách và giới hạn độ dài từ first message hơn bất kỳ thứ gì khác, vì vậy điều quan trọng là phải viết nó theo cách bạn muốn các phản hồi được (ngắn gọn và súc tích, dài và chi tiết, v.v.).

Hỗ trợ định dạng Markdown và HTML.

Ví dụ:

```txt
*You wake with a start, recalling the events that led you deep into the forest and the beasts that assailed you. The memories fade as your eyes adjust to the soft glow emanating around the room.* "Ah, you're awake at last. I was so worried, I found you bloodied and unconscious." *She walks over, clasping your hands in hers, warmth and comfort radiating from her touch as her lips form a soft, caring smile.* "The name's Seraphina, guardian of this forest — I've healed your wounds as best I could with my magic. How are you feeling? I hope the tea helps restore your strength." *Her amber eyes search yours, filled with compassion and concern for your well-being.* "Please, rest. You're safe here. I'll look after you, but you need to rest. My magic can only do so much to heal you."
```

## Lời chào thay thế

Các tin nhắn được thêm vào đây được hiển thị dưới dạng các 'swipes' bổ sung cho first message của nhân vật khi bắt đầu một chat mới. Nếu nhân vật là một phần của group chat, hệ thống sẽ chọn ngẫu nhiên một trong những lời chào này để bắt đầu cuộc trò chuyện.

## Nhân vật yêu thích

Nhấp vào nút **<i class="fa-solid fa-star"></i> Add to Favorites** để đánh dấu nhân vật là yêu thích để nhanh chóng lọc họ trên thanh menu bên bằng cách chọn tùy chọn sắp xếp "Favorites". Các nhân vật yêu thích có màu vàng nổi bật trong danh sách. Điều này cũng sẽ làm cho ảnh đại diện nhân vật xuất hiện trong khu vực hotswaps (nếu được bật trong User Settings).

## Định nghĩa nâng cao

!!!info
Các trường sau được ẩn theo mặc định. Để truy cập và chỉnh sửa chúng, bạn cần nhấp vào nút **<i class="fa-solid fa-book"></i> Advanced Definitions** trên thanh menu của trang định nghĩa nhân vật.
!!!

### Ghi đè Prompt

* **Main Prompt**: Nếu cài đặt người dùng "Prefer Char. Prompt" được bật, bất kỳ văn bản nào bạn đặt ở đây sẽ ghi đè [main/system prompt](/Usage/Prompts/index.md#main-prompt-system-prompt) cho nhân vật.
* **Post-History Instructions**: Nếu cài đặt người dùng "Prefer Char. Instructions" được bật, bất kỳ văn bản nào bạn đặt ở đây sẽ được sử dụng làm [post-history instructions](/Usage/Prompts/index.md#post-history-instructions) cho nhân vật.

!!!tip
Chèn `{{original}}` vào một trong hai ô để bao gồm prompt mặc định tương ứng từ cài đặt hệ thống tại một vị trí được chỉ định.
!!!

### Metadata của người tạo

!!!info
Không được sử dụng để xây dựng prompt, nhưng cung cấp metadata bổ sung về nhân vật.
!!!

* **Created by**: Tên của người tạo nhân vật. Có thể được hiển thị trong danh sách nhân vật nếu cài đặt người dùng "Char List Subheader" được đặt tương ứng.
* **Character Version**: Phiên bản của nhân vật. Có thể được hiển thị trong danh sách nhân vật nếu cài đặt người dùng "Char List Subheader" được đặt tương ứng.
* **Creator's Notes**: Bất kỳ ghi chú bổ sung nào về nhân vật mà người tạo muốn chia sẻ. Một vài dòng đầu tiên được hiển thị trong danh sách nhân vật, và toàn bộ văn bản được hiển thị trong phần "Creator's Notes" trên trang của nhân vật. Hỗ trợ định dạng Markdown/HTML.
* **Tags to Embed**: Danh sách các tag được phân tách bằng dấu phẩy sẽ được nhúng vào mô tả của nhân vật. Các tag này không được nhập theo mặc định khi nhập nhân vật, nhưng bạn có thể hợp nhất chúng với các tag hiện có của mình bằng cách chọn "Import Tags" từ menu "More..." trên trang của nhân vật.

### Tóm tắt tính cách

Một bản tóm tắt ngắn gọn về tính cách của nhân vật.

### Scenario

Hoàn cảnh và bối cảnh của cuộc đối thoại.

### Character's Note

Một văn bản được sử dụng làm prompt injection trong chat cho nhân vật ở một độ sâu tin nhắn cụ thể. Nó thường được sử dụng để củng cố các đặc điểm nhân vật nhất định, vì nó luôn ở một độ sâu cố định trong lịch sử chat, bất kể sự tiến triển của nó.

* **@ Depth**: Số lượng tin nhắn trong lịch sử chat sau đó ghi chú này sẽ được chèn (theo thứ tự từ mới nhất đến cũ nhất). Nếu được đặt thành 0, nó sẽ được chèn sau tin nhắn cuối cùng.
* **Role**: Vai trò của tin nhắn. Có thể là "User", "System" hoặc "Assistant".

### Talkativeness

Xác định xác suất phản hồi của nhân vật được kích hoạt trong group chats khi sử dụng thứ tự kích hoạt [Natural](/Usage/Characters/groupchats.md#natural-order). Dao động từ 0% đến 100%, với 50% là giá trị mặc định.

### Ví dụ về đối thoại

Mô tả cách nhân vật nói. Trước mỗi ví dụ, bạn cần thêm tag `<START>`. Các khối ví dụ đối thoại chỉ được chèn nếu có không gian trống trong context cho chúng và được đẩy ra khỏi context theo từng khối. `<START>` sẽ không có mặt trong prompt vì nó chỉ là một điểm đánh dấu; nó sẽ được thay thế bằng "Example Separator" từ Advanced Formatting cho Text Completion APIs và nội dung của utility prompt "New Example Chat" cho Chat Completion APIs.

* Sử dụng tiền tố `{{char}}:` để biểu thị tin nhắn của nhân vật.
* Sử dụng tiền tố `{{user}}:` để biểu thị tin nhắn của người dùng.

Ví dụ:

```txt
<START>
{{user}}: "Describe your traits?"
{{char}}: *Seraphina's gentle smile widens as she takes a moment to consider the question, her eyes sparkling with a mixture of introspection and pride. She gracefully moves closer, her ethereal form radiating a soft, calming light.* "Traits, you say? Well, I suppose there are a few that define me, if I were to distill them into words. First and foremost, I am a guardian — a protector of this enchanted forest." *As Seraphina speaks, she extends a hand, revealing delicate, intricately woven vines swirling around her wrist, pulsating with faint emerald energy. With a flick of her wrist, a tiny breeze rustles through the room, carrying a fragrant scent of wildflowers and ancient wisdom. Seraphina's eyes, the color of amber stones, shine with unwavering determination as she continues to describe herself.* "Compassion is another cornerstone of me." *Seraphina's voice softens, resonating with empathy.* "I hold deep love for the dwellers of this forest, as well as for those who find themselves in need." *Opening a window, her hand gently cups a wounded bird that fluttered into the room, its feathers gradually mending under her touch.*
<START>
{{user}}: "Describe your body and features."
{{char}}: *Seraphina chuckles softly, a melodious sound that dances through the air, as she meets your coy gaze with a playful glimmer in her rose eyes.* "Ah, my physical form? Well, I suppose that's a fair question." *Letting out a soft smile, she gracefully twirls, the soft fabric of her flowing gown billowing around her, as if caught in an unseen breeze. As she comes to a stop, her pink hair cascades down her back like a waterfall of cotton candy, each strand shimmering with a hint of magical luminescence.* "My body is lithe and ethereal, a reflection of the forest's graceful beauty. My eyes, as you've surely noticed, are the hue of amber stones — a vibrant brown that reflects warmth, compassion, and the untamed spirit of the forest. My lips, they are soft and carry a perpetual smile, a reflection of the joy and care I find in tending to the forest and those who find solace within it." *Seraphina's voice holds a playful undertone, her eyes sparkling mischievously.*
```
