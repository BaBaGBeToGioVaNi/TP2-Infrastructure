```console
hostname NREN1
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
ip address 100.101.0.1 255.255.255.255
ipv6 address 2001:11::1/128
!
interface GigabitEthernet1/0
description P2P Link to RREN
ip address 100.100.1.2 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:10:0:10::1/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
!Group1
interface GigabitEthernet3/0
description P2P Link to B11
ip address 100.101.1.1 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:11:0:10::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
!Group 2
interface GigabitEthernet4/0
description P2P Link to B21
ip address 100.101.1.5 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:11:0:11::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
!Group 3
interface GigabitEthernet5/0
description P2P Link to B31
ip address 100.101.1.9 255.255.255.252
no ip directed-broadcast
no ip redirects
no ip proxy-arp
ipv6 address 2001:11:0:12::/127
ipv6 nd prefix default no-advertise
ipv6 nd ra suppress
no shutdown
!
! inbound filter for AS10
ip prefix-list AS10-in permit 100.68.1.0/24
ipv6 prefix-list AS10-v6-in permit 2001:db8:1::/48
!
! inbound filter for AS20
ip prefix-list AS20-in permit 100.68.2.0/24
ipv6 prefix-list AS20-v6-in permit 2001:db8:2::/48
!
! inbound filter for AS30
ip prefix-list AS30-in permit 100.68.3.0/24
ipv6 prefix-list AS30-v6-in permit 2001:db8:3::/48
!
router bgp 101
bgp log-neighbor-changes
bgp deterministic-med
no bgp default ipv4-unicast
address-family ipv4
distance bgp 200 200 200
network 100.101.0.0 mask 255.255.0.0
neighbor 100.101.1.2 remote-as 10
neighbor 100.101.1.2 description eBGP with AS10
neighbor 100.101.1.2 password NSRC-BGP
neighbor 100.101.1.2 prefix-list AS10-in in
neighbor 100.101.1.2 activate

neighbor 100.101.1.6 remote-as 20
neighbor 100.101.1.6 description eBGP with AS20
neighbor 100.101.1.6 password NSRC-BGP
neighbor 100.101.1.6 prefix-list AS20-in in
neighbor 100.101.1.6 activate

neighbor 100.101.1.10 remote-as 30
neighbor 100.101.1.10 description eBGP with AS30
neighbor 100.101.1.10 password NSRC-BGP
neighbor 100.101.1.10 prefix-list AS30-in in
neighbor 100.101.1.10 activate
! 
neighbor 100.100.1.1 remote-as 100
neighbor 100.100.1.1 description eBGP with RREN (AS100)
neighbor 100.100.1.1 password NSRC-BGP
neighbor 100.100.1.1 activate
!
address-family ipv6
distance bgp 200 200 200
network 2001:11::/32
neighbor 2001:11:0:10::1 remote-as 10
neighbor 2001:11:0:10::1 description eBGP with AS10
neighbor 2001:11:0:10::1 password NSRC-BGP
neighbor 2001:11:0:10::1 prefix-list AS10-v6-in in
neighbor 2001:11:0:10::1 activate

neighbor 2001:11:0:11::1 remote-as 20
neighbor 2001:11:0:11::1 description eBGP with AS20
neighbor 2001:11:0:11::1 password NSRC-BGP
neighbor 2001:11:0:11::1 prefix-list AS20-v6-in in
neighbor 2001:11:0:11::1 activate

neighbor 2001:11:0:12::1 remote-as 30
neighbor 2001:11:0:12::1 description eBGP with AS30
neighbor 2001:11:0:12::1 password NSRC-BGP
neighbor 2001:11:0:12::1 prefix-list AS30-v6-in in
neighbor 2001:11:0:12::1 activate
!
neighbor 2001:10:0:10:: remote-as 100
neighbor 2001:10:0:10:: description eBGP with RREN (AS100)
neighbor 2001:10:0:10:: password NSRC-BGP
neighbor 2001:10:0:10:: activate
!
ip route 100.101.0.0 255.255.0.0 null0
ipv6 route 2001:11::/32 null0
```