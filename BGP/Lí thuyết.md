# Định nghĩa
BGP (Border Gateway Protocol) là giao thức định tuyến liên miền (inter-domain routing), dùng để:
Trao đổi thông tin định tuyến giữa các Autonomous System (AS)
Là giao thức xương sống của Internet
👉 Internet hoạt động được là nhờ BGP.

# Autonomous System (AS) là gì?
AS = tập hợp các router thuộc một tổ chức, dùng chính sách định tuyến chung
Mỗi AS có AS Number (ASN) duy nhất
Ví dụ:
AS 15169 → Google
AS 13335 → Cloudflare
ISP VNPT, Viettel mỗi bên là 1 AS

# BGP hoạt động như thế nào?
BGP không chọn đường đi ngắn nhất, mà chọn đường đi theo chính sách (policy-based routing).
🔹 Router BGP trao đổi:
Network prefix (VD: 8.8.8.0/24)
AS_PATH (danh sách AS đã đi qua)
📌 BGP dùng TCP port 179 (không dùng UDP như RIP/OSPF)

# Phân loại BGP
🔹 eBGP (External BGP)
Giữa 2 AS khác nhau
Ví dụ: ISP ↔ Google

🔹 iBGP (Internal BGP)
Bên trong cùng 1 AS
Dùng để lan truyền route học từ eBGP
📌 iBGP KHÔNG tự động full-mesh → cần:
Route Reflector
Confederation

# BGP là Path Vector Protocol
Khác với:
RIP → Distance Vector
OSPF → Link State
BGP lưu:
AS_PATH → tránh loop
Không cần metric như cost/hop

# Các thuộc tính (Attributes) quan trọng của BGP
BGP chọn route dựa vào attributes, không phải khoảng cách.

🔥 Thứ tự ưu tiên (rút gọn – hay hỏi thi)

1️⃣ Highest Weight (Cisco, local)
2️⃣ Highest Local Preference
3️⃣ Shortest AS_PATH
4️⃣ Lowest Origin type (IGP < EGP < Incomplete)
5️⃣ Lowest MED
6️⃣ eBGP > iBGP
7️⃣ Lowest IGP cost đến next-hop
8️⃣ Oldest path

👉 Weight & Local Preference thường dùng để điều khiển đường đi.

# BGP KHÔNG tự động quảng bá route
Muốn BGP hoạt động phải:
Có neighbor
Có network statement hoặc redistribute
Có policy (route-map / prefix-list)
📌 BGP rất “khó tính” nhưng cực kỳ kiểm soát được traffic

# Ưu điểm & nhược điểm của BGP
✅ Ưu điểm

✔ Rất ổn định, không loop
✔ Điều khiển traffic cực mạnh
✔ Chạy được trên Internet toàn cầu

❌ Nhược điểm

✖ Cấu hình phức tạp
✖ Hội tụ chậm
✖ Không phù hợp mạng nhỏ
# Khi nào nên dùng BGP
Dùng khi:
Có nhiều ISP (multi-homing)
Có AS riêng
Cần traffic engineering
Kết nối Internet quy mô lớn

❌ Không dùng cho mạng LAN nhỏ