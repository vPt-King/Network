\# Radius server là gì

RADIUS server (Remote Authentication Dial-In User Service) là máy chủ dùng để xác thực, phân quyền và ghi log truy cập cho người dùng khi họ kết nối vào hệ thống mạng. Nói ngắn gọn: ai được vào mạng, vào bằng cách nào, và đã làm gì đều do RADIUS quản lý.



Radius xoay quanh 3 chữ AAA

```

* Authentication - Xác thực : Bạn là ai Username/password, certificate, OTP/MFA
* Authorization - Phân quyền : Bạn được phép làm gì , vào VLAN nào, giới hạn băng thông như nào
* Accounting - Ghi log : Bạn  làm gì , thời gian đăng nhập, đăng xuất, IP được cấp

```



\# Radius hoajt động như nào

Luồng cơ bản :

`User > Switch / AP / VPN > Radius Server`

User đăng nhập (WiFi, VPN, LAN…)

Thiết bị mạng (NAS) gửi yêu cầu lên RADIUS

RADIUS kiểm tra user trong DB / AD / LDAP

Trả kết quả:

Accept + policy

Reject



RADIUS thường dùng ở đâu?



Rất nhiều luôn 👇

🔐 WiFi doanh nghiệp (802.1X)

🌐 VPN (SSL VPN, IPsec)

🖧 LAN có xác thực (NAC)

🧑‍💼 Xác thực user tập trung cho firewall, switch

💰 ISP tính cước (Accounting)



