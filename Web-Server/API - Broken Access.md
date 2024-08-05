# API - Broken Access

## Solution

- Hãy xem các tham số, API và kiểm tra nó như một người dùng đơn giản.

- Khi truy cập `/api/user/{user_id}` và cố gắng truy xuất một người dùng có `user_id` khác, có điều gì đó không hoạt động!

- Chỉ cần sửa request và đặt `user_id` là `1`

```
GET /api/user/1
```