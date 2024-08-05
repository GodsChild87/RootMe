# Backup file

## Solution

- Nhiều trình soạn thảo văn bản trên Linux (vim, gedit, v.v...) tạo các tệp sao lưu trong khi đang chỉnh sửa tệp. Điều này có nghĩa là nếu đang chỉnh sửa `index.php`, `index.php~` sẽ được tạo. Trong các cấu hình PHP (và các cấu hình khác), đây là một lỗi rất nguy hiểm, vì bây giờ mọi người đều có thể đọc tệp ở dạng văn bản thuần túy.

- Đây là một lỗi đơn giản nhưng phổ biến thường bị quản trị viên hệ thống bỏ qua.