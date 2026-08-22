---
order: -10
icon: code-review
route: /vi/for-contributors/function-calling/
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
4. [Weather](https://github.com/SillyTavern/Extension-Weather) - lấy thông tin thời tiết từ các API thời tiết.
5. [D&D Dice](https://github.com/SillyTavern/Extension-Dice) - tung xúc xắc cho các trò chơi D&D.

## Yêu cầu tiên quyết và hạn chế

1. Tính năng này chỉ có sẵn cho Chat Completion API, được hỗ trợ bởi các nguồn sau: Custom (tương thích OpenAI), AI/ML API, AI21, Azure OpenAI, Chutes, Claude, Cloudflare Workers AI, Cohere, DeepSeek, Electron Hub, Fireworks, Google AI Studio, Google Vertex AI, Groq, MiniMax, MistralAI, Moonshot (Kimi), NanoGPT, OpenAI, OpenRouter, Pollinations, SiliconFlow, xAI (Grok), và Z.AI (GLM).
2. Các API Text Completion không hỗ trợ function calls, nhưng một số backend được lưu trữ cục bộ như Ollama và TabbyAPI có thể chạy ở chế độ tương thích OpenAI tùy chỉnh dưới Chat Completion.
3. Sự hỗ trợ cho function calling phải được người dùng cho phép rõ ràng trước. Điều này được thực hiện bằng cách bật tùy chọn "Enable function calling" trong bảng AI Response Configuration.
4. Không có gì đảm bảo rằng một LLM sẽ thực hiện bất kỳ function call nào. Hầu hết chúng đều yêu cầu "kích hoạt" rõ ràng thông qua prompt (ví dụ: người dùng yêu cầu "Tung xúc xắc", "Lấy thời tiết", v.v.).
5. Không phải tất cả các prompt đều có thể kích hoạt tool call. Các continuation, impersonation, background ('quiet') prompt không được phép kích hoạt tool call. Chúng vẫn có thể sử dụng các tool call thành công trong quá khứ trong phản hồi của chúng.
6. Một số model có thể không hỗ trợ function calling, ngay cả khi nguồn API có hỗ trợ. Vui lòng tham khảo tài liệu của nhà cung cấp API của bạn để biết chi tiết về model nào hỗ trợ function calling.

## Giới hạn đệ quy của tool calling

Để ngăn chặn các vòng lặp vô hạn của tool call, có một giới hạn đệ quy (mặc định: 5 vòng). Nếu LLM tiếp tục gọi tool lặp đi lặp lại, việc thực thi sẽ dừng lại sau khi đạt đến giới hạn. Bạn có thể điều chỉnh giới hạn này trong cài đặt AI response configuration, bên cạnh tùy chọn "Enable function calling".

## Interleaved Thinking

Khi sử dụng tool calling với một [reasoning model](../Usage/Prompts/reasoning.md), bạn có thể tận dụng reasoning được trả về cùng với các yêu cầu tool-call để duy trì ngữ cảnh interleaved thinking. Đối với một số model, điều này là cần thiết để đảm bảo tính nhất quán và giữ lại các chi tiết quan trọng giữa các tool call.

Chọn một trong các cài đặt sau trong AI response configuration:

- Disabled: Không có ngữ cảnh reasoning nào được đưa vào các yêu cầu tool-call. Khả năng tương thích cao nhất, cài đặt mặc định.
- Since Last User Message: Bao gồm reasoning cho bất kỳ lượt tool nào xảy ra sau tin nhắn người dùng mới nhất.
- Active Tool Chain: Chỉ bao gồm reasoning trong khi vẫn còn trong vòng lặp tool chưa được giải quyết hiện tại; một khi phản hồi bình thường của trợ lý được tạo ra, reasoning của tool-chain cũ sẽ không còn được gửi lại nữa.

## Cách tạo một function tool

### Kiểm tra xem tính năng có được hỗ trợ không

Sử dụng `isToolCallingSupported()` từ `SillyTavern.getContext()` để kiểm tra xem API hiện tại có hỗ trợ function tool calling và nó có được bật trong cài đặt hay không:

```js
const { isToolCallingSupported } = SillyTavern.getContext();

if (isToolCallingSupported()) {
    console.log('Function tool calling is supported');
}
```

Bạn cũng có thể kiểm tra xem tool call có thể được thực hiện cho một loại generation cụ thể hay không. Các continuation, impersonation, và background ('quiet') prompt không được phép kích hoạt tool call:

```js
const { canPerformToolCalls } = SillyTavern.getContext();

if (canPerformToolCalls('normal')) {
    console.log('Can perform tool calls for this generation');
}
```

### Đăng ký một function tool

Sử dụng `registerFunctionTool()` từ `SillyTavern.getContext()` để đăng ký một tool. Định nghĩa tool tuân theo định dạng [JSON Schema](https://json-schema.org/) cho các tham số của nó:

```js
const { registerFunctionTool } = SillyTavern.getContext();

registerFunctionTool({
    name: 'get_weather',
    displayName: 'Get Weather',
    description: 'Get the current weather for a given location',
    parameters: {
        $schema: 'http://json-schema.org/draft-04/schema#',
        type: 'object',
        properties: {
            location: {
                type: 'string',
                description: 'The city name, e.g. "London"',
            },
            unit: {
                type: 'string',
                enum: ['celsius', 'fahrenheit'],
                description: 'Temperature unit',
            },
        },
        required: ['location'],
    },
    action: async ({ location, unit }) => {
        // Perform your logic here (API calls, computations, etc.)
        const data = await fetchWeatherData(location, unit);
        return JSON.stringify(data);
    },
    formatMessage: ({ location }) => `Checking weather for ${location}...`,
    shouldRegister: () => isWeatherFeatureEnabled(),
    stealth: false,
});
```

### Các trường đăng ký

| Trường | Bắt buộc | Mô tả |
|-------|----------|-------------|
| `name` | Có | Định danh duy nhất cho tool |
| `displayName` | Không | Tên hiển thị thân thiện với người dùng được hiển thị trong giao diện |
| `description` | Có | Mô tả được gửi đến LLM để giải thích tool làm gì và khi nào nên sử dụng |
| `parameters` | Có | JSON Schema định nghĩa các tham số đầu vào của tool |
| `action` | Có | Hàm được gọi khi LLM gọi tool. Nhận các tham số đã phân tích dưới dạng một đối tượng. Có thể là async. Phải trả về một kết quả dạng chuỗi (các giá trị không phải chuỗi sẽ được JSON-stringify). |
| `formatMessage` | Không | Hàm trả về một chuỗi được hiển thị dưới dạng toast khi tool đang thực thi. Trả về chuỗi rỗng để ẩn toast. |
| `shouldRegister` | Không | Hàm trả về một boolean; nếu `false`, tool sẽ bị loại khỏi yêu cầu hiện tại. Nếu không được cung cấp, tool sẽ được đăng ký cho mọi prompt. |
| `stealth` | Không | Nếu `true`, kết quả tool call sẽ không được ghi vào lịch sử trò chuyện hiển thị và không có generation tiếp theo nào được kích hoạt |

### Hủy đăng ký một function tool

Để vô hiệu hóa một function tool, gọi `unregisterFunctionTool()` với tên của tool:

```js
const { unregisterFunctionTool } = SillyTavern.getContext();

unregisterFunctionTool('get_weather');
```

## Mẹo và thủ thuật

1. Các tool call thành công được lưu như một phần của lịch sử trò chuyện hiển thị và được hiển thị trong giao diện trò chuyện, vì vậy bạn có thể kiểm tra các tham số và kết quả thực tế. Nếu điều đó không mong muốn, hãy đặt `stealth: true` khi đăng ký tool.
2. Để tùy chỉnh kiểu hoặc ẩn các tin nhắn tool call bằng CSS tùy chỉnh, hãy nhắm vào class `toolCall` trên các phần tử `.mes`, ví dụ: `.mes.toolCall { display: none; }` hoặc `.mes.toolCall { opacity: 0.5; }`.
3. Viết các mô tả rõ ràng, cụ thể cho các tool của bạn — LLM sử dụng chúng để quyết định khi nào và cách gọi chúng. Bao gồm hướng dẫn về thời điểm nên sử dụng tool.
4. Giữ cho schema tham số đơn giản và được tài liệu hóa tốt. `description` của mỗi property giúp LLM điền vào các giá trị chính xác.
5. Tool call có giới hạn đệ quy (mặc định: 5 vòng). Nếu LLM tiếp tục gọi tool lặp đi lặp lại, việc thực thi sẽ dừng lại sau khi đạt đến giới hạn.
