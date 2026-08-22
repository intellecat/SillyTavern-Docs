---
order: 50
route: /vi/usage/core-concepts/authors-note/
---

# Author's Note

## Nó là gì?

Author's Note là một công cụ mạnh mẽ để tùy chỉnh phản hồi của AI, cho phép chèn một đoạn văn bản vào prompt ở bất kỳ vị trí nào và với bất kỳ tần suất nào bạn muốn.

## Cách sử dụng

Author's Note có thể được tìm thấy trong menu Options ở phía bên trái của thanh nhập chat.

| Options Menu                          | Author's Note Panel                    |
|---------------------------------------|----------------------------------------|
| ![](/static/extensions/note-menu.png) | ![](/static/extensions/note-panel.png) |

## Cấu hình Author's Notes

### Author's Note riêng cho Chat

Ô ở đầu bảng Author's Note chứa Author's Note cho chat hiện tại của bạn.

**Nội dung của ô này không được tự động chuyển sang bất kỳ chat mới nào.**

### Tùy chọn vị trí

#### After Scenario

Điều này đặt Author's Note ở phía trên của context sau phần 'Scenario' trong Character Definition. Nếu không có scenario được chỉ định, nó sẽ được đặt sau phần cuối cùng của Character Definition và trước các Example messages.

#### In-chat

Điều này đặt Author's Note vào lịch sử chat ở độ sâu được chỉ định.

Depth 0 = đặt ở cuối cùng của lịch sử chat.

Depth 4 = đặt trước 3 tin nhắn gần nhất trong lịch sử chat, khiến nó trở thành thực thể thứ 4 trong lịch sử chat.

_Càng gần cuối prompt, Author's Note càng có tác động nhiều đến phản hồi AI tiếp theo._

### Tần suất chèn

Đây là tần suất bạn muốn Author's Note được đưa vào chat.

Frequency 0 = Author's Note sẽ không bao giờ được chèn.

Frequency 1 = Author's Note sẽ được chèn với mỗi lần người dùng nhập prompt.

Frequency 4 = Author's Note sẽ được chèn vào mỗi lần nhập prompt thứ 4 của người dùng.

### Default Author's Note

Ô ở cuối bảng chứa Default Author's Note sẽ được áp dụng cho mỗi chat mới.

## Các trường hợp sử dụng phổ biến

### Nhắc nhở AI về định dạng phản hồi

Author's Note có thể được sử dụng để chỉ định cách AI nên viết phản hồi của nó.

- [Your next response must be 300 tokens in length.]
- [Write your next reply in the style of Edgar Allan Poe]
- [Use markdown italics to signify unspoken actions, and quotation marks to specify spoken word.]

### Củng cố hướng dẫn

- [Remember the instructions you were given at the beginning of this chat.]

### Như World Info tạm thời, Character Bias hoặc Instruct cho các model không phải Instruct

- [\{\{char\}\} is in the library]
- [\{\{user\}\} has a fresh wound to his leg, so won't be able to run away.]
- [\{\{char\}\} cannot speak and must communicate using hand signals.]
