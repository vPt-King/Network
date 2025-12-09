VPN (Virtual Private Network) là mạng riêng ảo, một công nghệ cho phép bạn tạo kết nối an toàn, mã hóa giữa thiết bị của bạn và một mạng khác thông qua Internet.

Nói đơn giản:

VPN tạo một “đường hầm bảo mật” trên Internet để bạn truy cập mạng từ xa như đang ngồi trực tiếp trong mạng đó.

🔐 1. VPN dùng để làm gì?
✔ 1. Kết nối từ xa vào mạng công ty

Ví dụ: bạn ở nhà nhưng vẫn truy cập được:

File server

ERP / CRM

Camera

Database

Các ứng dụng nội bộ

→ Như đang ngồi trực tiếp trong công ty.

✔ 2. Kết nối hai chi nhánh (site-to-site)

Giúp các chi nhánh giao tiếp qua một tunnel bảo mật.

✔ 3. Ẩn IP, tăng bảo mật

VPN làm cho hacker không thấy IP thật của bạn.

✔ 4. Truy cập web an toàn

Mọi dữ liệu được mã hóa → tránh bị nghe lén trên wifi công cộng.

🔒 2. VPN hoạt động như thế nào?

Thiết bị của bạn kết nối đến VPN server

Hai bên trao đổi key để tạo kênh mã hóa

Từ đó về sau:

Dữ liệu ra/vào đều đi trong đường hầm VPN

Người bên ngoài không đọc được

🔀 3. Các loại VPN phổ biến
1. VPN Client-to-Site (Remote Access VPN)

Người dùng sử dụng VPN client để vào mạng công ty.
Ví dụ:

FortiClient

AnyConnect

OpenVPN

2. VPN Site-to-Site

Kết nối 2 chi nhánh lại với nhau qua tunnel VPN.

🔐 4. Các giao thức VPN phổ biến
⭐ 1. IPsec VPN

Bảo mật rất cao

Thường dùng để nối site-to-site

Chạy ở Layer 3

⭐ 2. SSL/TLS VPN

Dùng cho người dùng remote

Dễ triển khai

Chạy qua port 443 nên ít bị chặn (như Fortinet SSL VPN)

⭐ 3. OpenVPN

Mã nguồn mở

Bảo mật tốt

Xài nhiều cho cá nhân

⭐ 4. WireGuard

Hiệu năng cao

Cấu hình đơn giản