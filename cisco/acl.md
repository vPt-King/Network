1. Kiểm tra ACL
show access-lists
show ip access-lists
show running-config | include access-list

2. Tạo ACL dạng Standard (ACL số 1–99 hoặc 1300–1999)
➤ ACL Standard (dùng filter theo source IP)
Cú pháp
access-list <1-99> permit <source> <wildcard>
access-list <1-99> deny   <source> <wildcard>

Ví dụ
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any

3. Tạo ACL dạng Extended (ACL số 100–199 hoặc 2000–2699)

ACL Extended filter theo:

Source IP

Destination IP

Protocol (tcp, udp, icmp)

Port

Cú pháp
access-list <100-199> permit|deny <protocol> <src-ip> <src-wc> <dst-ip> <dst-wc> [eq <port>]

Ví dụ

Chặn HTTP từ mạng 192.168.10.0 → 192.168.20.0:

access-list 101 deny tcp 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 eq 80
access-list 101 permit ip any any


Chỉ cho phép ping:

access-list 102 permit icmp any any echo

4. ACL đặt tên (Named ACL)
Standard Named ACL
ip access-list standard ACL_NAME
 permit 192.168.1.0 0.0.0.255
 deny any

Extended Named ACL
ip access-list extended FIREWALL
 deny tcp any any eq 23
 permit ip any any
5. Áp dụng ACL vào Interface
Standard ACL → inbound/outbound?

Standard ACL nên đặt gần đích

Chỉ filter source IP

Cú pháp:
interface gi0/0
 ip access-group 10 in

Extended ACL → inbound/outbound?

Extended ACL nên đặt gần source

Lọc theo port + protocol

Ví dụ:
interface gi0/1
 ip access-group 101 out

6. ACL với VTY (SSH/Telnet)
access-list 15 permit 192.168.10.10
line vty 0 4
 access-class 15 in

7. ACL cho NAT
Inside NAT overload ví dụ:
access-list 1 permit 192.168.10.0 0.0.0.255

ip nat inside source list 1 interface g0/0 overload

8. ACL với OSPF

Muốn chỉ những mạng nào được quảng bá với OSPF:

access-list 50 permit 10.10.10.0 0.0.0.255
router ospf 1
 distribute-list 50 in

9. Wildcard mask ghi nhớ nhanh

Wildcard mask = đảo bit của netmask
Ví dụ:

/24 → 0.0.0.255

/16 → 0.0.255.255

Chỉ 1 host → 0.0.0.0

10. Một số ví dụ ACL thực tế
Chặn Facebook:
access-list 110 deny tcp any any eq 443


(Thực tế phải dùng L7 mới đúng, nhưng đây là ví dụ)

Chặn tất cả trừ một IP:
access-list 20 permit host 192.168.10.100
access-list 20 deny any

Chặn port FTP outbound:
access-list 101 deny tcp any any eq 21
access-list 101 permit ip any any

Chặn ping từ LAN ra ngoài:
access-list 105 deny icmp 192.168.10.0 0.0.0.255 any echo