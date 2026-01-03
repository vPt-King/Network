lab thực tế + dễ hiểu, gồm:
FortiGate Firewall (AS 65001)
Cisco Router (AS 65002)
Kết nối eBGP trực tiếp

# Mô hình mạng (simple eBGP)

        │   FortiGate FW   │
        │   AS 65001       │
        │  LAN: 10.1.1.0/24│
        │  WAN: 192.168.1.1│
        └─────────┬────────┘
                  │ eBGP
          192.168.1.0/30
                  │
        ┌─────────┴────────┐
        │  Cisco Router    │
        │   AS 65002       │
        │  LAN: 10.2.2.0/24│
        │  WAN: 192.168.1.2│
        └──────────────────┘
IP plan
Thiết bị	        Interface	            IP
FortiGate	        port1	                192.168.1.1/30
Cisco	            G0/0	                192.168.1.2/30

# Mục tiêu lab
✔ FortiGate quảng bá mạng 10.1.1.0/24
✔ Cisco quảng bá mạng 10.2.2.0/24
✔ Hai bên học route của nhau qua eBGP

# Cấu hình BGP trên FortiGate
```
config router bgp
    set as 65001    Khai báo AS number của FortiGate

Khai báo neighbor (Cisco Router)
config neighbor
        edit "192.168.1.2"                  IP peer BGP (router Cisco)
            set remote-as 65002             AS của neighbor → khác AS ⇒ eBGP              
        next
    end


Quảng bá network LAN
→ BGP chỉ quảng bá route có trong routing table
→ LAN này phải tồn tại (static/connected)

📌 Lỗi hay gặp: network không có trong routing table → không advertise
```

# Cấu hình BGP trên Cisco Router

```
Cấu hình IP interface
interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.252
 no shutdown


Bước 2: Khởi tạo BGP process
router bgp 65002

Bước 3: Khai báo neighbor FortiGate
neighbor 192.168.1.1 remote-as 65001
👉 Nếu remote-as khác → eBGP
👉 Nếu remote-as giống → iBGP

Bước 4: Quảng bá mạng LAN Cisco
network 10.2.2.0 mask 255.255.255.0
``` 

# Kiểm tra lại trạng thái BGP
```
🔹 Trên FortiGate
get router info bgp summary
Kết quả mong muốn:
State: Established

🔹 Trên Cisco
show ip bgp summary
Dòng quan trọng:
State/PfxRcd: 1
→ Đã nhận được 1 prefix từ FortiGate
```

# Kiểm tra route học được
Cisco:
show ip route bgp


Sẽ thấy:

B    10.1.1.0/24 [20/0] via 192.168.1.1

FortiGate:
get router info routing-table bgp


Sẽ thấy:

10.2.2.0/24 via 192.168.1.2




