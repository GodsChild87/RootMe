# Javascript - Obfuscation 2

## Solution

- Mở mã nguồn javascript.

- Thấy một biến có tên là `pass` được gán kết quả của một hàm. Nhìn kỹ hơn, nó sử dụng `fromCharCode` bên trong chuỗi. Tuy nhiên, nếu giải mã hóa URL sau hàm này, sẽ thấy chúng là số chứ không phải ký tự.

- Sử dụng javascript console để tìm mật khẩu.