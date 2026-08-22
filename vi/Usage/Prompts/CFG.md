---
order: 60
route: /vi/usage/prompts/cfg/
---

# CFG

Trang được viết bởi: kingbri

Đóng góp: kingbri, Guillaume "Vermeille" Sanchez, AliCat

## Nó là gì?

CFG, hay classifier-free guidance là một phương pháp được sử dụng để giúp làm cho các phần của prompt ít hoặc nhiều nổi bật hơn.

### Backend API được hỗ trợ

Hiện tại, các backend được hỗ trợ là oobabooga's textgen WebUI, NovelAI, và TabbyAPI.
NovelAI có [tài liệu riêng cho CFG](https://web.archive.org/web/20240917150051/https://docs.novelai.net/text/cfg.html).

CẢNH BÁO: CFG tăng việc sử dụng VRAM do xử lý nhiều hơn 1 prompt! Nếu bộ nhớ GPU của bạn hết trong khi tạo prompt với CFG bật, hãy xem xét giảm kích thước context, sử dụng model với ít tham số hơn, hoặc tắt CFG hoàn toàn.

---

## Cấu hình

Truy cập cài đặt CFG giống như truy cập Author's note:

![CFGhamburgermenupng](/static/cfg-hamburger.png)

Và đây là giao diện của panel CFG:

![CFGchatpanelpng](/static/cfg-panel.png)

Có bốn dropdown trong panel CFG:

- Chat CFG

  - Giới hạn CFG scale và prompts chỉ cho cuộc trò chuyện này
- Character CFG

  - Giới hạn CFG scale và prompts cho nhân vật được chỉ định
- Global CFG

  - Ghi đè toàn cục CFG scale và prompts (cũng ghi đè model preset!)
- CFG Advanced Settings (trước đây gọi là CFG Prompt Cascading)

  - Nơi để kết hợp prompts từ 3 dropdown trước và đặt insertion depth.

LƯU Ý: Nếu guidance scale được đặt là 1, sẽ không có gì được gửi vì đó là khi CFG ở trạng thái "tắt".

#### Trò chuyện nhóm

Trong trò chuyện nhóm, panel CFG scale trông như thế này:

![CFGpanelgcpng](/static/cfg-groups.png)

Thay đổi chính là character CFG bị loại bỏ và một checkbox có tên `Use Character CFG Scales` xuất hiện trong dropdown chat CFG. Điều này cho phép sử dụng guidance scale của nhân vật hiện tại thay vì guidance scale của chat CFG.

Tiện ích chính của tính năng này là thay đổi scale dựa trên nhu cầu riêng của từng nhân vật.

Ngoài ra, việc đánh dấu vào ô `Character Negatives` trong prompt cascading sẽ thêm các negative prompts độc lập của nhân vật cùng với các prompts của chat (nếu được bật).

---

## Khái niệm

### Điều này có trong Stable Diffusion không?

Có và không. CFG với LLM hoạt động theo cách khác với những gì người ta có thể quen thuộc trong Stable Diffusion. CFG dựa trên LLM hoạt động theo nguyên tắc "prompt mixing". Công thức CFG lấy một positive và negative prompt, sau đó trộn *sự khác biệt* giữa chúng. Từ đó, một prompt kết hợp được gửi và một response được tạo ra!

Đây là hình minh họa để giúp hình dung khái niệm này. Màu đỏ đại diện cho negative prompt, màu xanh đại diện cho neutral prompt, và màu tím đại diện cho kết quả trộn được giải thích. Tất cả khoảng trắng giống nhau trên cả 3 prompts, vì vậy chúng không được sử dụng cho CFG mixing.

![stcfgdiagrampng](/static/cfg-diagram.png)

Nếu bạn muốn biết thêm về CFG và LLM, bài báo gốc của Vermifuge nằm ở đây. Tôi khuyên bạn nên đọc/nghe:

- Paper - [[2306.17806] Stay on topic with Classifier-Free Guidance (arxiv.org)](https://arxiv.org/abs//2306.17806)

- Phiên bản audio - [https://www.youtube.com/watch?v=MGY00YFcyco](https://www.youtube.com/watch?v=MGY00YFcyco)


### Tôi có cần CFG prompts không?

Không! CFG prompts hoàn toàn tùy chọn. Chỉ cần điều chỉnh guidance scale trên `1` cũng sẽ giúp tạo ra hiệu ứng trên responses, có thể làm nổi bật các cuộc trò chuyện và tương tác nhân vật.

### Điều gì tạo nên một CFG prompt tốt?

Vậy, chúng ta đã xác định rằng CFG prompting không giống như negative tags và embeddings của Stable Diffusion. Làm thế nào để chúng ta tạo một prompt?

Cảnh báo: Điều này giả định rằng bạn đã tạo một nhân vật bằng PLists và Ali:Chat. Nếu chưa, hãy thoải mái thử nghiệm với các kỹ thuật prompting khác nhau.

Giả sử tôi có một nhân vật tên là "John". John được cho là cảm thấy vui vẻ và phấn khích mọi lúc từ các ví dụ đối thoại của anh ấy. Tuy nhiên, khi trò chuyện với John, đôi khi anh ấy buồn và chán nản.

Để loại bỏ điều này, CFG đến giải cứu! Chỉ cần làm negative prompt `[John's feelings: sad, depressed]` để giúp loại bỏ các phần buồn. Bạn có thể tùy chọn làm positive prompt `[John's feelings: happy, joyful]` để làm nổi bật thêm các phần vui vẻ của John.

### Positive Prompts

Tôi đã đề cập đến điều này trong phần trước, nhưng tôi muốn nhấn mạnh thêm một chút. Positive prompts được sử dụng để làm nổi bật thêm các phần của một nhân vật. Hãy sử dụng John làm ví dụ của chúng ta một lần nữa. Bằng cách làm cho anh ấy vui vẻ hơn với một positive prompt là `[John's feelings: happy, joyful]`, John nên bắt đầu tạo ra đối thoại với cảm giác vui vẻ hơn so với khi không bao gồm positive prompt.

### Nhưng...

Đây chỉ là **hướng dẫn lỏng lẻo** từ kinh nghiệm với một định dạng nhân vật cụ thể. Có nhiều cách khác để tạo prompts mà bạn nên thử nghiệm. Hãy thoải mái chia sẻ suy nghĩ của bạn với những người dùng khác!

### Guidance Scale

Đây là một quy tắc chung. Guidance scale là `1` có nghĩa là CFG bị vô hiệu hóa. Trên thực tế, SillyTavern sẽ không gửi bất cứ thứ gì đến backend của bạn nếu guidance scale là 1. Guidance scale `>1` sẽ cho các kết quả được hiển thị trong các phần khác ở các mức độ khác nhau.

Tuy nhiên, guidance scale `<1` sẽ cho hiệu ứng *ngược lại* vì negative prompt được sử dụng làm prompt chính ở đây.

Hãy sử dụng ví dụ với John một lần nữa. Negative prompt là `[John's feelings: sad, depressed]` và positive prompt là `[John's feelings: happy, joyful]` với guidance scale là `0.8`.

Điều này sẽ làm nổi bật *negative* prompt hơn và bạn sẽ thấy John bắt đầu hành động buồn hơn bình thường thay vì vui vẻ hơn.

TL;DR: Sử dụng guidance scale là `1.5` và điều chỉnh lên xuống từ đó dựa trên outputs của bạn.

### Prompt Cascading

Negatives và positives có thể được cascaded giữa các loại CFG (các loại là per-chat, per-character, và global overrides). Xem header Configuration để biết thêm thông tin.

### Insertion Depth

Tuân theo quy tắc cơ bản: Càng thấp một thứ gì đó nằm trong prompt, nó càng có ảnh hưởng đến response. Đối với trò chuyện, tôi khuyên sử dụng depth mặc định là `1` vì nó rất linh hoạt với các thành phần khác của SillyTavern.

Tuy nhiên, nếu bạn muốn thử nghiệm, insertion depth là `0` cũng có thể. Tuy nhiên, điều này có thể thay đổi đáng kể cách response của bạn sẽ trông như thế nào và KHÔNG khuyến nghị sử dụng prompt cascading ở đây!
