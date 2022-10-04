
<a href="https://ibb.co/6XRq5GN"><img src="https://i.ibb.co/7kJ0FBb/Screenshot-2022-10-04-10-49-23-AM.png" alt="Screenshot-2022-10-04-10-49-23-AM" border="0"></a>


<a href="https://ibb.co/mB9H4Tg"><img src="https://i.ibb.co/FW5Bz3C/Screenshot-2022-10-04-10-52-45-AM.png" alt="Screenshot-2022-10-04-10-52-45-AM" border="0"></a>
## Step 1: Configure an IPv4 static default route.
On Edge_Router, configure a directly connected IPv4 default static route. This primary default route should be through router ISP1.

        Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.1

## Step 2: Configure an IPv4 floating static default route.
 On Edge_Router, configure a directly connected IPv4 floating static default route. This default route should be through router ISP2. It should have an administrative distance of 5.

        Edge_Router(config)#ip route 0.0.0.0 0.0.0.0 10.10.10.5 5

# Part 2: Configure IPv6 Static and Floating Static Default Routes
## Step 1: Configure an IPv6 static default route.
On Edge_Router, configure a next hop static default route. This primary default route should be through router ISP1

        Edge_Router(config)# ipv6 route ::/0 2001:db8:a::1

## Step 2: Configure an IPv6 floating static default route.
On Edge_Router, configure a next hop IPv6 floating static default route. The route should be via router ISP2. Use an administrative distance of 5.

        Edge_Router(config)# ipv6 route ::/0 2001:db8:a:2::1 5

# Part 3: Configure IPv4 Static and Floating Static Routes to the Internal LANs
## Step 1: Configure IPv4 static routes to the internal LANs.

a. On ISP1, configure a next hop IPv4 static route to the LAN 1 network through Edge_Router.

        ISP1(config)# ip route 192.168.10.16 255.255.255.240 10.10.10.2

b. On ISP1, configure a next hop IPv4 static route to the LAN 2 network through Edge_Router.

        ISP1(config)# ip route 192.168.11.32 255.255.255.224 10.10.10.2


## Step 2: Configure IPv4 floating static routes to the internal LANs.
a. On ISP1, configure a directly connected floating static route to LAN 1 through the ISP2 router. Use an administrative distance of 5.

        ISP1(config)# ip route 192.168.10.16 255.255.255.240 g0/0 5

b. On ISP1, configure a directly connected floating static route to LAN 2 through the ISP2 router. Use an administrative distance of 5.

        ISP1(config)# ip route 192.168.11.32 255.255.255.224 g0/0 5

# Part 4: Configure IPv6 Static and Floating Static Routes to the Internal LANs.
## Step 1: Configure IPv6 static routes to the internal LANs.
c. On ISP1, configure a next hop IPv6 static route to the LAN 1 network through Edge_Router.

        ISP1(config)# ipv6 route 2001:DB8:1:10::/64 2001:DB8:A:1::2

d. On ISP1, configure a next hop IPv6 static route to the LAN 2 network through Edge_Router.

        ISP1(config)# ipv6 route 2001:DB8:1:11::/64 2001:DB8:A:1::2


## Step 2: Configure IPv6 floating static routes to the internal LANs.
a. On ISP1, configure a next hop IPv6 floating static route to LAN 1 through the ISP2 router. Use an administrative distance of 5.

        ISP1(config)# ipv6 route 2001:DB8:1:10::/64 2001:DB8:F:F::2 5

b. On ISP1, configure a next hop IPv6 floating static route to LAN 2 through the ISP2 router. Use an administrative distance of 5.

        ISP1(config)# ipv6 route 2001:DB8:1:11::/64 2001:DB8:F:F::2 5


# Part 5: Configure Host Routes
## Step 1: Configure IPv4 host routes.

a. On Edge Router, configure an IPv4 directly connected host route to the customer server.
       
        Edge_Router(config)# ip route 198.0.0.10 255.255.255.255 serial0/0/0

b. On Edger Router, configure an IPv4 directly connected floating host route to the customer sever. Use an administrative distance of 5.

        Edge_Router(config)# ip route 198.0.0.10 255.255.255.255 serial0/0/1

## Step 2: Configure IPv6 host routes.
a. On Edge Router, configure an IPv6 next hop host route to the customer server through the ISP1 router.

        Edge_Router(config)# ipv6 route 2001:db8:f:f::10/128 2001:db8:a:1::1


b. On Edger Router, configure an IPv6 directly connected floating host route to the customer sever through the ISP2 router. Use an administrative distance of 5.

        Edge_Router(config)# ipv6 route 2001:db8:f:f::10/128 2001:db8:a:2::1 5
        
## Verifikasi
Edge_Router#show ip route static

Edge_Router#show ip route | begin Gateway

Edge_Router# show ipv6 route

Edge_Router#show ipv6 route static
