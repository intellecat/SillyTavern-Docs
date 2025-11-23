---
order: 109
route: /installation/updating/migration-guide-1-09/
---

# Hướng dẫn di chuyển 1.9.0

## Làm thế nào để di chuyển sang nhánh mới nếu tôi sử dụng main/dev?

_**Nó được khuyến nghị thực hiện cài đặt mới.**_ Tuy nhiên, nếu bạn muốn sử dụng bản sao SillyTavern hiện có, vui lòng làm theo hướng dẫn dưới đây.

**QUAN TRỌNG!** Trước khi làm bất cứ điều gì, hãy tạo *một bản sao lưu hoàn chỉnh* của cài đặt của bạn. Bạn có thể *mất dữ liệu* trong quá trình này, vì vậy đừng bỏ qua cảnh báo này.

Không chắc chắn về tệp nào cần sao lưu? Xem danh sách tại đây: [Cách cập nhật SillyTavern](/Installation/Updating/index.md#updating-from-1120-to-1120)

### Cài đặt git

1. Mở một dòng lệnh terminal (cmd, PowerShell, Termux, v.v.) trong thư mục cài đặt SillyTavern của bạn.
2. Gõ `git fetch` và sau đó `git pull` để kéo các bản cập nhật.
3. Bạn có thể mất cài đặt của mình. Bạn đã tạo bản sao lưu chưa? `git switch release` hoặc `git switch staging` sẽ thay đổi nhánh của bạn, tương ứng
4. Bỏ qua mục tiếp theo nếu bạn không có lỗi. Bạn có thể có một cái gì đó như:
   ```
   error: Your local changes to the following files would be overwritten by checkout:
        config.conf
        public/css/bg_load.css
        public/settings.json
   ```
   Bạn sẽ thấy danh sách các tệp bị ảnh hưởng. Nếu bạn không quan tâm đến việc các tệp cài đặt đó bị thay thế `git switch -f release` hoặc `git switch -f staging` sẽ đặt nhánh của bạn.
   Nếu bạn quan tâm đến việc lưu các thay đổi đó, hãy khôi phục từ bản sao lưu.

5. Gõ `npm install` và sau đó `npm run start` để kiểm tra xem mọi thứ hoạt động chính xác.
6. Tận hưởng! Khôi phục dữ liệu của bạn từ bản sao lưu nếu cần.

### fatal: invalid reference: release

Điều này có thể xảy ra nếu bạn chỉ clone một nhánh từ remote cũ (trước khi di chuyển sang repository tổ chức). Để khắc phục điều này, bạn cần thêm và tìm nạp một nhánh từ remote mới:

```
git remote add st https://github.com/SillyTavern/SillyTavern
git fetch st
git checkout -t st/release
```

Sau đó tiến hành từ bước 5.

### Cài đặt ZIP

Không có gì thay đổi cho bạn. Chỉ cần tải xuống ZIP nhánh/phát hành như thường lệ.
