\# Kiểm tra IPsec Phase 2 selector

`diagnose vpn tunnel list`

Kiểm tra:



proxy-id có đúng subnet DMZ ↔ DC không?

có bị lệch subnet mask không?

có overlap không?

👉 Nếu selector sai, TCP handshake có thể lên nhưng BGP packet sau đó bị drop vì sai encryption domain.



\# Kiểm tra BGP state chi tiết

`get router info bgp summary`

Nếu thấy State = Active

thì xem tiếp 

```

diagnose ip router bgp all enable

diagnose debug enable

```

Quan sát log:

notification?

bad peer AS?

hold timer expired?

connection rejected?



Sau đó tắt debug:

`diagnose debug disable`





\# MTU

MTU (Maximum Transmission Unit) = kích thước tối đa (tính bằng byte) của một packet có thể truyền đi trên một đường mạng mà không bị chia nhỏ (fragment).



\# MTU liên quan gì tới IPsec?



Khi dùng IPsec:

Gói tin gốc 1500 bytes

IPsec thêm header mã hóa (~50–70 bytes)

=> Kích thước thực tế > 1500

=> Vượt MTU vật lý

=> Bị fragment hoặc drop



\# Vì sao MTU gây lỗi BGP của bạn?

TCP SYN nhỏ → đi được

BGP OPEN lớn hơn → vượt MTU tunnel

DF-bit bật → không được chia nhỏ

Firewall đầu kia drop

Session stuck ở Active



Mỗi đường truyền mạng có một giới hạn kích thước gói tin tối đa gọi là MTU (Maximum Transmission Unit), thông thường là 1500 bytes.



Nếu DF=0 (không bật): Nếu gói tin lớn hơn MTU của cổng ra, Fortigate sẽ tự động chia nhỏ gói tin đó thành các mảnh (fragmentation) để gửi đi.



Nếu DF=1 (có bật): Nếu gói tin lớn hơn MTU, Fortigate sẽ hủy bỏ (drop) gói tin đó và gửi ngược lại một thông báo lỗi ICMP "Fragmentation Needed" cho nguồn gửi.



\# MTU vs MSS khác nhau gì?

MTU = giới hạn toàn bộ packet

MSS (Maximum Segment Size) = kích thước phần TCP payload



👉 MSS thường = MTU - 40 byte (IP + TCP header)



Ví dụ:



MTU 1500



MSS ≈ 1460



\# config: 

```

config system interface

edit <tunnel-interface-name>

set mtu-override enable

set mtu 1400

next

end

```

mss

```

config firewall policy

edit <policy-id>

set tcp-mss-sender 1350

set tcp-mss-receiver 1350

next

end

```



\# kiểm tra hiện tại :

get system interface <tunnel-interface-name>


16h đúng 6/2, FGT-1000F bổng mất kết nối IBGP chạy trên nền S2S IPsec với Palo và Checkpoint ở DC, mọi giao tiếp với 2 vùng DMZ server mất liền lạc.
Kiểm tra bước bắt tay số 2 Connect IBGP vẫn thông TCP/179 vẫn chạy qua được nhưng bước số 3 Active lại failed (mismatch, gói tin cứ lên là bị đầu palo và checkpoint droped, phiên ko chuyển bước bắt tay được.
Thực hiện các thao tác dig debug lên quan đến bgp ở fgt ko có gì bất thường, ngồi debug 30p éo ra được, trong khi user đúc vào đít do toàn bộ system đang rớt, con FGT phụ trách cho HQ đến 2 zone server,  1 nhánh vậy chạy 2 tunnel, tổng 4 tunnel vậy mà rớt sạch. Cuối cùng nghĩ đến debug mss và mtu trên interface tunnel. 
FGT
execute ping-options size 1472
execute ping-options df-bit enable
execute ping <peer-ip>
- Kết quả failed - thấy là ko ổn rồi
Palo 
Ping host <peer-ip> size 1472 df-bit yes
- ok
Checkpoint 
ping -M do -s 1472 <peer-ip>
ok
Nghĩa là đầu palo và checkpoint gói mtu và msss ổn
Quyết định chỉnh mtu ở tunnel và giảm mss ở các rule s2s trên FGT
Mtu tunnel chỉnh về 1400
TCP mss chỉnh về 1350
Kết quả bước 3 active trong IBGP bắt tay lại ngay, chuyển trạng thái qua bước open sent -> open confirm -> established ngon lành trở lại ngay...debug mất 45p downtime toàn hệ thống

