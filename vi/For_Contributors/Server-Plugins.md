---
order: -30
icon: server
route: /for-contributors/server-plugins/
---

# Server Plugins

Các plugin này cho phép thêm chức năng không thể đạt được chỉ bằng cách sử dụng UI extension, chẳng hạn như tạo các API endpoint mới hoặc sử dụng các package Node.JS không có sẵn trong môi trường trình duyệt.

Các plugin được chứa trong thư mục `plugins` của SillyTavern và được tải khi khởi động server, nhưng *chỉ* khi `enableServerPlugins` được đặt thành `true` trong file `config.yaml`.

!!!warning Cảnh báo
 **Server Plugins không được sandbox. Điều này có nghĩa là chúng có khả năng truy cập vào toàn bộ hệ thống file của bạn hoặc gây ra nhiều lỗ hổng bảo mật theo cách mà các UI extension thông thường không thể. Chỉ cài đặt server plugin từ các nhà phát triển mà bạn tin tưởng!**
!!!

Để biết danh sách tất cả các server plugin chính thức, xem danh sách tổ chức GitHub: <https://github.com/search?q=topic%3Aplugin+org%3ASillyTavern&type=Repositories>

## Các loại plugin

### Files

Một file JavaScript thực thi với phần mở rộng ".js" (cho các module CommonJS) hoặc ".mjs" (cho các module ES) chứa một module xuất một hàm `init`. Hàm này chấp nhận một Express router (được tạo riêng cho plugin của bạn) làm tham số và trả về một Promise.

Module cũng nên xuất một đối tượng `info` chứa thông tin về plugin (các chuỗi `id`, `name`, và `description`). Điều này sẽ cung cấp thông tin về plugin cho trình tải.

Bạn có thể đăng ký các route thông qua router sẽ được đăng ký dưới đường dẫn `/api/plugins/{id}/{route}`. Ví dụ, `router.get('/foo')` cho plugin `example` sẽ tạo ra một route như thế này: `/api/plugins/example/foo`.

Một plugin cũng có thể *tùy chọn* xuất một hàm `exit` để thực hiện dọn dẹp khi tắt server. Nó không nên có tham số và phải trả về một Promise.

TypeScript contract cho plugin exports:

```ts
interface PluginInfo {
    id: string;
    name: string;
    description: string;
}

interface Plugin {
    init: (router: Router) => Promise<void>;
    exit: () => Promise<void>;
    info: PluginInfo;
}
```

Xem bên dưới để có ví dụ về plugin "Hello world!":

```js
/**
 * Initialize plugin.
 * @param {import('express').Router} router Express router
 * @returns {Promise<any>} Promise that resolves when plugin is initialized
 */
async function init(router) {
    // Do initialization here...
    router.get('/foo', req, res, function () {
       res.send('bar');
    });
    console.log('Example plugin loaded!');
    return Promise.resolve();
}

async function exit() {
    // Do some clean-up here...
    return Promise.resolve();
}

module.exports = {
    init,
    exit,
    info: {
        id: 'example',
        name: 'Example',
        description: 'My cool plugin!',
    },
};
```

### Directories

Bạn có thể tải một plugin từ một thư mục con trong thư mục `plugins` theo một trong các cách sau (theo thứ tự ưu tiên):

1. Một file `package.json` chứa đường dẫn đến file thực thi trong trường "main".
2. Một file `index.js` cho các module CommonJS.
3. Một file `index.mjs` cho các module ES.

File kết quả phải xuất một hàm `init` và một đối tượng `info` với các yêu cầu giống như đối với các file riêng lẻ.

Ví dụ về một plugin thư mục (với file `index.js`): <https://github.com/SillyTavern/SillyTavern-DiscordRichPresence-Server>

### Bundling

Tốt hơn nên sử dụng một bundler (như Webpack hoặc Browserify) để đóng gói tất cả các yêu cầu vào một file. Đảm bảo đặt "Node" làm mục tiêu build.

Repository mẫu cho các plugin sử dụng Webpack và TypeScript: <https://github.com/SillyTavern/Plugin-WebpackTemplate>
