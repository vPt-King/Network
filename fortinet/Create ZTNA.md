1) Tạo Posture Check & Posture Tag trên FortiClient EMS (step-by-step)

Đăng nhập vào FortiClient EMS (web UI).

Vào Endpoint Profiles (hoặc Profiles / Posture tùy version).

Tạo một Posture Profile / Posture Check mới:

New → đặt tên (ví dụ: Posture_A_Check).

Thêm các rule posture bạn cần, ví dụ:

OS version / OS family (Windows 10/11)

Antivirus must be ON (Windows Defender / 3rd party)

Firewall enabled

FortiClient version >= X

Disk encryption enabled

No malware found

Với mỗi rule chọn điều kiện (allow/require) và priority.

Lưu Posture Profile.

Tạo Posture Tag:

Vào Tags (hoặc Endpoint Tagging / Posture Tags).

New Tag → đặt tên (ví dụ: A-compliant) và mô tả.

Map Posture Profile → Posture Tag:

Trong Posture Profile có phần “Actions” khi profile là compliant → chọn “Assign Tag” và chọn A-compliant.

Có thể thêm tag khác cho trường hợp non-compliant (ví dụ A-noncompliant).

Gán profile cho device / group:

Vào Endpoint Groups / Clients → chọn group tương ứng (hoặc import group từ Entra ID trước).

Gán Posture Profile này vào group/device (hoặc bằng policy deployment).

Kiểm tra:

Trên EMS → Devices, chọn device thử nghiệm → xem phần Tags / Posture xem tag A-compliant đã xuất hiện hay chưa.

Nếu chưa thấy, force check: trên endpoint mở FortiClient → Sync; hoặc trên EMS trigger “Scan / Re-evaluate posture”.

2) Publish Posture Tag từ EMS → FortiGate (kết nối Security Fabric)

Mục tiêu: FortiGate phải “nhìn thấy” tag do EMS gán để dùng trong ZTNA policy.

Kiểm tra kết nối Fabric giữa EMS & FortiGate:

EMS cần cấu hình FortiGate như Fabric Connector / Fabric Device. Thường có 2 cách:

Cấu hình Fabric Connector (EMS) trên FortiGate (FortiGate là central fabric).

Hoặc cấu hình FortiGate trong EMS (add Fabric Device) với credentials admin/FMC.

Trên EMS:

Vào Integration / Fabric / FortiGate (tùy version) → Add FortiGate (IP + admin token/credentials).

Sau khi kết nối thành công, EMS sẽ show trạng thái “connected”.

Trên FortiGate:

Vào Security Fabric → Fabric Connectors / EMS → kiểm tra connector với EMS có trạng thái connected.

Nếu chưa có, add connector: chọn type EMS, nhập IP/credentials/PSK theo EMS cho phép.

Publish tags:

Thông thường EMS tự push posture tag qua fabric khi tag được gán cho device.

Nếu cần, trên EMS có nút “Sync to FortiGate” hoặc “Push Tags/Profiles” — chạy command đó.

Kiểm tra trên FortiGate:

GUI: Vào User & Device → Device Inventory hoặc Endpoint Control → Endpoints → chọn thiết bị → xem attributes, phải thấy Posture Tag (ví dụ A-compliant).

Logs: Security Fabric logs / Endpoint logs sẽ cho thấy tag update.

Nếu FortiManager đứng giữa (FortiManager quản lý FortiGate), vẫn cần đảm bảo FortiGate thực sự connected to EMS; FortiManager chỉ push config policy chứ không tự nhận posture tag thay FortiGate.

3) Tạo ZTNA Policy dùng Posture Tag trong FortiManager (step-by-step)

Lưu ý: Bạn quản lý FortiGate qua FortiManager => tạo policy trên FMG rồi push xuống FortiGate.

Đăng nhập vào FortiManager.

Vào phần Policy & Objects (hoặc Device Manager → Policy Packages).

Chọn Policy Package tương ứng device group chứa FortiGate đích. Nếu chưa có, tạo mới gán FortiGate target.

Trong policy package, tạo New Policy (ứng dụng ZTNA / IPv4 policy):

Name: ZTNA-Allow-KeyVault-A

Source Interface/Zone: interface mà VPN remote gateway xuất ra (vd: ssl.root hoặc vpn).

Source Address: có thể đặt all hoặc cụ thể (tốt nhất dùng user group source).

Source User / User Group: chọn Group A (đã sync từ Entra ID). Nếu chưa sync user groups vào FortiManager/FortiGate, import group trước (qua FortiGate integration with Azure AD / SAML).

Device Criteria / Endpoint Posture: Chọn Posture Tag = A-compliant (tag đã publish từ EMS).

Trường hợp GUI FortiManager hiển thị “Device Tag” hoặc “Endpoint Tag” — hãy chọn tag tương ứng.

Destination: định nghĩa address/URL của Azure KeyVault (IP/URL hoặc FQDN). Nếu KeyVault qua Site-to-Site VPN, dùng IP internal hoặc route đã configured.

Service: HTTPS (443) hoặc cụ thể port service.

Action: Allow.

Logging: enable logging & forward to log server để dễ debug.

Order / Policy placement:

Đặt rule này ở vị trí trước các rule rộng hơn (ví dụ trước allow toàn subnet).

Push policy:

Install / Deploy policy từ FortiManager -> chọn FortiGate target -> Install.

Kiểm tra & Test:

Trên endpoint (client) với FortiClient: ensure device nhận tag A-compliant.

Đăng nhập user thuộc Group A, kết nối VPN remote gateway.

Thử truy cập KeyVault endpoint (curl / browser). Kiểm tra nếu device compliant → truy cập thành công; nếu non-compliant → bị block.

Trên FortiGate Logs (Traffic / Event logs) xem match rule, hoặc trên FortiManager logs.

Kiểm tra & Troubleshooting (quick-list)

Nếu device không thấy tag trên FortiGate:

Kiểm tra EMS ↔ FortiGate Fabric connector (connected? credentials ok?)

Trên EMS: confirm device đã re-evaluate posture; re-run scan.

Kiểm tra FortiClient trên endpoint có gửi data được không (network/firewall).

Nếu policy không match:

Kiểm tra thứ tự policy (rule bị chặn bởi rule đứng trên).

Xác minh Source User đúng group (Entra ID group đã sync).

Xác minh Destination IP/port đúng.

Nếu user login OK nhưng vẫn bị block:

Kiểm tra device tag có phải đúng tag policy yêu cầu không.

Kiểm tra FortiGate nhận tag thời gian thực hay bị delay — đôi khi cần vài giây để sync.

Logs hữu ích:

EMS device logs (posture evaluations)

FortiGate Traffic logs (policy match)

FortiManager install logs (policy deploy)

Ví dụ ngắn gọn về logic policy (pseudo)

Condition: user in Group_A AND device has tag "A-compliant" AND dst is KeyVault_IP → Allow

Else → Deny (or Redirect to remediation portal)