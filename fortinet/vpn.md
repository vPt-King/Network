# IPsec là gì
IPsec là bộ giao thức bảo mật ở tầng mạng (Layer 3) dùng để:
Mã hóa dữ liệu (Encryption)
Xác thực nguồn (Authentication)
Đảm bảo dữ liệu không bị thay đổi (Integrity)
Ngăn chặn tấn công replay
Nó tạo ra VPN tunnel giữa hai thiết bị (firewall, router, VPN gateway).

## ESP
Đây là phần quan trọng nhất, dùng trong mọi VPN IPsec.
ESP xử lý:
+ Mã hóa dữ liệu
+ Xác thực
+ Chống sửa đổi
+ Chống replay
+ Tạo tunnel VPN
→ Site-to-Site VPN hiện nay gần như chỉ dùng ESP.

## Chế dộ hoạt động IPsec
1) Transport mode
Chỉ mã hóa payload
Header IP gốc vẫn giữ nguyên
→ Ít dùng, chỉ dùng trong server-to-server nội bộ.

2) Tunnel mode (phổ biến nhất)
Mã hóa toàn bộ packet
Bọc packet gốc trong packet mới (over IPsec tunnel)
→ Dùng cho Site-to-Site VPN giữa 2 mạng LAN.

## IPsec hoạt động như nào
IPsec xây dựng tunnel qua 2 pha:
Phase 1 – IKE (Internet Key Exchange)
Dùng để:
Xác thực 2 site
Tạo kênh bảo mật IKE SA
Sinh khóa mã hóa
Có 2 phiên bản:
IKEv1
IKEv2 (mới hơn, nhanh hơn, bảo mật hơn)

Phase 2 – IPsec SA (ESP)
Dùng để:
Tạo VPN tunnel thực tế
Trao đổi traffic đã mã hóa giữa hai mạng LAN