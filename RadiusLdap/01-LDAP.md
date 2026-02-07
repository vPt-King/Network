\# LDAP là gì

LDAP (Lightweight Directory Access Protocol) là giao thức để truy vấn \& quản lý danh bạ người dùng tập trung.



Hiểu nôm na : LDAP là kho "User"



LDAP lưu cái gì?

```

User / Group

Username, password (hash)

Email, phòng ban, chức vụ

Quyền logic (member of group)

Ví dụ LDAP quen thuộc:

Active Directory (AD)

OpenLDAP

```



\# LDAP dùng để làm gì?

```

Xác thực user (username/password)

Tra cứu thông tin user

Phân nhóm user

📌 LDAP không quan tâm:

VLAN

Bandwidth

Thời gian login

Accounting network

```

\# Luồng thực tế khi triển khai với Radius

`Use  → Switch / AP / VPN > Radius Server > LDAP/AD`

Luồng thực tế:

```

User nhập username/password



Thiết bị mạng gửi auth request đến RADIUS



RADIUS đi hỏi LDAP:

👉 “User này có tồn tại không? Password đúng không?”



LDAP trả lời



RADIUS:



Accept / Reject



Gán VLAN / policy



Ghi accounting



📌 LDAP không nói chuyện trực tiếp với switch / firewall

📌 RADIUS không lưu user, nó chỉ “hỏi hộ”

```

