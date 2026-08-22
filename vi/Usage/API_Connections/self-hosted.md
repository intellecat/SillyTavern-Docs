---
order: 90
icon: desktop-download
route: /vi/usage/how-to-use-a-self-hosted-model/
---

# Mô hình AI tự lưu trữ

!!!warning
Hướng dẫn này dựa trên kinh nghiệm và kiến thức cá nhân của tác giả và không phải là sự thật tuyệt đối. Tất cả các tuyên bố nên được xem với một chút hoài nghi. Nếu bạn có bất kỳ sửa đổi hoặc đề xuất nào, vui lòng liên hệ với chúng tôi trên Discord hoặc gửi PR đến [repository tài liệu SillyTavern](https://github.com/SillyTavern/SillyTavern-Docs).
!!!

## Giới Thiệu

Hướng dẫn này nhằm giúp bạn thiết lập để sử dụng SillyTavern với AI local chạy trên PC của bạn (chúng ta sẽ bắt đầu sử dụng thuật ngữ chính xác từ bây giờ và gọi nó là LLM). Đọc nó trước khi làm phiền mọi người với câu hỏi hỗ trợ kỹ thuật.

### Mô Hình Ngôn Ngữ Lớn tốt nhất là gì?

Không thể trả lời câu hỏi này vì không có thang đo chuẩn hóa về "Tốt nhất". Cộng đồng có đủ tài nguyên và thảo luận đang diễn ra trên Reddit và Discord để hình thành ít nhất một số ý kiến về mô hình được ưa thích / mặc định. Trải nghiệm của bạn có thể khác nhau.

### Cấu hình tốt nhất là gì?

Nếu có một thiết lập tốt nhất hoặc hiển nhiên, liệu có cần phải cấu hình không? Cấu hình tốt nhất là cấu hình phù hợp với bạn. Đó là một quá trình thử và sai.

## Yêu cầu phần cứng và định hướng

Đây là một chủ đề phức tạp, vì vậy tôi sẽ chỉ giữ những điều cần thiết và khái quát hóa.

* Có hàng nghìn LLM miễn phí bạn có thể tải xuống từ Internet, tương tự như cách Stable Diffusion có hàng tấn mô hình bạn có thể lấy để tạo hình ảnh.
* Chạy một LLM chưa sửa đổi yêu cầu một GPU khủng với một tấn VRAM (bộ nhớ GPU). Nhiều hơn bạn sẽ có.
* Có thể giảm yêu cầu VRAM bằng cách nén mô hình bằng kỹ thuật lượng tử hóa, chẳng hạn như GPTQ hoặc AWQ. Điều này làm cho mô hình kém khả năng hơn một chút, nhưng giảm đáng kể yêu cầu VRAM để chạy nó. Đột nhiên, điều này cho phép những người có GPU chơi game như 3080 chạy mô hình 13B. Mặc dù nó không tốt bằng mô hình chưa lượng tử hóa, nó vẫn tốt.
* Nó còn tốt hơn: cũng tồn tại một định dạng mô hình và lượng tử hóa được gọi là GGUF (trước đây là GGML) đã trở thành định dạng lựa chọn cho những người bình thường không có GPU khủng. Điều này cho phép bạn sử dụng LLM mà không cần GPU. Nó sẽ chỉ sử dụng CPU và RAM. Điều này chậm hơn nhiều (có lẽ 15 lần) so với chạy LLM trên GPU bằng GPTQ/AWQ, đặc biệt là trong quá trình xử lý prompt, nhưng khả năng của mô hình vẫn tốt. Người tạo GGUF sau đó đã tối ưu hóa GGUF hơn nữa bằng cách thêm một tùy chọn cấu hình cho phép những người có GPU cấp game offload các phần của mô hình lên GPU, cho phép họ chạy một phần của mô hình với tốc độ GPU (lưu ý rằng điều này không giảm yêu cầu RAM, nó chỉ cải thiện tốc độ tạo của bạn).
* Có các kích thước mô hình khác nhau, được đặt tên dựa trên số lượng tham số mà chúng được đào tạo. Bạn sẽ thấy các tên như 7B, 13B, 30B, 70B, v.v. Bạn có thể nghĩ về chúng như kích thước não của mô hình. Mô hình 13B sẽ có khả năng hơn 7B từ cùng một họ mô hình: chúng được đào tạo trên cùng dữ liệu, nhưng bộ não lớn hơn có thể giữ lại kiến thức tốt hơn và suy nghĩ mạch lạc hơn. Các mô hình lớn hơn cũng yêu cầu nhiều VRAM/RAM hơn.
* Có một số mức độ lượng tử hóa (8-bit, 5-bit, 4-bit, v.v). Càng thấp, mô hình càng suy giảm, nhưng yêu cầu phần cứng càng thấp. Vì vậy, ngay cả trên phần cứng xấu, bạn có thể chạy phiên bản 4-bit của mô hình mong muốn. Thậm chí còn có lượng tử hóa 3-bit và 2-bit nhưng ở thời điểm này, bạn đang đánh một con ngựa chết. Cũng có các loại con lượng tử hóa thêm được đặt tên là k_s, k_m, k_l, v.v. k_m tốt hơn k_s nhưng yêu cầu nhiều tài nguyên hơn.
* Kích thước context (cuộc trò chuyện của bạn có thể dài đến mức nào mà không có mô hình bỏ các phần của nó) cũng ảnh hưởng đến yêu cầu VRAM/RAM. May mắn thay, đây là một cài đặt có thể cấu hình, cho phép bạn sử dụng context nhỏ hơn để giảm yêu cầu VRAM/RAM. (Lưu ý: kích thước context của các mô hình dựa trên Llama2 là 4k. Mistral được quảng cáo là 8k, nhưng thực tế là 4k.)
* Vào khoảng năm 2023, NVIDIA đã thay đổi trình điều khiển GPU của họ để nếu bạn cần nhiều VRAM hơn GPU của bạn có, thay vì tác vụ bị crash, nó sẽ bắt đầu sử dụng RAM thông thường làm dự phòng. Điều này sẽ làm hỏng tốc độ viết của LLM, nhưng mô hình vẫn sẽ hoạt động và cho chất lượng đầu ra như nhau. May mắn thay, hành vi này [có thể bị vô hiệu hóa](https://nvidia.custhelp.com/app/answers/detail/a_id/5490).

Với tất cả những điều trên, yêu cầu phần cứng và hiệu suất hoàn toàn khác nhau tùy thuộc vào họ mô hình, loại mô hình, kích thước mô hình, phương pháp lượng tử hóa, v.v.

#### Model size calculator
Bạn có thể sử dụng [Nyx's Model Size Calculator](https://huggingface.co/spaces/NyxKrage/LLM-Model-VRAM-Calculator) để xác định bạn cần bao nhiêu RAM/VRAM.

Hãy nhớ, bạn muốn chạy mô hình lớn nhất, ít được lượng tử hóa nhất có thể vừa với bộ nhớ của bạn, tức là không gây ra [disk swapping](https://serverfault.com/a/48487).

## Tải Xuống LLM

Để bắt đầu, bạn sẽ cần tải xuống một LLM. Nơi phổ biến nhất để tìm và tải xuống LLM là trên HuggingFace. Có hàng nghìn mô hình có sẵn. Một cách tốt để tìm mô hình GGUF là kiểm tra trang tài khoản của bartowski: <https://huggingface.co/bartowski>. Nếu bạn không muốn GGUF, anh ấy liên kết trang mô hình gốc nơi bạn có thể tìm thấy các định dạng khác cho cùng mô hình đó.

Trên trang của một mô hình nhất định, bạn sẽ tìm thấy một loạt các file.

* Bạn có thể không cần tất cả chúng! Đối với GGUF, bạn chỉ cần file mô hình .gguf (thường 4-11GB). Nếu bạn tìm thấy nhiều file lớn, nó thường là tất cả các lượng tử hóa khác nhau của cùng một mô hình, bạn chỉ cần chọn một.
* Đối với các file .safetensors (có thể là GPTQ hoặc AWQ hoặc HF quantized hoặc unquantized), nếu bạn thấy một chuỗi số trong tên file như model-00001-of-00003.safetensors, thì bạn cần tất cả 3 file .safetensors đó + tất cả các file khác trong repository (tokenizer, configs, v.v.) để có mô hình đầy đủ.
* Tính đến tháng 1 năm 2024, Mixtral MOE 8x7B được coi rộng rãi là nghệ thuật tiên tiến cho LLM local. Nếu bạn có 32GB RAM để chạy nó, nhất định hãy thử. Nếu bạn có ít hơn 32GB RAM, thì sử dụng Kunoichi-DPO-v2-7B, mặc dù kích thước của nó nhưng nó rất xuất sắc ngay từ đầu.

### Hướng dẫn tải xuống Kunoichi-DPO-v2-7B

Chúng ta sẽ sử dụng mô hình Kunoichi-DPO-v2-7B cho phần còn lại của hướng dẫn này. Đây là một mô hình xuất sắc dựa trên Mistral 7B, chỉ yêu cầu 7GB RAM, và vượt xa trọng lượng của nó. Lưu ý: Kunoichi sử dụng prompting Alpaca.

* Truy cập <https://huggingface.co/brittlewis12/Kunoichi-DPO-v2-7B-GGUF>
* Nhấp 'Files and versions'. Bạn sẽ thấy danh sách một số file. Đây là tất cả cùng một mô hình nhưng được cung cấp trong các tùy chọn lượng tử hóa khác nhau. Nhấp vào file 'kunoichi-dpo-v2-7b.Q6_K.gguf', cho chúng ta lượng tử hóa 6-bit.
* Nhấp nút 'download'. Quá trình tải xuống của bạn sẽ bắt đầu.

### Cách xác định loại mô hình

Những người tải mô hình tốt như TheBloke đặt tên mô tả. Nhưng nếu họ không:

* Tên file kết thúc bằng .gguf: mô hình CPU GGUF (hiển nhiên)
* Tên file kết thúc bằng .safetensors: có thể là unquantized, hoặc HF quantized, hoặc GPTQ, hoặc AWQ
* Tên file là pytorch-***.bin: giống như trên, nhưng đây là định dạng file mô hình cũ hơn cho phép mô hình thực thi script Python tùy ý khi mô hình được tải và được coi là không an toàn. Bạn vẫn có thể sử dụng nó nếu bạn tin tưởng người tạo mô hình, hoặc tuyệt vọng, nhưng hãy chọn .safetensors nếu bạn có tùy chọn.
* config.json tồn tại? Xem nếu nó có quant_method.
* q4 có nghĩa là lượng tử hóa 4-bit, q5 là lượng tử hóa 5-bit, v.v.
* Bạn thấy một số như -16k? Đó là kích thước context tăng (tức là cuộc trò chuyện của bạn có thể dài đến mức nào trước khi mô hình quên phần đầu của cuộc trò chuyện)! Lưu ý rằng kích thước context cao hơn yêu cầu nhiều VRAM hơn.

## Cài Đặt máy chủ LLM: Oobabooga hoặc KoboldAI

Với LLM bây giờ trên PC của bạn, chúng ta cần tải xuống một công cụ sẽ hoạt động như một người trung gian giữa SillyTavern và mô hình: nó sẽ tải mô hình và expose chức năng của nó dưới dạng web API HTTP local mà SillyTavern có thể giao tiếp, cùng cách mà SillyTavern giao tiếp với các dịch vụ web trả phí như OpenAI GPT hoặc Claude. Công cụ bạn sử dụng nên là KoboldAI hoặc Oobabooga (hoặc các công cụ tương thích khác).

Hướng dẫn này bao gồm cả hai tùy chọn, bạn chỉ cần một.

!!!warning
Nếu bạn đang lưu trữ SillyTavern trên Docker, hãy sử dụng **http://host.docker.internal:\<port\>** thay vì **http://127.0.0.1:\<port\>**. Điều này là do SillyTavern kết nối với endpoint API từ máy chủ chạy trong container Docker. Ngăn xếp mạng của Docker tách biệt với của host, và vì vậy các giao diện loopback không được chia sẻ.
!!!

### Tải xuống và sử dụng KoboldCpp (Không cần cài đặt, mô hình GGUF)

1. Truy cập https://koboldai.org/cpp nơi bạn sẽ thấy phiên bản mới nhất với các file khác nhau bạn có thể tải xuống.
Tại thời điểm viết, phiên bản CUDA mới nhất họ liệt kê là cu12 sẽ hoạt động tốt nhất trên GPU Nvidia hiện đại, nếu bạn có GPU cũ hơn hoặc một thương hiệu khác, bạn có thể sử dụng koboldcpp.exe thông thường. Nếu bạn có CPU cũ, có thể KoboldCpp sẽ crash khi bạn cố gắng tải mô hình, trong trường hợp đó hãy thử phiên bản _oldcpu để xem nó có giải quyết vấn đề của bạn không.
2. KoboldCpp không cần được cài đặt, khi bạn khởi động KoboldCpp, bạn sẽ ngay lập tức có thể chọn mô hình GGUF của mình như mô hình được liên kết ở trên bằng nút Browse bên cạnh trường Model.
3. Theo mặc định, KoboldCpp chạy ở tối đa context 4K ngay cả khi bạn đặt cao hơn trong SillyTavern, nếu bạn muốn chạy mô hình ở context cao hơn, hãy đảm bảo điều chỉnh thanh trượt context trên màn hình này trước khi khởi chạy mô hình. Hãy nhớ rằng kích thước context nhiều hơn có nghĩa là yêu cầu bộ nhớ (video) cao hơn, nếu bạn đặt quá cao hoặc tải mô hình quá lớn cho hệ thống của bạn, KoboldCpp sẽ tự động bắt đầu sử dụng CPU của bạn cho các layer không thể vừa trên GPU của bạn, điều này sẽ chậm hơn nhiều.
4. Nhấp Launch, nếu mọi thứ suôn sẻ, một trang web mới sẽ mở với KoboldAI Lite nơi bạn có thể kiểm tra xem mọi thứ có hoạt động đúng không.
5. Mở SillyTavern và nhấp API Connections (nút thứ 2 trong thanh trên cùng)
6. Đặt API thành Text Completion và API Type thành KoboldCpp.
7. Đặt server URL thành <http://127.0.0.1:5001/> hoặc liên kết mà KoboldCpp cung cấp cho bạn trong trường hợp nó không chạy trên cùng hệ thống (Bạn có thể kích hoạt chế độ Remote Tunnel của KoboldCpp để có được liên kết có thể truy cập từ bất cứ đâu).
8. Nhấp Connect. Nó sẽ kết nối thành công và phát hiện kunoichi-dpo-v2-7b.Q6_K.gguf là mô hình.
9. Trò chuyện với một nhân vật để kiểm tra xem nó có hoạt động không.

### Mẹo để tối ưu hóa tốc độ của KoboldCpp
1. Flash Attention sẽ giúp giảm yêu cầu bộ nhớ, nó có thể nhanh hơn hoặc chậm hơn tùy thuộc vào hệ thống của bạn và sẽ cho phép bạn vừa nhiều layer hơn trên GPU của bạn so với mặc định.
2. KoboldCpp sẽ để lại một số không gian cho phần mềm khác khi nó đoán các layer để ngăn chặn vấn đề, nếu bạn có ít chương trình mở và không thể vừa mô hình hoàn toàn trong GPU, bạn có thể thêm một vài layer bổ sung.
3. Nếu mô hình sử dụng quá nhiều bộ nhớ cho kích thước context, bạn có thể giảm điều này bằng cách Quantizing the KV. Điều này sẽ giảm chất lượng đầu ra nhưng có thể giúp bạn đặt nhiều layer hơn trên GPU. Để làm điều này, bạn vào tab Tokens trong KoboldCpp và sau đó vô hiệu hóa Context Shifting và bật Flash Attention. Điều này sẽ mở khóa thanh trượt Quantized KV Cache, số thấp hơn có nghĩa là ít bộ nhớ / trí thông minh của mô hình hơn.
4. Chạy KoboldCpp trên một hệ thống chậm hơn nơi mà xử lý prompt mất nhiều thời gian? Context Shifting hoạt động tốt nhất khi bạn tránh sử dụng Lorebook, randomization hoặc các tính năng khác thay đổi đầu vào một cách động. Để Context shifting được bật, KoboldCpp sẽ giúp bạn tránh thời gian xử lý lại dài.

### Cài Đặt Oobabooga

!!!note
Tùy thuộc vào cách bạn đã cài đặt Oobabooga, đường dẫn file có thể hơi khác nhau; tức là `/text-generation-webui/user_data` nếu bạn cài đặt qua git clone, và `/text-generation-webui-main/user_data` nếu bạn dùng phương pháp .zip.
!!!

Đây là thủ tục cài đặt chính xác/chống ngu hơn:

1. git clone <https://github.com/oobabooga/text-generation-webui> (hoặc tải xuống repo của họ dưới dạng .zip trong trình duyệt của bạn, sau đó giải nén nó)
2. Chạy `start_windows.bat` hoặc bất kỳ OS nào của bạn
3. Khi được hỏi, chọn loại GPU của bạn. Ngay cả khi bạn dự định sử dụng GGUF/CPU, nếu GPU của bạn có trong danh sách, hãy chọn nó bây giờ, vì nó sẽ cho bạn tùy chọn sử dụng tối ưu hóa tốc độ sau này được gọi là GPU sharding (mà không cần phải cài đặt lại từ đầu). Nếu bạn không có dGPU cấp game (NVIDIA, AMD), hãy chọn None.
4. Đợi quá trình cài đặt hoàn tất
5. Đặt kunoichi-dpo-v2-7b.Q6_K.gguf vào `text-generation-webui/user_data/models`
6. Mở `text-generation-webui/user_data/CMD_FLAGS.txt`, xóa mọi thứ bên trong và viết: `--api`
7. Khởi động lại Oobabooga
8. Truy cập <http://127.0.0.1:5000/docs>. Nó có tải trang FastAPI không? Nếu không, bạn đã làm sai ở đâu đó.

### Tải mô hình của chúng ta trong Oobabooga

1. Mở <http://127.0.0.1:7860/> trong trình duyệt của bạn
2. Nhấp tab Model
3. Trong dropdown, chọn mô hình Kunoichi DPO v2 của chúng ta. Nó nên tự động chọn loader llama.cpp.
4. (Tùy chọn) Chúng ta đã đề cập 'GPU offload' nhiều lần trước đó: đó là cài đặt n-gpu-layers trên trang này. Nếu bạn muốn sử dụng nó, hãy đặt giá trị trước khi tải mô hình. Như một tham khảo cơ bản, đặt nó thành 30 sử dụng dưới 6GB VRAM cho mô hình 13B trở xuống. (nó thay đổi với kiến trúc mô hình và kích thước)
5. Nhấp Load


### Cấu hình SillyTavern để giao tiếp với Oobabooga

1. Nhấp API Connections (nút thứ 2 trong thanh trên cùng)
2. Đặt API thành Text Completion
3. Đặt API Type thành Default (Oobabooga)
4. Đặt server URL thành <http://127.0.0.1:5000/>
5. Nhấp Connect. Nó sẽ kết nối thành công và phát hiện kunoichi-dpo-v2-7b.Q6_K.gguf là mô hình.
6. Trò chuyện với một nhân vật để kiểm tra xem nó có hoạt động không

## Kết Luận

Chúc mừng, bây giờ bạn nên có một LLM local hoạt động.
