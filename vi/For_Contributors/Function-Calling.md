---
order: -10
icon: code-review
route: /for-contributors/function-calling/
---

# Function Calling

Function Calling cho phép thêm chức năng động vào extension của bạn bằng cách để LLM sử dụng dữ liệu có cấu trúc mà bạn có thể sử dụng để kích hoạt một chức năng cụ thể của extension.

## Các trường hợp sử dụng ví dụ

1. Truy vấn các API bên ngoài để lấy thông tin bổ sung (tin tức, thời tiết, tìm kiếm web, v.v.).
2. Thực hiện các phép tính hoặc chuyển đổi dựa trên đầu vào của người dùng.
3. Lưu trữ và gọi lại các ký ức hoặc sự kiện quan trọng, bao gồm RAG và truy vấn cơ sở dữ liệu.
4. Đưa tính ngẫu nhiên thực sự vào cuộc trò chuyện (xúc xắc, tung đồng xu, v.v.).

## Các extension chính thức được hỗ trợ sử dụng function calling

1. [Image Generation](/extensions/Stable-Diffusion.md) (tích hợp sẵn) - tạo hình ảnh dựa trên lời nhắc của người dùng.
2. [Web Search](/extensions/WebSearch.md) - kích hoạt tìm kiếm web cho một truy vấn.
3. [RSS](https://github.com/SillyTavern/Extension-RSS/) - lấy tin tức mới nhất từ nguồn cấp RSS.
4. [AccuWeather](https://github.com/SillyTavern/Extension-AccuWeather) - lấy thông tin thời tiết từ AccuWeather.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - tung xúc xắc cho các trò chơi D&D.

## Yêu cầu tiên quyết và hạn chế

1. Tính năng này chỉ có sẵn cho một số nguồn Chat Completion nhất định: OpenAI, Claude, MistralAI, Groq, Cohere, OpenRouter, AI21, Google AI Studio, Google Vertex AI, DeepSeek, AI/ML API và các nguồn Custom API.
2. Các API Text Completion không hỗ trợ function calls, nhưng một số backend được lưu trữ cục bộ như Ollama và TabbyAPI có thể chạy ở chế độ tương thích OpenAI tùy chỉnh dưới Chat Completion.
3. Sự hỗ trợ cho function calling phải được người dùng cho phép rõ ràng trước. Điều này được thực hiện bằng cách bật tùy chọn "Enable function calling" trong bảng AI Response Configuration.
4. Không có gì đảm bảo rằng một LLM sẽ thực hiện bất kỳ function call nào. Hầu hết chúng đều yêu cầu "kích hoạt" rõ ràng thông qua prompt (ví dụ: người dùng yêu cầu "Tung xúc xắc", "Lấy thời tiết", v.v.).
5. Không phải tất cả các prompt đều có thể kích hoạt tool call. Các continuation, impersonation, background ('quiet') prompt không được phép kích hoạt tool call. Chúng vẫn có thể sử dụng các tool call thành công trong quá khứ trong phản hồi của chúng.

## Cách tạo một function tool

### Kiểm tra xem tính năng có được hỗ trợ không

Để xác định xem tính năng function tool calling có được hỗ trợ hay không, bạn có thể gọi `isToolCallingSupported` từ đối tượng `SillyTavern.getContext()`. Điều này sẽ kiểm tra xem API hiện tại có hỗ trợ function tool calling và nó có được bật trong cài đặt hay không. Dưới đây là ví dụ về cách kiểm tra xem tính năng có được hỗ trợ không:

```ts
if (SillyTavern.getContext().isToolCallingSupported()) {
    console.log("Function tool calling is supported");
} else {
    console.log("Function tool calling is not supported");
}
```

### Đăng ký một function

Để đăng ký một function tool, bạn cần gọi hàm `registerFunctionTool` từ đối tượng `SillyTavern.getContext()` và truyền các tham số cần thiết. Dưới đây là ví dụ về cách đăng ký một function tool:

```ts
SillyTavern.getContext().registerFunctionTool({
    // Internal name of the function tool. Must be unique.
    name: "myFunction",
    // Display name of the function tool. Will be shown in the UI. (Optional)
    displayName: "My Function",
    // Description of the function tool. Must describe what the function does and when to use it.
    description: "My function description. Use when you need to do something.",
    // JSON schema for the parameters of the function tool. See: https://json-schema.org/
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            param1: {
                type: 'string',
                description: 'Parameter 1 description',
            },
            param2: {
                type: 'string',
                description: 'Parameter 2 description',
            },
        },
        required: [
            'param1', 'param2',
        ],
    },
    // Function to call when the tool is triggered. Can be async.
    // If the result is not a string, it will be JSON-stringified.
    action: async ({ param1, param2 }) => {
        // Your function code here
        console.log(`Function called with parameters: ${param1}, ${param2}`);
        return "Function result";
    },
    // Optional function to format the toast message displayed when the function is invoked.
    // If an empty string is returned, no toast message will be displayed.
    formatMessage: ({ param1, param2 }) => {
        return `Function is called with: ${param1} and ${param2}`;
    },
    // Optional function that returns a boolean value indicating whether the tool should be registered for the current prompt.
    // If no shouldRegister function is provided, the tool will be registered for every prompt.
    shouldRegister: () => {
        return true;
    },
    // Optional flag. If set to true, the function call will be performed, but the result won't be recorded to the visible chat history.
    stealth: false,
});
```

### Hủy đăng ký một function

Để vô hiệu hóa một function tool, bạn cần gọi hàm `unregisterFunctionTool` từ đối tượng `SillyTavern.getContext()` và truyền tên của function tool để vô hiệu hóa. Dưới đây là ví dụ về cách hủy đăng ký một function tool:

```ts
SillyTavern.getContext().unregisterFunctionTool("myFunction");
```

## Mẹo và thủ thuật

1. Các tool call thành công được lưu như một phần của lịch sử hiển thị và sẽ được hiển thị trong giao diện trò chuyện, vì vậy bạn có thể kiểm tra các tham số và kết quả thực tế. Nếu điều đó không mong muốn, hãy đặt cờ `stealth: true` khi đăng ký một function tool.
2. Nếu bạn không muốn nhìn thấy tool call trong lịch sử trò chuyện. Nếu bạn muốn tùy chỉnh kiểu hoặc ẩn chúng bằng CSS tùy chỉnh, hãy nhắm vào class `toolCall` trên các phần tử `.mes`, tức là `.mes.toolCall { display: none; }` hoặc `.mes.toolCall { color: #999; }`.
