---
order: -20
icon: file-added
templating: false
route: /vi/for-contributors/writing-extensions/
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
    },
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate"
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
* `hooks` là một đối tượng tùy chọn chỉ định tên hàm [lifecycle hook](#lifecycle-hooks) được export từ module JS entry point.

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

### Thực hành tốt nhất cho việc khởi tạo extension

* Sử dụng hook `activate` cho việc thiết lập đồng bộ cần chạy trong giai đoạn tải của SillyTavern trong khi loader chặn vẫn đang hoạt động.
* Sử dụng sự kiện `APP_INITIALIZED` cho việc thiết lập cần chạy sau khi tất cả extension và các thành phần giao diện đã được tải và thiết lập, nhưng trong khi loader vẫn đang chặn.
* Sử dụng sự kiện `APP_READY` cho việc thiết lập không đồng bộ không cần chặn SillyTavern khỏi việc sẵn sàng sử dụng. Nó nên sử dụng một timer hoặc cơ chế tương tự để trì hoãn việc xử lý, vì trình xử lý sự kiện sẽ được await.

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
* `Fuse` - Thư viện tìm kiếm mờ. [Docs](https://www.fusejs.io/).
* `DOMPurify` - Thư viện làm sạch HTML. [Docs](https://github.com/cure53/DOMPurify).
* `hljs` - Thư viện làm nổi bật cú pháp. [Docs](https://highlightjs.org/).
* `localforage` - Thư viện lưu trữ trình duyệt (lớp trừu tượng IndexedDB/localStorage). [Docs](https://localforage.github.io/localForage/).
* `Handlebars` - Thư viện templating. [Docs](https://handlebarsjs.com/).
* `css` - Công cụ phân tích/chuyển đổi CSS. [Docs](https://github.com/nicolo-ribaudo/css-tools).
* `Bowser` - Thư viện phát hiện trình duyệt/nền tảng. [Docs](https://github.com/bowser-js/bowser).
* `DiffMatchPatch` - Thư viện diff, so khớp và vá văn bản. [Docs](https://github.com/google/diff-match-patch).
* `Readability` / `isProbablyReaderable` - Thư viện trích xuất bài viết của Mozilla. [Docs](https://github.com/mozilla/readability).
* `SVGInject` - Thư viện chèn SVG nội tuyến. [Docs](https://github.com/nicolo-ribaudo/svg-inject).
* `showdown` - Thư viện chuyển đổi Markdown. [Docs](https://showdownjs.com/).
* `moment` - Thư viện thao tác ngày/giờ. [Docs](http://momentjs.com/).
* `seedrandom` - Trình tạo số ngẫu nhiên có seed. [Docs](https://github.com/davidbau/seedrandom).
* `Popper` - Công cụ định vị tooltip/popover. [Docs](https://popper.js.org/).
* `droll` - Thư viện tung xúc xắc. [Docs](https://github.com/thebinarypenguin/droll).
* `morphdom` - Thư viện diffing/vá DOM nhanh. [Docs](https://github.com/patrick-steele-iber/morphdom).
* `slideToggle` - Hiệu ứng trượt bật/tắt bằng JS thuần. [Docs](https://github.com/nicolo-ribaudo/slidetoggle).
* `chalk` - Tạo kiểu chuỗi terminal (sử dụng hạn chế trong trình duyệt). [Docs](https://github.com/chalk/chalk).
* `yaml` - Trình phân tích và chuyển đổi YAML. [Docs](https://eemeli.org/yaml/).
* `chevrotain` - Bộ công cụ xây dựng parser. [Docs](https://chevrotain.io/).
* `gzipSync` / `gzip` - Các tiện ích nén nhanh từ fflate. [Docs](https://github.com/101arrowz/fflate).

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

### HTML templates

Extension có thể sử dụng template HTML Handlebars để xây dựng giao diện của chúng. Đặt các file template `.html` trong thư mục extension của bạn và render chúng bằng hàm `renderExtensionTemplateAsync()` từ `getContext()`.

Hàm này nhận tên thư mục extension của bạn, tên file template (không có `.html`), và một đối tượng dữ liệu tùy chọn cho các biến template Handlebars. HTML trả về được tự động làm sạch bằng DOMPurify và bản địa hóa với các thuộc tính `data-i18n`.

```js
const { renderExtensionTemplateAsync } = SillyTavern.getContext();

// Renders 'third-party/my-extension/settings.html' with the given data
const settingsHtml = await renderExtensionTemplateAsync(
    'third-party/my-extension',
    'settings',
    { title: 'My Extension', version: '1.0', defaultValue: 'test' }
);

// Append to the extensions settings panel
$('#extensions_settings2').append(settingsHtml);
```

**Ví dụ file template** (`settings.html`):

```html
<div class="my-extension-settings">
    <div class="inline-drawer">
        <div class="inline-drawer-toggle inline-drawer-header">
            <b data-i18n="{{title}}">{{title}}</b>
            <div class="inline-drawer-icon fa-solid fa-circle-chevron-down down"></div>
        </div>
        <div class="inline-drawer-content">
            <label for="my_ext_option">
                <span data-i18n="Option">Option</span>
            </label>
            <input id="my_ext_option" type="text" value="{{defaultValue}}" />
        </div>
    </div>
</div>
```

!!!warning
`renderExtensionTemplate()` (đồng bộ) đã lỗi thời. Luôn sử dụng `renderExtensionTemplateAsync()` thay thế.
!!!

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

**Vòng đời ứng dụng:**

* `APP_INITIALIZED`: ứng dụng đã được khởi tạo và gần sẵn sàng, nhưng loader vẫn đang hiển thị. Các sửa đổi giao diện có thể được thực hiện tại đây. Nó sẽ tự động kích hoạt mỗi khi một listener mới được đính kèm sau khi ứng dụng được khởi tạo.
* `APP_READY`: ứng dụng đã được tải đầy đủ và sẵn sàng sử dụng. Nó sẽ tự động kích hoạt mỗi khi một listener mới được đính kèm sau khi ứng dụng sẵn sàng.

**Tin nhắn:**

* `MESSAGE_SENT`: tin nhắn được gửi bởi người dùng và ghi vào đối tượng `chat` nhưng chưa được hiển thị trong giao diện người dùng.
* `MESSAGE_RECEIVED`: tin nhắn LLM được tạo và ghi vào đối tượng `chat` nhưng chưa được hiển thị trong giao diện người dùng.
* `USER_MESSAGE_RENDERED`: tin nhắn được gửi bởi người dùng được hiển thị trong giao diện người dùng.
* `CHARACTER_MESSAGE_RENDERED`: tin nhắn LLM được tạo được hiển thị trong giao diện người dùng.
* `MESSAGE_EDITED`: một tin nhắn đã được người dùng chỉnh sửa.
* `MESSAGE_DELETED`: một tin nhắn đã bị xóa.
* `MESSAGE_SWIPED`: một lượt swipe tin nhắn đã được kích hoạt.
* `STREAM_TOKEN_RECEIVED`: một token mới đã được nhận trong quá trình tạo streaming.

**Tạo:**

* `GENERATION_AFTER_COMMANDS`: việc tạo sắp bắt đầu sau khi xử lý các slash command.
* `GENERATION_STARTED`: việc tạo đã bắt đầu.
* `GENERATION_STOPPED`: việc tạo đã bị dừng bởi người dùng.
* `GENERATION_ENDED`: việc tạo đã hoàn thành hoặc đã gặp lỗi.

**Trò chuyện:**

* `CHAT_CHANGED`: cuộc trò chuyện đã được chuyển đổi (ví dụ: chuyển sang nhân vật khác hoặc cuộc trò chuyện khác được tải).
* `CHAT_CREATED`: một cuộc trò chuyện mới đã được tạo.
* `CHAT_DELETED`: một cuộc trò chuyện đã bị xóa.

**Nhân vật:**

* `CHARACTER_EDITED`: dữ liệu của một nhân vật đã bị thay đổi.
* `CHARACTER_DELETED`: một nhân vật đã bị xóa.
* `CHARACTER_DUPLICATED`: một nhân vật đã được nhân bản.

**Persona:**

* `PERSONA_CHANGED`: persona đang hoạt động đã được thay đổi.
* `PERSONA_CREATED`: một persona mới đã được tạo.
* `PERSONA_UPDATED`: một persona đã được cập nhật.
* `PERSONA_RENAMED`: một persona đã được đổi tên.
* `PERSONA_DELETED`: một persona đã bị xóa.

**Cài đặt và preset:**

* `SETTINGS_UPDATED`: cài đặt ứng dụng đã được cập nhật.
* `PRESET_CHANGED`: preset đang hoạt động đã được thay đổi.
* `MAIN_API_CHANGED`: loại API chính đã được chuyển đổi.
* `CHATCOMPLETION_SOURCE_CHANGED`: nguồn chat completion đã thay đổi.
* `CHATCOMPLETION_MODEL_CHANGED`: model chat completion đã thay đổi.
* `CONNECTION_PROFILE_LOADED`: một connection profile đã được tải.

**World Info:**

* `WORLDINFO_UPDATED`: dữ liệu world info đã được cập nhật.
* `WORLDINFO_SETTINGS_UPDATED`: cài đặt world info đã được thay đổi.

**Tool calling:**

* `TOOL_CALLS_PERFORMED`: các tool call đã được thực thi.
* `TOOL_CALLS_RENDERED`: kết quả tool call đã được hiển thị trong trò chuyện.

**Text-to-Speech:**

* `TTS_JOB_STARTED`: một tác vụ TTS đã bắt đầu.
* `TTS_AUDIO_READY`: dữ liệu âm thanh TTS đã sẵn sàng để phát.
* `TTS_JOB_COMPLETE`: một tác vụ TTS đã hoàn thành.

Danh sách đầy đủ các loại sự kiện có thể được tìm thấy [trong mã nguồn](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/events.js).

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

## Lifecycle Hooks

Extension có thể định nghĩa các lifecycle hook trong `manifest.json` được gọi tại các điểm cụ thể trong vòng đời của extension. Mỗi hook ánh xạ đến một **hàm được export** từ module JS entry point của extension (file được chỉ định trong trường `js`).

Tất cả các hook đều là tùy chọn. Hàm hook có thể trả về một `Promise` sẽ được await (với timeout 5 giây). Nếu một hook vượt quá thời gian chờ, một cảnh báo sẽ được ghi log và việc thực thi tiếp tục. Lỗi trong hook được bắt và ghi log mà không chặn hoạt động.

### Các hook có sẵn

| Hook | Khi nào được gọi |
|------|-----------------|
| `activate` | Khi extension được kích hoạt thành công trong quá trình tải trang |
| `install` | Sau khi extension được cài đặt và cài đặt của nó được tải |
| `update` | Sau khi cập nhật extension thành công (trước toast thông báo tải lại) |
| `delete` | Trước khi extension bị xóa khỏi server |
| `enable` | Trước khi extension được bật và cài đặt được lưu |
| `disable` | Trước khi extension bị tắt và cài đặt được lưu |
| `clean` | Khi người dùng nhấp vào nút "Clean extension data" trong trình quản lý extension, hoặc chọn một tùy chọn để dọn dẹp khi xóa extension |

### Cấu hình manifest

Thêm một đối tượng `hooks` vào `manifest.json` của bạn ánh xạ tên hook đến tên hàm được export:

```json
{
    "display_name": "My Extension",
    "js": "index.js",
    // Other fields here...
    "hooks": {
        "install": "onInstall",
        "update": "onUpdate",
        "delete": "onDelete",
        "enable": "onEnable",
        "disable": "onDisable",
        "activate": "onActivate",
        "clean": "onClean"
    }
}
```

Tên có thể được chọn tự do, miễn là chúng là tên hàm JS hợp lệ.
Bạn có thể cấu hình bao nhiêu hook tùy thích, bạn không cần phải điền và triển khai tất cả chúng.

### Triển khai

Export các hàm hook từ JS entry point chính của bạn. Mỗi hàm không nhận tham số và có thể tùy chọn trả về một `Promise`:

```js
// index.js - your extension's entry point

export async function onInstall() {
    console.log('Extension installed! Performing first-time setup...');
    // e.g., initialize default data, create storage entries
}

export async function onActivate() {
    console.log('Extension activated during page load');
}

export async function onUpdate() {
    console.log('Extension updated! Running migrations...');
    // e.g., migrate data from old format to new format
}

export async function onDelete() {
    console.log('Extension about to be deleted. Cleaning up...');
    // e.g., remove stored data, clean up localStorage
    const { localforage } = SillyTavern.libs;
    await localforage.removeItem('my_extension_data');
}

export function onEnable() {
    console.log('Extension enabled');
}

export function onDisable() {
    console.log('Extension disabled');
}

export async function onClean() {
    console.log('Extension data cleaned');
    // e.g., cleanup of the extension's data here
}
```

!!!warning
Hàm hook có **timeout 5 giây**. Nếu hook của bạn mất nhiều thời gian hơn, việc thực thi sẽ tiếp tục và một cảnh báo sẽ được ghi log. Giữ cho logic hook nhanh và nhẹ.
!!!

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

### Hệ thống macro mới

Cách được khuyến nghị để đăng ký macro là thông qua hàm `macros.register()` có sẵn qua `SillyTavern.getContext()`. Hệ thống này hỗ trợ các tham số, danh mục, mô tả và metadata tài liệu phong phú.

```js
const { macros } = SillyTavern.getContext();

// Simple macro with a handler function
macros.register('tomorrow', {
    description: 'Returns tomorrow\'s date',
    handler: () => {
        return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
    },
});

// Macro with unnamed arguments and a category
macros.register('greet', {
    description: 'Generates a greeting for the given name',
    category: macros.category.UTILITY,
    unnamedArgs: [
        { name: 'name', description: 'The name to greet' },
    ],
    handler: ({ unnamedArgs }) => {
        const [name] = unnamedArgs;
        return `Hello, ${name}!`;
    },
});
```

Hàm `handler` nhận một đối tượng [MacroExecutionContext](https://github.com/SillyTavern/SillyTavern/blob/staging/public/scripts/macros/engine/MacroRegistry.js) chứa

* `args` - Tất cả các tham số không có tên được truyền vào macro.
* `unnamedArgs` - Các tham số vị trí khớp với danh sách tham số được định nghĩa.
* `list` - Các tham số dạng danh sách (sau các tham số không có tên), hoặc `null` nếu list không được bật.
* `env` - Môi trường macro với quyền truy cập vào dữ liệu nhân vật, trạng thái trò chuyện, v.v.
* `resolve(text)` - Hàm để phân giải các macro lồng nhau trong văn bản (khi `delayArgResolution` là `true`).

Và nhiều hơn nữa.

Handler sẽ chạy đồng bộ, vì vậy chúng không bao giờ có thể trả về một `Promise` hoặc gọi các hành động không đồng bộ một cách đồng bộ.

Để hủy đăng ký một macro:

```js
const { macros } = SillyTavern.getContext();

macros.registry.unregisterMacro('greet');
```

Bạn cũng có thể đăng ký các alias cho các macro hiện có:

```js
const { macros } = SillyTavern.getContext();

macros.registerAlias('greet', 'hello', { visible: true });
```

### Hệ thống macro cũ (đã lỗi thời)

!!!warning
`registerMacro()` và `unregisterMacro()` từ `getContext()` đã **lỗi thời**. Sử dụng `macros.register()` và `macros.registry.unregisterMacro()` thay thế.
!!!

API cũ vẫn khả dụng để tương thích ngược, nhưng sẽ bị loại bỏ trong một bản phát hành tương lai:

```js
const { registerMacro, unregisterMacro } = SillyTavern.getContext();

// Simple string macro
registerMacro('fizz', 'buzz');
// Function macro (must be synchronous)
registerMacro('tomorrow', () => {
    return new Date(Date.now() + 24 * 60 * 60 * 1000).toLocaleDateString();
});

// Unregister
unregisterMacro('fizz');
```

## Message formatting hooks

!!!warning Tính năng Staging
Điều này hiện chỉ khả dụng trên nhánh `staging` của SillyTavern, và không phải là một phần của bản phát hành mới nhất.
!!!

Extension có thể móc vào pipeline định dạng tin nhắn để biến đổi văn bản tin nhắn trước khi nó đến DOM. Điều này hữu ích để thêm chú thích (ruby tag, tooltip), làm nổi bật, hoặc các biến đổi văn bản tùy chỉnh.

!!!warning
Hook chạy đồng bộ và **phải trả về một chuỗi**. Các hàm async và các giá trị trả về không phải chuỗi sẽ ném ra một `TypeError` tại thời điểm đăng ký hoặc bị bỏ qua một cách âm thầm khi chạy với một cảnh báo trên console. Không thực hiện các hoạt động tốn kém trong các hook này — chúng chạy trên mỗi lần render tin nhắn.
!!!

### Các giai đoạn pipeline

Hook có thể được đăng ký cho ba giai đoạn pipeline. Tất cả các giai đoạn chạy **trước** khi làm sạch bằng DOMPurify, vì vậy đầu ra luôn an toàn:

| Giai đoạn | Khi nào nó chạy | Định dạng văn bản |
|-------|--------------|-------------|
| `beforeRegex` | Sau khi loại bỏ prompt-bias, trước các quy tắc regex tùy chỉnh | Văn bản thuần |
| `afterRegex` | Sau các quy tắc regex tùy chỉnh, trước khi chuyển đổi Markdown | Văn bản thuần |
| `afterMarkdown` | Sau khi chuyển đổi Markdown sang HTML (showdown), trước DOMPurify | Chuỗi HTML |

Giai đoạn `afterMarkdown` là mặc định và là điểm chèn phổ biến nhất cho các extension muốn chú thích HTML đã render.

### Đăng ký một hook

Truy cập `messageFormatter` từ `getContext()`:

```js
const { messageFormatter } = SillyTavern.getContext();

// Simple hook - transforms message text
messageFormatter.addHook((mes, ctx) => {
    // Skip user messages
    if (ctx.isUser) return mes;

    // Add furigana to Japanese text
    return addFurigana(mes);
});

// Hook with explicit stage and order
messageFormatter.addHook((mes, ctx) => {
    // Transform after Markdown conversion but before sanitization
    return mes.replace(/\*\*(.+?)\*\*/g, '<mark>$1</mark>');
}, {
    stage: messageFormatter.stage.AFTER_MARKDOWN,
    order: messageFormatter.order.EARLY,
});
```

### Ngữ cảnh hook

Hook nhận một đối tượng ngữ cảnh bất biến với metadata tin nhắn:

| Thuộc tính | Kiểu | Mô tả |
|----------|------|-------------|
| `characterName` | `string` | Tên nhân vật liên kết với tin nhắn |
| `isSystem` | `boolean` | Tin nhắn có phải là tin nhắn hệ thống hay không |
| `isUser` | `boolean` | Tin nhắn có được gửi bởi người dùng hay không |
| `messageId` | `number` | Chỉ số của tin nhắn trong mảng chat, hoặc `-1` cho các tin nhắn tạm thời (bản xem trước streaming) |
| `isReasoning` | `boolean` | Tin nhắn có phải là đầu ra reasoning/thinking hay không |
| `stage` | `string` | Giai đoạn pipeline hiện đang được thực thi |

Đối tượng ngữ cảnh được đóng băng bằng `Object.freeze()` — việc cố gắng sửa đổi nó sẽ không có tác dụng.

### Thứ tự hook

Các hook trong một giai đoạn chạy theo thứ tự tăng dần. Sử dụng tùy chọn `order` để kiểm soát thứ tự thực thi:

```js
const { hook_order } = messageFormatter;

// Predefined constants
hook_order.EARLIEST;  // 0
hook_order.EARLY;     // 10
hook_order.NORMAL;    // 50 (default)
hook_order.LATE;      // 90
hook_order.LATEST;    // 100

// Custom numeric value
messageFormatter.addHook(myHook, { order: 25 });
```

Số nhỏ hơn chạy trước. Điều này hữu ích khi nhiều extension biến đổi cùng một văn bản — ví dụ, một extension có thể trích xuất dữ liệu sớm, và một extension khác có thể định dạng nó sau đó.

### Xử lý lỗi

Việc thực thi hook được bọc trong try/catch. Nếu một hook ném lỗi, nó sẽ bị bỏ qua và một lỗi console được ghi log — pipeline tiếp tục với các hook còn lại.

Nếu một hook trả về một giá trị không phải chuỗi (bao gồm `undefined` hoặc một `Promise`), một cảnh báo console sẽ được phát ra và giá trị trả về sẽ bị bỏ qua. Pipeline tiếp tục với văn bản trước đó không thay đổi.

### Thứ tự pipeline đầy đủ

Để tham khảo, pipeline định dạng tin nhắn hoàn chỉnh là:

1. Loại bỏ prompt-bias (chỉ tin nhắn 0)
2. Chuẩn hóa comment / tin nhắn ẩn
3. Hook extension `beforeRegex`
4. Các quy tắc regex tùy chỉnh (`getRegexedString`)
5. Hook extension `afterRegex`
6. Tự động sửa Markdown (`fixMarkdown`)
7. Mã hóa thẻ HTML (`encode_tags`)
8. Chuyển đổi Showdown Markdown → HTML
9. Hook extension `afterMarkdown`
10. Loại bỏ tiền tố tên (`allow_name2_display`)
11. Làm sạch DOMPurify

Tất cả các hook extension (bước 3, 5, 9) chạy **trước** DOMPurify để đầu ra của chúng luôn được làm sạch.

## Function tool calling

Extension có thể đăng ký các function tool tùy chỉnh mà LLM có thể gọi trong quá trình chat completion. Điều này cho phép extension của bạn phản ứng với dữ liệu có cấu trúc từ model — ví dụ, truy vấn API, thực hiện tính toán, hoặc kích hoạt các tính năng của extension.

Để có hướng dẫn đầy đủ bao gồm các yêu cầu tiên quyết, API được hỗ trợ, các trường đăng ký, và mẹo, xem trang [Function Calling](./Function-Calling.md) chuyên dụng.

**Ví dụ nhanh:**

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
            location: { type: 'string', description: 'City name' },
        },
        required: ['location'],
    },
    action: async ({ location }) => {
        const data = await fetchWeatherData(location);
        return JSON.stringify(data);
    },
});
```

## Action loader

Action loader cung cấp một lớp phủ tải và hệ thống thông báo toast cho các hoạt động chạy lâu. Nó thay thế các hàm `showLoader()` / `hideLoader()` đã lỗi thời.

Truy cập nó qua `loader` từ `getContext()`:

```js
const { loader } = SillyTavern.getContext();

// Basic blocking loader with a stoppable toast
const handle = loader.show({ message: 'Processing data...' });
try {
    const result = await someExpensiveOperation();
} finally {
    await handle.hide();
}
```

### Tùy chọn

| Tùy chọn | Mặc định | Mô tả |
|--------|---------|-------------|
| `blocking` | `true` | Hiển thị một lớp phủ toàn màn hình chặn tương tác |
| `message` | `'Generating...'` | Thông điệp được hiển thị trong thông báo toast |
| `title` | `''` | Tiêu đề tùy chọn cho toast |
| `toastMode` | `'stoppable'` | `'stoppable'` (có nút dừng), `'static'` (không có nút), hoặc `'none'` (không có toast) |
| `stopTooltip` | `'Stop'` | Văn bản tooltip cho nút dừng |
| `onStop` | `null` | Trình xử lý dừng tùy chỉnh. Mặc định là `stopGeneration()` |
| `onHide` | `null` | Được gọi khi loader bị ẩn (không phải dừng) |
| `overlayContent` | `null` | Phần tử HTML hoặc chuỗi tùy chỉnh thay thế spinner mặc định |

### Xếp chồng loader

Nhiều loader có thể hoạt động đồng thời. Lớp phủ vẫn hiển thị miễn là có ít nhất một blocking loader đang hoạt động:

```js
const { loader } = SillyTavern.getContext();

const loader1 = loader.show({ message: 'Task 1...' });
const loader2 = loader.show({ message: 'Task 2...' });
await loader1.hide(); // Overlay stays — loader2 is still active
await loader2.hide(); // Now overlay hides
```

### Loader không chặn

Đối với các tác vụ nền không nên chặn giao diện:

```js
const { loader } = SillyTavern.getContext();

const handle = loader.show({
    blocking: false,
    message: 'Downloading in background...',
    onStop: () => abortDownload(),
});
```

## Popups và phản hồi người dùng

### Popup helper

SillyTavern cung cấp các popup helper tiện lợi qua `Popup.show` từ `getContext()`:

```js
const { Popup } = SillyTavern.getContext();

// Confirmation dialog — returns POPUP_RESULT.AFFIRMATIVE or POPUP_RESULT.NEGATIVE
const confirmed = await Popup.show.confirm('Confirm Action', 'Are you sure you want to proceed?');

// Text input dialog — returns the entered string, or null if cancelled
const userInput = await Popup.show.input('Enter Name', 'Please provide a name:', 'default value');

// Information display — returns the clicked button result
await Popup.show.text('Info', 'Operation completed successfully.');
```

### Popup tùy chỉnh

Đối với các popup phức tạp hơn, khởi tạo `Popup` trực tiếp với đầy đủ tùy chọn:

```js
const { Popup, POPUP_TYPE, POPUP_RESULT } = SillyTavern.getContext();

const popup = new Popup(
    '<div>Custom HTML content here</div>',
    POPUP_TYPE.TEXT,
    '',
    {
        wide: true,              // Wide display mode
        okButton: 'Save',       // Custom OK button text
        cancelButton: 'Discard', // Custom Cancel button text
        customButtons: [
            {
                text: 'Export',
                icon: 'fa-download',
                result: POPUP_RESULT.CUSTOM1,
            },
        ],
        customInputs: [
            {
                id: 'my_checkbox',
                label: 'Enable feature',
                type: 'checkbox',
                defaultState: false,
            },
        ],
        allowVerticalScrolling: true,
    }
);

const result = await popup.show();

if (result === POPUP_RESULT.AFFIRMATIVE) {
    // OK was clicked
} else if (result === POPUP_RESULT.CUSTOM1) {
    // Export button was clicked
}

// Read custom input values
const checkboxValue = popup.inputResults?.get('my_checkbox');
```

### Các loại Popup

| Loại | Mô tả |
|------|-------------|
| `POPUP_TYPE.TEXT` | Popup nội dung chung với các nút |
| `POPUP_TYPE.CONFIRM` | Hộp thoại xác nhận Có/Không |
| `POPUP_TYPE.INPUT` | Popup với một trường nhập văn bản |
| `POPUP_TYPE.DISPLAY` | Popup chỉ có nội dung với một nút đóng |
| `POPUP_TYPE.CROP` | Popup cắt hình ảnh |

### Thông báo Toast

Đối với phản hồi nhẹ, sử dụng `toastr` (khả dụng toàn cục):

```js
toastr.success('Data saved successfully');
toastr.error('Failed to connect to API');
toastr.warning('This feature is experimental');
toastr.info('Processing...');
```

## Data bank scraper

Extension có thể đăng ký các data scraper tùy chỉnh cho tính năng Data Bank. Scraper cung cấp một cách để nhập dữ liệu từ các nguồn tùy chỉnh (ví dụ: trang web, API, định dạng file):

```js
const { registerDataBankScraper } = SillyTavern.getContext();

await registerDataBankScraper({
    id: 'my_scraper',
    name: 'My Data Source',
    description: 'Import data from My Data Source',
    iconClass: 'fa-solid fa-database',
    iconAvailable: true,
    isAvailable: async () => true,
    scrape: async () => {
        // Return an array of File objects
        const content = await fetchDataFromSource();
        return [new File([content], 'data.txt', { type: 'text/plain' })];
    },
});
```

## Debug functions

Extension có thể đăng ký các hàm debug tùy chỉnh xuất hiện trong Debug Menu (có thể truy cập qua cài đặt power user). Điều này hữu ích để lộ diện các công cụ chẩn đoán, chức năng cache/cleanup hoặc các trigger thủ công trong quá trình phát triển:

```js
const { registerDebugFunction } = SillyTavern.getContext();

registerDebugFunction(
    'my_ext_clear_cache',        // Unique function ID
    'Clear My Extension Cache',   // Display name
    'Clears all cached data for My Extension', // Description
    async () => {
        const { localforage } = SillyTavern.libs;
        await localforage.removeItem('my_extension_cache');
        toastr.success('Cache cleared');
    }
);
```

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

## Thực hành tốt nhất

### Bảo mật

**Không bao giờ lưu trữ API key hoặc bí mật trong `extensionSettings`**

Cài đặt extension có thể truy cập được bởi tất cả các extension khác và được lưu trữ dưới dạng văn bản thuần. Không lưu trữ dữ liệu nhạy cảm ở phía client:

```js
// BAD - Don't do this!
extensionSettings[MODULE_NAME].apiKey = 'secret_key_123';

// NOTE: There is no secure way to store secrets in client-side extensions.
// If you need to handle sensitive data, use server plugins instead.
// See: https://docs.sillytavern.app/for-contributors/server-plugins/
```

**Làm sạch đầu vào của người dùng**

Luôn xác thực và làm sạch dữ liệu từ đầu vào của người dùng trước khi sử dụng nó trong các lệnh, lời gọi API, hoặc thao tác DOM:

```js
// Validate input type first
if (typeof userInput !== 'string') {
    toastr.error('Invalid input type');
    return;
}
// Use DOMPurify to sanitize HTML input
const { DOMPurify } = SillyTavern.libs;
const cleanInput = DOMPurify.sanitize(userInput);
```

**Tránh sử dụng `eval()` hoặc các constructor `Function()`**

Chúng có thể thực thi mã tùy ý và gây ra rủi ro bảo mật. Nếu bạn cần đánh giá động, hãy sử dụng các phương án thay thế an toàn hơn hoặc hạn chế đầu vào một cách cẩn thận.

### Hiệu năng

**Không lưu trữ dữ liệu lớn trong `extensionSettings`**

Cài đặt extension được tải vào bộ nhớ và lưu thường xuyên. Dữ liệu lớn có thể gây ra vấn đề về hiệu năng:

```js
// BAD - Don't store large data
extensionSettings[MODULE_NAME].largeDataset = { /* megabytes of data */ };

// GOOD - Use localforage (abstraction over IndexedDB/localStorage)
const { localforage } = SillyTavern.libs;
await localforage.setItem(`${MODULE_NAME}_data`, largeData);

// Or use localStorage for smaller data
localStorage.setItem(`${MODULE_NAME}_data`, JSON.stringify(smallData));
```

**Dọn dẹp các event listener**

Loại bỏ các event listener khi chúng không còn cần thiết để tránh rò rỉ bộ nhớ:

```js
function cleanup() {
    eventSource.removeListener(event_types.MESSAGE_RECEIVED, handleMessage);
    document.getElementById('myElement').removeEventListener('click', handleClick);
}
```

**Không chặn luồng giao diện (UI thread)**

Đối với các hoạt động nặng, sử dụng async/await hoặc web worker:

```js
// Use async for I/O operations
async function processData() {
    const result = await fetch('/api/process');
    return result.json();
}

// Break up heavy computations
async function heavyComputation(data) {
    for (let i = 0; i < data.length; i++) {
        // Process chunk
        if (i % 1000 === 0) {
            await new Promise(resolve => setTimeout(resolve, 0)); // Yield to UI
        }
    }
}
```

### Khả năng tương thích

**Ưu tiên `getContext()` hơn là import trực tiếp**

API ngữ cảnh ổn định hơn và ít có khả năng bị hỏng khi SillyTavern cập nhật:

```js
// GOOD - Stable API
const { chat, characters, saveSettingsDebounced } = SillyTavern.getContext();

// AVOID - May break with internal changes
import { chat, characters } from '../../../../script.js';
```

**Sử dụng tên module duy nhất**

Ngăn chặn xung đột với các extension khác bằng cách sử dụng một tên module mô tả, duy nhất:

```js
// GOOD - Specific and unique
const MODULE_NAME = 'my_extension_name';

// BAD - Too generic, likely to conflict
const MODULE_NAME = 'settings';
```

### Trải nghiệm người dùng

**Cung cấp phản hồi rõ ràng**

Sử dụng `toastr` cho các thông báo nhẹ và `Popup` cho các tương tác quan trọng của người dùng. Xem phần [Popups và phản hồi người dùng](#popups-va-phan-hoi-nguoi-dung) để biết đầy đủ chi tiết.

Đối với các hoạt động chạy lâu, sử dụng [Action loader](#action-loader) thay vì chặn giao diện một cách âm thầm.

**Cung cấp thông điệp console hữu ích**

Sử dụng một tiền tố nhất quán cho các log console của bạn. Nhưng không spam console với quá nhiều log trong môi trường production:

```js
const MODULE_NAME = 'MyExtension';

console.log(`[${MODULE_NAME}] Extension loaded`);
console.debug(`[${MODULE_NAME}] Processing data:`, data);
console.error(`[${MODULE_NAME}] Error occurred:`, error);
```

### Chất lượng code

**Sử dụng các thư viện được đóng gói sẵn từ `lib.js`**

Trước khi thêm dependency mới, hãy kiểm tra phần [Shared libraries](#shared-libraries) — SillyTavern đóng gói nhiều thư viện phổ biến (lodash, Fuse, DOMPurify, moment, yaml, v.v.) có sẵn qua `SillyTavern.libs`.

**Khởi tạo cài đặt đúng cách**

Luôn cung cấp giá trị mặc định và xử lý các khóa bị thiếu:

```js
function loadSettings() {
    // Merge with defaults to handle new keys after updates and initialize if it doesn't exist.
    extensionSettings[MODULE_NAME] = SillyTavern.libs.lodash.merge(
        structuredClone(defaultSettings),
        extensionSettings[MODULE_NAME]
    );
}
```
