# Directory traversal

## Solution

- Truy cập Trang Challenge: http://challenge01.root-me.org/web-serveur/ch15/ch15.php

- Click vào Tab (ví dụ: Device)

- Trong thanh URL, thấy cách truy cập các Galarie: `?galerie=devices`

- Bây giờ sử dụng Directory Traversal:

  - Xóa `devices` sau `galarie=`
    
  - Bây giờ URL sẽ trông như thế này: http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=./
 
  - Truy cập URL

- Bây giờ có 6 Icon mới hiển thị trực tiếp trên trang

- Mở Source của Trang web

- Kiểm tra href cho từng Icon, sẽ thấy: http://challenge01.root-me.org/web-serveur/ch15/galerie/86hwnX2r

- Chèn tên mới vào URL gốc. Nó sẽ trông như thế này: http://challenge01.root-me.org/web-serveur/ch15/ch15.php?galerie=86hwnX2r

- Bây giờ có các icon mới khi load lại URL

- Mở lại Source

- Có một file có tên là `password.txt` với link: http://challenge01.root-me.org/web-serveur/ch15/galerie/86hwnX2r/password.txt

- Mở link: http://challenge01.root-me.org/web-serveur/ch15/galerie/86hwnX2r/password.txt và lấy password
