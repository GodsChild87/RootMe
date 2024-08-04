# Javascript - Obfuscation 1

## Solution

- Đầu tiên hãy xem mã nguồn của trang và tìm giá trị của biến `pass`

- Sau đó sử dụng hàm unescape của javascript để tìm mật khẩu bằng bất kỳ công cụ trực tuyến nào.

```
var pass='%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64'
document.write(unescape(pass) + "<br>")
```