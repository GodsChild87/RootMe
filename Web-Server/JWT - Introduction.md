# JWT - Introduction

## Solution

- Ném JWT từ đăng nhập guest vào https://jwt.io/

- Lấy phần đầu tiên được giải mã ở định dạng json và thay đổi `alg` thành `none` sau đó encode bằng https://www.base64encode.org/

- Trên jwt.io, thay đổi phần đầu tiên của hàm băm (trước `.`) thành phần mới

- Trên jwt, thay đổi payload `guest` -> `admin`

- Lấy JWT mới và thay đổi giá trị cookie

- Refresh trang
