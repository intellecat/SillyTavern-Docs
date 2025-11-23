---
order: -20
icon: file-added
templating: false
route: /for-contributors/writing-extensions/
---

# UI Extensions

UI extension mở rộng chức năng của SillyTavern bằng cách kết nối vào các sự kiện và API của nó. Chúng chạy trong ngữ cảnh trình duyệt và có quyền truy cập hầu như không hạn chế vào DOM, JavaScript API và ngữ cảnh SillyTavern. Extension có thể sửa đổi giao diện người dùng, gọi các API nội bộ và tương tác với dữ liệu trò chuyện. Hướng dẫn này giải thích cách tạo extension của riêng bạn (yêu cầu kiến thức JavaScript).

!!!tip Chỉ muốn cài đặt extension?
Đi tới đây: [Extensions](../extensions/index.md).
!!!

Để mở rộng chức năng của server Node.js, xem trang [Server Plugins](./Server-Plugins.md).

**Không thể viết JavaScript?**

* Hãy xem xét [STscript](./st-script.md) như một giải pháp thay thế đơn giản hơn để viết một extension đầy đủ.
* Tham gia [Khóa học MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript) và quay lại khi bạn hoàn thành.

## Gửi extension

Bạn muốn đóng góp extension của mình vào [repository nội dung chính thức](https://github.com/SillyTavern/SillyTavern-Content)? Liên hệ với chúng tôi!

Để đảm bảo rằng tất cả các extension đều an toàn và dễ sử dụng, chúng tôi có một vài yêu cầu:

1. Extension của bạn phải là mã nguồn mở và có giấy phép tự do (xem [Choose a License](https://choosealicense.com/licenses/)). Nếu bạn không chắc chắn, AGPLv3 là một lựa chọn tốt.
2. Extension phải tương thích với phiên bản phát hành mới nhất của SillyTavern. Hãy sẵn sàng cập nhật extension của bạn nếu có thay đổi trong core.
3. Extension phải được ghi chép rõ ràng. Điều này bao gồm file README với hướng dẫn cài đặt, ví dụ sử dụng và danh sách các tính năng.
4. Extension yêu cầu server plugin để hoạt động sẽ không được chấp nhận.

## Ví dụ

Xem các ví dụ trực tiếp về extension SillyTavern đơn giản:

* <https://github.com/city-unit/st-extension-example> - mẫu extension cơ bản. Giới thiệu cách tạo manifest, nhập script cục bộ, thêm bảng giao diện cài đặt và sử dụng cài đặt extension liên tục.
* <https://github.com/search?q=topic%3Aextension+org%3ASillyTavern&type=Repositories> - danh sách tất cả các extension SillyTavern chính thức trên GitHub.

## Bundling

Extension cũng có thể sử dụng bundling để tách biệt khỏi các module khác và sử dụng bất kỳ dependency nào từ NPM, bao gồm các framework UI như Vue, React, v.v.

* <https://github.com/SillyTavern/Extension-WebpackTemplate> - repository mẫu của extension sử dụng TypeScript và Webpack (không có React).
* <https://github.com/SillyTavern/Extension-ReactTemplate> - repository mẫu của extension cơ bản sử dụng React và Webpack.

Để sử dụng import tương đối từ bundle, bạn có thể cần tạo một import wrapper. Dưới đây là ví dụ cho Webpack:

```js
/**
 * Import a member from a module by URL, bypassing webpack.
 * @param {string} url URL to import from
 * @param {string} what Name of the member to import
 * @param {any} defaultValue Fallback value
 * @returns {Promise<any>} Imported member
 */
export async function importFromUrl(url, what, defaultValue = null) {
    try {
        const module = await import(/* webpackIgnore: true */ url);
        if (!Object.hasOwn(module, what)) {
            throw new Error(`No ${what} in module`);
        }
        return module[what];
    } catch (error) {
        console.error(`Failed to import ${what} from ${url}: ${error}`);
        return defaultValue;
     }
}

// Import a function from 'script.js' module
const generateRaw = await importFromUrl('/script.js', 'generateRaw');
```

## manifest.json

Mỗi extension phải có một thư mục trong `data/<user-handle>/extensions` và một file `manifest.json`, file này chứa metadata về extension và đường dẫn đến file JS script là điểm bắt đầu của extension.

Các extension có thể tải xuống được mount vào thư mục `/scripts/extensions/third-party` khi phục vụ qua HTTP, vì vậy các import tương đối nên được sử dụng dựa trên điều đó. Để dễ dàng phát triển cục bộ, hãy xem xét đặt repository extension của bạn trong thư mục `/scripts/extensions/third-party` (tùy chọn "Install for all users").

```json
{
    "display_name": "The name of the extension",
    "loading_order": 1,
    "requires": [],
    "optional": [],
    "dependencies": [],
    "js": "index.js",
    "css": "style.css",
    "author": "Your name",
    "version": "1.0.0",
    "homePage": "https://github.com/your/extension",
    "auto_update": true,
    "minimum_client_version": "1.0.0",
    "i18n": {
        "de-de": "i18n/de-de.json"
    }
}
```

### Các trường manifest

* `display_name` là bắt buộc. Nó được hiển thị trong menu "Manage Extensions".
* `loading_order` là tùy chọn. Số lớn hơn sẽ tải sau.
* `js` là tham chiếu file JS chính và là bắt buộc.
* `css` là tham chiếu file style tùy chọn.
* `author` là bắt buộc. Nó nên chứa tên hoặc thông tin liên hệ của (các) tác giả.
* `auto_update` được đặt thành `true` nếu extension nên tự động cập nhật khi phiên bản của gói ST thay đổi.
* `i18n` là một đối tượng tùy chọn chỉ định các locale được hỗ trợ và các file JSON tương ứng của chúng (xem bên dưới).
* `dependencies` là một mảng tùy chọn các chuỗi chỉ định các **extension** khác mà extension này phụ thuộc vào.
* `generate_interceptor` là một chuỗi tùy chọn chỉ định tên của một hàm toàn cục được gọi trên các yêu cầu tạo văn bản.
* `minimum_client_version` là một chuỗi tùy chọn chỉ định phiên bản SillyTavern tối thiểu cần thiết để extension này hoạt động.

### Dependencies

Extension cũng có thể phụ thuộc vào các extension SillyTavern khác. Extension sẽ không tải nếu bất kỳ dependency nào trong số này bị thiếu hoặc bị vô hiệu hóa.

Dependency được chỉ định bằng **tên thư mục** như chúng xuất hiện trong thư mục `public/extensions`.

Ví dụ:

* Extension tích hợp: `"vectors"`, `"caption"`
* Extension của bên thứ ba: `"third-party/Extension-WebLLM"`, `"third-party/Extension-Mermaid"`

### Các trường đã lỗi thời

* `requires` là một mảng tùy chọn các chuỗi chỉ định các **module Extras** bắt buộc. Extension sẽ không được tải nếu Extras API được kết nối không cung cấp tất cả các module được liệt kê.
* `optional` là một mảng tùy chọn các chuỗi chỉ định các **module Extras** tùy chọn. Extension vẫn sẽ tải nếu chúng bị thiếu và extension nên xử lý sự vắng mặt của chúng một cách duyên dáng.

Để kiểm tra module nào hiện đang được cung cấp bởi Extras API được kết nối, hãy import mảng `modules` từ `scripts/extensions.js`.

## Scripting

### Sử dụng getContext

Hàm `getContext()` trong đối tượng toàn cục `SillyTavern` cho bạn quyền truy cập vào ngữ cảnh SillyTavern, đây là một tập hợp của tất cả các đối tượng trạng thái ứng dụng chính, các hàm hữu ích và tiện ích.

```js
const context = SillyTavern.getContext();
context.chat; // Chat log - MUTABLE
context.characters; // Character list
context.characterId; // Index of the current character
context.groups; // Group list
context.groupId; // ID of the current group
// And many more...
```

Bạn có thể tìm thấy danh sách đầy đủ các thuộc tính và hàm có sẵn trong [mã nguồn SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/st-context.js).

!!!
Nếu bạn thiếu bất kỳ hàm/thuộc tính nào trong `getContext`, vui lòng liên hệ với các nhà phát triển hoặc gửi cho chúng tôi một pull request!
!!!

### Shared libraries

Hầu hết các thư viện npm được sử dụng nội bộ bởi frontend SillyTavern được chia sẻ trong thuộc tính `libs` của đối tượng toàn cục `SillyTavern`.

* `lodash` - Thư viện tiện ích. [Docs](https://lodash.com/).
* `localforage` - Thư viện lưu trữ trình duyệt. [Docs](https://localforage.github.io/localForage/).
* `Fuse` - Thư viện tìm kiếm mờ. [Docs](https://www.fusejs.io/).
* `DOMPurify` - Thư viện làm sạch HTML. [Docs](https://github.com/cure53/DOMPurify).
* `Handlebars` - Thư viện templating. [Docs](https://handlebarsjs.com/).
* `moment` - Thư viện thao tác ngày/giờ. [Docs](http://momentjs.com/).
* `showdown` - Thư viện chuyển đổi Markdown. [Docs](https://showdownjs.com/).

Bạn có thể tìm thấy danh sách đầy đủ các thư viện được xuất trong [mã nguồn SillyTavern](https://github.com/SillyTavern/SillyTavern/blob/staging/public/lib.js).

**Ví dụ:** Sử dụng thư viện DOMPurify.

```js
const { DOMPurify } = SillyTavern.libs;

const sanitizedHtml = DOMPurify.sanitize('<script>"dirty HTML"</script>');
```

### Lưu ý về TypeScript

Nếu bạn muốn truy cập vào autocomplete cho tất cả các phương thức trong đối tượng toàn cục `SillyTavern` (và bạn có thể muốn), bao gồm `getContext()` và `libs`, bạn nên thêm một khai báo module TypeScript `.d.ts`. Khai báo này nên import các kiểu toàn cục từ nguồn SillyTavern, tùy thuộc vào vị trí extension của bạn. Dưới đây là ví dụ hoạt động cho cả hai loại cài đặt: "all users" và "current user."

**global.d.ts** - đặt file này vào thư mục gốc của extension của bạn (bên cạnh `manifest.json`):

```ts
export {};

// 1. Import for user-scoped extensions
import '../../../../public/global';
// 2. Import for server-scoped extensions
import '../../../../global';

// Define additional types if needed...
declare global {
    // Add global type declarations here
}
```

### Import từ các file khác

!!!warning
Việc sử dụng import từ code SillyTavern là không đáng tin cậy và có thể bị hỏng bất cứ lúc nào nếu cấu trúc nội bộ của các module ST thay đổi. `getContext` cung cấp một API ổn định hơn.
!!!

Trừ khi bạn đang xây dựng một extension được đóng gói, bạn có thể import các biến và hàm từ các file JS khác.

Ví dụ, đoạn code này sẽ tạo một phản hồi từ API hiện đang được chọn trong nền:

```js
import { generateQuietPrompt } from "../../../../script.js";

async function handleMessage(data) {
    const text = data.message;
    const translated = await generateQuietPrompt({ quietPrompt: text });
    // ...
}
```

## Quản lý trạng thái

### Cài đặt liên tục

Khi một extension cần duy trì trạng thái của nó, nó có thể sử dụng đối tượng `extensionSettings` từ hàm `getContext()` để lưu trữ và truy xuất dữ liệu. Một extension có thể lưu trữ bất kỳ dữ liệu nào có thể JSON-serialize trong đối tượng cài đặt và phải sử dụng một khóa duy nhất để tránh xung đột với các extension khác.

Để duy trì cài đặt, sử dụng hàm `saveSettingsDebounced()`, hàm này sẽ lưu cài đặt vào server.

```js
const { extensionSettings, saveSettingsDebounced } = SillyTavern.getContext();

// Define a unique identifier for your extension
const MODULE_NAME = 'my_extension';

// Define default settings
const defaultSettings = Object.freeze({
    enabled: false,
    option1: 'default',
    option2: 5
});

// Define a function to get or initialize settings
function getSettings() {
    // Initialize settings if they don't exist
    if (!extensionSettings[MODULE_NAME]) {
        extensionSettings[MODULE_NAME] = structuredClone(defaultSettings);
    }

    // Ensure all default keys exist (helpful after updates)
    for (const key of Object.keys(defaultSettings)) {
        if (!Object.hasOwn(extensionSettings[MODULE_NAME], key)) {
            extensionSettings[MODULE_NAME][key] = defaultSettings[key];
        }
    }

    return extensionSettings[MODULE_NAME];
}

// Use the settings
const settings = getSettings();
settings.option1 = 'new value';

// Save the settings
saveSettingsDebounced();
```

### Chat metadata

Để liên kết một số dữ liệu với một cuộc trò chuyện cụ thể, bạn có thể sử dụng đối tượng `chatMetadata` từ hàm `getContext()`. Đối tượng này cho phép bạn lưu trữ dữ liệu tùy ý được liên kết với một cuộc trò chuyện, điều này có thể hữu ích để lưu trữ trạng thái cụ thể của extension.

Để duy trì metadata, sử dụng hàm `saveMetadata()`, hàm này sẽ lưu metadata vào server.

!!!warning
Không lưu tham chiếu đến `chatMetadata` trong một biến tồn tại lâu, vì tham chiếu sẽ thay đổi khi chuyển đổi cuộc trò chuyện. Luôn sử dụng `SillyTavern.getContext().chatMetadata` để truy cập metadata cuộc trò chuyện hiện tại.
!!!

```js
const { chatMetadata, saveMetadata } = SillyTavern.getContext();

// Set some metadata for the current chat
chatMetadata['my_key'] = 'my_value';

// Get the metadata for the current chat
const value = chatMetadata['my_key'];

// Save the metadata to the server
await saveMetadata();
```

!!!tip
Sự kiện `CHAT_CHANGED` được phát ra khi cuộc trò chuyện được chuyển đổi, vì vậy bạn có thể lắng nghe sự kiện này để cập nhật trạng thái extension của bạn tương ứng. Xem thêm trong phần [Listening to events](#listening-to-events).
!!!

### Character cards

SillyTavern hỗ trợ đầy đủ [Character Cards V2 Specification](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md), cho phép lưu trữ dữ liệu tùy ý trong dữ liệu JSON của character card.

Điều này hữu ích cho các extension cần lưu trữ dữ liệu bổ sung liên quan đến nhân vật và làm cho nó có thể chia sẻ khi xuất character card.

Để ghi dữ liệu vào trường [extensions](https://github.com/malfoyslastname/character-card-spec-v2/blob/main/spec_v2.md#extensions) của character card, sử dụng hàm `writeExtensionField` từ hàm `getContext()`. Hàm này nhận một character ID, một khóa chuỗi và một giá trị để ghi. Giá trị phải có thể JSON-serialize.

!!!warning Điều kỳ lạ phía trước
Mặc dù được gọi là `characterId`, nó không phải là một định danh duy nhất "thực sự" mà là một chỉ số của nhân vật trong mảng `characters`.

Chỉ số của nhân vật hiện tại được cung cấp bởi thuộc tính `characterId` trong ngữ cảnh. Nếu bạn muốn ghi dữ liệu vào nhân vật hiện đang được chọn, hãy sử dụng `SillyTavern.getContext().characterId`. Nếu bạn cần lưu trữ dữ liệu cho nhân vật khác, hãy tìm chỉ số bằng cách tìm kiếm nhân vật trong mảng `characters`.

**Cảnh báo: `characterId` là `undefined` trong các cuộc trò chuyện nhóm hoặc khi không có nhân vật nào được chọn!**
!!!

```js
const { writeExtensionField, characterId } = SillyTavern.getContext();

// Write some data to the character card
await writeExtensionField(characterId, 'my_extension_key', {
    someData: 'value',
    anotherData: 42
});

// Read the data back from the character card
const character = SillyTavern.getContext().characters[characterId];
// The data is stored in the `extensions` object of the character's data
const myData = character.data?.extensions?.my_extension_key;
```

### Settings presets

Dữ liệu JSON tùy ý có thể được lưu trữ trong các preset cài đặt của các loại API chính. Nó sẽ được xuất và nhập cùng với JSON preset, vì vậy bạn có thể sử dụng nó để lưu trữ cài đặt cụ thể của extension cho preset. Các loại API sau hỗ trợ extension dữ liệu trong các preset cài đặt:

* Chat Completion
* Text Completion
* NovelAI
* KoboldAI / AI Horde

Để đọc hoặc ghi dữ liệu, trước tiên bạn cần lấy instance PresetManager từ ngữ cảnh:

```js
const { getPresetManager } = SillyTavern.getContext();

// Get the preset manager for the current API type
const pm = getPresetManager();

// Write data to the preset extension field:
// - path: the path to the field in the preset data
// - value: the value to write
// - name (optional): the name of the preset to write to, defaults to the currently selected preset
await pm.writePresetExtensionField({ path: 'hello', value: 'world' });

// Read data from the preset extension field:
// - path: the path to the field in the preset data
// - name (optional): the name of the preset to read from, defaults to the currently selected preset
const value = pm.readPresetExtensionField({ path: 'hello' });
```

!!!tip
Các sự kiện `PRESET_CHANGED` và `MAIN_API_CHANGED` được phát ra khi preset được thay đổi hoặc API chính được chuyển đổi, vì vậy bạn có thể lắng nghe các sự kiện này để cập nhật trạng thái extension của bạn tương ứng. Xem thêm trong phần [Listening to events](#listening-to-events).
!!!

## Internationalization

!!!
Để biết thông tin chung về việc cung cấp bản dịch, xem trang [Internationalization](/For_Contributors/i18n.md).
!!!

Extension có thể cung cấp các chuỗi bản địa hóa bổ sung để sử dụng với các hàm `t`, `translate` và thuộc tính `data-i18n` trong các template HTML.

Xem danh sách các locale được hỗ trợ tại đây (khóa `lang`): <https://github.com/SillyTavern/SillyTavern/blob/release/public/locales/lang.json>

### Gọi trực tiếp `addLocaleData`

Truyền một mã locale và một đối tượng với các bản dịch vào hàm `addLocaleData`. Ghi đè các khóa hiện có *KHÔNG* được phép. Nếu mã locale được truyền không phải là locale hiện đang được chọn, dữ liệu sẽ bị bỏ qua một cách im lặng.

```js
SillyTavern.getContext().addLocaleData('fr-fr', { 'Hello': 'Bonjour' });
SillyTavern.getContext().addLocaleData('de-de', { 'Hello': 'Hallo' });
```

### Thông qua extension manifest

Thêm một đối tượng i18n với danh sách các locale được hỗ trợ và các đường dẫn file JSON tương ứng của chúng (tương đối với thư mục extension của bạn) vào manifest.

```json
{
  "display_name": "Foobar",
  "js": "index.js",
  // rest of the fields
  "i18n": {
    "fr-fr": "i18n/french.json",
    "de-de": "i18n/german.json"
  }
}
```

## Đăng ký slash command (cách mới)

Trong khi `registerSlashCommand` vẫn tồn tại để tương thích ngược, các slash command mới bây giờ nên được đăng ký thông qua `SlashCommandParser.addCommandObject()` để cung cấp chi tiết mở rộng về lệnh và các tham số của nó cho parser (và đến lượt nó, cho autocomplete và trợ giúp lệnh).

```javascript
SlashCommandParser.addCommandObject(SlashCommand.fromProps({ name: 'repeat',
    callback: (namedArgs, unnamedArgs) => {
        return Array(namedArgs.times ?? 5)
            .fill(unnamedArgs.toString())
            .join(isTrueBoolean(namedArgs.space.toString()) ? ' ' : '')
        ;
    },
    aliases: ['example-command'],
    returns: 'the repeated text',
    namedArgumentList: [
        SlashCommandNamedArgument.fromProps({ name: 'times',
            description: 'number of times to repeat the text',
            typeList: ARGUMENT_TYPE.NUMBER,
            defaultValue: '5',
        }),
        SlashCommandNamedArgument.fromProps({ name: 'space',
            description: 'whether to separate the texts with a space',
            typeList: ARGUMENT_TYPE.BOOLEAN,
            defaultValue: 'off',
            enumList: ['on', 'off'],
        }),
    ],
    unnamedArgumentList: [
        SlashCommandArgument.fromProps({ description: 'the text to repeat',
            typeList: ARGUMENT_TYPE.STRING,
            isRequired: true,
        }),
    ],
    helpString: `
        <div>
            Repeats the provided text a number of times.
        </div>
        <div>
            <strong>Example:</strong>
            <ul>
                <li>
                    <pre><code class="language-stscript">/repeat foo</code></pre>
                    returns "foofoofoofoofoo"
                </li>
                <li>
                    <pre><code class="language-stscript">/repeat times=3 space=on bar</code></pre>
                    returns "bar bar bar"
                </li>
            </ul>
        </div>
    `,
}));
```

Tất cả các lệnh đã đăng ký có thể được sử dụng trong [STscript](/For_Contributors/st-script.md) theo mọi cách có thể.

## Events

### Lắng nghe sự kiện

Sử dụng `eventSource.on(eventType, eventHandler)` để lắng nghe các sự kiện:

```js
const { eventSource, event_types } = SillyTavern.getContext();

eventSource.on(event_types.MESSAGE_RECEIVED, handleIncomingMessage);

function handleIncomingMessage(data) {
    // Handle message
}
```

Các loại sự kiện chính là:

* `APP_READY`: ứng dụng đã được tải đầy đủ và sẵn sàng sử dụng. Nó sẽ tự động kích hoạt mỗi khi một listener mới được đính kèm sau khi ứng dụng sẵn sàng.
* `MESSAGE_RECEIVED`: tin nhắn LLM được tạo và ghi vào đối tượng `chat` nhưng chưa được hiển thị trong giao diện người dùng.
* `MESSAGE_SENT`: tin nhắn được gửi bởi người dùng và ghi vào đối tượng `chat` nhưng chưa được hiển thị trong giao diện người dùng.
* `USER_MESSAGE_RENDERED`: tin nhắn được gửi bởi người dùng được hiển thị trong giao diện người dùng.
* `CHARACTER_MESSAGE_RENDERED`: tin nhắn LLM được tạo được hiển thị trong giao diện người dùng.
* `CHAT_CHANGED`: cuộc trò chuyện đã được chuyển đổi (ví dụ: chuyển sang nhân vật khác hoặc cuộc trò chuyện khác được tải).
* `GENERATION_AFTER_COMMANDS`: việc tạo sắp bắt đầu sau khi xử lý các slash command.
* `GENERATION_STOPPED`: việc tạo đã bị dừng bởi người dùng.
* `GENERATION_ENDED`: việc tạo đã hoàn thành hoặc đã gặp lỗi.
* `SETTINGS_UPDATED`: cài đặt ứng dụng đã được cập nhật.

Phần còn lại có thể được tìm thấy [trong mã nguồn](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

!!!info Dữ liệu sự kiện
Cách mỗi sự kiện truyền dữ liệu của nó cho listener không đồng nhất. Một số sự kiện không phát ra bất kỳ dữ liệu nào; một số truyền một đối tượng hoặc một giá trị nguyên thủy. Vui lòng tham khảo mã nguồn nơi sự kiện được phát ra để xem dữ liệu nào nó truyền, hoặc kiểm tra với debugger.
!!!

### Phát ra sự kiện

Bạn có thể tạo ra bất kỳ sự kiện ứng dụng nào từ extension, bao gồm các sự kiện tùy chỉnh, bằng cách gọi `eventSource.emit(eventType, ...eventData)`:

```js
const { eventSource } = SillyTavern.getContext();

// Can be a built-in event_types field or any string.
const eventType = 'myCustomEvent';

// Use `await` to ensure all event handlers complete before continuing execution.
await eventSource.emit(eventType, { data: 'custom event data' });
```

## Prompt Interceptors

Prompt Interceptor cung cấp một cách cho extension thực hiện bất kỳ hoạt động nào như sửa đổi dữ liệu trò chuyện, thêm injection hoặc hủy bỏ việc tạo trước khi một yêu cầu tạo văn bản được thực hiện.

Interceptor từ các extension khác nhau được chạy tuần tự. Thứ tự được xác định bởi trường `loading_order` trong các file `manifest.json` tương ứng của chúng. Extension với giá trị `loading_order` thấp hơn sẽ chạy sớm hơn. Nếu `loading_order` không được chỉ định, `display_name` được sử dụng làm phương án dự phòng. Nếu cả hai đều không được chỉ định, thứ tự là không xác định.

### Đăng ký một Interceptor

Để định nghĩa một prompt interceptor, thêm trường `generate_interceptor` vào file `manifest.json` của extension của bạn. Giá trị phải là tên của một hàm toàn cục sẽ được gọi bởi SillyTavern.

```json
{
    "display_name": "My Interceptor Extension",
    "loading_order": 10, // Affects execution order
    "generate_interceptor": "myCustomInterceptorFunction",
    // ... other manifest properties
}
```

### Hàm Interceptor

Hàm `generate_interceptor` là một hàm toàn cục sẽ được gọi khi có các yêu cầu tạo không phải là dry run. Nó phải được định nghĩa trong phạm vi toàn cục (ví dụ: `globalThis.myCustomInterceptorFunction = async function(...) { ... }`) và có thể trả về một `Promise` nếu nó cần thực hiện bất kỳ hoạt động không đồng bộ nào.

Hàm interceptor nhận các tham số sau:

* `chat`: Một mảng các đối tượng tin nhắn đại diện cho lịch sử trò chuyện sẽ được sử dụng để xây dựng prompt. Bạn có thể sửa đổi mảng này trực tiếp (ví dụ: thêm, xóa hoặc thay đổi tin nhắn). Xin lưu ý rằng tin nhắn là có thể thay đổi, vì vậy bất kỳ thay đổi nào bạn thực hiện đối với mảng sẽ được phản ánh trong lịch sử trò chuyện thực tế. Nếu bạn muốn các thay đổi là tạm thời, hãy sử dụng `structuredClone` để tạo một bản sao sâu của đối tượng tin nhắn.
* `contextSize`: Một số chỉ ra kích thước ngữ cảnh hiện tại (theo token) được tính toán cho việc tạo sắp tới.
* `abort`: Một hàm khi được gọi, sẽ báo hiệu để ngăn việc tạo văn bản tiếp tục. Nó chấp nhận một tham số boolean ngăn bất kỳ interceptor tiếp theo nào chạy nếu `true`.
* `type`: Một chuỗi chỉ ra loại hoặc trigger của việc tạo (ví dụ: `'quiet'`, `'regenerate'`, `'impersonate'`, `'swipe'`, v.v.). Điều này giúp interceptor áp dụng logic một cách có điều kiện dựa trên cách việc tạo được khởi xướng.

**Ví dụ thực hiện:**

```javascript
globalThis.myCustomInterceptorFunction = async function(chat, contextSize, abort, type) {
    // Example: Add a system note before the last user message
    const systemNote = {
        is_user: false,
        name: "System Note",
        send_date: Date.now(),
        mes: "This was added by my extension!"
    };
    // Insert before the last message
    chat.splice(chat.length - 1, 0, systemNote);
}
```

## Tạo văn bản

SillyTavern cung cấp một số hàm để tạo văn bản trong các ngữ cảnh khác nhau bằng cách sử dụng LLM API hiện đang được chọn. Các hàm này cho phép bạn tạo văn bản trong ngữ cảnh của một cuộc trò chuyện, tạo thô không có ngữ cảnh hoặc với đầu ra có cấu trúc.

### Trong ngữ cảnh trò chuyện

Hàm `generateQuietPrompt()` được sử dụng để tạo văn bản trong ngữ cảnh của một cuộc trò chuyện với một "quiet" prompt được thêm vào (hướng dẫn sau lịch sử) trong nền (đầu ra không được hiển thị trong giao diện người dùng). Điều này hữu ích để tạo văn bản mà không làm gián đoạn trải nghiệm người dùng trong khi vẫn giữ nguyên dữ liệu trò chuyện và nhân vật có liên quan, chẳng hạn như tạo bản tóm tắt hoặc lời nhắc hình ảnh.

```js
const { generateQuietPrompt } = SillyTavern.getContext();

const quietPrompt = 'Generate a summary of the chat history.';

const result = await generateQuietPrompt({
    quietPrompt,
});
```

### Tạo thô

Hàm `generateRaw()` được sử dụng để tạo văn bản mà không có bất kỳ ngữ cảnh trò chuyện nào. Nó hữu ích khi bạn muốn kiểm soát hoàn toàn quá trình xây dựng prompt.

Nó chấp nhận một `prompt` là chuỗi Text Completion hoặc một mảng các đối tượng Chat Completion, xây dựng yêu cầu với định dạng phù hợp tùy thuộc vào loại API được chọn, ví dụ: chuyển đổi giữa các chế độ chat/văn bản, áp dụng định dạng hướng dẫn, v.v. Bạn cũng có thể truyền một `systemPrompt` và `prefill` bổ sung vào hàm để kiểm soát nhiều hơn quá trình tạo.

```js
const { generateRaw } = SillyTavern.getContext();

const systemPrompt = 'You are a helpful assistant.';
const prompt = 'Generate a story about a brave knight.';
const prefill = 'Once upon a time,';

/*
In Chat Completion mode, will produce a prompt like this:
[
  {role: 'system', content: 'You are a helpful assistant.'},
  {role: 'user', content: 'Generate a story about a brave knight.'},
  {role: 'assistant', content: 'Once upon a time,'}
]
*/

/*
In Text Completion mode (no instruct), will produce a prompt like this:
"You are a helpful assistant.\nGenerate a story about a brave knight.\nOnce upon a time,"
*/

const result = await generateRaw({
    systemPrompt,
    prompt,
    prefill,
});
```

### Structured Outputs

!!!info
Hiện chỉ được hỗ trợ bởi Chat Completion API. Tính khả dụng khác nhau dựa trên nguồn và mô hình được chọn. Nếu mô hình được chọn không hỗ trợ structured output, việc tạo sẽ thất bại hoặc sẽ trả về một đối tượng rỗng (`'{}'`). Kiểm tra tài liệu cho API cụ thể bạn đang sử dụng để xem structured output có được hỗ trợ không.
!!!

Bạn có thể sử dụng tính năng structured output để đảm bảo mô hình tạo ra một đối tượng JSON hợp lệ tuân thủ một [JSON Schema](https://json-schema.org/learn) được cung cấp. Điều này hữu ích cho các extension yêu cầu dữ liệu có cấu trúc, chẳng hạn như theo dõi trạng thái, phân loại dữ liệu, v.v.

Để sử dụng structured output, bạn phải truyền một đối tượng JSON schema vào `generateRaw()` hoặc `generateQuietPrompt()`. Mô hình sau đó sẽ tạo ra một phản hồi khớp với schema, và nó sẽ được trả về dưới dạng một đối tượng JSON được chuỗi hóa.

!!!warning
Các đầu ra không được xác thực theo schema, bạn phải tự xử lý việc phân tích cú pháp và xác thực đầu ra được tạo. Nếu mô hình không tạo ra một đối tượng JSON hợp lệ, hàm sẽ trả về một đối tượng rỗng (`'{}'`).

[Zod](https://zod.dev/json-schema) là một thư viện phổ biến để tạo và xác thực JSON schema. Việc sử dụng nó sẽ không được đề cập ở đây.
!!!

```js
const { generateRaw, generateQuietPrompt } = SillyTavern.getContext();

// Define a JSON schema for the expected output
const jsonSchema = {
    // Required: a name for the schema
    name: 'StoryStateModel',
    // Optional: a description of the schema
    description: 'A schema for a story state with location, plans, and memories.',
    // Optional:  the schema will be used in strict mode, meaning that only the fields defined in the schema will be allowed
    strict: true,
    // Required: a definition of the schema
    value: {
        '$schema': 'http://json-schema.org/draft-04/schema#',
        'type': 'object',
        'properties': {
            'location': {
                'type': 'string'
            },
            'plans': {
                'type': 'string'
            },
            'memories': {
                'type': 'string'
            }
        },
        'required': [
            'location',
            'plans',
            'memories'
        ],
    },
};

const prompt = 'Generate a story state with location, plans, and memories. Output as a JSON object.';

const rawResult = await generateRaw({
    prompt,
    jsonSchema,
});

const quietResult = await generateQuietPrompt({
    quietPrompt: prompt,
    jsonSchema,
});
```

## Đăng ký macro tùy chỉnh

Bạn có thể đăng ký các macro tùy chỉnh có thể được sử dụng ở bất cứ đâu hỗ trợ thay thế macro, ví dụ: trong các trường character card, lệnh STscript, template prompt, v.v.

Để đăng ký một macro, sử dụng hàm `registerMacro()` từ đối tượng `SillyTavern.getContext()`. Hàm chấp nhận một tên macro phải là một chuỗi duy nhất, và một chuỗi hoặc một hàm trả về một chuỗi. Hàm sẽ được gọi với một chuỗi `nonce` duy nhất sẽ khác nhau giữa mỗi lần gọi `substituteParams`.

```js
const { registerMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});
```

Khi một macro tùy chỉnh không còn cần thiết nữa, hãy xóa nó bằng hàm `unregisterMacro()`:

```js
const { unregisterMacro } = SillyTavern.getContext();

// Unregister the 'fizz' macro
unregisterMacro('fizz');
```

**Chi tiết quan trọng và hạn chế đã biết liên quan đến macro tùy chỉnh:**

1. Hiện tại chỉ hỗ trợ các macro thay thế chuỗi đơn giản. Chúng tôi đang làm việc để thêm hỗ trợ cho các macro phức tạp hơn trong tương lai.
2. Các macro sử dụng hàm để cung cấp giá trị *phải* là đồng bộ. Trả về một `Promise` sẽ không hoạt động.
3. Bạn không cần bao bọc tên macro trong dấu ngoặc nhọn kép (`{{ }}`) khi đăng ký nó. SillyTavern sẽ làm điều đó cho bạn.
4. Vì macro là các thay thế biểu thức chính quy thuần túy, việc đăng ký nhiều macro sẽ gây ra vấn đề về hiệu suất, vì vậy hãy sử dụng chúng một cách tiết kiệm.

## Thực hiện yêu cầu Extras

!!!warning
Extras API đã lỗi thời. Không khuyến nghị sử dụng nó trong các extension mới.
!!!

Hàm `doExtrasFetch()` cho phép bạn thực hiện các yêu cầu đến server SillyTavern Extras API của bạn.

Ví dụ, để gọi endpoint `/api/summarize`:

```js
import { getApiUrl, doExtrasFetch } from "../../extensions.js";

const url = new URL(getApiUrl());
url.pathname = '/api/summarize';

const apiResult = await doExtrasFetch(url, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Bypass-Tunnel-Reminder': 'bypass',
    },
    body: JSON.stringify({
        // Request body
    })
});
```

`getApiUrl()` trả về URL cơ sở của server Extras.

Hàm `doExtrasFetch()`:

* Thêm các header `Authorization` và `Bypass-Tunnel-Reminder`
* Xử lý việc lấy kết quả
* Trả về kết quả (đối tượng phản hồi)

Điều này giúp dễ dàng gọi Extras API của bạn từ extension của bạn.

Bạn có thể chỉ định:

* Phương thức yêu cầu: GET, POST, v.v.
* Các header bổ sung
* Body cho các yêu cầu POST
* Bất kỳ tùy chọn fetch nào khác
