# Show configuration
```
interfaces {
    ge-0/0/0 {
        description "Ket noi den Core Switch";
        speed 1g;
        unit 0 {
            family ethernet-switching {
                interface-mode access;
                vlan {
                    members vlan-10;
                }
            }
        }
    }
}
```
A. Tên Interface (Ví dụ: ge-0/0/0)
ge: Loại interface (Gigabit Ethernet). Các loại khác: fe (Fast Ethernet), xe (10Gbps), et (40/100Gbps).

0/0/0: Thứ tự Slot/Module/Port. Trong EVE-NG, thường là ge-0/0/0, ge-0/0/1,...

B. Physical Properties (Lớp vật lý)
Nằm ngay dưới tên interface, dùng để định nghĩa các thông số vật lý:

description: Ghi chú mục đích của cổng (giúp quản trị viên dễ nhận biết).

speed / link-mode: Ép tốc độ hoặc chế độ duplex (full/half).

disable: Vô hiệu hóa cổng (tương đương shutdown bên Cisco).

C. Logical Interface (Unit)
Đây là điểm khác biệt lớn nhất của Juniper.

unit 0: Juniper chia một cổng vật lý thành nhiều cổng logic.

Đối với switch (Layer 2), thường chỉ dùng unit 0.

Đối với Router hoặc cổng Trunk (Layer 3), bạn có thể chia unit 10, unit 20 tương ứng với các sub-interface (VLAN).

D. Family (Gia đình giao thức)
Dùng để xác định interface này chạy ở lớp nào:

family ethernet-switching: Dùng cho Layer 2 (Switching). Đây là cấu hình bạn sẽ thấy nhiều nhất trên Switch.

interface-mode access: Cổng nối vào máy tính/thiết bị cuối.

interface-mode trunk: Cổng kết nối giữa các Switch để truyền nhiều VLAN.

vlan members: Xác định VLAN nào được phép đi qua cổng này.

family inet: Dùng cho Layer 3 (IPv4). Dùng khi bạn muốn đặt IP trực tiếp lên cổng.

Ví dụ: address 192.168.1.1/24.

family inet6: Dùng cho IPv6.


```
Nếu bạn thấy cách hiển thị phân tầng (dấu ngoặc nhọn { }) khó đọc, hãy dùng lệnh sau: show configuration interfaces | display set

Nó sẽ hiển thị dưới dạng các câu lệnh set đơn dòng, giúp bạn dễ dàng copy-paste hoặc đối chiếu với các hướng dẫn trên mạng.
```

# show interface
Hiển thị các interface