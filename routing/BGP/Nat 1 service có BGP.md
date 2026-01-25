# Bối cảnh thực tế

Giả sử:

Công ty không phải ISP, nhưng:
Thuê public IP block (/29 hoặc /24)
Thuê BGP service từ ISP
Công ty có:

FortiGate làm firewall
Router ISP ở phía ngoài
Frontend service (Web/App/API) chạy trong LAN/DMZ

📌 Mục tiêu:
Public truy cập service frontend
Công ty chủ động route, sẵn sàng cho:
Multi-ISP (sau này)
Failover
Traffic control

# Mô hình tổng thể (chuẩn doanh nghiệp nhỏ)
```

                 INTERNET
                     │
              ┌──────┴──────┐
              │   ISP AS    │
              │   AS 65000  │
              └──────┬──────┘
                     │ eBGP
        Public Transit│ 203.0.113.0/30
                     │
          ┌──────────┴──────────┐
          │     FortiGate FW    │
          │     AS 65010        │
          │                     │
          │ WAN: 203.0.113.2    │
          │ Public Block:       │
          │ 203.0.114.0/29      │
          └──────────┬──────────┘
                     │
                DMZ / LAN
                     │
        ┌────────────┴────────────┐
        │   Frontend Server        │
        │   203.0.114.2            │
        │   Web / API / App        │
        └─────────────────────────┘
```

# IP & AS plan (ví dụ)
Thành phần	            Giá trị
ISP ASN	                65000
Company ASN	            65010
Transit link	        203.0.113.0/30
FortiGate WAN	        203.0.113.2
ISP Router	            203.0.113.1
Public block	        203.0.114.0/29
Frontend server	        203.0.114.2

Public block được route, KHÔNG NAT

# Luồng traffic (rất quan trọng)
🔹 Inbound (Internet → Service)
Client → Internet → ISP
       → BGP thấy 203.0.114.0/29 thuộc AS 65010
       → chuyển về FortiGate
       → Frontend server

🔹 Outbound (Service → Internet)
Frontend → FortiGate
         → BGP default route từ ISP
         → Internet

👉 Không cần NAT, vì server dùng IP public thật

# BGP trên FortiGate (logic thực tế)
🔹 Mục tiêu BGP
✔ Advertise public block 203.0.114.0/29
✔ Nhận default route hoặc full route từ ISP