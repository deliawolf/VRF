# VRF

A VRF instance consists of an IP routing table, a derived forwarding table, a set of interfaces that use the forwarding table, and a set of rules and routing protocols that determine what goes into the forwarding table. The use of VRF technology allows the customer to virtualize a network device from a Layer 3 standpoint, creating different "virtual routers" in the same physical device.

```
ISP(config)#ip vrf CUST-A
ISP(config-vrf)#exit
ISP(config)#ip vr
ISP(config)#ip vrf CUST-B
ISP(config-vrf)#exit
```

```
ISP(config)#interface ethernet 0/0
ISP(config-if)#ip vrf forwarding CUST-A
% Interface Ethernet0/0 IPv4 disabled and address(es) removed due to enabling VRF CUST-A
ISP(config-if)#ip address 172.16.1.18 255.255.255.240

ISP(config)#interface ethernet 0/1
ISP(config-if)#ip vrf forwarding CUST-A
% Interface Ethernet0/1 IPv4 disabled and address(es) removed due to enabling VRF CUST-A
ISP(config-if)#ip address 172.16.2.18 255.255.255.240
```
```
ISP(config)#interface ethernet 0/2
ISP(config-if)#ip vrf forwarding CUST-B
% Interface Ethernet0/2 IPv4 disabled and address(es) removed due to enabling VRF CUST-B
ISP(config-if)#ip address 172.16.1.24 255.255.255.240

ISP(config)#interface ethernet 0/3
ISP(config-if)#ip vrf forwarding CUST-B
% Interface Ethernet0/3 IPv4 disabled and address(es) removed due to enabling VRF CUST-B
ISP(config-if)#ip address 172.16.2.34 255.255.255.240
```

```
ISP#show ip vrf
  Name                             Default RD            Interfaces
  CUST-A                           <not set>             Et0/0
                                                         Et0/1
  CUST-B                           <not set>             Et0/2
                                                         Et0/3
ISP#show ip vrf interfaces
Interface              IP-Address      VRF                              Protocol
Et0/0                  172.16.1.18     CUST-A                           up      
Et0/1                  172.16.2.18     CUST-A                           up      
Et0/2                  172.16.1.24     CUST-B                           up      
Et0/3                  172.16.2.34     CUST-B                           up      
ISP#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

ISP#show ip route vrf CUST-A connected

Routing Table: CUST-A
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
C        172.16.1.16/28 is directly connected, Ethernet0/0
L        172.16.1.18/32 is directly connected, Ethernet0/0
C        172.16.2.16/28 is directly connected, Ethernet0/1
L        172.16.2.18/32 is directly connected, Ethernet0/1
ISP#show ip route vrf CUST-B connected

Routing Table: CUST-B
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area 
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override

Gateway of last resort is not set

      172.16.0.0/16 is variably subnetted, 4 subnets, 2 masks
C        172.16.1.16/28 is directly connected, Ethernet0/2
L        172.16.1.24/32 is directly connected, Ethernet0/2
C        172.16.2.32/28 is directly connected, Ethernet0/3
L        172.16.2.34/32 is directly connected, Ethernet0/3
ISP#
```
PING TEST
```
ISP# ping vrf CUST-A 172.16.1.17
```

```
ISP(config)#router ospf 100 vrf CUST-A
ISP(config-router)#router-id 0.0.0.100
ISP(config-router)#network 172.16.1.16 0.0.0.15 area 0
ISP(config-router)#network 172.16.2.16 0.0.0.15 area 0
```

```
ISP(config)#router ospf 200 vrf CUST-B
ISP(config-router)#router-id 0.0.0.200 
ISP(config-router)#network 172.16.1.32 0.0.0.15 area 0
ISP(config-router)#network 172.16.2.32 0.0.0.15 area 0
```
