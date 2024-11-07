# BR-RTR
```
configure
interface gigabitethernet 1/0/1
  ip firewall disable
  ip address 172.16.5.2/28
  no shutdown
exit
interface gigabitethernet 1/0/2
  ip firewall disable
  ip address 192.168.1.1/27
  no shutdown
exit
tunnel gre 1
  ttl 16
  mtu 1400
  ip firewall disable
  local address 172.16.5.2
  remote address 172.16.4.2
  ip address 172.16.1.2/30
  enable
exit
ip route 0.0.0.0/0 172.16.5.1

commit
confirm
```
```
key-chain ospfkey
  key 1
    key-string ascii-text Demo2025
  exit
exit

router ospf 1
  area 0.0.0.0
    enable
  exit
  enable
exit

interface gigabitethernet 1/0/2
  ip ospf instance 1
  ip ospf
exit
tunnel gre 1
  ip ospf instance 1
  ip ospf network point-to-point
  ip ospf authentication key-chain ospfkey
  ip ospf authentication algorithm md5
  ip ospf
exit

commit
confirm
```
```
sh ip ospf neighbor
sh ip route
```
```
object-group network LOCAL_NET
  ip address-range 192.168.1.1-192.168.1.30  [Пишем пул адресов подсети BR-SRV. Т.е пишем пул который получился для 32 адресов ]
exit

nat source
  ruleset SNAT
    to interface gigabitethernet 1/0/1
    rule 1
      match source-address LOCAL_NET
      action source-nat interface
      enable
    exit
  exit
exit
!
ping 8.8.8.8
```
```
configure
username net_admin
password P@ssw0rd
privilege 15
end
!
commit
confirm
!
```
```
configure
clock timezone gmt +5
end
commit
confirm
```

Проверяем:

```
show date
```
