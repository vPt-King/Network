Mặc định, các port trên Juniper Switch đã ở trạng thái "On"
# Cách kiểm tra và thay đổi trạng thái Port
`run show interfaces terse`
(Nếu cột Admin là up và Link là up nghĩa là port đang hoạt động bình thường).

# Shutdown 1 port
```
set interfaces ge-0/0/0 disable
commit
```

# Bật lại 1 port
```
delete interfaces ge-0/0/0 disable
commit
```