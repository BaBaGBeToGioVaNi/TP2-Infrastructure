## Configuration de ISP1
```console
enable
conf t
hostname ISP1
aaa new-model
aaa authentication login default local
aaa authentication enable default enable
username nrenlab secret nsrc-PW
enable secret nsrc-EN
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
ip address 100.121.0.1 255.255.255.255
ipv6 address 2001:18::1/128
!
interface FastEthernet0/0
description Workshop Backbone
ip address 10.10.0.235 255.255.255.0
no ip directed-broadcast
no ip redirects
no ip proxy-arp
no shutdown
!
interface GigabitEthernet1/0
description Link to IXP
ip address 100.127.1.1 255.255.255.0
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:DB8:FFFF:1::1/64
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress all
no shutdown
!
! Link to Group 1
interface GigabitEthernet3/0
description P2P Link to B12
ip address 100.121.1.1 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:18:0:10::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress all
no shutdown
!
! Link to Group 2
interface GigabitEthernet4/0
description P2P Link to B22
ip address 100.121.1.5 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:18:0:11::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress all
no shutdown
!
! Link to Group 3
interface GigabitEthernet5/0
description P2P Link to B32
ip address 100.121.1.9 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:18:0:12::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress all
no shutdown
!
! Routes to Group 1
ip route 100.68.1.0 255.255.255.0 100.121.1.2
ipv6 route 2001:db8:1::/48 2001:18:0:10::1
!
! Routes to Group 2
ip route 100.68.2.0 255.255.255.0 100.121.1.6
ipv6 route 2001:db8:2::/48 2001:18:0:11::1
!
! Routes to Group 3
ip route 100.68.3.0 255.255.255.0 100.121.1.10
ipv6 route 2001:db8:3::/48 2001:18:0:12::1
!
! Routes to Group 4
ip route 100.68.4.0 255.255.255.0 100.127.1.2
ipv6 route 2001:db8:4::/48 2001:DB8:FFFF:1::2
!
! Routes to Group 5
ip route 100.68.5.0 255.255.255.0 100.127.1.2
ipv6 route 2001:db8:5::/48 2001:DB8:FFFF:1::2
!
! Routes to Group 6
ip route 100.68.6.0 255.255.255.0 100.127.1.2
ipv6 route 2001:db8:6::/48 2001:DB8:FFFF:1::2
!
! Routes to ISP2 address space
ip route 100.122.0.0 255.255.0.0 100.127.1.2
ipv6 route 2001:19::/32 2001:DB8:FFFF:1::2
!
! Pull-up routes for address blocks of ISP1
ip route 100.121.0.0 255.255.0.0 null0
ipv6 route 2001:18::/32 null0
!
```

```console
no ip route 100.68.1.0 255.255.255.0 100.121.1.2
no ip route 100.68.2.0 255.255.255.0 100.121.1.6
no ip route 100.68.3.0 255.255.255.0 100.121.1.10
!
no ipv6 route 2001:db8:1::/48 2001:18:0:10::1
no ipv6 route 2001:db8:2::/48 2001:18:0:11::1
no ipv6 route 2001:db8:3::/48 2001:18:0:12::1
!
! Filters for Group 1
ip prefix-list AS10-in permit 100.68.1.0/24
ipv6 prefix-list AS10-v6-in permit 2001:db8:1::/48
!
! Filters for Group 2
ip prefix-list AS20-in permit 100.68.2.0/24
ipv6 prefix-list AS20-v6-in permit 2001:db8:1::/48
!
! Filters for Group 3
ip prefix-list AS30-in permit 100.68.3.0/24
ipv6 prefix-list AS30-v6-in permit 2001:db8:3::/48
!
router bgp 121
bgp log-neighbor-changes
no bgp default ipv4-unicast
bgp deterministic-med
address-family ipv4
distance bgp 200 200 200
network 100.121.0.0 mask 255.255.0.0
neighbor 100.101.2.1 remote-as 101
neighbor 100.101.2.1 description eBGP with NREN1 (AS101)
neighbor 100.101.2.1 password NSRC-BGP
neighbor 100.101.2.1 activate
neighbor 100.121.1.2 remote-as 10
neighbor 100.121.1.2 description eBGP with AS10
neighbor 100.121.1.2 password NSRC-BGP
neighbor 100.121.1.2 prefix-list AS10-in in
neighbor 100.121.1.2 activate
neighbor 100.121.1.6 remote-as 20
neighbor 100.121.1.6 description eBGP with AS20
neighbor 100.121.1.6 password NSRC-BGP
neighbor 100.121.1.6 prefix-list AS20-in in
neighbor 100.121.1.6 activate 
neighbor 100.121.1.10 remote-as 30
neighbor 100.121.1.10 description eBGP with AS30
neighbor 100.121.1.10 password NSRC-BGP
neighbor 100.121.1.10 prefix-list AS30-in in
neighbor 100.121.1.10 activate 

neighbor 100.127.1.2 remote-as 122
neighbor 100.127.1.2 description eBGP with ISP2 (AS122)
neighbor 100.127.1.2 password NSRC-BGP
neighbor 100.127.1.3 activate
neighbor 100.127.1.3 remote-as 100
neighbor 100.127.1.3 description eBGP with RREN (AS100)
neighbor 100.127.1.3 password NSRC-BGP
neighbor 100.127.1.3 activate
!
address-family ipv6
distance bgp 200 200 200
network 2001:18::/32
neighbor 2001:11:0:20:: remote-as 101
neighbor 2001:11:0:20:: description eBGP with NREN1 (AS101)
neighbor 2001:11:0:20:: password NSRC-BGP
neighbor 2001:11:0:20:: activate
neighbor 2001:18:0:10::1 remote-as 10
neighbor 2001:18:0:10::1 description eBGP with AS10
neighbor 2001:18:0:10::1 password NSRC-BGP
neighbor 2001:18:0:10::1 prefix-list AS10-v6-in in
neighbor 2001:18:0:10::1 activate 
neighbor 2001:18:0:11::1 remote-as 20
neighbor 2001:18:0:11::1 description eBGP with AS20
neighbor 2001:18:0:11::1 password NSRC-BGP
neighbor 2001:18:0:11::1 prefix-list AS20-v6-in in
neighbor 2001:18:0:11::1 activate
neighbor 2001:18:0:12::1 remote-as 30
neighbor 2001:18:0:12::1 description eBGP with AS30
neighbor 2001:18:0:12::1 password NSRC-BGP
neighbor 2001:18:0:12::1 prefix-list AS30-v6-in in
neighbor 2001:18:0:12::1 activate

neighbor 2001:DB8:FFFF:1::2 remote-as 122
neighbor 2001:DB8:FFFF:1::2 description eBGP with ISP2 (AS122)
neighbor 2001:DB8:FFFF:1::2 password NSRC-BGP
neighbor 2001:DB8:FFFF:1::2 activate
neighbor 2001:DB8:FFFF:1::3 remote-as 100
neighbor 2001:DB8:FFFF:1::3 description eBGP with RREN (AS100)
neighbor 2001:DB8:FFFF:1::3 password NSRC-BGP
neighbor 2001:DB8:FFFF:1::3 activate
!
! Default IPv4 Route to Classroom Gateway
ip route 0.0.0.0 0.0.0.0 10.10.0.254
!
ip route 100.121.0.0 255.255.0.0 null0
ipv6 route 2001:18::/32 null0
```

```console
configure terminal
interface GigabitEthernet2/0
description P2P Link to NREN1
ip address 100.101.2.2 255.255.255.252
no ip redirects
no ip proxy-arp
ipv6 address 2001:11:0:20::1/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress all
no shutdown
!
router bgp 121
address-family ipv4
neighbor 100.101.2.1 remote-as 101
neighbor 100.101.2.1 description eBGP with NREN1 (AS101)
neighbor 100.101.2.1 password NSRC-BGP
neighbor 100.101.2.1 activate
!
address-family ipv6
neighbor 2001:11:0:20:: remote-as 101
neighbor 2001:11:0:20:: description eBGP with NREN1 (AS101)
neighbor 2001:11:0:20:: password NSRC-BGP
neighbor 2001:11:0:20:: activate
!
```
