🟦 1. Định nghĩa vị trí NAT (bắt buộc)

Trước khi NAT phải khai báo interface nào là inside/outside:

interface g0/0
 ip nat inside

interface g0/1
 ip nat outside

🟩 2. Static NAT – ánh xạ 1:1
Cú pháp:
ip nat inside source static <inside-local> <inside-global>

Ví dụ:

Server LAN 192.168.10.10 → IP public 100.1.1.10

ip nat inside source static 192.168.10.10 100.1.1.10


➡ Cho phép từ ngoài internet truy cập vào server

🟧 3. Dynamic NAT – ánh xạ theo pool IP
Tạo pool IP public
ip nat pool MYPOOL 100.1.1.20 100.1.1.30 netmask 255.255.255.0

Tạo ACL xác định dải IP cần NAT
access-list 10 permit 192.168.10.0 0.0.0.255

Gán ACL + pool vào NAT
ip nat inside source list 10 pool MYPOOL

🟥 4. NAT Overload (PAT) – NAT ra 1 IP (phổ biến nhất)
ACL xác định range NAT được
access-list 1 permit 192.168.10.0 0.0.0.255

Cấu hình NAT Overload
ip nat inside source list 1 interface g0/1 overload


➡ Tất cả thiết bị LAN dùng 1 IP public nhưng phân biệt bằng port.

🟪 5. Static PAT – ánh xạ 1 port vào 1 port
Cú pháp
ip nat inside source static tcp <inside-ip> <inside-port> <public-ip> <public-port>

Ví dụ: NAT port 80 của web server
ip nat inside source static tcp 192.168.10.10 80 100.1.1.10 80

🟫 6. Policy NAT (NAT theo điều kiện – nâng cao)
Ví dụ NAT chỉ khi đi ra Internet:
route-map NAT-CONDITION permit 10
 match ip address 10

ip nat inside source route-map NAT-CONDITION interface g0/1 overload

🟨 7. Clear NAT
clear ip nat translation *


Nếu muốn xóa NAT cụ thể:

clear ip nat translation tcp <inside-ip> <inside-port> <outside-ip> <outside-port>

🟧 8. Kiểm tra NAT
show ip nat translations
show ip nat statistics
show running-config | include nat

🟦 9. NAT66 / NAT64 (Cisco IPv6 – CCNP)
NAT 6 → 6
ipv6 nat
ipv6 nat prefix 2001:db8:100::/48

NAT 6 → 4
ipv6 nat64 prefix 64:ff9b::/96


(Chủ yếu CCNP, CCIE dùng.)

📌 10. TÓM TẮT LỆNH NGẮN GỌN
Đánh dấu interface
ip nat inside
ip nat outside

NAT tĩnh
ip nat inside source static <local> <global>

NAT port (Static PAT)
ip nat inside source static tcp <local-ip> <port> <public-ip> <port>

Dynamic NAT
ip nat pool X A.B.C.D A.B.C.E netmask 255.255.255.0
access-list 10 permit <LAN-subnet>
ip nat inside source list 10 pool X

NAT Overload (PAT)
access-list 1 permit <LAN-subnet>
ip nat inside source list 1 interface g0/1 overload