# Javascript - Native code

## Solution

- Sử dụng console trong firefox, mã sẽ hiển thị ở dạng có thể đọc được.

- Chỉ cần bỏ dấu () cuối cùng và thêm `toString()`: `[...].toString()`

```
function anonymous() {
a=prompt('Entrez le mot de passe');if(a=='toto123lol'){alert('bravo');}else{alert('fail...');}
```