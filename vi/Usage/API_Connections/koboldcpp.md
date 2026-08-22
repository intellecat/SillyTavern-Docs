---
route: /vi/usage/api-connections/koboldcpp/
---

# KoboldCpp

KoboldCpp là một API độc lập cho các mô hình GGML và GGUF.

[VRAM Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) này của Nyx sẽ cho bạn biết mô hình của bạn yêu cầu khoảng bao nhiêu RAM/VRAM.

## Hướng Dẫn Nhanh Nvidia GPU

Hướng dẫn này giả định bạn đang sử dụng Windows.

* Tải xuống bản phát hành mới nhất: <https://github.com/LostRuins/koboldcpp/releases>
* Khởi chạy KoboldCpp. Bạn có thể thấy cửa sổ bật lên từ Microsoft Defender, nhấp `Run Anyway`.
* Tính đến phiên bản 1.58, KoboldCpp sẽ trông như thế này:

![KoboldCpp 1.58](/static/koboldcpp.png)

* Trong tab `Quick Launch`, chọn mô hình và `Context Size` mà bạn thích.
* Chọn `Use CuBLAS` và đảm bảo văn bản màu vàng bên cạnh `GPU ID` khớp với GPU của bạn.
* Không đánh dấu `Low VRAM`, ngay cả khi bạn có VRAM thấp.
* Trừ khi bạn có GPU Nvidia series 10 hoặc cũ hơn, bỏ đánh dấu `Use QuantMatMul (mmq)`.
* `GPU Layers` nên được điền khi bạn tải mô hình của mình. Để nó ở đó bây giờ.
* Trong tab `Hardware`, đánh dấu `High Priority`.
* Nhấp `Save` để bạn không phải cấu hình KoboldCpp mỗi lần khởi chạy.
* Nhấp `Launch` và đợi mô hình tải.

Bạn sẽ thấy điều gì đó như thế này:

```txt
Load Model OK: True
Embedded Kobold Lite loaded.
Starting Kobold API on port 5001 at http://localhost:5001/api/
Starting OpenAI Compatible API on port 5001 at http://localhost:5001/v1/
======
Please connect to custom endpoint at http://localhost:5001
```

Bây giờ bạn có thể kết nối với KoboldCpp trong SillyTavern với `http://localhost:5001` làm API URL và bắt đầu trò chuyện.

**Chúc mừng! Bạn đã hoàn thành!**

Kiểu như vậy.

### GPU Layers

KoboldCpp đang hoạt động, nhưng bạn có thể cải thiện hiệu suất bằng cách đảm bảo rằng càng nhiều layer càng tốt được offload lên GPU. Bạn sẽ thấy điều gì đó như thế này trong terminal:

```txt
llm_load_tensors: offloading 9 repeating layers to GPU
llm_load_tensors: offloaded 9/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  7043.34 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1479.19 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   578.81 MiB
```

Đừng sợ các con số; phần này dễ hơn nó trông. `CPU buffer size` đề cập đến lượng RAM hệ thống đang được sử dụng. Bỏ qua cái đó. `CUDA0 buffer size` đề cập đến lượng GPU VRAM đang được sử dụng. `CUDA_Host KV buffer size` và `CUDA0 KV buffer size` đề cập đến lượng GPU VRAM được dành riêng cho context của mô hình của bạn. Trong trường hợp này, KoboldCpp đang sử dụng khoảng 9 GB VRAM.

Tôi có 12 GB VRAM và chỉ 2 GB VRAM được sử dụng cho context, vì vậy tôi còn khoảng 10 GB VRAM để tải mô hình. Vì 9 layer sử dụng khoảng 7 GB VRAM và `7000 / 9 = 777.77` chúng ta có thể giả định mỗi layer sử dụng khoảng `777.77 MIB` VRAM. `10,000 MIB / 777.77 = 12.8`, vì vậy tôi sẽ làm tròn xuống và tải 12 layer với mô hình này từ bây giờ.

Bây giờ hãy tự tính toán của bạn bằng cách sử dụng mô hình, kích thước context và VRAM cho hệ thống của bạn, và khởi động lại KoboldCpp:

* Nếu bạn thông minh, bạn đã nhấp `Save` trước đó và bây giờ bạn có thể tải cấu hình trước đó của mình bằng `Load`. Nếu không, hãy chọn cùng cài đặt bạn đã chọn trước đó.
* Thay đổi `GPU Layers` thành số được tối ưu hóa VRAM mới của bạn (12 layer trong trường hợp của tôi).
* Nhấp `Save` để lưu cấu hình đã cập nhật của bạn.

Bây giờ bạn sẽ thấy điều gì đó như thế này:

```txt
llm_load_tensors: offloading 12 repeating layers to GPU
llm_load_tensors: offloaded 12/33 layers to GPU
llm_load_tensors:        CPU buffer size = 25215.88 MiB
llm_load_tensors:      CUDA0 buffer size =  9391.12 MiB
....................................................................................................
llama_kv_cache_init:  CUDA_Host KV buffer size =  1286.25 MiB
llama_kv_cache_init:      CUDA0 KV buffer size =   771.75 MiB
```

KoboldCpp đang sử dụng khoảng 11.5 GB trong số 12 GB VRAM của tôi. Điều này sẽ hoạt động tốt hơn nhiều so với các cài đặt được tạo tự động bởi KoboldCpp.

**Chúc mừng! Bạn (thực sự) đã hoàn thành!**

Để có cái nhìn sâu hơn về cài đặt KoboldCpp, hãy xem [Simple Llama + SillyTavern Setup Guide](https://rentry.org/llama_v2_sillytavern) của Kalomaze.
