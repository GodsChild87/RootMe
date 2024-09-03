# PHP - register globals

# Solution

Khi đã xác định được file backup là http://challenge01.root-me.org//web-serveur/ch17/index.php.bak, mọi thứ trở nên dễ dàng hơn:

Sau đây là phần quan trọng của file này:

```
...
function auth($password, $hidden_password){
    $res=0;
    if (isset($password) && $password!=""){
    if ( $password == $hidden_password ){
        $res=1;
    }
    }
    $_SESSION["logged"]=$res;
    return $res;
}
...
if (( isset ($password) && $password!="" && auth($password,$hidden_password)==1) || $_SESSION["logged"]==1){
    $aff=display("well done, you can validate with the password : $hidden_password");
...
```

Bây giờ có hai cách để giải quyết challenge này:

Có thể sử dụng thực tế là `register_globals` thực sự được bật và lấy password chỉ bằng cách truy cập vào url này http://.../ch17/?_SESSION[logged]=1

Hoặc trước tiên có thể truy cập vào url này http://.../ch17/?password=foo&hidden_password=foo, để biến session logged ($_SESSION['logged']) được đặt thành 1 trong hàm `auth()`. Sau đó chỉ cần truy cập lại challenge mà không có bất kỳ tham số GET nào và nó sẽ in ra password.

Lưu ý: đối với cách thứ 2, nếu đang thực hiện việc này trên command line, hãy đảm bảo cũng gửi cookie đã nhận được ở bước đầu tiên!

Trong cả hai trường hợp, đây là những gì nhận được ở cuối:

```
well done, you can validate with the password : NoiQYdpcd5kgNwG
```