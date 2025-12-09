SSL (Secure Sockets Layer) là một giao thức bảo mật dùng để mã hóa dữ liệu truyền giữa client và server, nhằm bảo vệ thông tin khỏi bị nghe lén, sửa đổi hoặc giả mạo.

⚠️ Lưu ý: SSL đã lỗi thời và được thay thế bởi TLS (Transport Layer Security), nhưng người ta vẫn quen gọi chung là “SSL”.

🔐 1. SSL dùng để làm gì?

Khi bạn truy cập một website có https://, nghĩa là dữ liệu của bạn được mã hóa bởi SSL/TLS.

SSL giúp bảo vệ:

Username / Password

Số thẻ ngân hàng

Cookie / Session

File upload

Dữ liệu API

🔐 2. SSL hoạt động như thế nào? (hiểu nhanh)

SSL dùng certificate (chứng chỉ số) để:

1️⃣ Xác thực server
→ trình duyệt biết bạn kết nối đúng server thật, không phải server giả.

2️⃣ Tạo khóa bí mật giữa client và server
→ dữ liệu được mã hóa khi gửi.

3️⃣ Truyền dữ liệu đã mã hóa, người ngoài không đọc được.

🔐 3. SSL certificate là gì?

SSL certificate (chứng chỉ SSL) chứa:

Tên miền (domain)

Public key

Tên tổ chức / công ty (tùy loại)

Ngày hết hạn

Chữ ký số của CA (Certificate Authority)

Dạng file thường là:

.crt

.cer

.pem

.pfx

🔐 4. Các loại SSL certificate
Loại	Đặc điểm	Mức độ tin cậy
DV (Domain Validation)	Chỉ xác minh tên miền	Thấp
OV (Organization Validation)	Xác minh tên công ty	Trung bình
EV (Extended Validation)	Kiểm tra toàn diện	Cao
🔐 5. Ứng dụng trong thực tế
✔ Website HTTPS

→ WordPress, PHP, .NET, NodeJS, v.v.

✔ VPN SSL trên Fortinet / Cisco

→ dùng SSL tunnel để truy cập mạng nội bộ.

✔ API bảo mật

→ Mobile App ↔ Backend

✔ Email bảo mật

→ SMTPS, IMAPS, POP3S

🔐 6. SSL so với TLS
Giao thức	Trạng thái
SSL 2.0	Bị cấm
SSL 3.0	Bị cấm
TLS 1.0 / 1.1	Không khuyến nghị
TLS 1.2 / 1.3	Chuẩn hiện tại

Ngày nay, khi bạn nói “SSL VPN” thì thực chất là TLS VPN.

🔐 7. Giải thích cực ngắn gọn dễ nhớ

SSL = công nghệ mã hóa giúp giao tiếp an toàn

HTTPS = website dùng SSL/TLS

SSL VPN = VPN dùng SSL/TLS để mã hóa