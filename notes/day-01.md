# Day 1 — Networking Foundations

## Topics Studied

- Private and public IP addresses
- Default gateway
- DNS
- TCP and UDP
- Port numbers
- TCP three-way handshake
- Encapsulation and decapsulation
- The life of a network packet

## IP Address

An IP address identifies a device, also called a host, on a network.

Example:

- Private IP: `192.168.1.x`
- Public IP: Assigned by the internet service provider

The three private IPv4 ranges are:

- `10.0.0.0 – 10.255.255.255`
- `172.16.0.0 – 172.31.255.255`
- `192.168.0.0 – 192.168.255.255`

## Default Gateway

A default gateway is usually the router that connects a local network to other networks.

When my computer needs to communicate with a destination outside the local network, it sends the packet to the default gateway.

Example:

`Computer → Default Gateway → ISP → Internet → Destination`

## DNS

DNS translates domain names into IP addresses.

Example:

`google.com → destination IP address`

This allows users to visit websites without memorizing numerical IP addresses.

## TCP and UDP

TCP is connection-oriented and provides reliable, ordered data delivery.

TCP establishes a connection using:

1. SYN
2. SYN-ACK
3. ACK

UDP is connectionless. It does not guarantee packet delivery but has less overhead and can be faster.

## Port Numbers

An IP address identifies the destination host.

A port number identifies the application or service running on that host.

Examples:

- HTTPS: TCP port 443
- DNS: UDP or TCP port 53
- SSH: TCP port 22

There are approximately 65,000 port numbers.

## Encapsulation

As application data moves down the networking layers, each layer adds its own header.

The process is:

`Data → TCP Segment / UDP Datagram → IP Packet → Ethernet Frame → Bits`

The receiving device removes these headers in reverse order. This is called decapsulation.

## Traceroute Observation

I ran a traceroute to Google.

The first hop was my local router:

`192.168.1.x`

The packet then travelled through several ISP routers before reaching Google's network. The destination was reached in approximately ten hops.

Public IP addresses have been masked to avoid publishing unnecessary network information.

## Five Key Takeaways

1. Private IP addresses cannot be routed directly over the public internet.
2. IP addresses identify hosts, while ports identify applications and services.
3. TCP prioritizes reliability; UDP prioritizes speed and low overhead.
4. Routers inspect the destination IP address to forward packets.
5. The receiving device reverses encapsulation to extract the application data.
