# ⚡ CP+* (C-Plus-Plus-Star)

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-orange)](https://github.com/your-repo/cpps-native)

**CP+*** là một ngôn ngữ lập trình **hoàn toàn mới**, được thiết kế từ đầu với triết lý riêng: kết hợp sức mạnh của C++, sự thanh lịch của Python, và những ý tưởng hiện đại từ Rust, Go và Swift. Không phải là bản sao của bất kỳ ngôn ngữ nào — CP+* có cú pháp, hệ thống kiểu và mô hình thực thi độc đáo của riêng mình.

Dự án bao gồm một trình thông dịch (interpreter) viết bằng Python, hỗ trợ đầy đủ các tính năng như lập trình hướng đối tượng, quản lý quyền sở hữu (ownership), đồng thời (concurrency), pattern matching, macros và reflection — tất cả đều được tích hợp trong một ngôn ngữ thống nhất.

---

## 🔥 Tại sao CP+*?

- **Cú pháp riêng biệt, không lai căng** — Mọi ký hiệu (`:=`, `:: mut`, `++`, `??`, `<>`, `~>`, `<-`, `!!`, `?~`, `@`) đều được định nghĩa rõ ràng và nhất quán.
- **Không kế thừa từ C hay C++** — CP+* không tương thích ngược với C, không có tiền xử lý, không có con trỏ thô. Mọi thứ đều an toàn và hiện đại.
- **Ownership và Borrowing** — Hệ thống quyền sở hữu giúp quản lý bộ nhớ an toàn mà không cần garbage collector.
- **Đồng thời đơn giản** — Goroutine và Channel được tích hợp sẵn, dễ dùng.
- **Macros và Reflection** — Cho phép metaprogramming mạnh mẽ ngay trong ngôn ngữ.
- **Pattern Matching** — Xử lý dữ liệu phức tạp một cách tự nhiên.
- **Hỗ trợ Unicode** — Viết code bằng tiếng Việt, tiếng Trung, tiếng Ả Rập, v.v.

---

## ✨ Các tính năng nổi bật

### Cú pháp độc đáo

| Ký hiệu | Ý nghĩa |
|---------|---------|
| `:=` | Khai báo biến bất biến |
| `:: mut` | Khai báo biến có thể thay đổi |
| `++` | Định nghĩa hàm |
| `<~` | Mũi tên tham số |
| `->` | Kiểu trả về / lambda |
| `**` | Mở đầu thân hàm/block |
| `??` | Câu điều kiện `if` |
| `<>` | Vòng lặp `for-each` |
| `~>` | In ra / pipe |
| `<-` | Lệnh `return` |
| `!!` | Lệnh `panic` |
| `!>` | Lệnh `break` |
| `!>>` | Lệnh `continue` |
| `?~` | Pattern matching |
| `@` | Self reference |
| `@.field` | Truy cập field của self |
| `::` | Gọi method / namespace |
| `@@` | Annotation (ví dụ `@@override`) |

### Lập trình hướng đối tượng đầy đủ

```cpp
class Dog : Animal -> {
    name :: mut string = ""
    
    ++ new <~ (n: string) ** {
        @.name = n
    }
    
    @@override
    ++ speak <~ () -> void ** {
        ~> io::println("{} says: Woof!", @.name)
    }
}
```

Ownership và Borrowing

```cpp
own<int> x := 42        -- sở hữu độc quyền
share<string> y := "shared"  -- tham chiếu chia sẻ
borrow<int> z := x      -- mượn tạm thời
```

Goroutine và Channel

```cpp
ch := Channel(10)       -- buffered channel

go ** {
    ch:send(42)
}

value := ch:recv()
~> io::println("Received: {}", value)
```

Pattern Matching

```cpp
?~ score {
    90..=100 => { ~> io::println("Xuất sắc!") },
    70..89   => { ~> io::println("Tốt") },
    _        => { ~> io::println("Cần cố gắng") }
}
```

Xử lý lỗi

```cpp
try ** {
    result := risky_operation()
    ~> io::println("Success: {}", result)
} catch (e) ** {
    ~> io::println("Error: {}", e)
} finally ** {
    ~> io::println("Cleanup")
}
```

---

📦 Cài đặt và sử dụng

Yêu cầu

· Python 3.8 trở lên

Tải về

```bash
git clone https://github.com/your-repo/cpps-native.git
cd cpps-native
```

Chạy file CP+*

```bash
python cpps.py hello.cpps
```

REPL

```bash
python cpps.py
```

Chạy inline

```bash
python cpps.py -e 'x := 42; ~> io::println("x = {}", x)'
```

Xem AST

```bash
python cpps.py --ast hello.cpps
```

Xem danh sách token

```bash
python cpps.py --tokens hello.cpps
```

---

📝 Ví dụ hoàn chỉnh

```cpp
-- Chương trình tính giai thừa với pattern matching và goroutine

++ factorial <~ (n: int) -> int ** {
    ?? n <= 1 ** { <- 1 }
    -- else ** { <- n * factorial(n - 1) }
}

++ main <~ () -> int ** {
    ~> io::println("Nhập một số:")
    n := input()
    num := int(n)
    
    go ** {
        result := factorial(num)
        ~> io::println("{}! = {}", num, result)
    }
    
    -- Chờ goroutine hoàn thành
    time::sleep(1.0)
    <- 0
}
```

---

🏗️ Kiến trúc hệ thống

Module Vai trò
cpps.py Entry point, CLI, REPL
lexer.py Tokenizer hỗ trợ Unicode, template strings, raw strings, comment lồng nhau
tokens.py Định nghĩa hệ thống token (120+ loại)
parser.py Recursive descent parser, xây dựng AST
interpreter.py Tree-walking interpreter, thực thi AST, quản lý scope và runtime

Luồng xử lý

```
Source (.cpps) → Lexer → Tokens → Parser → AST → Interpreter → Kết quả
```

---

🧪 Các lệnh REPL đặc biệt

Lệnh Mô tả
:help, :h Hiển thị trợ giúp
:quit, :q, :exit Thoát REPL
:clear, :c Xóa màn hình
:reset Reset interpreter
:env Hiển thị các biến trong scope
:ast <code> Hiện AST của code
:tokens <code> Hiện token
:time <code> Đo thời gian thực thi
:type <expr> Hiển thị kiểu dữ liệu
:load <file> Load và chạy file .cpps

---

📚 Tài liệu tham khảo

· Cú pháp chi tiết (sắp ra mắt)
· Hướng dẫn Ownership (sắp ra mắt)
· Macros và Reflection (sắp ra mắt)
· Thư viện chuẩn (sắp ra mắt)

---

🤝 Đóng góp

Chúng tôi hoan nghênh mọi đóng góp! Hãy tạo issue hoặc pull request.

1. Fork dự án
2. Tạo branch mới (git checkout -b feature/AmazingFeature)
3. Commit thay đổi (git commit -m 'Add some AmazingFeature')
4. Push lên branch (git push origin feature/AmazingFeature)
5. Mở Pull Request

---

📄 Giấy phép

Dự án được phân phối dưới giấy phép MIT. Xem file LICENSE để biết thêm chi tiết.

---

🌟 Tác giả

· Your Name – Initial work – YourGitHub

---

🙏 Cảm ơn

Cảm ơn cộng đồng lập trình đã truyền cảm hứng để tạo ra một ngôn ngữ mới, khác biệt.

---

Xây dựng với ❤️ và Python

```
