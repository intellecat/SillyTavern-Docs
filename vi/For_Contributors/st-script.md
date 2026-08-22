---
order: 0
icon: file-symlink-file
route: /vi/usage/st-script/
templating: false
---

# Tài liệu tham khảo ngôn ngữ STscript

## STscript là gì?

Đây là một ngôn ngữ lập trình đơn giản nhưng mạnh mẽ có thể được sử dụng để mở rộng chức năng của SillyTavern mà không cần lập trình nghiêm túc, cho phép bạn:

- Tạo mini-game hoặc thử thách tốc độ
- Xây dựng thông tin chi tiết trò chuyện được hỗ trợ bởi AI
- Thả sức sáng tạo của bạn và chia sẻ với người khác

STscript được xây dựng bằng cách sử dụng công cụ slash command, tận dụng command batching, data piping, macro và biến.
Các khái niệm này sẽ được mô tả trong tài liệu sau.

### Biện pháp phòng ngừa an ninh

Với sức mạnh lớn đi kèm trách nhiệm lớn. Hãy cẩn thận và luôn kiểm tra các script trước khi thực thi chúng.

## Hello, World!

Để chạy script đầu tiên của bạn, hãy mở bất kỳ trò chuyện SillyTavern nào và nhập nội dung sau vào thanh nhập trò chuyện:

```stscript
/pass Hello, World! | /echo
```

| ![Hello World](/static/scripts/hello-world.png) |
|-------------------------------------------------|

Bạn sẽ thấy tin nhắn trong toast ở đầu màn hình. Bây giờ hãy phân tích nó từng phần một.

Một script là một lô các lệnh, mỗi lệnh bắt đầu bằng dấu gạch chéo, có hoặc không có các đối số có tên và không có tên, và được kết thúc bằng ký tự phân tách lệnh: `|`.

Các lệnh được thực thi tuần tự, lần lượt và truyền dữ liệu giữa chúng.

1. Lệnh `/pass` chấp nhận một giá trị hằng số "Hello, World!" làm đối số không có tên và ghi nó vào pipe.
2. Lệnh `/echo` nhận giá trị thông qua pipe từ lệnh trước đó và hiển thị nó dưới dạng thông báo toast.

!!!tip
**Gợi ý:** Để xem danh sách tất cả các lệnh có sẵn, hãy nhập `/help slash` vào trò chuyện.
!!!

Vì các đối số không có tên hằng số và pipe có thể hoán đổi cho nhau, chúng ta có thể viết lại script này đơn giản như:

```stscript
/echo Hello, World!
```

## Đầu vào người dùng

Bây giờ hãy thêm một chút tương tác vào script. Chúng ta sẽ chấp nhận giá trị đầu vào từ người dùng và hiển thị nó trong thông báo.

```stscript
/input Enter your name |
/echo Hello, my name is {{pipe}}
```

1. Lệnh `/input` được sử dụng để hiển thị hộp nhập với lời nhắc được chỉ định trong đối số không có tên và sau đó ghi đầu ra vào pipe.
2. Vì `/echo` đã có một đối số không có tên đặt template cho đầu ra, chúng ta sử dụng macro `{{pipe}}` để chỉ định một vị trí mà giá trị pipe sẽ được hiển thị.

| ![Slim Shady Input](/static/scripts/slim-input.png) | ![Slim Shady Output](/static/scripts/slim-output.png) |
|-----------------------------------------------------|-------------------------------------------------------|

### Các lệnh nhập/xuất khác

- `/popup (text)` — hiển thị một popup chặn, hỗ trợ định dạng HTML nhẹ, ví dụ: `/popup <font color=red>I'm red!</font>`.
- `/setinput (text)` — thay thế nội dung của thanh nhập người dùng bằng văn bản được cung cấp.
- `/speak voice="name" (text)` — kể văn bản bằng công cụ TTS đã chọn và tên nhân vật từ voice map, ví dụ: `/speak name="Donald Duck" Quack!`.
- `/buttons labels=["a","b"] (text)` — hiển thị một popup chặn với văn bản và nhãn nút được chỉ định. `labels` phải là một mảng JSON-serialized các chuỗi hoặc tên biến chứa mảng như vậy. Trả về nhãn nút được nhấp vào pipe hoặc chuỗi rỗng nếu bị hủy. Văn bản hỗ trợ định dạng HTML nhẹ.
- `/beep` — phát âm thanh thông báo tin nhắn.

#### Các đối số cho `/popup` và `/input`

`/popup` và `/input` hỗ trợ các đối số có tên bổ sung sau:
- `large=on/off` - tăng kích thước dọc của popup. Mặc định: `off`.
- `wide=on/off` - tăng kích thước ngang của popup. Mặc định: `off`.
- `okButton=string` - thêm khả năng tùy chỉnh văn bản trên nút "Ok". Mặc định: `Ok`.
- `rows=number` - (chỉ cho `/input`) tăng kích thước của điều khiển nhập. Mặc định: 1.
- `placeholder=string` - đặt văn bản placeholder trong trường nhập.
- `tooltip=string` - đặt một tooltip được hiển thị khi di chuột qua.
- `icon=string` - đặt một class icon Font Awesome cho popup.

Ví dụ:
```stscript
/popup large=on wide=on okButton="Accept" Please accept our terms and conditions....
```

#### Các đối số cho `/echo`

`/echo` hỗ trợ các giá trị sau cho đối số `severity` bổ sung đặt kiểu của tin nhắn hiển thị.
  - `warning`
  - `error`
  - `info` (mặc định)
  - `success`

Ví dụ:

```stscript
/echo severity=error Something really bad happened.
```

## Biến

Biến được sử dụng để lưu trữ và thao tác dữ liệu trong script, sử dụng lệnh hoặc macro. Các biến có thể là một trong các loại sau:

- Biến cục bộ — được lưu vào metadata của cuộc trò chuyện hiện tại và duy nhất cho nó.
- Biến toàn cục — được lưu vào settings.json và tồn tại ở mọi nơi trong ứng dụng.

1. `/getvar name` hoặc `{{getvar::name}}` — lấy giá trị của biến cục bộ.
2. `/setvar key=name value` hoặc `{{setvar::name::value}}` — đặt giá trị của biến cục bộ.
3. `/addvar key=name increment` hoặc `{{addvar::name::increment}}` — thêm `increment` vào giá trị của biến cục bộ.
4. `/incvar name` hoặc `{{incvar::name}}` — tăng giá trị của biến cục bộ lên 1.
5. `/decvar name` hoặc `{{decvar::name}}` — giảm giá trị của biến cục bộ xuống 1.
6. `/getglobalvar name` hoặc `{{getglobalvar::name}}` — lấy giá trị của biến toàn cục.
7. `/setglobalvar key=name` hoặc `{{setglobalvar::name::value}}` — đặt giá trị của biến toàn cục.
8. `/addglobalvar key=name` hoặc `{{addglobalvar::name:increment}}` — thêm `increment` vào giá trị của biến toàn cục.
9. `/incglobalvar name` hoặc `{{incglobalvar::name}}` — tăng giá trị của biến toàn cục lên 1.
10. `/decglobalvar name` hoặc `{{decglobalvar::name}}` — giảm giá trị của biến toàn cục xuống 1.
11. `/flushvar name` — xóa giá trị của biến cục bộ.
12. `/flushglobalvar name` — xóa giá trị của biến toàn cục.

- Giá trị mặc định của các biến chưa được định nghĩa trước đó là một chuỗi rỗng hoặc số không nếu nó được sử dụng lần đầu trong lệnh `/addvar`, `/incvar`, `/decvar`.
- Increment trong lệnh `/addvar` thực hiện phép cộng hoặc trừ của giá trị nếu cả increment và giá trị biến có thể được chuyển đổi thành số, hoặc ngược lại thực hiện nối chuỗi.
- Nếu một đối số lệnh chấp nhận tên biến và cả biến cục bộ và toàn cục tồn tại với cùng tên, thì *biến cục bộ* được ưu tiên.
- Tất cả *slash command* để thao tác biến ghi giá trị kết quả vào pipe cho lệnh tiếp theo sử dụng.
- Đối với *macro*, chỉ macro loại "get", "inc" và "dec" trả về giá trị, "add" và "set" được thay thế bằng chuỗi rỗng.

Bây giờ, hãy xem xét ví dụ sau:

```stscript
/input What do you want to generate? |
/setvar key=SDinput |
/echo Requesting an image of {{getvar::SDinput}} |
/getvar SDinput |
/imagine
```

1. Giá trị của đầu vào người dùng được lưu trong biến cục bộ có tên `SDinput`.
2. Macro `getvar` được sử dụng để hiển thị giá trị trong lệnh `/echo`.
3. Lệnh `getvar` được sử dụng để truy xuất giá trị của biến và truyền nó qua pipe.
4. Giá trị được truyền đến lệnh `/imagine` (được cung cấp bởi plugin Image Generation) để được sử dụng làm lời nhắc đầu vào của nó.

Vì các biến được lưu và không bị xóa giữa các lần thực thi script, bạn có thể tham chiếu biến trong các script khác và qua macro, và nó sẽ giải quyết thành cùng giá trị như trong quá trình thực thi script ví dụ. Để đảm bảo rằng giá trị sẽ bị loại bỏ, hãy thêm lệnh `/flushvar` vào script.

### Mảng và đối tượng

Giá trị biến có thể chứa các mảng JSON-serialized hoặc các cặp khóa-giá trị (đối tượng).

Ví dụ:
- Mảng: `["apple","banana","orange"]`
- Đối tượng: `{"fruits":["apple","banana","orange"]}`

Các sửa đổi sau có thể được áp dụng cho các lệnh để làm việc với các biến này:

- Lệnh `/len` lấy số lượng mục trong mảng.
- Đối số có tên `index=number/string` có thể được thêm vào `/getvar` hoặc `/setvar` và các đối tác toàn cục của chúng để lấy hoặc đặt các giá trị phụ bằng chỉ số dựa trên 0 cho mảng hoặc khóa chuỗi cho đối tượng.
  - Nếu chỉ số số được sử dụng trên một biến không tồn tại, biến sẽ được tạo dưới dạng một mảng rỗng `[]`.
  - Nếu chỉ số chuỗi được sử dụng trên một biến không tồn tại, biến sẽ được tạo dưới dạng một đối tượng rỗng `{}`.
- Các lệnh `/addvar` và `/addglobalvar` hỗ trợ đẩy một giá trị mới vào các biến kiểu mảng.

## Kiểm soát luồng - điều kiện

Bạn có thể sử dụng lệnh `/if` để tạo các biểu thức điều kiện phân nhánh thực thi dựa trên các quy tắc đã định nghĩa.

```stscript
/if left=valueA right=valueB rule=comparison else={: /echo (command on false) :} {: /echo (command on true) :}
```

Lưu ý rằng

```stscript
/if left=valueA right=valueB rule=comparison else="(command on false)" "(command on true)"
```

cú pháp cũng được hỗ trợ, tuy nhiên `{: closures :}` sẽ giúp bạn viết các script sạch hơn.

Hãy xem xét ví dụ sau:

```stscript
/input What's your favorite drink? |
/if left={{pipe}} right="black tea" rule=eq else={: /echo You shall not pass | /abort :} {: /echo Welcome to the club, {{user}} :}
```

Script này đánh giá đầu vào của người dùng so với một giá trị yêu cầu và hiển thị các tin nhắn khác nhau, tùy thuộc vào giá trị đầu vào.

### Các đối số cho `/if`

1. `left` là toán hạng đầu tiên. Hãy gọi nó là A.
2. `right` là toán hạng thứ hai. Hãy gọi nó là B.
3. `rule` là phép toán được áp dụng cho các toán hạng.
4. `else` là chuỗi tùy chọn của các subcommand được thực thi nếu kết quả của phép so sánh boolean là false.
5. Đối số không có tên là subcommand được thực thi nếu kết quả của phép so sánh boolean là true.

Các giá trị toán hạng được đánh giá theo thứ tự sau:

1. Literal số
2. Tên biến cục bộ
3. Tên biến toàn cục
4. Literal chuỗi

Các giá trị chuỗi của các đối số có tên có thể được thoát bằng dấu ngoặc kép để cho phép các chuỗi nhiều từ. Dấu ngoặc kép sau đó bị loại bỏ.

### Các phép toán Boolean

Các quy tắc được hỗ trợ cho phép so sánh boolean như sau. Một phép toán được áp dụng cho các toán hạng dẫn đến một giá trị true hoặc false.

1. `eq` (bằng) => A = B
2. `neq` (không bằng) => A != B
3. `lt` (nhỏ hơn) => A < B
4. `gt` (lớn hơn) => A > B
5. `lte` (nhỏ hơn hoặc bằng) => A <= B
6. `gte` (lớn hơn hoặc bằng) => A >= B
7. `not` (phủ định đơn) => !A
8. `in` (bao gồm chuỗi con) => A bao gồm B, không phân biệt chữ hoa chữ thường
9. `nin` (không bao gồm chuỗi con) => A không bao gồm B, không phân biệt chữ hoa chữ thường

### Subcommand

Một subcommand là một chuỗi chứa một danh sách các slash command để thực thi.

1. Để sử dụng command batching trong subcommand, ký tự phân tách lệnh phải được thoát (xem bên dưới).
2. Vì các giá trị macro được thực thi khi điều kiện được nhập, không phải khi subcommand được thực thi, một macro có thể được thoát thêm để trễ việc đánh giá của chúng đến thời gian thực thi subcommand.
3. Kết quả của việc thực thi subcommand được piped đến lệnh sau `/if`.
4. Lệnh `/abort` ngắt việc thực thi script khi gặp phải.

Các lệnh `/if` có thể được sử dụng như một toán tử bậc ba.
Ví dụ sau sẽ truyền một chuỗi "true" đến lệnh tiếp theo nếu biến `a` bằng 5, và một chuỗi "false" nếu ngược lại.

```stscript
/if left=a right=5 rule=eq else={: /pass false:} {: /pass true :} |
/echo
```

## Chuỗi thoát

### Macro

Việc thoát macro hoạt động giống như trước đây. Tuy nhiên, với closure, bạn sẽ cần thoát macro ít hơn nhiều so với trước đây. Thoát hai dấu ngoặc nhọn mở, hoặc cả cặp mở và đóng.

```stscript
/echo \{\{char}} |
/echo \{\{char\}\}
```

### Pipe

Pipe không cần được thoát trong closure (khi được sử dụng như phân tách lệnh). Ở mọi nơi bạn muốn sử dụng ký tự pipe theo nghĩa đen thay vì phân tách lệnh, bạn cần thoát nó.

```stscript
/echo title="a\|b" c\|d |
/echo title=a\|b c\|d |
```

Với cờ parser `STRICT_ESCAPING` bạn không cần thoát pipe trong các giá trị được trích dẫn.

```stscript
/parser-flag STRICT_ESCAPING |
/echo title="a|b" c\|d |
/echo title=a\|b c\|d |
```

### Dấu ngoặc kép

Để sử dụng ký tự dấu ngoặc kép theo nghĩa đen bên trong một giá trị được trích dẫn, ký tự phải được thoát.

```stscript
/echo title="a \"b\" c" d "e" f
```

### Khoảng trắng

Để sử dụng khoảng trắng trong giá trị của đối số có tên, bạn phải bao quanh giá trị bằng dấu ngoặc kép hoặc thoát ký tự khoảng trắng.

```stscript
/echo title="a b" c d |
/echo title=a\ b c d
```

### Delimiter Closure

Nếu bạn muốn sử dụng các kết hợp ký tự được sử dụng để đánh dấu đầu hoặc cuối của một closure, bạn phải thoát chuỗi bằng một dấu gạch chéo ngược.

```stscript
/echo \{: |
/echo \:}
```

## Pipe Breaker

```stscript
||
```

Để ngăn đầu ra của lệnh trước tự động được tiêm làm đối số không có tên vào lệnh tiếp theo, đặt pipe kép giữa hai lệnh.

```stscript
/echo we don't want to pass this on ||
/world
```

## Closure

```stscript
{: ... :}
```

Closure (block statement, lambda, anonymous function, bất cứ điều gì bạn muốn gọi chúng) là một chuỗi các lệnh được bao bọc giữa `{:` và `:}`, chỉ được đánh giá khi phần code đó được thực thi.

### Sub-Command

Closure làm cho việc sử dụng sub-command dễ dàng hơn nhiều và loại bỏ nhu cầu thoát pipe và macro.

```stscript
// if without closures |
/if left=1 rule=eq right=1
    else="
        /echo not equal \|
        /return 0
    "
    /echo equal \|
    /return \{\{pipe}}
```

```stscript
// if with closures |
/if left=1 rule=eq right=1
    else={:
        /echo not equal |
        /return 0
    :}
    {:
        /echo equal |
        /return {{pipe}}
    :}
```

### Scope

Closure có scope riêng và hỗ trợ các biến scoped. Các biến scoped được khai báo bằng `/let`, giá trị của chúng được đặt và truy xuất bằng `/var`. Một cách khác để lấy một biến scoped là macro `{{var::}}`.

```stscript
/let x |
/let y 2 |
/var x 1 |
/var y |
/echo x is {{var::x}} and y is {{pipe}}.
```

Trong một closure, bạn có quyền truy cập vào tất cả các biến được khai báo trong cùng closure đó hoặc trong một trong các closure tổ tiên của nó. Bạn không có quyền truy cập vào các biến được khai báo trong các closure con của một closure.
Nếu một biến được khai báo với cùng tên với một biến đã được khai báo trong một trong các closure tổ tiên của closure, bạn không có quyền truy cập vào biến tổ tiên trong closure này và các closure con của nó.

```stscript
/let x this is root x |
/let y this is root y |
/return {:
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /let x this is level-1 x |
    /echo called from level-1: x is "{{var::x}}" and y is "{{var::y}}" |
    /delay 500 |
    /return {:
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /let x this is level-2 x |
        /echo called from level-2: x is "{{var::x}}" and y is "{{var::y}}" |
        /delay 500
    :}()
:}() |
/echo called from root: x is "{{var::x}}" and y is "{{var::y}}"
```

### Named Closure

```stscript
/let x {: ... :} | /:x
```

Closure có thể được gán cho các biến (chỉ các biến scoped) để được gọi tại một thời điểm sau hoặc được sử dụng làm sub-command.

```stscript
/let myClosure {:
    /echo this is my closure
:} |
/:myClosure
```

```stscript
/let myClosure {:
    /echo this is my closure |
    /delay 500
:} |
/times 3 {{var::myClosure}}
```

`/:` cũng có thể được sử dụng để thực thi Quick Reply, vì nó chỉ là một shorthand cho `/run`.

```stscript
/:QrSetName.QrButtonLabel |
/run QrSetName.QrButtonLabel
```

### Closure Argument

Named closure có thể nhận các đối số có tên, giống như slash command. Các đối số có thể có giá trị mặc định.

```stscript
/let myClosure {: a=1 b=
    /echo a is {{var::a}} and b is {{var::b}}
:} |
/:myClosure b=10
```

### Closure và Piped Argument

Giá trị được piped từ một closure cha sẽ không tự động được tiêm vào lệnh đầu tiên của một closure con.
Bạn vẫn có thể tham chiếu rõ ràng giá trị được piped của cha bằng `{{pipe}}`, nhưng nếu bạn để trống đối số không có tên của lệnh đầu tiên bên trong một closure, giá trị sẽ *không* được tự động tiêm.

```stscript
/* This used to attempt to change the model to "foo"
   because the value "foo" from the /echo outside of
   the loop was injected into the /model command
   inside of the loop.
   Now it will simply echo the current model without
   attempting to change it.
*|
/echo foo |
/times 2 {:
	/model |
	/echo |
:} |
```
```stscript
/* You can still recreate the old behavior by
   explicitly using the {{pipe}} macro.
*|
/echo foo |
/times 2 {:
	/model {{pipe}} |
	/echo |
:} |
```

### Immediately Executed Closure

```stscript
{: ... :}()
```

Closure có thể được thực thi ngay lập tức, có nghĩa là chúng sẽ được thay thế bằng giá trị trả về của chúng. Điều này hữu ích ở những nơi không có hỗ trợ rõ ràng cho closure và để rút ngắn một số lệnh mà nếu không sẽ yêu cầu nhiều biến trung gian.

```stscript
// a simple length comparison of two strings without closures |
/len foo |
/var lenOfFoo {{pipe}} |
/len bar |
/var lenOfBar {{pipe}} |
/if left={{var::lenOfFoo}} rule=eq right={{var:lenOfBar}} /echo yay!
```

```stscript
// the same comparison with immediately executed closures |
/if left={:/len foo:}() rule=eq right={:/len bar:}() /echo yay!
```

Ngoài việc chạy các named closure được lưu bên trong các biến scoped, lệnh `/run` cũng có thể được sử dụng để thực thi closure ngay lập tức.

```stscript
/run {:
	/add 1 2 3 4 |
:} |
/echo |
```

## Chú thích

```stscript
// ... | /# ...
```

Chú thích là một lời giải thích hoặc chú thích có thể đọc được trong code script. Chú thích không phá vỡ pipe.

```stscript
// this is a comment |
/echo foo |
/# this is also a comment
```

### Block Comment

Block comment có thể được sử dụng để nhanh chóng comment out nhiều lệnh cùng một lúc. Chúng sẽ không kết thúc trên một pipe.

```stscript
/echo foo |
/*
/echo bar |
/echo foobar |
*|
/echo foo again |
```

## Kiểm soát luồng

### Vòng lặp: `/while` và `/times`

Nếu bạn cần chạy một số lệnh trong một vòng lặp cho đến khi một điều kiện nhất định được đáp ứng, hãy sử dụng lệnh `/while`.

```stscript
/while left=valueA right=valueB rule=operation guard=on "commands"
```

Trên mỗi bước của vòng lặp, nó so sánh giá trị của biến A với giá trị của biến B, và nếu điều kiện tạo ra true, sau đó thực thi bất kỳ slash command hợp lệ nào được đóng trong dấu ngoặc kép, nếu không thoát khỏi vòng lặp. Lệnh này không ghi gì vào pipe đầu ra.

#### Các đối số cho `/while`

**Tập hợp các phép so sánh boolean có sẵn, xử lý biến, giá trị literal và subcommand giống như lệnh `/if`.**

Đối số có tên `guard` tùy chọn (`on` theo mặc định) được sử dụng để bảo vệ chống lại các vòng lặp vô tận, giới hạn số lần lặp ở 100.
Để vô hiệu hóa và cho phép các vòng lặp vô tận, đặt `guard=off`.

Ví dụ này thêm 1 vào giá trị của `i` cho đến khi nó đạt 10, sau đó xuất giá trị kết quả (10 trong trường hợp này).

```stscript
/setvar key=i 0 |
/while left=i right=10 rule=lt "/addvar key=i 1" |
/echo {{getvar::i}} |
/flushvar i
```

#### Các đối số cho `/times`

Chạy một subcommand một số lần được chỉ định.

`/times (repeats) "(command)"` – bất kỳ slash command hợp lệ nào được đóng trong dấu ngoặc kép lặp lại một số lần, ví dụ: `/setvar key=i 1 | /times 5 "/addvar key=i 1"` thêm 1 vào giá trị của "i" 5 lần.
- {{timesIndex}} được thay thế bằng số lần lặp (dựa trên 0), ví dụ: `/times 4 {:/echo {{timesIndex}}:}` echo các số từ 0 đến 4.
- Vòng lặp được giới hạn ở 100 lần lặp theo mặc định, truyền `guard=off` để vô hiệu hóa.

### Thoát khỏi vòng lặp và Closure

```stscript
/break |
```

Lệnh `/break` có thể được sử dụng để thoát khỏi vòng lặp (`/while` hoặc `/times`) hoặc một closure sớm. Đối số không có tên của `/break` có thể được sử dụng để truyền một giá trị khác với pipe hiện tại.
`/break` hiện được triển khai trong các lệnh sau:
- `/while` - thoát khỏi vòng lặp sớm
- `/times` - thoát khỏi vòng lặp sớm
- `/run` (với một closure hoặc closure qua biến) - thoát khỏi closure sớm
- `/:` (với một closure) - thoát khỏi closure sớm

```stscript
/times 10 {:
	/echo {{timesIndex}}
	/delay 500 |
	/if left={{timesIndex}} rule=gt right=3 {:
		/break
	:} |
:} |
```

```stscript
/let x {: iterations=2
	/if left={{var::iterations}} rule=gt right=10 {:
		/break too many iterations! |
	:} |
	/times {{var::iterations}} {:
		/delay 500 |
		/echo {{timesIndex}} |
	:} |
:} |
/:x iterations=30 |
/echo the final result is: {{pipe}}
```

```stscript
/run {:
	/break 1 |
	/pass 2 |
:} |
/echo pipe will be one: {{pipe}} |
```

```stscript
/let x {:
	/break 1 |
	/pass 2 |
:} |
/:x |
/echo pipe will be one: {{pipe}} |
```

## Các phép toán toán học

- Tất cả các phép toán sau chấp nhận một chuỗi các số hoặc tên biến và xuất kết quả vào pipe.
- Các phép toán không hợp lệ (như chia cho 0) và các phép toán dẫn đến giá trị NaN hoặc vô cùng trả về 0.
- Phép nhân, cộng, tối thiểu và tối đa chấp nhận số lượng đối số không giới hạn được phân tách bằng khoảng trắng.
- Phép trừ, chia, lũy thừa và modulo chấp nhận hai đối số được phân tách bằng khoảng trắng.
- Sin, cosin, logarit tự nhiên, căn bậc hai, giá trị tuyệt đối và làm tròn chấp nhận một đối số.

**Danh sách các phép toán:**

1. `/add (a b c d)` – thực hiện phép cộng của tập hợp các giá trị, ví dụ: `/add 10 i 30 j`
2. `/mul (a b c d)` – thực hiện phép nhân của tập hợp các giá trị, ví dụ: `/mul 10 i 30 j`
3. `/max (a b c d)` – trả về giá trị tối đa từ tập hợp các giá trị, ví dụ: `/max 1 0 4 k`
4. `/min (a b c d)` – trả về giá trị tối thiểu từ tập hợp các giá trị, ví dụ: `/min 5 4 i 2`
5. `/sub (a b)` – thực hiện phép trừ của hai giá trị, ví dụ: `/sub i 5`
6. `/div (a b)` – thực hiện phép chia của hai giá trị, ví dụ: `/div 10 i`
7. `/mod (a b)` – thực hiện phép toán modulo của hai giá trị, ví dụ: `/mod i 2`
8. `/pow (a b)` – thực hiện phép toán lũy thừa của hai giá trị, ví dụ: `/pow i 2`
9. `/sin (a)` – thực hiện phép toán sin của một giá trị, ví dụ: `/sin i`
10. `/cos (a)` – thực hiện phép toán cosin của một giá trị, ví dụ: `/cos i`
11. `/log (a)` – thực hiện phép toán logarit tự nhiên của một giá trị, ví dụ: `/log i`
12. `/abs (a)` – thực hiện phép toán giá trị tuyệt đối của một giá trị, ví dụ: `/abs -10`
13. `/sqrt (a)`– thực hiện phép toán căn bậc hai của một giá trị, ví dụ: `/sqrt 9`
14. `/round (a)` – thực hiện phép toán làm tròn đến số nguyên gần nhất của một giá trị, ví dụ: `/round 3.14`
15. `/rand (round=round|ceil|floor from=number=0 to=number=1)` – trả về một số ngẫu nhiên giữa from và to, ví dụ: `/rand` hoặc `/rand 10` hoặc `/rand from=5 to=10`. Phạm vi là bao gồm. Giá trị trả về sẽ chứa một phần phân số. Sử dụng đối số có tên `round` để lấy một giá trị nguyên, ví dụ: `/rand round=ceil` để làm tròn lên, `round=floor` để làm tròn xuống và `round=round` để làm tròn đến gần nhất.

### Ví dụ 1: lấy diện tích của hình tròn có bán kính 50.

```stscript
/setglobalvar key=PI 3.1415 |
/setvar key=r 50 |
/mul r r PI |
/round |
/echo Circle area: {{pipe}}
```

### Ví dụ 2: tính giai thừa của 5.

```stscript
/setvar key=input 5 |
/setvar key=i 1 |
/setvar key=product 1 |
/while left=i right=input rule=lte "/mul product i \| /setvar key=product \| /addvar key=i 1" |
/getvar product |
/echo Factorial of {{getvar::input}}: {{pipe}} |
/flushvar input |
/flushvar i |
/flushvar product
```

## Sử dụng LLM

Script có thể thực hiện các yêu cầu đến LLM API hiện đang được kết nối của bạn bằng các lệnh sau:

- `/gen (prompt)` — tạo văn bản bằng lời nhắc được cung cấp cho nhân vật đã chọn và bao gồm tin nhắn trò chuyện.
- `/genraw (prompt)` — tạo văn bản chỉ bằng lời nhắc được cung cấp, bỏ qua nhân vật và trò chuyện hiện tại.
- `/trigger` — kích hoạt một thế hệ bình thường (tương đương với nhấp vào nút "Send"). Nếu trong trò chuyện nhóm, bạn có thể tùy chọn cung cấp chỉ số thành viên nhóm dựa trên 1 hoặc tên nhân vật để họ trả lời, nếu không kích hoạt một vòng nhóm theo cài đặt nhóm.
- `/swipe` — kích hoạt một lượt swipe trên tin nhắn nhân vật cuối cùng.
- `/regenerate` — tạo lại tin nhắn nhân vật cuối cùng.
- `/continue` — cố gắng tiếp tục tin nhắn cuối cùng.

### Các đối số cho `/gen` và `/genraw`

```stscript
/genraw lock=on/off stop=[] instruct=on/off (prompt)
```

- `lock` — có thể là `on` hoặc `off`. Chỉ định xem đầu vào của người dùng có nên bị chặn trong khi thế hệ đang tiến hành hay không. Mặc định: `off`.
- `stop` — Mảng JSON-serialized các chuỗi. Thêm một chuỗi dừng tùy chỉnh (nếu API hỗ trợ nó) chỉ cho thế hệ này. Mặc định: không có.
- `instruct` (chỉ `/genraw`) — có thể là `on` hoặc `off`. Cho phép sử dụng định dạng hướng dẫn trên lời nhắc đầu vào (nếu chế độ hướng dẫn được bật và API hỗ trợ nó). Đặt thành `off` để buộc lời nhắc thuần túy. Mặc định: `on`.
- `as` (cho Text Completion API) — có thể là `system` (mặc định) hoặc `char`. Xác định cách dòng lời nhắc cuối cùng sẽ được định dạng. `char` sẽ sử dụng tên nhân vật, `system` sẽ không sử dụng hoặc sử dụng tên trung tính.

Văn bản được tạo sau đó được truyền qua pipe đến lệnh tiếp theo và có thể được lưu vào một biến hoặc được hiển thị bằng khả năng I/O:

```stscript
/genraw Write a funny message from Cthulhu about taking over the world. Use emojis. |
/popup <h3>Cthulhu says:</h3><div>{{pipe}}</div>
```

| ![Cthulhu Says](/static/scripts/cthulhu-says.png) |
|---------------------------------------------------|

hoặc để chèn tin nhắn được tạo dưới dạng phản hồi từ nhân vật của bạn:

```stscript
/genraw You have been memory wiped, your name is now Lisa and you're tearing me apart. You're tearing me apart Lisa! |
/sendas name={{char}} {{pipe}}
```

## Nhân vật tạm thời

Nếu bạn không ở trong trò chuyện nhóm, script có thể tạm thời thực hiện một yêu cầu đến LLM hiện đang được kết nối với tư cách là một nhân vật khác.

- `/ask (prompt)` — tạo văn bản bằng lời nhắc được cung cấp cho một nhân vật được chỉ định và bao gồm tin nhắn trò chuyện. Xin lưu ý rằng các swipe của phản hồi từ nhân vật này sẽ trở lại nhân vật hiện tại.

```stscript
/ask name=... (prompt)
```
### Các đối số cho `/ask`

- `name` — **Bắt buộc**. Tên của nhân vật để hỏi (hoặc một định danh nhân vật duy nhất, chẳng hạn như khóa avatar). Điều này phải được cung cấp như một đối số có tên.
- `return` — Chỉ định cách giá trị trả về nên được cung cấp. Mặc định là `pipe` (đầu ra qua command pipe). Các tùy chọn khác có thể được chỉ định nếu được API hỗ trợ.

```stscript
/ask name=Alice What is your favorite color?
```

## Prompt injection

Script có thể thêm các prompt injection LLM tùy chỉnh, về cơ bản làm cho nó tương đương với Author's Note không giới hạn.

- `/inject (text)` — chèn bất kỳ văn bản nào vào prompt LLM bình thường cho cuộc trò chuyện hiện tại và yêu cầu một định danh duy nhất. Được lưu vào metadata trò chuyện.
- `/listinjects` — hiển thị danh sách tất cả các prompt injection được thêm bởi script cho cuộc trò chuyện hiện tại trong một tin nhắn hệ thống.
- `/flushinjects` — xóa tất cả các prompt injection được thêm bởi script cho cuộc trò chuyện hiện tại.
- `/note (text)` — đặt giá trị Author's Note cho cuộc trò chuyện hiện tại. Được lưu vào metadata trò chuyện.
- `/interval` — đặt khoảng chèn Author's Note cho cuộc trò chuyện hiện tại.
- `/depth` — đặt độ sâu chèn Author's Note cho vị trí trong trò chuyện.
- `/position` — đặt vị trí Author's Note cho cuộc trò chuyện hiện tại.

### Các đối số cho `/inject`

```stscript
/inject id=IdGoesHere position=chat depth=4 My prompt injection
```

- `id` — một chuỗi định danh hoặc tham chiếu đến một biến. Các lần gọi tiếp theo của `/inject` với cùng ID sẽ ghi đè văn bản injection trước đó. **Đối số bắt buộc.**
- `position` — đặt vị trí cho injection. Mặc định: `after`. Các giá trị có thể:
  - `after`: sau lời nhắc chính.
  - `before`: trước lời nhắc chính.
  - `chat`: trong trò chuyện.
- `depth` — đặt độ sâu injection cho vị trí trong trò chuyện. 0 có nghĩa là chèn sau tin nhắn cuối cùng, 1 - trước tin nhắn cuối cùng, v.v. Mặc định: 4.
- Đối số không có tên là văn bản cần được injection. Một chuỗi rỗng sẽ hủy đặt giá trị trước đó cho định danh được cung cấp.

## Truy cập tin nhắn trò chuyện

### Đọc tin nhắn

Bạn có thể truy cập tin nhắn trong cuộc trò chuyện hiện đang được chọn bằng lệnh `/messages`.

```stscript
/messages names=on/off start-finish
```

- Đối số `names` được sử dụng để chỉ định xem bạn có muốn bao gồm tên nhân vật hay không, mặc định: `on`.
- Trong đối số không có tên, nó chấp nhận một chỉ số tin nhắn hoặc phạm vi ở định dạng `start-finish`. Phạm vi là bao gồm!
- Nếu phạm vi không thể đáp ứng, tức là một chỉ số không hợp lệ hoặc nhiều tin nhắn hơn tồn tại được yêu cầu, thì một chuỗi rỗng được trả về.
- Tin nhắn bị ẩn khỏi lời nhắc (được ký hiệu bằng biểu tượng ma) bị loại trừ khỏi đầu ra.
- Nếu bạn muốn biết chỉ số của tin nhắn mới nhất, hãy sử dụng macro `{{lastMessageId}}` và `{{lastMessage}}` sẽ lấy cho bạn chính tin nhắn đó.

Để tính chỉ số bắt đầu cho một phạm vi, ví dụ: khi bạn cần lấy N tin nhắn cuối cùng, hãy sử dụng phép trừ biến.
Ví dụ này sẽ lấy cho bạn 3 tin nhắn cuối cùng trong trò chuyện:

```stscript
/setvar key=start {{lastMessageId}} |
/addvar key=start -2 |
/messages names=off {{getvar::start}}-{{lastMessageId}} |
/setinput
```

### Gửi tin nhắn

Một script có thể gửi tin nhắn dưới dạng người dùng, nhân vật, persona, kể chuyện trung lập hoặc thêm chú thích.

1. `/send (text)` — thêm một tin nhắn dưới dạng persona hiện đang được chọn.
2. `/sendas name=charname (text)` — thêm một tin nhắn dưới dạng bất kỳ nhân vật nào, khớp theo tên của họ. Đối số `name` là bắt buộc. Sử dụng macro `{{char}}` để gửi dưới dạng nhân vật hiện tại.
3. `/sys (text)` — thêm một tin nhắn từ kể chuyện trung lập không thuộc về người dùng hoặc nhân vật. Tên hiển thị hoàn toàn là mỹ phẩm và có thể được tùy chỉnh bằng lệnh `/sysname`.
4. `/comment (text)` — thêm một chú thích ẩn được hiển thị trong trò chuyện nhưng không hiển thị cho lời nhắc.
5. `/addswipe (text)` — thêm một swipe vào tin nhắn nhân vật cuối cùng. Không thể thêm swipe vào tin nhắn người dùng hoặc tin nhắn ẩn.
6. `/hide (message id or range)` — ẩn một hoặc nhiều tin nhắn khỏi lời nhắc dựa trên chỉ số tin nhắn được cung cấp hoặc phạm vi bao gồm ở định dạng `start-finish`.
7. `/unhide (message id or range)` — trả về một hoặc nhiều tin nhắn vào lời nhắc dựa trên chỉ số tin nhắn được cung cấp hoặc phạm vi bao gồm ở định dạng `start-finish`.

`/send`, `/sendas`, `/sys` và `/comment` lệnh tùy chọn chấp nhận một đối số có tên `at` với một giá trị số dựa trên 0 (hoặc tên biến chứa giá trị như vậy) chỉ định vị trí chèn tin nhắn chính xác. Theo mặc định, tin nhắn mới được chèn vào cuối nhật ký trò chuyện.

Điều này sẽ chèn một tin nhắn người dùng vào đầu lịch sử cuộc trò chuyện:

```stscript
/send at=0 Hi, I use Linux.
```

### Xóa tin nhắn

**Các lệnh này có khả năng phá hủy và không có chức năng "undo". Kiểm tra thư mục /backups/ nếu bạn vô tình xóa thứ gì đó quan trọng.**

1. `/cut (message id or range)` — cắt một hoặc nhiều tin nhắn từ trò chuyện dựa trên chỉ số tin nhắn được cung cấp hoặc phạm vi bao gồm ở định dạng `start-finish`.
2. `/del (number)` — xóa N tin nhắn cuối cùng từ trò chuyện.
3. `/delswipe (1-based swipe id)` — xóa một swipe từ tin nhắn nhân vật cuối cùng dựa trên swipe ID dựa trên 1 được cung cấp.
4. `/delname (character name)` — xóa tất cả tin nhắn trong cuộc trò chuyện hiện tại thuộc về một nhân vật với tên được chỉ định.
5. `/delchat` — xóa cuộc trò chuyện hiện tại.

## Các lệnh quản lý nhân vật

1. `/char-create` — tạo một nhân vật mới bằng dữ liệu được cung cấp với các đối số có tên.
2. `/char-update` — cập nhật nhân vật hiện tại bằng dữ liệu được cung cấp với các đối số có tên.
3. `/char-get` — lấy dữ liệu của nhân vật hiện tại dưới dạng một đối tượng JSON và truyền nó vào pipe.
4. `/char-delete (name)` — xóa nhân vật có tên được chỉ định.
5. `/char-duplicate (name)` — nhân bản nhân vật có tên được chỉ định.
6. `/tag-import (name)` — nhập các tag từ một tệp thẻ nhân vật.

## Các lệnh Loader

Hệ thống loader cung cấp một lớp phủ có thể tái sử dụng cho các tác vụ tốn thời gian cần cung cấp phản hồi trực quan và/hoặc tạm thời chặn giao diện.

1. `/loader-show (text)` — hiển thị một lớp phủ đang tải với văn bản được chỉ định.
2. `/loader-hide` — ẩn lớp phủ đang tải.
3. `/loader-wrap (closure)` — hiển thị một lớp phủ đang tải, thực thi closure được cung cấp, và ẩn lớp phủ khi hoàn tất.
4. `/loader-stop` — dừng và loại bỏ lớp phủ đang tải.

## Các lệnh World Info

World Info (còn được gọi là Lorebook) là một công cụ hữu ích cao để chèn dữ liệu động vào lời nhắc. Xem trang chuyên dụng để biết giải thích chi tiết hơn: [World Info](/Usage/worldinfo.md).

1. `/getchatbook` – lấy tên của file World Info liên kết với trò chuyện hoặc tạo một file mới nếu chưa được liên kết, và truyền nó xuống pipe.
2. `/findentry file=bookName field=fieldName [text]` – tìm UID của bản ghi từ file được chỉ định (hoặc một biến trỏ đến tên file) sử dụng khớp mờ của giá trị trường với văn bản được cung cấp (trường mặc định: `key`) và truyền UID xuống pipe, ví dụ: `/findentry file=chatLore field=key Shadowfang`.
3. `/getentryfield file=bookName field=field [UID]` – lấy giá trị trường (trường mặc định: `content`) của bản ghi với UID từ file World Info được chỉ định (hoặc một biến trỏ đến tên file) và truyền giá trị xuống pipe, ví dụ: `/getentryfield file=chatLore field=content 123`.
4. `/setentryfield file=bookName uid=UID field=field [text]` – đặt giá trị trường (trường mặc định: `content`) của bản ghi với UID (hoặc một biến trỏ đến UID) từ file World Info được chỉ định (hoặc một biến trỏ đến tên file). Để đặt nhiều giá trị cho các trường khóa, sử dụng danh sách phân tách bằng dấu phẩy làm giá trị văn bản, ví dụ: `/setentryfield file=chatLore uid=123 field=key Shadowfang,sword,weapon`.
5. `/createentry file=bookName key=keyValue [content text]` – tạo một bản ghi mới trong file được chỉ định (hoặc một biến trỏ đến tên file) với khóa và nội dung (cả hai đối số này đều *tùy chọn*) và truyền UID xuống pipe, ví dụ: `/createentry file=chatLore key=Shadowfang The sword of the king`.

### Các trường entry hợp lệ

| Trường             | Phần tử UI        | Loại giá trị      |
|:-------------------|:------------------|:----------------|
| `content`          | Content           | String          |
| `comment`          | Title / Memo      | String          |
| `key`              | Primary Keywords  | List of strings |
| `keysecondary`     | Optional Filter   | List of strings |
| `constant`         | Constant Status   | Boolean (1/0)   |
| `disable`          | Disabled Status   | Boolean (1/0)   |
| `order`            | Order             | Number          |
| `selectiveLogic`   | Logic             | (xem bên dưới)     |
| `excludeRecursion` | Non-recursable    | Boolean (1/0)   |
| `probability`      | Trigger%          | Number (0-100)  |
| `depth`            | Depth             | Number (0-999)  |
| `position`         | Position          | (xem bên dưới)     |
| `role`             | Depth Role        | (xem bên dưới)     |
| `scanDepth`        | Scan Depth        | Number (0-100)  |
| `caseSensitive`    | Case-Sensitive    | Boolean (1/0)   |
| `matchWholeWords`  | Match Whole Words | Boolean (1/0)   |
| `vectorized`       | Vectorized Status | Boolean (1/0)   |
| `automationId`     | Automation ID     | String          |
| `group`            | Inclusion Group   | String          |
| `groupOverride`    | Inclusion Group Prioritize | Boolean (1/0) |
| `groupWeight`      | Inclusion Group Weight | Number (0-100) |
| `useGroupScoring`  | Group Scoring     | Boolean (1/0)   |
| `characterFilterExclude` | Character Filter Exclude Mode | List of strings |
| `characterFilterNames` | Character Filter Names | List of strings |
| `characterFilterTags` | Character Filter Tags | List of strings |
| `matchCharacterDepthPrompt` | Match Character Depth Prompt | Boolean (1/0) |
| `matchCharacterDescription` | Match Character Description | Boolean (1/0) |
| `matchCharacterPersonality` | Match Character Personality | Boolean (1/0) |
| `matchCreatorNotes` | Match Creator Notes | Boolean (1/0) |
| `matchPersonaDescription` | Match Persona Description | Boolean (1/0) |
| `matchScenario` | Match Scenario | Boolean (1/0) |

**Giá trị Logic**

- 0 = AND ANY
- 1 = NOT ALL
- 2 = NOT ANY
- 3 = AND ALL

**Giá trị Position**

- 0 = before main prompt
- 1 = after main prompt
- 2 = top of Author's Note
- 3 = bottom of Author's Note
- 4 = in-chat at depth
- 5 = top of example messages
- 6 = bottom of example messages

**Giá trị Role** (chỉ Position = 4)
- 0 = System
- 1 = User
- 2 = Assistant

### Ví dụ 1: Đọc nội dung từ lorebook trò chuyện theo khóa

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Shadowfang |
/getentryfield file={{getvar::chatLore}} field=key |
/echo
```

### Ví dụ 2: Tạo một entry lorebook trò chuyện với khóa và nội dung

```stscript
/getchatbook | /setvar key=chatLore |
/createentry file={{getvar::chatLore}} key="Milla" Milla Basset is a friend of Lilac and Carol. She is a hush basset puppy who possesses the power of alchemy. |
/echo
```

### Ví dụ 3: Mở rộng một entry lorebook hiện có với thông tin mới từ trò chuyện

```stscript
/getchatbook | /setvar key=chatLore |
/findentry file={{getvar::chatLore}} field=key Milla |
/setvar key=millaUid |
/getentryfield file={{getvar::chatLore}} field=content |
/setvar key=millaContent |
/gen lock=on Tell me more about Milla Basset based on the provided conversation history. Incorporate existing information into your reply: {{getvar::millaContent}} |
/setvar key=millaContent |
/echo New content: {{pipe}} |
/setentryfield file={{getvar::chatLore}} uid=millaUid field=content {{getvar::millaContent}}
```

## Thao tác văn bản

Có nhiều lệnh tiện ích thao tác văn bản hữu ích để sử dụng trong các kịch bản script khác nhau.

1. `/trimtokens` — cắt đầu vào thành số lượng token văn bản được chỉ định từ đầu hoặc từ cuối và xuất kết quả vào pipe.
2. `/trimstart` — cắt đầu vào đến đầu của câu hoàn chỉnh đầu tiên và xuất kết quả vào pipe.
3. `/trimend` — cắt đầu vào đến cuối của câu hoàn chỉnh cuối cùng và xuất kết quả vào pipe.
4. `/fuzzy` — thực hiện khớp mờ của văn bản đầu vào với danh sách các chuỗi, xuất chuỗi khớp tốt nhất vào pipe.
5. `/regex name=scriptName [text]` — thực thi một script regex từ extension Regex cho văn bản được chỉ định. Script phải được bật.

### Các đối số cho `/trimtokens`

```stscript
/trimtokens limit=number direction=start/end (input)
```

1. `direction` đặt hướng để cắt, có thể là `start` hoặc `end`. Mặc định: `end`.
2. `limit` đặt số lượng token để còn lại trong đầu ra. Cũng có thể chỉ định tên biến chứa số. **Đối số bắt buộc.**
3. Đối số không có tên là văn bản đầu vào cần được cắt.

### Các đối số cho `/fuzzy`

```stscript
/fuzzy list=["candidate1","candidate2"] (input)
```

1. `list` là một mảng JSON-serialized các chuỗi chứa các ứng cử viên. Cũng có thể chỉ định tên biến chứa danh sách. **Đối số bắt buộc.**
2. Đối số không có tên là văn bản đầu vào cần được khớp. Đầu ra là một trong các ứng cử viên khớp với đầu vào gần nhất.

## Autocomplete

- Autocomplete được bật cả trên đầu vào trò chuyện và trình chỉnh sửa Quick Reply lớn.
- Autocomplete hoạt động ở bất cứ đâu trong đầu vào của bạn. Ngay cả với nhiều lệnh được piped và closure lồng nhau.
- Autocomplete hỗ trợ ba cách tra cứu các lệnh khớp (*User Settings* -> *STscript Matching*).

1. **Starts with** Cách "cũ". Chỉ các lệnh bắt đầu chính xác với giá trị đã nhập mới hiển thị.
2. **Includes** Tất cả các lệnh *bao gồm* giá trị đã nhập sẽ hiển thị. Ví dụ: Khi nhập `/delete`, các lệnh `/qr-delete` và `/qr-set-delete` sẽ hiển thị trong danh sách autocomplete (/qr-**delete**, /qr-set-**delete**).
3. **Fuzzy** Tất cả các lệnh có thể được khớp mờ với giá trị đã nhập sẽ hiển thị. Ví dụ: Khi nhập `/seas`, lệnh `/sendas` sẽ hiển thị trong danh sách autocomplete (/**se**nd**as**).

- Các đối số lệnh cũng được autocomplete hỗ trợ. Danh sách sẽ hiển thị cho các đối số bắt buộc tự động. Đối với các đối số tùy chọn, nhấn *Ctrl*+*Space* để mở danh sách các tùy chọn có sẵn.
- Khi nhập `/:` để thực thi một closure hoặc QR, autocomplete sẽ hiển thị danh sách các biến scoped và QR.
- Autocomplete có hỗ trợ hạn chế cho macro (trong slash command). Nhập `{{` để lấy danh sách các macro có sẵn.
- Sử dụng *phím mũi tên lên* và *xuống* để chọn một tùy chọn từ danh sách các tùy chọn autocomplete.
- Nhấn *Enter* hoặc *Tab* hoặc *nhấp* vào một tùy chọn để đặt tùy chọn tại con trỏ.
- Nhấn *Escape* để đóng danh sách autocomplete.
- Nhấn *Ctrl*+*Space* để mở danh sách autocomplete hoặc chuyển đổi chi tiết của tùy chọn đã chọn.

## Parser Flag

```stscript
/parser-flag
```

Parser chấp nhận các cờ để sửa đổi hành vi của nó. Các cờ này có thể được bật và tắt tại bất kỳ điểm nào trong script và tất cả đầu vào sau sẽ được đánh giá tương ứng.
Bạn có thể đặt cờ mặc định của mình trong cài đặt người dùng.

### Strict Escaping

```stscript
/parser-flag STRICT_ESCAPING on |
```

Các thay đổi với `STRICT_ESCAPING` được bật như sau.

#### Pipe

Pipe không cần được thoát trong các giá trị được trích dẫn.

```stscript
/echo title="a|b" c\|d
```

#### Dấu gạch chéo ngược

Một dấu gạch chéo ngược trước một ký hiệu có thể được thoát để cung cấp dấu gạch chéo ngược theo nghĩa đen tiếp theo là ký hiệu chức năng.

```stscript
// this will echo "foo \", then echo "bar" |
/echo foo \\|
/echo bar
```

```stscript
/echo \\|
/echo \\\|
```

### Replace Variable Macro

```stscript
/parser-flag REPLACE_GETVAR on |
```

Cờ này giúp tránh các thay thế kép khi các giá trị biến chứa văn bản có thể được hiểu là macro. Các macro `{{var::}}` được thay thế cuối cùng và không có thay thế thêm nào xảy ra trên văn bản / giá trị biến kết quả.

Thay thế tất cả các macro `{{getvar::}}` và `{{getglobalvar::}}` bằng `{{var::}}`.
Đằng sau hậu trường, parser sẽ chèn một chuỗi các command executor trước lệnh với các macro đã thay thế:

- gọi `/let` để lưu `{{pipe}}` hiện tại vào một biến scoped
- gọi `/getvar` hoặc `/getglobalvar` để lấy biến được sử dụng trong macro
- gọi `/let` để lưu biến đã truy xuất vào một biến scoped
- gọi `/return` với giá trị `{{pipe}}` đã lưu để khôi phục giá trị piped chính xác cho lệnh tiếp theo

```stscript
// the following will echo the last message's id / number |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

```stscript
// this will echo the literal text {{lastMessageId}} |
/parser-flag REPLACE_GETVAR |
/setvar key=x \{\{lastMessageId}} |
/echo {{getvar::x}}
```

## Quick Reply: thư viện script và tự động thực thi

Quick Reply là một extension tích hợp SillyTavern cung cấp một cách dễ dàng để lưu trữ và thực thi các script của bạn.

### Cấu hình Quick Reply

Để bắt đầu, hãy bật mở bảng extension (biểu tượng khối xếp chồng) và mở rộng menu Quick Reply.

<div style="display:flex;justify-content:center">

![Quick Reply](/static/scripts/quick-reply.png)

</div>

**Quick Reply bị vô hiệu hóa theo mặc định, bạn cần bật chúng trước.** Sau đó, bạn sẽ thấy một thanh xuất hiện phía trên thanh nhập trò chuyện của bạn.

Bạn có thể đặt nhãn văn bản nút hiển thị (chúng tôi khuyến nghị sử dụng emoji cho ngắn gọn) và script sẽ được thực thi khi bạn nhấp vào nút.

Số lượng nút được kiểm soát bởi cài đặt **Number of slots** (tối đa = 100), điều chỉnh nó theo nhu cầu của bạn và nhấp "Apply" khi hoàn thành.

**Inject user input automatically** được khuyến nghị vô hiệu hóa khi sử dụng STscript, nếu không nó có thể can thiệp vào đầu vào của bạn, sử dụng macro `{{input}}` để lấy giá trị hiện tại của thanh nhập trong script thay thế.

**Quick Reply preset** cho phép có nhiều bộ Quick Reply được xác định trước và chuyển đổi giữa chúng bằng tay hoặc bằng cách sử dụng lệnh `/qrset (name of set)`.
Đừng quên nhấp "Update" trước khi chuyển sang một bộ khác để ghi các thay đổi của bạn vào preset hiện đang được sử dụng!

### Thực thi thủ công

Bây giờ bạn có thể thêm script đầu tiên của mình vào thư viện. Chọn bất kỳ slot miễn phí nào (hoặc tạo một slot), nhập "Click me" vào hộp bên trái để đặt nhãn, sau đó dán cái này vào hộp bên phải:

```stscript
/addvar key=clicks 1 |
/if left=clicks right=5 rule=eq else="/echo Keep going..." "/echo You did it!  \| /flushvar clicks"
```

Sau đó nhấp 5 lần vào nút xuất hiện phía trên thanh trò chuyện.
Mỗi lần nhấp tăng biến `clicks` lên một và hiển thị một tin nhắn khác nhau khi giá trị bằng 5 và đặt lại biến.

### Tự động thực thi

Mở menu modal bằng cách nhấp vào nút `⋮` cho lệnh đã tạo.

| ![Automatic execution](/static/scripts/autoexecute.png) |
|---------------------------------------------------------|

Trong menu này, bạn có thể làm như sau:

- Chỉnh sửa script trong một trình chỉnh sửa toàn màn hình thuận tiện
- Ẩn nút khỏi thanh trò chuyện, làm cho nó chỉ có thể truy cập cho tự động thực thi.
- Bật tự động thực thi trên một hoặc nhiều điều kiện sau:
  * Khởi động ứng dụng
  * Gửi tin nhắn người dùng đến trò chuyện
  * Nhận tin nhắn AI trong trò chuyện
  * Mở trò chuyện nhân vật hoặc nhóm
  * Kích hoạt phản hồi từ thành viên nhóm
  * Kích hoạt một entry World Info sử dụng cùng Automation ID
- Cung cấp tooltip tùy chỉnh cho quick reply (văn bản hiển thị khi di chuột qua quick reply trong giao diện người dùng của bạn)
- Thực thi script cho mục đích thử nghiệm

Các lệnh chỉ được thực thi tự động nếu extension Quick Reply được bật.

Ví dụ: bạn có thể hiển thị một tin nhắn sau khi gửi năm tin nhắn người dùng bằng cách thêm script sau và đặt nó tự động thực thi trên tin nhắn người dùng.

```stscript
/addvar key=usercounter 1 |
/echo You've sent {{pipe}} messages. |
/if left=usercounter right=5 rule=gte "/echo Game over! \| /flushvar usercounter"
```

### Debugger

Một debugger cơ bản tồn tại bên trong trình chỉnh sửa Quick Reply mở rộng. Đặt breakpoint với `/breakpoint |` ở bất cứ đâu trong script của bạn. Khi thực thi script từ trình chỉnh sửa QR, việc thực thi sẽ bị gián đoạn tại điểm đó, cho phép bạn kiểm tra các biến hiện có sẵn, pipe, đối số lệnh và nhiều hơn nữa, và từng bước qua phần còn lại của code từng phần một.

```stscript
/let x {: n=1
	/echo n is {{var::n}} |
	/mul n n |
:} |
/breakpoint |
/:x n=3 |
/echo result is {{pipe}} |
```

| ![QR Editor Debugger](/static/scripts/st-debugger.png) |
|--------------------------------------------------------|

### Gọi procedure

Một lệnh `/run` có thể gọi các script được định nghĩa trong Quick Reply bằng nhãn của chúng, về cơ bản cung cấp khả năng định nghĩa các procedure và trả về kết quả từ chúng. Điều này cho phép có các khối script có thể tái sử dụng mà các script khác có thể tham chiếu. Kết quả cuối cùng từ pipe của procedure được truyền đến lệnh tiếp theo sau nó.

```stscript
/run ScriptLabel
```

Hãy tạo hai Quick Reply:

***
**Label:**

`GetRandom`

**Command:**

```stscript
/pass {{roll:d100}}
```
***
**Label:**

`GetMessage`

**Command:**
```stscript
/run GetRandom | /echo Your lucky number is: {{pipe}}
```
***

Nhấp vào nút `GetMessage` sẽ gọi procedure `GetRandom` sẽ giải quyết macro `{{roll}}` và truyền số cho caller, hiển thị nó cho người dùng.

- Procedure không chấp nhận các đối số có tên hoặc không có tên, nhưng có thể tham chiếu các biến giống như caller.
- Tránh đệ quy khi gọi procedure vì nó có thể tạo ra lỗi "call stack exceeded" nếu được xử lý không cẩn thận.

#### Gọi procedure từ một Quick Reply preset khác

Bạn có thể gọi một procedure từ một quick reply preset khác bằng cú pháp `a.b`, trong đó a = tên QR preset và b = tên nhãn QR

```stscript
/run QRpreset1.QRlabel1
```

Theo mặc định, hệ thống sẽ tìm kiếm nhãn quick reply `a.b` trước, vì vậy nếu một trong các nhãn của bạn là chính xác "QRpreset1.QRlabel1" nó sẽ cố gắng chạy cái đó. Nếu không tìm thấy nhãn như vậy, nó sẽ tìm kiếm tên QR preset "QRpreset1" với một QR được gắn nhãn "QRlabel1".

### Các lệnh quản lý Quick Reply

#### Tạo Quick Reply

* `/qr-create (arguments, [message])` – tạo một Quick Reply mới, ví dụ: `/qr-create set=MyPreset label=MyButton /echo 123`

Các đối số:
- `label` - string - văn bản trên nút, ví dụ: `label=MyButton`
- `set` - string - tên của bộ QR, ví dụ: `set=PresetName1`
- `hidden` - bool - nút có nên được ẩn hay không, ví dụ: `hidden=true`
- `startup` - bool - tự động thực thi khi khởi động ứng dụng, ví dụ: `startup=true`
- `user` - bool - tự động thực thi trên tin nhắn người dùng, ví dụ: `user=true`
- `bot` - bool - tự động thực thi trên tin nhắn AI, ví dụ: `bot=true`
- `load` - bool - tự động thực thi khi tải trò chuyện, ví dụ: `load=true`
- `title` - bool - title / tooltip được hiển thị trên nút, ví dụ: `title="My Fancy Button"`

#### Xóa Quick Reply

* `/qr-delete (set=string [label])` – xóa Quick Reply

#### Cập nhật Quick Reply

* `/qr-update (arguments, [message])` – cập nhật Quick Reply, ví dụ: `/qr-update set=MyPreset label=MyButton newlabel=MyRenamedButton /echo 123`

Các đối số:
- `newlabel` - string - văn bản mới cho nút, ví dụ: `newlabel=MyRenamedButton`
- `label` - string - văn bản trên nút, ví dụ: `label=MyButton`
- `set` - string - tên của bộ QR, ví dụ: `set=PresetName1`
- `hidden` - bool - nút có nên được ẩn hay không, ví dụ: `hidden=true`
- `startup` - bool - tự động thực thi khi khởi động ứng dụng, ví dụ: `startup=true`
- `user` - bool - tự động thực thi trên tin nhắn người dùng, ví dụ: `user=true`
- `bot` - bool - tự động thực thi trên tin nhắn AI, ví dụ: `bot=true`
- `load` - bool - tự động thực thi khi tải trò chuyện, ví dụ: `load=true`
- `title` - bool - title / tooltip được hiển thị trên nút, ví dụ: `title="My Fancy Button"`

####

* `qr-get` - truy xuất tất cả các thuộc tính của Quick Reply, ví dụ: `/qr-get set=myQrSet id=42`

#### Tạo hoặc cập nhật QR preset

* `/qr-presetupdate (arguments [label])` hoặc `/qr-presetadd (arguments [label])`

Các đối số:
- `enabled` - bool - bật hoặc vô hiệu hóa preset
- `nosend` - bool - vô hiệu hóa gửi / chèn vào đầu vào người dùng (không hợp lệ cho slash command)
- `before` - bool - đặt QR trước đầu vào người dùng
- `slots` - int - số lượng slot
- `inject` - bool - tiêm đầu vào người dùng tự động (nếu vô hiệu hóa sử dụng `{{input}}`)

Tạo một preset mới (ghi đè các preset hiện có), ví dụ: `/qr-presetadd slots=3 MyNewPreset`

#### Thêm QR context menu

* `/qr-contextadd (set=string label=string chain=bool [preset name])` – thêm context menu preset vào một QR, ví dụ: `/qr-contextadd set=MyPreset label=MyButton chain=true MyOtherPreset`

#### Xóa tất cả context menu

* `/qr-contextclear (set=string [label])` – xóa tất cả context menu preset từ một QR, ví dụ: `/qr-contextclear set=MyPreset MyButton`

#### Xóa một context menu

* `/qr-contextdel (set=string label=string [preset name])` – xóa context menu preset từ một QR, ví dụ: `/qr-contextdel set=MyPreset label=MyButton MyOtherPreset`

### Thoát giá trị Quick Reply

`|{}` có thể được thoát bằng dấu gạch chéo ngược trong tin nhắn / lệnh QR.

Ví dụ, sử dụng `/qr-create label=MyButton /getvar myvar \| /echo \{\{pipe\}\}` để tạo một QR gọi `/getvar myvar | /echo {{pipe}}`.

## Các lệnh Extension

Các extension SillyTavern (cả tích hợp, có thể tải xuống và của bên thứ ba) có thể thêm slash command riêng của chúng. Dưới đây chỉ là một ví dụ về khả năng trong các extension chính thức. Danh sách có thể không đầy đủ, đảm bảo kiểm tra `/help slash` để có danh sách đầy đủ nhất các lệnh có sẵn.

1. `/websearch (query)` — tìm kiếm các đoạn của các trang web trực tuyến cho truy vấn được chỉ định và trả về kết quả vào pipe. Được cung cấp bởi extension Web Search.
2. `/imagine (prompt)` — tạo một hình ảnh bằng lời nhắc được cung cấp. Được cung cấp bởi extension Image Generation.
3. `/emote (sprite)` — đặt một sprite cho nhân vật đang hoạt động bằng cách khớp mờ tên của nó. Được cung cấp bởi extension Character Expressions.
4. `/costume (subfolder)` — đặt ghi đè bộ sprite cho nhân vật đang hoạt động. Được cung cấp bởi extension Character Expressions.
5. `/music (name)` — buộc thay đổi file nhạc nền được phát theo tên của nó. Được cung cấp bởi extension Dynamic Audio.
6. `/ambient (name)` — buộc thay đổi file âm thanh môi trường được phát theo tên của nó. Được cung cấp bởi extension Dynamic Audio.
7. `/roll (dice formula)` — thêm một tin nhắn ẩn vào trò chuyện với kết quả của một lần tung xúc xắc. Được cung cấp bởi extension D&D Dice.

## Tương tác UI

Script cũng có thể tương tác với giao diện người dùng của SillyTavern: điều hướng qua các cuộc trò chuyện hoặc thay đổi các tham số kiểu.

### Điều hướng nhân vật

1. `/random` — mở một trò chuyện với nhân vật ngẫu nhiên.
2. `/go (name)` — mở một trò chuyện với nhân vật có tên được chỉ định. Đầu tiên, tìm kiếm khớp tên chính xác, sau đó theo tiền tố, sau đó theo chuỗi con.

### Kiểu UI

1. `/bubble` — đặt kiểu tin nhắn thành kiểu "bubble chat".
2. `/flat` — đặt kiểu tin nhắn thành kiểu "flat chat".
3. `/single` — đặt kiểu tin nhắn thành kiểu "single document".
4. `/movingui (name)` — kích hoạt một preset MovingUI theo tên.
5. `/resetui` — đặt lại trạng thái bảng MovingUI về vị trí ban đầu của chúng.
6. `/panels` — chuyển đổi khả năng hiển thị bảng UI: thanh trên, drawer trái và phải.
7. `/bg (name)` — tìm và đặt một nền bằng cách sử dụng khớp tên mờ. Tôn trọng trạng thái khóa nền trò chuyện.
8. `/lockbg` — khóa hình nền cho cuộc trò chuyện hiện tại.
9. `/unlockbg` — mở khóa hình nền cho cuộc trò chuyện hiện tại.

## Thêm ví dụ

### Tạo tóm tắt trò chuyện (bởi @IkariDevGIT)

```stscript
/setglobalvar key=summaryPrompt Summarize the most important facts and events that have happened in the chat given to you in the Input header. Limit the summary to 100 words or less. Your response should include nothing but the summary. |
/setvar key=tmp |
/messages 0-{{lastMessageId}} |
/trimtokens limit=3000 direction=end |
/setvar key=s1 |
/echo Generating, please wait... |
/genraw lock=on instruct=off {{instructInput}}{{newline}}{{getglobalvar::summaryPrompt}}{{newline}}{{newline}}{{instructInput}}{{newline}}{{getvar::s1}}{{newline}}{{newline}}{{instructOutput}}{{newline}}The chat summary:{{newline}} |
/setvar key=tmp |
/echo Done! |
/setinput {{getvar::tmp}} |
/flushvar tmp |
/flushvar s1
```

### Sử dụng popup button

```stscript
/setglobalvar key=genders ["boy", "girl", "other"] |
/buttons labels=genders Who are you? |
/echo You picked: {{pipe}}
```

### Lấy số Fibonacci thứ N (sử dụng công thức Binet)

!!!tip
**Gợi ý**: Đặt giá trị của `fib_no` thành số mong muốn
!!!

```stscript
/setvar key=fib_no 5 |
/pow 5 0.5 | /setglobalvar key=SQRT5 |
/setglobalvar key=PHI 1.618033 |
/pow PHI fib_no | /div {{pipe}} SQRT5 |
/round |
/echo {{getvar::fib_no}}th Fibonacci's number is: {{pipe}}
```

### Giai thừa đệ quy (sử dụng closure)

```stscript
/let fact {: n=
    /if left={{var::n}} rule=gt right=1
        else={:
            /return 1
        :}
        {:
            /sub {{var::n}} 1 |
            /:fact n={{pipe}} |
            /mul {{var::n}} {{pipe}}
        :}
:} |

/input Calculate factorial of: |
/let n {{pipe}} |
/:fact n={{var::n}} |
/echo factorial of {{var::n}} is {{pipe}}
```
