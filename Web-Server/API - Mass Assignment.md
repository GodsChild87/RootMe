# API - Mass Assignment

## Solution

Trong thử thách này, cần có quyền quản trị để truy cập nội dung của /api/flag.

- Như tên của thử thách "API - Mass Assignment" ám chỉ một loại lỗ hổng.

- Kiểm tra các phương thức HTTP được phép trên mỗi endpoint bằng BurpSuite:

    - OPTIONS /api/signup

    - OPTIONS /api/login

    - OPTIONS /api/user

    - OPTIONS /api/note

    - OPTIONS /api/flag

    Phát hiện ra rằng /api/user đã bật phương thức 'PUT'.

- Tạo một người dùng có tên người dùng = 'u5' từ /api/signup

- Đăng nhập bằng người dùng mới tạo và phản hồi của máy chủ trông như thế này

```
{"note":"",
"status":"guest",
"userid":77,
"username":"u5"}
```

- Ở đây có thể thấy rằng 'status' là 'guest', phải đổi thành 'admin'.

- Bây giờ, kiểm tra rằng /api/user này có cho phép phương thức 'PUT' hay không và phương thức PUT được sử dụng để cập nhật nội dung web/máy chủ.

- Tạo yêu cầu:

-> thay đổi phương thức GET thành PUT

-> thêm tiêu đề 'Content-Type: application/json' # tiêu đề này được sử dụng để khiến máy chủ chấp nhận nội dung json bằng cách cho máy chủ biết rằng dữ liệu được gửi là json

-> cuối cùng thêm nội dung chính ở định dạng json như sau:
```
{
"status":"admin"
}
```

- Phản hồi từ máy chủ:

```
{"message":"User updated sucessfully."}
```

- Bây giờ, thực hiện yêu cầu GET tới /api/flag với cookie người dùng đã cập nhật trong request.

- Đã nhận được flag, challenge đã được giải quyết.