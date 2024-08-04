# HTTP - IP restriction bypass

## Solution

Trong thử thách này nói về cách bypass IP filtering bằng một kỹ thuật nổi tiếng là chèn header HTTP. Một số header cho phép chỉ định rằng địa chỉ IP của máy khách khác với địa chỉ IP nguồn trong gói tin mà máy chủ nhận được. Các header này đặc biệt hữu ích khi các kết nối đi qua load-balancer hoặc proxy. Tuy nhiên, cũng có thể sử dụng các header này để chỉ ra IP mà chúng ta muốn. Vì vậy có thể request với `curl` và header HTTP này.

```
curl 'http://challenge01.root-me.org/web-serveur/ch68/' --header 'X-Forwarded-For: 192.168.1.1'
```

Và có:

```
Well done validation password is : <strong>FLAGFLAGFLAGFLAGFLAGFLAG</strong>
```