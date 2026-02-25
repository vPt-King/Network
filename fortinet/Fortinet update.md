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

