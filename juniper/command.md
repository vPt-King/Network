🔹 1. Vào chế độ cấu hình
```
cli
configure
```
🔹 2. Kiểm tra cấu hình
```
show configuration
show configuration | display set       # Hiển thị dạng lệnh set
```

🔹 3. Lưu cấu hình
```
commit
commit and-quit
```

🔹 4. Cấu hình VLAN
```
Tạo VLAN:
set vlans VLAN10 vlan-id 10

Gán interface vào VLAN (access):
set interfaces ge-0/0/1 unit 0 family ethernet-switching port-mode access
set interfaces ge-0/0/1 unit 0 family ethernet-switching vlan members VLAN10
```

🔹 5. Cấu hình Trunk (Tagged port)
```
set interfaces ge-0/0/2 unit 0 family ethernet-switching port-mode trunk
set interfaces ge-0/0/2 unit 0 family ethernet-switching vlan members [ VLAN10 VLAN20 VLAN30 ]
```

🔹 6. Cấu hình Native VLAN (tương đương Cisco native / untagged)
```
set interfaces ge-0/0/2 unit 0 family ethernet-switching native-vlan-id 10
```

🔹 7. Cấu hình LACP (Link Aggregation / Trunk port)
```
Tạo LACP:
set interfaces ae0 aggregated-ether-options lacp active

Thêm port vào LAG:
set interfaces ge-0/0/3 ether-options 802.3ad ae0
set interfaces ge-0/0/4 ether-options 802.3ad ae0

ae0 trunk VLAN:
set interfaces ae0 unit 0 family ethernet-switching port-mode trunk
set interfaces ae0 unit 0 family ethernet-switching vlan members [ VLAN10 VLAN20 ]
```

🔹 8. Cấu hình STP

```
Juniper dùng RSTP mặc định.

Bật RSTP:
set protocols rstp

Set priority (root switch):
set protocols rstp bridge-priority 4096

Disable STP trên port:
set protocols rstp interface ge-0/0/5 disable
```

🔹 9. Cấu hình quản lý IP cho switch (out-of-band hoặc in-band)
```
Có thể đặt IP trên vlan.0 hoặc irb.0.

Ví dụ IP cho VLAN 10:
set vlans VLAN10 l3-interface irb.10
set interfaces irb unit 10 family inet address 192.168.10.1/24
```

🔹 10. Cấu hình Default Gateway
```
set routing-options static route 0.0.0.0/0 next-hop 192.168.10.254
```

🔹 11. Cấu hình SSH / Web / HTTPS
```
Bật SSH:
set system services ssh

Tắt Telnet:
delete system services telnet

Thiết lập user:
set system login user admin class super-user authentication plain-text-password
```

🔹 12. Cấu hình SNMP
```
set snmp community public authorization read-only
set snmp location "Server Room"
set snmp contact "Admin"
```

🔹 13. Cấu hình DHCP Snooping
```
set ethernet-switching-options secure-access-port interface ge-0/0/1
set ethernet-switching-options secure-access-port vlan VLAN10
set ethernet-switching-options dhcp-snooping vlan VLAN10
set ethernet-switching-options dhcp-snooping trust interface ge-0/0/2  # Uplink
```


🔹 14. Chặn BPDU trên access port (BPDU protection)
`set ethernet-switching-options bpdu-block interface ge-0/0/1`

🔹 15. Xóa cấu hình
Xóa một dòng:
```
delete interfaces ge-0/0/1

Xóa toàn bộ cấu hình:
load factory-default
commit
```

🔹 16. Reboot switch
`request system reboot`