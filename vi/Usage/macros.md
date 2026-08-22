---
order: 140
icon: codescan
route: /vi/usage/core-concepts/macros/
templating: false
---

# Macros

!!!tip Experimental Macro Engine
Experimental Macro Engine hỗ trợ lồng nhau, thứ tự thay thế ổn định, và các cải tiến khác. Nó được bật theo mặc định cho các bản cài đặt mới. Các bản cài đặt hiện có có thể bật nó trong **User Settings** > **Chat/Message Handling** > **Experimental Macro Engine**.
!!!

Macros là các placeholder động được thay thế bằng giá trị thực tế khi văn bản được xử lý. Chúng được sử dụng xuyên suốt SillyTavern trong prompts, character cards, lorebooks, Quick Replies, và nhiều nơi khác.

## Tìm các Macro có sẵn

SillyTavern cung cấp tài liệu tích hợp sẵn cho tất cả các macro có sẵn:

- **Lệnh Slash**: Nhập `/? macros` vào ô nhập chat để hiển thị danh sách tất cả các macro đã đăng ký cùng với mô tả của chúng.
- **Autocomplete**: Xem [Macro Autocomplete](#macro-autocomplete) bên dưới để biết chi tiết về cách nhận gợi ý khi gõ.

### Macro Autocomplete

Macro autocomplete cung cấp gợi ý cho các macro có sẵn khi bạn gõ. Nó hoạt động trong tất cả các trường văn bản hỗ trợ macro trên toàn bộ SillyTavern.

Gõ `{{` để bắt đầu autocomplete cho macro, hiển thị các macro có sẵn và các đối số của chúng, các [Macro Flags](#macro-flags) tiềm năng, [Variable Shorthands](#variable-shorthands), và nhiều hơn nữa.

**Nơi autocomplete xuất hiện theo mặc định:**

- Ô nhập chat của người dùng
- Trình chỉnh sửa mở rộng (chỉnh sửa văn bản toàn màn hình, mở qua nút 'Expand' bên cạnh các trường văn bản)
- Trình chỉnh sửa Prompt Manager

**Kích hoạt autocomplete trong các trường khác:**

- Nhấn **Ctrl+Space** trong bất kỳ trường văn bản hỗ trợ macro nào để mở popup autocomplete
- Bật **Settings → AutoComplete Settings → Show in all macro fields** để autocomplete tự động xuất hiện trong tất cả các trường macro

## Cú pháp Cơ bản

Macros được bọc trong dấu ngoặc nhọn kép:

```txt
{{macroName}}
```

Tên macro **không phân biệt chữ hoa chữ thường**. `{{User}}`, `{{USER}}`, và `{{user}}` đều được phân giải thành cùng một macro.

Ví dụ:

```txt
{{user}}        // Trả về tên người dùng/persona hiện tại
{{char}}        // Trả về tên nhân vật hiện tại
{{time}}        // Trả về thời gian hiện tại
{{date}}        // Trả về ngày hiện tại
```

## Đối số (Arguments)

Nhiều macro chấp nhận đối số để tùy chỉnh hành vi của chúng.

### Dấu phân cách Khoảng trắng

Đối với các macro có một đối số duy nhất, một khoảng trắng có thể phân tách tên macro với đối số của nó:

```txt
{{macroName argument}}
```

Ví dụ:

```txt
{{getvar myVariable}}
{{roll 1d20}}
{{reverse Hello World}}
```

### Dấu phân cách Hai dấu hai chấm

Sử dụng `::` để phân tách nhiều đối số:

```txt
{{macroName::arg1::arg2::arg3}}
```

Ví dụ:

```txt
{{setvar::myVariable::Hello World}}
{{random::red::green::blue}}
{{roll::2d6+3}}
```

Cả khoảng trắng và `::` đều là cú pháp được khuyến nghị cho đối số macro.

### Dấu phân cách Một dấu hai chấm (Legacy)

Một dấu `:` đơn cũng có thể được sử dụng để đưa vào đối số, nhưng cú pháp này được coi là legacy và không được khuyến nghị cho nội dung mới:

```txt
{{macroName:argument}}
```

Ví dụ:

```txt
{{roll:1d20}}
```

## Khoảng trắng trong Định nghĩa Macro

Khoảng trắng giữa tên macro, dấu phân cách và đối số bị bỏ qua. Điều này cho phép định dạng dễ đọc hơn:

```txt
{{ macroName :: arg1 :: arg2 }}
{{ setvar :: myVariable :: some value }}
{{ if :: condition }}
```

Tất cả các ví dụ trên đều tương đương với dạng thu gọn của chúng không có khoảng trắng thừa.

## Macros Lồng nhau

Macros có thể được lồng vào bên trong các macro khác. Macro bên trong được phân giải trước:

```txt
{{getvar::{{char}}_mood}}
```

Điều này trước tiên phân giải `{{char}}` (ví dụ, thành "Alice"), sau đó phân giải `{{getvar::Alice_mood}}`.

Thêm ví dụ:

```txt
{{setvar::greeting::Hello, {{user}}!}}
```

Đặt một biến với nội dung bao gồm tên của người dùng.

```txt
{{if {{getvar::showDetails}}}}Details here{{/if}}
```

Bản thân điều kiện là một macro lấy giá trị của một biến.

## Scoped Macros

Bất kỳ macro nào chấp nhận ít nhất một đối số đều hỗ trợ cú pháp scoped. Nội dung giữa thẻ mở và thẻ đóng trở thành **đối số cuối cùng** của macro.

### Cú pháp Scoped

Thay vì viết đối số cuối cùng inline, nó có thể được đặt giữa thẻ mở và thẻ đóng:

```txt
{{macroName argument}}
  scoped content here
{{/macroName}}
```

Thẻ đóng sử dụng flag `/` trước tên macro: `{{/macroName}}`.

Điều này tương đương với việc viết:

```txt
{{macroName::argument::scoped content here}}
```

### Ví dụ

Đặt một biến với nội dung nhiều dòng:

```txt
{{ setvar backstory }}
  This character was born in a small village
  and grew up to become a renowned scholar.
{{ /setvar }}
```

Sử dụng `reverse` với nội dung scoped:

```txt
{{ reverse }}
  Hello World
{{ /reverse }}
```

Điều này trả về "dlroW olleH".

### Cắt tỉa Nội dung (Content Trimming)

Theo mặc định, nội dung scoped được tự động cắt tỉa:

- Khoảng trắng đầu và cuối bị loại bỏ
- Thụt lề nhất quán bị de-dented (thụt lề của dòng đầu tiên không rỗng bị loại bỏ khỏi tất cả các dòng)

Điều này cho phép định dạng gọn gàng:

```txt
{{ if condition }}
    # Heading
    Some content here
{{ /if }}
```

Tạo ra `# Heading\nSome content here` (không có khoảng trắng đầu dòng).

Để giữ lại toàn bộ khoảng trắng bao gồm cả dòng mới ở đầu/cuối, sử dụng flag `#`. Xem [Macro Flags](#macro-flags) để biết chi tiết.

## Macros Điều kiện

Macro `{{if}}` hiển thị nội dung có điều kiện dựa trên việc một giá trị là truthy hay falsy.

### Điều kiện Đơn giản

```txt
{{ if description }}
  # Character Description
  {{ description }}
{{ /if }}
```

Điều này chỉ hiển thị tiêu đề và mô tả nếu `description` trả về một giá trị không rỗng.

Điều kiện có thể là:

- Một tên macro (tự động được phân giải nếu không yêu cầu đối số)
- Bất kỳ giá trị nào từ một macro lồng nhau như `{{getvar::flag}}`
- Một variable shorthand như `.myFlag` hoặc `$globalFlag` (xem [Variable Shorthands](#variable-shorthands))
- Bất kỳ văn bản nào bạn muốn (sẽ tự động phân giải thành truthy hoặc falsy dựa trên nội dung của nó)

Giá trị falsy: chuỗi rỗng, `false`, `0`, `off`, `no`.

### Sử dụng Variable Shorthands trong Điều kiện

Variable shorthands cung cấp một cách ngắn gọn để kiểm tra giá trị biến trong các điều kiện:

```txt
{{ if .isEnabled }}
  Feature is enabled.
{{ /if }}

{{ if !$globalDisabled }}
  Not globally disabled.
{{ /if }}
```

Xem [Variable Shorthands](#variable-shorthands) để biết thêm chi tiết về ký hiệu shorthand.

### Điều kiện Đảo ngược

Thêm tiền tố `!` vào điều kiện để đảo ngược nó:

```txt
{{ if !personality }}
  No personality defined for this character.
{{ /if }}
```

### Nhánh If/Else

Sử dụng `{{else}}` bên trong một khối `{{if}}` để định nghĩa một nhánh thay thế:

```txt
{{ if personality }}
  {{ personality }}
{{ else }}
  No personality defined.
{{ /if }}
```

Ví dụ khác:

```txt
{{ if {{getvar::details-block}} }}
  # Details Block
  {{ getvar::details-block }}
{{ else }}
  No details currently exist.
{{ /if }}
```

## Macro Flags

Flags là các ký tự ký hiệu đặc biệt đặt giữa dấu ngoặc mở và tên macro để điều chỉnh hành vi của macro.

### Cú pháp

```txt
{{!macroName}}
{{#macroName}}
```

Các flag có thể được kết hợp:

```txt
{{!?macroName}}
```

Khoảng trắng được cho phép giữa các flag và tên macro:

```txt
{{ / macroName }}
{{ # macroName }}
```

### Các Flag đã được Triển khai

| Flag | Tên | Mô tả |
|------|------|-------------|
| `/` | Closing Block | Đánh dấu một thẻ đóng cho scoped macros. Ví dụ: `{{/if}}` |
| `#` | Preserve Whitespace | Ngăn việc tự động cắt tỉa nội dung scoped. |

### Các Flag đã lên Kế hoạch (Chưa được Triển khai)

| Flag | Tên | Mô tả |
|------|------|-------------|
| `!` | Immediate | Phân giải macro này trước các macro khác trong cùng văn bản. |
| `?` | Delayed | Phân giải macro này sau các macro khác trong cùng văn bản. |
| `~` | Re-evaluate | Đánh dấu macro này để đánh giá lại. |
| `>` | Filter | Bật các bộ lọc đầu ra dựa trên pipe cho macro này. |

### Toán tử tiền tố giống flag

Cú pháp variable shorthand sử dụng các toán tử tiền tố (`.` và `$`) hoạt động tương tự như flag nhưng bản thân chúng không phải là flag.
Xem phần [Variable Shorthands](#variable-shorthands) để biết chi tiết.

### Flag Preserve Whitespace

Sử dụng flag `#` khi bạn cần giữ lại toàn bộ khoảng trắng trong nội dung scoped, bao gồm cả dòng mới đầu/cuối và thụt lề:

```txt
{{ # setvar code }}
    function hello() {
        return "world";
    }
{{ /setvar }}
```

Nếu không có `#`, nội dung sẽ bị cắt tỉa và de-dented. Với `#`, toàn bộ khoảng trắng được giữ nguyên chính xác như đã viết—bao gồm cả dòng mới sau thẻ mở và trước thẻ đóng.

## Bình luận (Comments)

Sử dụng macro comment để thêm ghi chú sẽ không xuất hiện trong đầu ra:

```txt
{{// This is a comment and will be removed}}
```

Đối với bình luận nhiều dòng, sử dụng cú pháp scoped:

```txt
{{ // }}
  This entire block is a comment.
  It can span multiple lines.
{{ /// }}
```

## Escaping Macros

Để hiển thị dấu ngoặc nhọn theo nghĩa đen mà không bị phân giải như macro, hãy escape chúng bằng dấu gạch chéo ngược:

```txt
\{\{notAMacro\}\}
```

Điều này xuất ra `{{notAMacro}}` dưới dạng văn bản thuần túy.

## Variable Shorthands

Variable shorthands cung cấp một cú pháp ngắn gọn cho các thao tác biến thông thường. Sử dụng `.` cho biến cục bộ và `$` cho biến toàn cục.

### Tiền tố Variable Shorthands

| Tiền tố | Tên            | Mô tả                                                     |
| ------ | --------------- | --------------------------------------------------------- |
| `.`    | Biến Cục bộ  | Shorthand cho các thao tác biến cục bộ. Ví dụ: `{{.myvar}}`  |
| `$`    | Biến Toàn cục | Shorthand cho các thao tác biến toàn cục. Ví dụ: `{{$myvar}}` |

Các toán tử tiền tố này phải được đặt **ngay trước** tên biến, sau bất kỳ [Macro Flags](#macro-flags) nào xuất hiện tùy chọn. Chúng không được coi là macro flag, mà là thêm các chỉ báo rằng một variable shorthand đang được chèn vào, thay vì một macro theo tên. Các toán tử tiền tố không phải là một phần của chính tên biến, mà là các bộ điều chỉnh thay đổi cách biến được truy cập.

### Tên Biến

Tên biến tuân theo các quy tắc giống như định danh macro: bắt đầu bằng một chữ cái, theo sau là các chữ cái, chữ số, dấu gạch dưới, hoặc dấu gạch ngang. Ký tự cuối cùng không được phép là dấu gạch dưới hoặc dấu gạch ngang.

```txt
{{.my-var}}       // Hợp lệ
{{.my_var}}       // Hợp lệ
{{.myVar123}}     // Hợp lệ
```

Nếu biến của bạn có một định danh không khớp với các quy tắc tiêu chuẩn, bạn phải sử dụng cú pháp macro biến đầy đủ (ví dụ: `{{getvar::my§var----}}`), hoặc đổi tên/di chuyển giá trị biến của bạn.

### Macros Lồng nhau trong Giá trị

Giá trị biến có thể chứa các macro lồng nhau:

```txt
{{.greeting = Hello, {{user}}!}}
```

Phân giải thành một biến lưu `Hello, User!` bên trong. (Nếu `{{user}}` được đặt tên là "User")

### Xử lý Khoảng trắng

Khoảng trắng xung quanh toán tử được cho phép:

```txt
{{ .myvar = spaced value }}
{{ .counter ++ }}
```

### Toán tử Variable Shorthand

Các toán tử sau có thể được sử dụng với variable shorthands. Mỗi toán tử tuân theo mẫu `{{.varName operator value}}` hoặc `{{$varName operator value}}`.

| Toán tử | Tên                      | Ví dụ                | Mô tả                                            |
| -------- | ------------------------- | ---------------------- | ------------------------------------------------------ |
| *(không có)* | [Get](#get-variable)      | `{{.myvar}}`           | Trả về giá trị biến                             |
| `=`      | [Set](#set-variable)      | `{{.myvar = value}}`   | Đặt biến thành một giá trị, không trả về gì         |
| `++`     | [Increment](#increment)   | `{{.counter++}}`       | Tăng thêm 1, trả về giá trị mới                     |
| `--`     | [Decrement](#decrement)   | `{{.counter--}}`       | Giảm đi 1, trả về giá trị mới                     |
| `+=`     | [Add](#add)               | `{{.score += 10}}`     | Thêm vào biến (số hoặc nối chuỗi), không trả về gì |
| `-=`     | [Subtract](#subtract)     | `{{.health -= 5}}`     | Trừ khỏi biến (chỉ dạng số), không trả về gì |
| `||`   | [Logical Or](#logical-or) | `{{.name || Guest}}` | Trả về giá trị dự phòng nếu biến là falsy                  |
| `??`     | [Nullish Coalescing](#nullish-coalescing) | `{{.name ?? Guest}}` | Chỉ trả về giá trị dự phòng nếu biến không được định nghĩa |
| `||=`  | [Logical Or Assign](#logical-or-assign) | `{{.name ||= Guest}}` | Đặt giá trị nếu biến là falsy, trả về giá trị mới |
| `??=`    | [Nullish Coalescing Assign](#nullish-coalescing-assign) | `{{.name ??= Guest}}` | Chỉ đặt giá trị nếu biến không được định nghĩa, trả về giá trị mới |
| `==`     | [Equals](#equals)         | `{{.status == active}}`| So sánh giá trị, trả về `"true"` hoặc `"false"`         |
| `!=`     | [Not Equals](#not-equals) | `{{.status != active}}`| So sánh giá trị, trả về `"true"` nếu không bằng nhau         |
| `>`      | [Greater Than](#greater-than) | `{{.score > 50}}` | Trả về `"true"` nếu biến lớn hơn giá trị     |
| `>=`     | [Greater Than or Equal](#greater-than-or-equal) | `{{.level >= 10}}` | Trả về `"true"` nếu biến lớn hơn hoặc bằng giá trị |
| `<`      | [Less Than](#less-than)   | `{{.health < 20}}`     | Trả về `"true"` nếu biến nhỏ hơn giá trị        |
| `<=`     | [Less Than or Equal](#less-than-or-equal) | `{{.health <= 0}}` | Trả về `"true"` nếu biến nhỏ hơn hoặc bằng giá trị |

#### Get Variable

Lấy giá trị biến với một tiền tố đơn giản:

```txt
{{.myvar}}       // Lấy biến cục bộ "myvar"
{{$myvar}}       // Lấy biến toàn cục "myvar"
```

Tương đương với `{{getvar::myvar}}` và `{{getglobalvar::myvar}}`.

#### Set Variable

Sử dụng toán tử `=` để đặt giá trị biến:

```txt
{{ .myvar = Hello World }}     // Đặt biến cục bộ
{{ $myvar = Some value }}      // Đặt biến toàn cục
```

Tương đương với `{{setvar::myvar::Hello World}}` và `{{setglobalvar::myvar::Hello World}}`. Trả về một chuỗi rỗng.

#### Increment

Sử dụng `++` để tăng một biến số lên 1:

```txt
{{.counter++}}    // Tăng biến cục bộ, trả về giá trị mới
{{$counter++}}    // Tăng biến toàn cục, trả về giá trị mới
```

Tương đương với `{{incvar counter}}` và `{{incglobalvar counter}}`. Trả về giá trị mới sau khi tăng.

#### Decrement

Sử dụng `--` để giảm một biến số đi 1:

```txt
{{.counter--}}    // Giảm biến cục bộ, trả về giá trị mới
{{$counter--}}    // Giảm biến toàn cục, trả về giá trị mới
```

Tương đương với `{{decvar counter}}` và `{{decglobalvar counter}}`. Trả về giá trị mới sau khi giảm.

#### Add

Sử dụng `+=` để thêm một giá trị số vào một biến:

```txt
{{.score += 10}}     // Thêm 10 vào biến cục bộ
{{$total += 5}}      // Thêm 5 vào biến toàn cục
```

Tương đương với `{{addvar::score::10}}` và `{{addglobalvar::total::5}}`. Trả về một chuỗi rỗng.

Toán tử add cũng hỗ trợ nối chuỗi vào một biến chuỗi hiện có, nếu không có bên nào trong số chúng là số:

```txt
{{.myvar += {{noop}} | Second block}}   // Phân giải thành "Content | Second block" khi biến trước đó là "Content".
                                        // Sử dụng `{{noop}}` để có thể thêm khoảng trắng, mà nếu không sẽ tự động bị cắt tỉa.
```

#### Subtract

Sử dụng `-=` để trừ một giá trị số khỏi một biến:

```txt
{{.health -= 10}}    // Trừ 10 khỏi biến cục bộ
{{$points -= 5}}     // Trừ 5 khỏi biến toàn cục
```

Tương đương với `{{addvar::score::10}}` và `{{addglobalvar::total::5}}`, nhưng với một số âm/đảo dấu. Trả về một chuỗi rỗng.
Nếu giá trị không phải là một số hợp lệ, một cảnh báo sẽ được ghi lại và biến không thay đổi.

#### Logical Or

Sử dụng `||` để cung cấp một giá trị dự phòng khi biến là falsy (chuỗi rỗng, `0`, `false`):

```txt
{{.name || Anonymous}}     // Trả về "Anonymous" nếu .name rỗng hoặc falsy
{{$setting || default}}    // Trả về "default" nếu $setting là falsy
```

Trả về giá trị biến nếu truthy, ngược lại trả về giá trị dự phòng. Giá trị dự phòng **chỉ được đánh giá khi cần** (lazy evaluation).

#### Nullish Coalescing

Sử dụng `??` để cung cấp một giá trị dự phòng chỉ khi biến không tồn tại:

```txt
{{.name ?? Guest}}         // Chỉ trả về "Guest" nếu .name không được định nghĩa
{{$config ?? default}}     // Chỉ trả về "default" nếu $config không tồn tại
```

Không giống như `||`, cái này trả về giá trị biến ngay cả khi nó falsy (chuỗi rỗng, `0`, `false`) — miễn là biến tồn tại. Giá trị dự phòng **chỉ được đánh giá khi cần** (lazy evaluation).

#### Logical Or Assign

Sử dụng `||=` để đặt một biến thành một giá trị chỉ khi nó hiện đang falsy:

```txt
{{.name ||= Anonymous}}    // Đặt và trả về "Anonymous" nếu .name là falsy
{{$count ||= 0}}           // Đặt và trả về "0" nếu $count là falsy
```

Nếu biến đã truthy, trả về giá trị hiện tại mà không sửa đổi. Trả về giá trị cuối cùng (giá trị hiện có hoặc mới được đặt).

#### Nullish Coalescing Assign

Sử dụng `??=` để đặt một biến thành một giá trị chỉ khi nó không tồn tại:

```txt
{{.name ??= Guest}}        // Chỉ đặt và trả về "Guest" nếu .name không được định nghĩa
{{$config ??= default}}    // Chỉ đặt và trả về "default" nếu $config không tồn tại
```

Không giống như `||=`, cái này giữ lại các giá trị falsy (chuỗi rỗng, `0`, `false`) nếu biến đã tồn tại. Trả về giá trị cuối cùng (giá trị hiện có hoặc mới được đặt).

#### Equals

Sử dụng `==` để so sánh giá trị biến với một giá trị khác:

```txt
{{.status == active}}      // Trả về "true" nếu .status bằng "active", ngược lại "false"
{{$mode == dark}}          // Trả về "true" nếu $mode bằng "dark", ngược lại "false"
```

Thực hiện so sánh chuỗi và trả về chuỗi nghĩa đen `"true"` hoặc `"false"`.
Coi các biến không tồn tại, biến null và biến rỗng là như nhau.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{.status == active}} }}Active mode{{/if}}
```

#### Not Equals

Sử dụng `!=` để so sánh giá trị biến với một giá trị khác để kiểm tra sự không bằng nhau:

```txt
{{.status != inactive}}    // Trả về "true" nếu .status KHÔNG PHẢI là "inactive", ngược lại "false"
{{$mode != light}}         // Trả về "true" nếu $mode KHÔNG PHẢI là "light", ngược lại "false"
```

Thực hiện so sánh chuỗi và trả về `"true"` nếu các giá trị khác nhau, `"false"` nếu chúng bằng nhau.
Coi các biến không tồn tại, biến null và biến rỗng là như nhau.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{.status != disabled}} }}Feature enabled{{/if}}
```

#### Greater Than

Sử dụng `>` để kiểm tra xem giá trị số của một biến có lớn hơn một giá trị khác không:

```txt
{{.score > 50}}        // Trả về "true" nếu .score lớn hơn 50
{{$level > 5}}         // Trả về "true" nếu $level lớn hơn 5
```

Thực hiện so sánh số và trả về chuỗi nghĩa đen `"true"` hoặc `"false"`.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{.score > 100}} }}High score!{{/if}}
```

#### Greater Than or Equal

Sử dụng `>=` để kiểm tra xem giá trị số của một biến có lớn hơn hoặc bằng một giá trị khác không:

```txt
{{.level >= 10}}       // Trả về "true" nếu .level ít nhất là 10
{{$points >= 100}}     // Trả về "true" nếu $points là 100 hoặc nhiều hơn
```

Thực hiện so sánh số và trả về chuỗi nghĩa đen `"true"` hoặc `"false"`.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{$level >= 10}} }}Unlocked advanced features{{/if}}
```

#### Less Than

Sử dụng `<` để kiểm tra xem giá trị số của một biến có nhỏ hơn một giá trị khác không:

```txt
{{.health < 20}}       // Trả về "true" nếu .health dưới 20
{{$timer < 0}}         // Trả về "true" nếu $timer là số âm
```

Thực hiện so sánh số và trả về chuỗi nghĩa đen `"true"` hoặc `"false"`.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{.health < 20}} }}Low health warning!{{/if}}
```

#### Less Than or Equal

Sử dụng `<=` để kiểm tra xem giá trị số của một biến có nhỏ hơn hoặc bằng một giá trị khác không:

```txt
{{.health <= 0}}       // Trả về "true" nếu .health là 0 hoặc thấp hơn
{{$attempts <= 3}}     // Trả về "true" nếu $attempts là 3 hoặc ít hơn
```

Thực hiện so sánh số và trả về chuỗi nghĩa đen `"true"` hoặc `"false"`.

Hữu ích trong các điều kiện `{{if}}`:

```txt
{{if {{.health <= 0}} }}Game over{{/if}}
```

## Cú pháp Legacy

Để tương thích ngược, các dấu ngoặc nhọn (angle bracket) vẫn được hỗ trợ:

| Legacy | Macro tương đương |
|--------|------------------|
| `<USER>` | `{{user}}` |
| `<BOT>` | `{{char}}` |
| `<CHAR>` | `{{char}}` |
| `<GROUP>` | `{{group}}` |
| `<CHARIFNOTGROUP>` | `{{charIfNotGroup}}` |

Các dấu này được tự động chuyển đổi thành macro tương đương của chúng trong quá trình xử lý.

> **Lưu ý:** Cú pháp legacy không được khuyến nghị. Sử dụng cú pháp `{{macro}}` tương đương thay thế cho nội dung mới.

## Các Macro Phổ biến theo Danh mục

!!!tip
Sử dụng `/? macros` để có danh sách đầy đủ các macro có sẵn và mô tả chi tiết của chúng.
!!!

### Tên & Người tham gia

| Macro | Mô tả |
|-------|-------------|
| `{{user}}` | Tên người dùng/persona hiện tại |
| `{{char}}` | Tên nhân vật hiện tại |
| `{{group}}` | Danh sách tên thành viên group được phân tách bằng dấu phẩy (bao gồm cả bị tắt tiếng) hoặc tên nhân vật trong các cuộc chat đơn |
| `{{groupNotMuted}}` | Danh sách tên thành viên group được phân tách bằng dấu phẩy, loại trừ các thành viên bị tắt tiếng |
| `{{charIfNotGroup}}` | Tên nhân vật (rỗng trong group) |
| `{{notChar}}` | Danh sách được phân tách bằng dấu phẩy của tất cả người tham gia ngoại trừ người nói hiện tại |

### Character Card & Trường Persona

| Macro | Mô tả |
|-------|-------------|
| `{{description}}` | Mô tả nhân vật |
| `{{personality}}` | Tính cách nhân vật |
| `{{scenario}}` | Scenario nhân vật |
| `{{persona}}` | Mô tả persona người dùng |
| `{{charPrompt}}` | Ghi đè Main Prompt của nhân vật |
| `{{charInstruction}}` | Ghi đè Post-History Instructions của nhân vật |
| `{{charDepthPrompt}}` | @ Depth Note của nhân vật |
| `{{charCreatorNotes}}` | Ghi chú của người tạo từ character card |
| `{{charVersion}}` | Số phiên bản của nhân vật |
| `{{mesExamples}}` | Ví dụ đối thoại của nhân vật, được định dạng cho instruct mode |
| `{{mesExamplesRaw}}` | Ví dụ đối thoại chưa định dạng từ character card |
| `{{charFirstMessage}}` | Tin nhắn đầu tiên (lời chào) của nhân vật. Chấp nhận một chỉ số tùy chọn cho các lời chào thay thế, ví dụ `{{charFirstMessage::1}}` |
| `{{original}}` | Nội dung tin nhắn gốc để thay thế trong các ghi đè prompt nhân vật |

### Lịch sử Chat & Tin nhắn

| Macro | Mô tả |
|-------|-------------|
| `{{lastMessage}}` | Tin nhắn cuối cùng trong chat |
| `{{lastMessageId}}` | Chỉ số của tin nhắn cuối cùng trong chat |
| `{{lastUserMessage}}` | Tin nhắn người dùng cuối cùng trong chat |
| `{{lastCharMessage}}` | Tin nhắn nhân vật/bot cuối cùng trong chat |
| `{{firstIncludedMessageId}}` | Chỉ số của tin nhắn đầu tiên được bao gồm trong context hiện tại |
| `{{firstDisplayedMessageId}}` | Chỉ số của tin nhắn đầu tiên được hiển thị trong chat |
| `{{lastSwipeId}}` | Chỉ số dựa trên 1 của swipe cuối cùng cho tin nhắn cuối cùng |
| `{{currentSwipeId}}` | Chỉ số dựa trên 1 của swipe hiện tại |
| `{{allChatRange}}` | Cung cấp phạm vi của toàn bộ chat (ví dụ `0-{{lastMessageId}}`), hữu ích cho các lệnh chấp nhận phạm vi tin nhắn |
| `{{summary}}` | Bản tóm tắt chat mới nhất từ tiện ích mở rộng "Summarize" (khi có sẵn) |

### Thời gian & Ngày tháng

| Macro | Mô tả |
|-------|-------------|
| `{{time}}` | Thời gian cục bộ hiện tại |
| `{{time::UTC±(offset)}}` | Thời gian với offset UTC |
| `{{date}}` | Ngày cục bộ hiện tại ở định dạng ngắn |
| `{{weekday}}` | Ngày trong tuần hiện tại |
| `{{isotime}}` | Thời gian hiện tại theo định dạng HH:mm |
| `{{isodate}}` | Ngày hiện tại theo định dạng YYYY-MM-DD |
| `{{datetimeformat::format}}` | Ngày/giờ được định dạng tùy chỉnh (ví dụ: `YYYY-MM-DD HH:mm:ss`) |
| `{{idleDuration}}` | Khoảng thời gian dễ đọc kể từ tin nhắn người dùng cuối cùng |
| `{{timeDiff::left::right}}` | Chênh lệch dễ đọc giữa hai thời điểm |

### Biến (Variables)

| Macro | Mô tả |
|-------|-------------|
| `{{getvar::name}}` | Lấy giá trị biến cục bộ |
| `{{setvar::name::value}}` | Đặt biến cục bộ |
| `{{addvar::name::value}}` | Thêm giá trị vào biến cục bộ (số hoặc nối chuỗi) |
| `{{incvar::name}}` | Tăng biến cục bộ lên 1 và trả về giá trị mới |
| `{{decvar::name}}` | Giảm biến cục bộ đi 1 và trả về giá trị mới |
| `{{hasvar::name}}` | Kiểm tra xem một biến cục bộ có tồn tại không (trả về "true" hoặc "false") |
| `{{deletevar::name}}` | Xóa một biến cục bộ |
| `{{getglobalvar::name}}` | Lấy giá trị biến toàn cục |
| `{{setglobalvar::name::value}}` | Đặt biến toàn cục |
| `{{addglobalvar::name::value}}` | Thêm giá trị vào biến toàn cục (số hoặc nối chuỗi) |
| `{{incglobalvar::name}}` | Tăng biến toàn cục lên 1 và trả về giá trị mới |
| `{{decglobalvar::name}}` | Giảm biến toàn cục đi 1 và trả về giá trị mới |
| `{{hasglobalvar::name}}` | Kiểm tra xem một biến toàn cục có tồn tại không (trả về "true" hoặc "false") |
| `{{deleteglobalvar::name}}` | Xóa một biến toàn cục |

### Ngẫu nhiên hóa (Randomization)

| Macro | Mô tả |
|-------|-------------|
| `{{random::a::b::c}}` | Lựa chọn ngẫu nhiên (đổ lại mỗi lần) |
| `{{pick::a::b::c}}` | Lựa chọn ngẫu nhiên ổn định (nhất quán theo chat và vị trí). Có thể roll lại bằng lệnh `/reroll-pick` |
| `{{roll::1d20}}` | Đổ xúc xắc sử dụng cú pháp droll |

### Trạng thái Runtime

| Macro | Mô tả |
|-------|-------------|
| `{{maxPrompt}}` | Kích thước context prompt tối đa (prompt tokens = context tokens - response tokens) |
| `{{maxContextTokens}}` | Số lượng token context tối đa cho cài đặt tạo hiện tại |
| `{{maxResponseTokens}}` | Số lượng token phản hồi tối đa cho cài đặt tạo hiện tại |
| `{{model}}` | Tên model cho API hiện đang được chọn |
| `{{isMobile}}` | "true" nếu đang chạy trong môi trường di động, ngược lại "false" |
| `{{lastGenerationType}}` | Loại yêu cầu tạo được xếp hàng cuối cùng (ví dụ: "normal", "impersonate", "regenerate", "quiet", "swipe", "continue") |
| `{{hasExtension::name}}` | Kiểm tra xem một tiện ích mở rộng có đang hoạt động không (trả về "true" hoặc "false"). Khớp theo tên tiện ích mở rộng, không phân biệt chữ hoa chữ thường |

### Mẫu Prompt (Prompt Templates)

| Macro | Mô tả |
|-------|-------------|
| `{{systemPrompt}}` | Văn bản system prompt đang hoạt động (có thể được ghi đè bởi nhân vật) |
| `{{defaultSystemPrompt}}` | System prompt mặc định |
| `{{authorsNote}}` | Nội dung của Author's Note |
| `{{charAuthorsNote}}` | Nội dung của Character Author's Note |
| `{{defaultAuthorsNote}}` | Nội dung của Default Author's Note |
| `{{instructStoryStringPrefix}}` | Tiền tố story string của Instruct |
| `{{instructStoryStringSuffix}}` | Hậu tố story string của Instruct |
| `{{instructUserPrefix}}` | Chuỗi tiền tố đầu vào/người dùng của Instruct |
| `{{instructUserSuffix}}` | Chuỗi hậu tố đầu vào/người dùng của Instruct |
| `{{instructAssistantPrefix}}` | Chuỗi tiền tố đầu ra/assistant của Instruct |
| `{{instructAssistantSuffix}}` | Chuỗi hậu tố đầu ra/assistant của Instruct |
| `{{instructSeparator}}` | Chuỗi phân tách của Instruct |
| `{{instructSystemPrefix}}` | Chuỗi tiền tố system của Instruct |
| `{{instructSystemSuffix}}` | Chuỗi hậu tố system của Instruct |
| `{{instructFirstAssistantPrefix}}` | Chuỗi tiền tố assistant/đầu ra đầu tiên của Instruct |
| `{{instructLastAssistantPrefix}}` | Chuỗi tiền tố assistant/đầu ra cuối cùng của Instruct |
| `{{instructFirstUserPrefix}}` | Chuỗi tiền tố user/đầu vào đầu tiên của Instruct |
| `{{instructLastUserPrefix}}` | Chuỗi tiền tố user/đầu vào cuối cùng của Instruct |
| `{{instructStop}}` | Chuỗi dừng của Instruct |
| `{{instructUserFiller}}` | Văn bản điền căn chỉnh người dùng của Instruct |
| `{{instructSystemInstructionPrefix}}` | Chuỗi tiền tố hướng dẫn system của Instruct |
| `{{chatSeparator}}` | Dấu phân tách giữa các khối chat mẫu trong text completion prompt |
| `{{chatStart}}` | Điểm đánh dấu bắt đầu chat trong text completion prompt |
| `{{reasoningPrefix}}` | Chuỗi tiền tố được dùng trước các reasoning block |
| `{{reasoningSuffix}}` | Chuỗi hậu tố được dùng sau các reasoning block |
| `{{reasoningSeparator}}` | Dấu phân tách giữa nội dung và phản hồi |
| `{{charPrefix}}` | Tiền tố prompt Image Generation tích cực (positive) của nhân vật |
| `{{charNegativePrefix}}` | Tiền tố prompt Image Generation tiêu cực (negative) của nhân vật |

### Tiện ích (Utility)

| Macro | Mô tả |
|-------|-------------|
| `{{newline}}` | Chèn ký tự dòng mới |
| `{{newline::count}}` | Chèn nhiều dòng mới |
| `{{space}}` | Chèn ký tự khoảng trắng |
| `{{space::count}}` | Chèn nhiều khoảng trắng |
| `{{noop}}` | Không làm gì, tạo ra chuỗi rỗng |
| `{{trim}}` | Loại bỏ các dòng mới xung quanh |
| `{{reverse::text}}` | Đảo ngược một chuỗi |
| `{{input}}` | Nội dung ô nhập chat hiện tại |
| `{{banned::word}}` | Cấm một từ cho backend Text Completion |
| `{{outlet::key}}` | Trả về world info outlet prompt cho một outlet key đã cho |
