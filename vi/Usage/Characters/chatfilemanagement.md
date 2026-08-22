---
order: 80
route: /vi/usage/core-concepts/chatfilemanagement/
---

# Quản lý tệp Chat

Trang này mô tả các cách bạn có thể quản lý các tệp chat AI của mình.

!!!info Lưu ý
Một số tùy chọn này có sẵn trong hộp thoại "Manage chat files" mở từ menu tùy chọn ở góc dưới bên trái.
!!!

## Solo Chats vs Group Chats

Cách đơn giản nhất để sử dụng character card là Solo chat; chỉ cần nhấp vào card của họ và bắt đầu chat.

Khi bạn đã có một vài character cards, bạn cũng có thể sử dụng nút "Create New Chat Group" để tạo một [group chat](/Usage/Characters/groupchats.md) bao gồm nhiều nhân vật sau đó sẽ tương tác với nhau và với bạn.

## Nhập chat

**Nhập các cuộc chat từ Character.AI vào SillyTavern.**

Để nhập các cuộc chat và bot từ Character.AI, hãy sử dụng tiện ích mở rộng trình duyệt CAI Tools: [https://github.com/irsat000/CAI-Tools](https://github.com/irsat000/CAI-Tools).

Các chương trình và công cụ khác mà bạn có thể nhập chat từ đó bao gồm:

* TavernAI (original): <https://github.com/TavernAI/TavernAI>
* Text Generation WebUI (oobabooga): <https://github.com/oobabooga/text-generation-webui>
* Agnai: <https://github.com/agnaistic/agnai>
* KoboldAI Lite: <https://github.com/LostRuins/lite.koboldai.net>
* RisuAI: <https://github.com/kwaroran/RisuAI>

## Xuất dưới dạng .jsonl

Khi nhấp vào "Manage chat files", mỗi mục trong danh sách tệp chat sẽ có một nút để xuất nó ở định dạng có thể được nhập lại như cũ. Sử dụng điều này để chia sẻ hoặc di chuyển các cuộc chat bao gồm tất cả metadata của chúng (nhưng không bao gồm hình ảnh và tệp đính kèm).

Nếu bạn quan tâm đến quyền riêng tư, hãy chắc chắn kiểm tra tệp JSONL đã xuất và xóa bất kỳ thứ gì bạn không muốn chia sẻ.

## Xuất dưới dạng .txt

Bạn cũng có thể xuất phiên bản chỉ văn bản đơn giản hóa với nút "Download chat as plain text document". Nó không thể được nhập lại vì nó mất metadata quan trọng!

## Checkpoints

"Checkpoints" là bản sao của chat hiện tại, theo nghĩa là chúng sao chép tất cả các tin nhắn từ chat đã cho cho đến một điểm nhất định và chúng lưu trữ một liên kết đến nguồn (theo tên tệp chat).

Từ nút ba chấm ở bên phải của mỗi tin nhắn chat, bạn có hai cách để tạo checkpoints:

* "Create Branch" sẽ sao chép chat hiện tại cho đến tin nhắn đó và chuyển sang nó
* "Create Checkpoint" sẽ sao chép chat hiện tại cho đến tin nhắn đó, yêu cầu tên và tạo nó nhưng KHÔNG chuyển sang nó

Bạn có thể coi chúng gần giống như "open link in new tab" và "open link in new tab in the background" trong trình duyệt.

Bạn có thể quay lại parent từ một checkpoint bằng cách vào nút menu burger ở bên trái của hộp văn bản tin nhắn, sau đó nhấp vào "Back to parent chat".

## Đổi tên Chat

Theo mặc định, các tệp chat được đặt tên với ngày và giờ chúng được bắt đầu.

Bạn có thể thay đổi điều này bằng cách nhấp vào biểu tượng bút chì và nhập tên mới.

Lưu ý rằng điều này sẽ phá vỡ các liên kết đến chat đó từ các checkpoints (vì chúng được liên kết theo tên tệp chat).
