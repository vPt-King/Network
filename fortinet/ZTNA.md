🎯 GIẢI THÍCH CHUẨN: KHI NÀO CẦN / KHÔNG CẦN POSTURE TAG
✔️ 1. Nếu bạn muốn kiểm soát quyền truy cập dựa trên GROUP của Entra ID (user-based)

→ KHÔNG cần posture tag.

Bạn chỉ cần:

Sync group từ Entra ID về FortiGate (hoặc về EMS nếu dùng EMS làm Identity Provider)

Tạo ZTNA policy với nguồn là user group

Không liên quan đến posture tag

👉 Trường hợp này phụ thuộc vào Identity, không phụ thuộc vào security posture của thiết bị.

✔️ 2. Nếu bạn muốn kiểm soát quyền dựa vào tình trạng thiết bị (device security posture)

→ CÓ cần posture tag.

Ví dụ:

Máy phải có Windows Defender bật

Máy phải có FortiClient bản mới

Máy phải không có malware

Máy phải encrypt ổ đĩa

Máy phải là device của công ty

→ EMS sẽ gán Posture Tag cho thiết bị → FortiGate dùng tag này trong ZTNA policy để quyết định allow/deny.

👉 Trong case của bạn, guide bảo bạn dùng Posture Tag → Vậy tức là policy kiểm tra cả device posture.

Nghĩa là:

User thuộc group A chưa đủ

Máy của user cũng phải đạt chuẩn EMS → mới được truy cập KeyVault

Đây là mô hình Zero Trust:
User OK + Device OK = Allow
User OK + Device Not OK = Deny

🧭 CHI TIẾT QUY TRÌNH ĐÚNG CHUẨN CHO CASE NÀY
🔹 (1) Tạo nhóm A trong Entra ID

→ Cho đúng user vào nhóm.

🔹 (2) EMS import nhóm từ Entra ID

Lý do: EMS cần biết group nào có user nào để attach posture tag cho đúng người.

➡ Vào EMS → Security Fabric → Identity → Azure AD → Import Group
(hoặc Manage Domain tùy version)

🔹 (3) Tạo Device Posture Check trên EMS

VD:

Windows Defender must be ON

OS must be Windows 10/11

FortiClient version >= 7.2

No malware

🔹 (4) Tạo Posture Tag tương ứng

VD: "A-secure-device"

Liên kết posture check → Tag sẽ được gán khi thiết bị đạt chuẩn.

🔹 (5) FortiGate ZTNA Policy

Source:

User Group = Group A (đến từ Entra ID)

Device = Posture Tag “A-secure-device”

Destination:

Azure KeyVault endpoint IP/URL

Action: Allow

🧠 VẬY CÓ PHẢI BẠN PHẢI TẠO POSTURE TAG TRÊN EMS?

👉 Nếu policy yêu cầu kiểm tra device posture → BẮT BUỘC phải tạo Posture Tag trên EMS.

👉 Nếu policy chỉ cần user-based (theo group Entra ID) → KHÔNG cần posture tag.

📌 GIẢI THÍCH TẠI SAO GUIDE CỦA BẠN BẢO DÙNG POSTURE TAG

Vì họ muốn tăng mức bảo mật:

User thuộc group A

AND máy phải đạt security posture của công ty
→ Máy không comply thì dù user có trong group A cũng không được vào KeyVault.

Đây là Zero Trust chính hiệu.