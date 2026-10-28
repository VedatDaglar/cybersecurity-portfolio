# Network Traffic Analysis with Wireshark and tcpdump

## Purpose

Practise reading packet captures, filtering traffic, and explaining
findings using evidence.

This report covers two guided TryHackMe exercises. Each exercise
uses a different capture file.

## Progress

| Exercise | Progress |
|---|---|
| Wireshark: The Basics | Completed |
| Tcpdump: The Basics | Practised reading captures, filtering traffic, and displaying packet details |

## Environment

| Item | Wireshark exercise | tcpdump exercise |
|---|---|---|
| Platform | TryHackMe | TryHackMe |
| Tool | Wireshark | tcpdump |
| Capture file | `Exercise.pcapng` | `traffic.pcap` |
| Traffic source | Provided training capture | Provided training capture |

## Exercise 1: Wireshark Basics

### Wireshark's Three Main Panes

| Pane | Purpose |
|---|---|
| Packet List | Lists captured packets, including their source, destination, and protocol. |
| Packet Details | Displays the selected packet's protocol fields in an expandable tree. |
| Packet Bytes | Displays the selected packet's bytes in hexadecimal and ASCII. |

### Investigation Steps

1. Opened `Exercise.pcapng`.
2. Explored the three main panes.
3. Opened **Analyze → Expert Information**.
4. Selected packet `9100` and inspected its reported error.
5. Used **Follow → TCP Stream** to examine conversation data.
6. Read the comment attached to packet `12`.
7. Used a display filter to locate packet `39765`.
8. Inspected its HTTP and JPEG details.

### Observations

| Evidence | What I observed |
|---|---|
| Packet `9100` | Wireshark reported malformed HTTP and illegal characters in a header name. |
| TCP stream `2` | The ASCII view contained unreadable characters and a `JFIF` marker, consistent with JPEG image data. |
| Packet `12` | Its comment provided instructions for examining another packet. |
| Packet `39765` | Wireshark displayed `HTTP/1.1 200 OK` and a JPEG File Interchange Format section. |
| Reassembled HTTP body | Wireshark showed a de-chunked entity body of `3324` bytes for the JPEG response. |

### Display Filters

#### Select a specific packet

```text
frame.number == 39765
```

Displays packet `39765`.

#### Select a TCP conversation

```text
tcp.stream eq 2
```

Displays packets belonging to TCP stream `2`.

Display filters change which packets are shown. They do not
delete packets from the capture file.

### Interpretation

- Expert Information highlights findings that need investigation.
- A malformed HTTP error alone does not prove an attack.
- Binary image data can look unreadable in an ASCII view.
- Unreadable characters do not automatically mean encryption.
- Following a TCP stream provides context beyond one packet.
- The JPEG response in packet `39765` does not, by itself,
  explain the malformed HTTP error in packet `9100`.

## Exercise 2: Packet Analysis with tcpdump

### Goal

Read a saved packet capture, apply filters, and interpret the output.

### Initial Troubleshooting

My first attempt to read `traffic.pcap` failed because the file
was not in my current directory on the AttackBox.

I switched to the provided lab machine and checked the directory:

```bash
pwd
ls
```

The working directory was `/home/user`, and `traffic.pcap`
was listed there.

I also received a permission error when running tcpdump without
`-r`, because it attempted to capture live traffic.

Reading the provided capture with `-r` worked without `sudo`.

### Read the First Five Packets

```bash
tcpdump -r traffic.pcap -nn -c 5
```

| Option | Purpose |
|---|---|
| `-r traffic.pcap` | Reads the saved capture. |
| `-nn` | Keeps addresses and port numbers numeric. |
| `-c 5` | Reads up to five packets matching the filter, if one is specified. |

The output included TCP traffic using port `22` and STP packets.

### Count ICMP Packets

```bash
tcpdump -r traffic.pcap -nn icmp | wc -l
```

Observed result:

```text
26
```

The `icmp` filter selects ICMP packets. With the default
one-line packet output, `wc -l` counts the matching packets.

This shows that the capture contains 26 ICMP packets.
The count alone does not indicate an attack.

### Inspect ARP Traffic

```bash
tcpdump -r traffic.pcap -nn arp
```

I observed:

- An ARP request from `192.168.124.148` asking for the MAC
  address of `192.168.124.137`.
- A reply stating that `192.168.124.137` was associated with
  MAC address `52:54:00:23:60:2b`.

ARP is used to find the MAC address associated with an IPv4
address on the local network.

To include Ethernet source and destination MAC addresses:

```bash
tcpdump -r traffic.pcap -nn -e arp
```

On an ARP request line, the MAC address before `>` is the
Ethernet source address.

### Inspect a DNS Query

```bash
tcpdump -r traffic.pcap -nn -vv -c 1 'udp port 53'
```

I observed a query with these details:

| Field | Observed value |
|---|---|
| Source IP | `192.168.124.137` |
| Source port | `33672` |
| Destination IP | `192.168.124.1` |
| Destination port | `53` |
| Transport protocol | UDP |
| DNS query | A record for `mirrors.rockylinux.org` |

The A-record query requested an IPv4 address for the domain.

The filter matches UDP traffic with source or destination
port 53. The selected packet was a DNS query.

### Filter Packets with Only the TCP RST Flag

```bash
tcpdump -r traffic.pcap -nn 'tcp[tcpflags] == tcp-rst' | wc -l
```

This selects packets whose TCP flags field contains only RST.

Packets with both RST and ACK are excluded.

The final RST packet count is not recorded in this report.

### Compare TCP Flag Filters

| Filter | What it selects |
|---|---|
| `tcp[tcpflags] == tcp-syn` | Only SYN is set. |
| `tcp[tcpflags] & tcp-syn != 0` | SYN is set, with or without other flags. |
| `tcp[tcpflags] == tcp-rst` | Only RST is set. |

For example, SYN + ACK matches the second filter but not
the first filter.

### Packet Display Options

These options change how packet information is displayed.

| Option | Purpose |
|---|---|
| `-q` | Prints brief packet information. |
| `-e` | Includes the link-layer header, such as Ethernet MAC addresses. |
| `-v` / `-vv` | Adds protocol details. |
| `-A` | Displays packet bytes as ASCII, excluding the link-layer header. |
| `-xx` | Displays packet bytes in hex, including the link-layer header. |
| `-X` | Displays packet bytes in hex and ASCII, excluding the link-layer header. |

Example commands for comparing the first two packets:

```bash
tcpdump -r traffic.pcap -nn -c 2 -q
tcpdump -r traffic.pcap -nn -c 2 -e
tcpdump -r traffic.pcap -nn -c 2 -A
tcpdump -r traffic.pcap -nn -c 2 -xx
tcpdump -r traffic.pcap -nn -c 2 -X
```

These are reference commands for further practice.

Displaying encrypted bytes as ASCII or hex does not decrypt them.

## Review Questions

### What are the three main panes in Wireshark?

Packet List, Packet Details, and Packet Bytes.

### Does a Wireshark display filter delete packets?

No. It only changes which packets are displayed.

### Does an Expert Information finding prove an attack?

No. It needs investigation and context.

### Do unreadable characters always mean encryption?

No. Binary data, such as JPEG image data, can also look
unreadable when displayed as text.

### Why follow a TCP stream?

To examine data exchanged in a TCP conversation rather
than looking at one packet alone.

### What is the difference between `-r` and `-w`?

`-r` reads packets from a file. `-w` writes packets to a file.

### Which option shows Ethernet MAC addresses?

`-e`.

### How do I select packets with only RST set?

Use:

```text
tcp[tcpflags] == tcp-rst
```

### Does `-A` decrypt HTTPS traffic?

No. It changes the display format, not the encryption.

## Results

I completed Wireshark: The Basics and practised inspecting
packets, reading comments, using display filters, reviewing
expert findings, and following TCP streams.

In the tcpdump exercise, I read a saved capture, counted
26 ICMP packets, inspected an ARP exchange, and examined
a DNS query. I also studied TCP flag filters and packet
display options.

These observations do not establish that an attack occurred.

## Limitations

- These were guided exercises using provided training captures.
- I used AI assistance to understand commands, interpret findings,
  and review my notes.
- I have not independently confirmed the cause of the malformed
  HTTP error.
- This report does not record a verified image-export result,
  MD5 value, or final TCP RST packet count.
- The display examples are reference commands, not evidence that
  every command was independently tested.
- Completing an exercise does not mean I can explain every packet
  without assistance.

