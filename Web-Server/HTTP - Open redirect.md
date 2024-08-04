# HTTP - Open redirect

## Solution

Từ source code của trang, có thể đoán được tham số `h` là giá trị MD5 của địa chỉ được chuyển hướng, vì vậy hãy thay đổi các tham số thành

```
url=https://facebook1.com&h=984e5894c03dde98ebfb884abef05f8b
```

Sau đó có thể lấy được flag.