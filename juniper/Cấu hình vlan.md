# show vlan
```
cli
configure
show vlan
```
# Tạo vlan
```
cli
configure
set vlans MGMT vlan-id 200
set vlans Test vlan-id 10
```

# Port trunk
```
cli
configure
set interfaces ge-0/0/8  description ***uplink-router***
set interfaces ge-0/0/8 unit 0 family ethernet-switching port-mode trunk
set interfaces ge-0/0/8 unit 0 family ethernet-switching members [10 200]
```

# Port access
```
cli
configure
set interface ge-0/0/9 unit 0 family ethernet-switching port-mode access
set interface ge-0/0/9 unit 0 family ethernet-switching vlan members 10
``` 
