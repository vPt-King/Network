Để kiểm tra log (nhật ký hệ thống) trên Juniper Switch, bạn chủ yếu sử dụng lệnh show log trong chế độ Operational mode (dấu nhắc >).

# Dưới đây là các lệnh phổ biến và hữu ích nhất để bạn làm việc với log:

## Các lệnh xem log cơ bản
Xem file log chính (messages): Đây là nơi lưu trữ hầu hết các sự kiện quan trọng của hệ thống.
`show log messages`

## Xem log cấu hình (chứng thực, thay đổi cấu hình):
`show log interactive-commands`

## Xem log khởi động (Boot logs):
`show system boot-messages`

# Cách lọc log để tìm thông tin nhanh hơn
## Xem các dòng cuối cùng (giống lệnh tail trên Linux):
`show log messages | last 20`

## Tìm nội dung cụ thể (ví dụ: tìm log liên quan đến port ge-0/0/0):
`show log messages | match ge-0/0/0`

## Theo dõi log trực tiếp (Real-time): Lệnh này sẽ đẩy log mới liên tục ra màn hình khi có sự kiện xảy ra.

```
monitor start messages
# Để dừng theo dõi, gõ:
monitor stop
```

# Mẹo nhỏ: Kiểm tra thời gian của Log
Khi xem log, hãy đảm bảo thời gian trên Switch được cấu hình đúng để đối chiếu sự cố chính xác. Bạn có thể bật hiển thị thời gian trên CLI bằng lệnh:
`set cli timestamp`



