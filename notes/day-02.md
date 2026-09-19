# Day 2 — Network Services and Diagnostics

## Topics Studied

Today I studied the following networking concepts:

- DHCP and the DORA process
- ARP and IP-to-MAC address resolution
- ICMP, ping, and traceroute
- Routing protocols
- NAT and PAT
- Local and public IP addresses

## DHCP

DHCP automatically provides devices with network configuration such as:

- IP address
- Subnet mask
- Default gateway
- DNS server

The four DHCP steps are called DORA:

1. Discover
2. Offer
3. Request
4. Acknowledge

A client without an IP address sends DHCP Discover from `0.0.0.0` to the broadcast address `255.255.255.255`.

DHCP servers use UDP port `67`, while clients use UDP port `68`.

## ARP

ARP finds the MAC address associated with an IP address on the local network.

An ARP Request is sent to the broadcast MAC address:

```text
ff:ff:ff:ff:ff:ff
```

The device that owns the requested IP address responds with an ARP Reply.

On macOS, the ARP table can be displayed with:

```bash
arp -a
```

## ICMP and Ping

ICMP is used for network diagnostics and error reporting.

The `ping` command sends an ICMP Echo Request and waits for an Echo Reply:

```bash
ping -c 4 google.com
```

My recorded test sent 10 packets and received 10 replies, resulting in `0%` packet loss.

A failed ping does not always mean that the target is offline because a firewall may block ICMP traffic.

## Traceroute

Traceroute displays the router hops between the local device and a destination:

```bash
traceroute google.com
```

During my test, Google was reached at approximately the tenth hop.

A `*` in traceroute means that a response was not received from that router. It does not necessarily mean that the connection failed.

## Routing

Routers inspect the destination IP address and use their routing tables to select the next hop.

Routing protocols include:

- OSPF — commonly used inside large organizational networks
- EIGRP — associated mainly with Cisco networks
- BGP — used between large networks on the Internet
- RIP — selects routes using hop count

## NAT and PAT

NAT translates private IP addresses into public IP addresses.

PAT allows multiple private devices to share one public IP address by assigning different port numbers to their connections.

The public IPv4 address can be checked with:

```bash
curl -4 -s ifconfig.me
```

## Practical Work

I documented three example networks, macOS networking commands, and a simple network diagram here:

- [IP Addressing Basics](../network/ip-basics.md)

## Key Takeaways

1. DHCP automatically configures devices on a network.
2. ARP maps local IP addresses to MAC addresses.
3. Ping tests reachability and round-trip time.
4. Traceroute displays the hops towards a destination.
5. Routers forward packets according to their routing tables.
6. NAT and PAT allow private devices to share a public IP address.

## Review Point

I need to continue practising subnet calculations and the relationship between IP addresses, subnet masks, and default gateways.
