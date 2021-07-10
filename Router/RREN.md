```console
en
conf t
hostname RREN
aaa new-model
aaa authentication login default local
aaa authentication enable default enable
username nrenlab secret lab-PW
enable secret lab-EN
service password-encryption
line vty 0 4
transport preferred none
line console 0
transport preferred none
no logging console
logging buffered 8192 debugging
no ip domain-lookup
ipv6 unicast-routing
ipv6 cef
no ip source-route
no ipv6 source-route
!
interface Loopback0
ip address 100.100.0.1 255.255.255.255
ipv6 address 2001:10::1/128
!
interface GigabitEthernet1/0
description P2P Link to NREN1
ip address 100.100.1.1 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:10:0:10::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
interface GigabitEthernet2/0
description P2P Link to NREN2
ip address 100.100.1.5 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:10:0:11::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
interface GigabitEthernet3/0
description Link to IXP
ip address 100.127.1.3 255.255.255.0
no ip redirects
no ip proxy-arp
ipv6 address 2001:DB8:FFFF:1::3/64
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
router bgp 100
bgp log-neighbor-changes
bgp deterministic-med
no bgp default ipv4-unicast
address-family ipv4
distance bgp 200 200 200
network 100.100.0.0 mask 255.255.0.0
neighbor 100.100.1.2 remote-as 101
neighbor 100.100.1.2 description eBGP with NREN1 (AS101)
neighbor 100.100.1.2 password NSRC-BGP
neighbor 100.100.1.2 activate
neighbor 100.100.1.6 remote-as 102
neighbor 100.100.1.6 description eBGP with NREN2 (AS102)
neighbor 100.100.1.6 password NSRC-BGP
neighbor 100.100.1.6 activate
neighbor 100.127.1.1 remote-as 121
neighbor 100.127.1.1 description eBGP with ISP1 (AS121)
neighbor 100.127.1.1 password NSRC-BGP
neighbor 100.127.1.1 activate
neighbor 100.127.1.2 remote-as 122
neighbor 100.127.1.2 description eBGP with ISP2 (AS122)
neighbor 100.127.1.2 password NSRC-BGP
neighbor 100.127.1.2 activate
!
address-family ipv6
distance bgp 200 200 200
network 2001:10::/32
neighbor 2001:10:0:10::1 remote-as 101
neighbor 2001:10:0:10::1 description eBGP with NREN1 (AS101)
neighbor 2001:10:0:10::1 password NSRC-BGP
neighbor 2001:10:0:10::1 activate
neighbor 2001:10:0:11::1 remote-as 102
neighbor 2001:10:0:11::1 description eBGP with NREN2 (AS102)
neighbor 2001:10:0:11::1 password NSRC-BGP
neighbor 2001:10:0:11::1 activate
neighbor 2001:DB8:FFFF:1::1 remote-as 121
neighbor 2001:DB8:FFFF:1::1 description eBGP with ISP1 (AS121)
neighbor 2001:DB8:FFFF:1::1 password NSRC-BGP
neighbor 2001:DB8:FFFF:1::1 activate
neighbor 2001:DB8:FFFF:1::2 remote-as 122
neighbor 2001:DB8:FFFF:1::2 description eBGP with ISP2 (AS122)
neighbor 2001:DB8:FFFF:1::2 password NSRC-BGP
neighbor 2001:DB8:FFFF:1::2 activate
!
ip route 100.100.0.0 255.255.0.0 null0
ipv6 route 2001:10::/32 null0
end
write memory
```
