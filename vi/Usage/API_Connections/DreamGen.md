---
route: /vi/usage/api-connections/dreamgen/
---

# DreamGen

DreamGen là một ứng dụng và API dành cho nhập vai và viết truyện được hỗ trợ bởi AI. Họ có gói miễn phí, cũng như gói đăng ký trả phí cho phép truy cập không giới hạn hàng tháng vào các mô hình tạo văn bản chất lượng cao được tạo riêng cho mục đích nhập vai AI và viết truyện có thể điều khiển. Tạo tài khoản để bắt đầu: <https://dreamgen.com/>.

Tín dụng (miễn phí) được đặt lại vào đầu mỗi tháng dương lịch. Xem [pricing](https://dreamgen.com/pricing) để biết chi phí tín dụng cho từng mô hình và [usage](https://dreamgen.com/account/usage) để xem tín dụng còn lại của bạn.

## Kết Nối Tới DreamGen

### Lấy API Key

Truy cập trang [DreamGen API keys](https://dreamgen.com/account/api-keys) và nhấp nút "New API Key". Đảm bảo API Key được sao chép vào clipboard của bạn.

![Create New DreamGen API key](/static/dreamgen/dreamgen_api_keys_new.jpg)
![Copy DreamGen API key](/static/dreamgen/dreamgen_api_keys_copy.jpg)

### Kết Nối

1. Vào cài đặt kết nối của SillyTavern.
2. Chọn API: Text Completion
3. Chọn API Type: DreamGen
4. Nhập API key
5. (tùy chọn) Chọn một mô hình

![Connecting to DreamGen](/static/dreamgen/dreamgen_st_connection.png)

## Mô Hình

DreamGen API cung cấp nhiều mô hình với các kích thước khác nhau.

- Lucid Max (trong API được gọi là `lucid-v1-max` hoặc `lucid-v1-extra-large`)
- Lucid Base (trong API được gọi là `lucid-v1-base` hoặc `lucid-v1-medium`) -- tương ứng với [Lucid V1 Nemo](https://dreamgen.com/docs/models/lucid-v1/huggingface) có sẵn trọng số.

Lucid Base sử dụng ít tín dụng hơn và nhanh hơn, trong khi Lucid Max sáng tạo hơn và có khả năng xử lý các hướng dẫn và câu chuyện phức tạp hơn.

## Cài Đặt

Các mô hình DreamGen Lucid V1 sử dụng phần mở rộng của chat template Llama 3 được tối ưu hóa cho nhập vai và viết lách. Chúng hoạt động tốt nhất với một system prompt cụ thể.

Chúng tôi đề xuất mạnh mẽ bắt đầu với một trong các master preset sau:

- Đảm bảo rằng bạn đã bật instruct mode và chọn tất cả các hộp kiểm khi import.
- [DreamGen Lucid V1 Role-Play preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/role-play)
- [DreamGen Lucid V1 Story preset](https://dreamgen.com/docs/models/lucid-v1/sillytavern/master-preset/story)

Các preset này đi kèm với hỗ trợ tích hợp cho `/sys` để gửi hướng dẫn đến mô hình. Bạn có thể sử dụng chúng để điều khiển cốt truyện hoặc kiểm soát hành động của nhân vật.

![DreamGen preset selected](/static/dreamgen/dreamgen_st_preset.png)

Các tài nguyên khác:

- [**Hướng dẫn chi tiết DreamGen + SillyTavern**](https://dreamgen.com/docs/models/lucid-v1/sillytavern)
- [Tài liệu chi tiết về định dạng prompt Lucid V1](https://dreamgen.com/docs/models/lucid-v1).
- [Demo nhập vai DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
- [Demo viết truyện DreamGen + SillyTavern](https://imgur.com/a/dreamgen-lucid-sillytavern-writing-demo-JLv5iO3)
- [Mẹo tạo kịch bản của riêng bạn](https://v2.dreamgen.com/docs/scenario-editor)

## FAQ

### Làm thế nào để tạo phản hồi dài hơn hoặc ngắn hơn?

Bạn có thể thiết lập `Last Assistant Prefix` trong formatting presets của bạn.

Đối với tin nhắn dài:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at least 100 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Đối với tin nhắn ngắn:

```txt
<|start_header_id|>user<|end_header_id|>

The next message is from {{char}} and is at most 50 words long<|eot_id|><|start_header_id|>writer character {{char}}<|end_header_id|>

```

Đảm bảo giữ nguyên tất cả các dòng mới, bao gồm cả hai dòng ở cuối.

![Long Message Prefix](/static/dreamgen/dreamgen_st_long_response_prefix.png)

Bạn cũng có thể bao gồm mô tả phong cách viết trong card hoặc system prompt của bạn, ví dụ:

```txt
## Style

<your description>
```

Xem ["Style" documentation](https://v2.dreamgen.com/docs/scenario-editor#style) để tìm hiểu thêm và xem một số ví dụ.

### Làm thế nào để điều khiển nhập vai / câu chuyện?

Sử dụng tùy chọn `/sys` để gửi hướng dẫn đến mô hình. Một số ví dụ:

> The innkeeper offers Daria and the others a pint of ale.

> The next message is from Draco and should be at least 200 words, focusing on his inner conflict about the decision.

[Xem hoạt động thực tế.](https://imgur.com/a/dreamgen-lucid-sillytavern-roleplay-demo-bhzQpto)
