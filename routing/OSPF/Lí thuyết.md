# OSPF là gì? (cốt lõi)

OSPF (Open Shortest Path First) là:
Giao thức định tuyến nội bộ (IGP)
Dạng Link-State
Chuẩn mở (không vendor-lock)

👉 Mỗi router:
Biết toàn bộ topology của mạng
Tự tính đường đi ngắn nhất bằng SPF (Dijkstra)

# Vì sao OSPF mạnh hơn RIP?
RIP	                            OSPF
Distance Vector	                Link State
Metric = hop	                Metric = cost
Update toàn bảng	            Chỉ update khi thay đổi
Chậm	                        Rất nhanh

👉 OSPF hội tụ nhanh, phù hợp mạng lớn.

# Thành phần cốt lõi của OSPF (PHẢI NHỚ)
🔹 Router ID (RID)

ID duy nhất của router
Dạng IPv4 (32-bit)

Thứ tự chọn:
router-id cấu hình tay
IP cao nhất của loopback
IP cao nhất của interface active
📌 RID không đổi trừ khi restart OSPF process.

# Area

OSPF chia mạng thành area để scale tốt.
Area 0 = backbone (bắt buộc)
Các area khác phải kết nối về Area 0
```
Area 1 ----\
Area 2 ----- Area 0 ---- Area 3
```
👉 Không có Area 0 → OSPF lỗi thiết kế

4️⃣ Metric trong OSPF (Cost)

Cost = Reference Bandwidth / Interface Bandwidth

Mặc định:

Reference BW = 100 Mbps

Ví dụ:
100 Mbps → cost 1
10 Mbps → cost 10
📌 Thực tế phải chỉnh:
`auto-cost reference-bandwidth 100000`
(cho mạng Gigabit / 10G)

# LSA and LSDB
Trong định tuyến OSPF, LSA và LSDB là hai khái niệm cốt lõi giúp các router hiểu được sơ đồ mạng và tính toán đường đi ngắn nhất. Bạn có thể hình dung LSA là các "mảnh thông tin", còn LSDB là "cuốn sổ nhật ký" chứa tất cả các mảnh đó.

Dưới đây là chi tiết về từng khái niệm:

## LSA (Link State Advertisement)
LSA là các bản tin chứa thông tin về trạng thái liên kết của một router. Thay vì gửi toàn bộ bảng định tuyến, router OSPF chỉ gửi LSA để thông báo cho các láng giềng về những gì nó "nhìn thấy".

Nội dung: LSA chứa thông tin về các interface, IP, subnet mask, chi phí (cost) của đường truyền và các router láng giềng kết nối trực tiếp.

Cơ chế: Khi có một thay đổi trong mạng (ví dụ: một đường truyền bị đứt), router sẽ tạo ra một LSA mới và lan tỏa (flooding) nó đến tất cả các router khác trong cùng một vùng (Area).

Các loại LSA phổ biến: 
```
Type 1 (Router LSA): Do mọi router tạo ra để mô tả chính nó.
Type 2 (Network LSA): Do Designated Router (DR) tạo ra trong mạng đa truy cập (như Ethernet).
Type 3 (Summary LSA): Do Area Border Router (ABR) tạo ra để quảng bá thông tin giữa các Area.
```
## LSDB (Link State Database)
LSDB là một cơ sở dữ liệu tập hợp tất cả các LSA mà một router nhận được từ toàn bộ mạng (hoặc trong cùng một Area).

Tính đồng nhất: Trong cùng một vùng (Area), tất cả các router OSPF bắt buộc phải có LSDB hoàn toàn giống hệt nhau. Điều này đảm bảo mọi router đều có cùng một cái nhìn về sơ đồ mạng (topology).

Vai trò: LSDB giống như một bản đồ chi tiết của toàn vùng. Router sẽ dùng dữ liệu từ LSDB này để chạy thuật toán SPF (Shortest Path First) hay còn gọi là thuật toán Dijkstra.

## Mối quan hệ giữa LSA, LSDB và Bảng định tuyến
Quy trình hoạt động của OSPF diễn ra theo các bước sau:

Thu thập LSA: Các router trao đổi LSA với nhau.
Xây dựng LSDB: Mỗi router lưu trữ các LSA nhận được vào bộ nhớ, hình thành nên LSDB.
Tính toán SPF: Router chạy thuật toán Dijkstra lên LSDB để tìm ra đường đi ngắn nhất đến từng đích đến.
Hình thành Routing Table: Kết quả tốt nhất từ thuật toán SPF sẽ được nạp vào Bảng định tuyến (Routing Table) để chuyển tiếp dữ liệu thực tế.

# OSPF hoạt động như thế nào? (FLOW)
## Bước 1: Discover neighbor

Gửi Hello
Kiểm tra:
```
+ Area ID
+ Hello/Dead timer
+ Authentication
+ Network type
```
## Bước 2: Form adjacency

Trạng thái:

`Down → Init → 2-Way → ExStart → Exchange → Loading → Full`

## Bước 3: Exchange LSDB

Trao đổi LSA
Mỗi router có LSDB giống nhau

## Bước 4: SPF Calculation
Chạy thuật toán Dijkstra
Sinh routing table

