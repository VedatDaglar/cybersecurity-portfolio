# Network Traffic Analysis — Wireshark Basics

## Status

Completed the TryHackMe room: **Wireshark: The Basics**.

## Purpose

Practise inspecting network packets, using display filters,
and investigating Wireshark findings.

## Environment

| Item | Details |
|---|---|
| Platform | TryHackMe |
| Room | Wireshark: The Basics |
| Tool | Wireshark |
| Capture file | `Exercise.pcapng` |
| Traffic source | Provided training capture |

## Wireshark's Three Main Panes

| Pane | Purpose |
|---|---|
| Packet List | Lists captured packets, including their source, destination, and protocol. |
| Packet Details | Displays the selected packet's protocol fields in an expandable tree. |
| Packet Bytes | Displays packet data in hexadecimal and ASCII. |

## Investigation Steps

1. Opened the provided capture file.
2. Explored the Packet List, Packet Details, and Packet Bytes panes.
3. Opened **Analyze → Expert Information**.
4. Selected packet `9100` and inspected its warnings.
5. Used **Follow → TCP Stream** to examine the conversation.
6. Read the comment attached to packet `12`.
7. Used a display filter to locate packet `39765`.
8. Inspected its HTTP and JPEG protocol details.

## Observations

| Evidence | What I observed |
|---|---|
| Packet `9100` | Wireshark reported a malformed HTTP error and illegal characters in a header name. |
| TCP stream `2` | The ASCII view contained unreadable characters and a `JFIF` marker, suggesting JPEG image data. |
| Packet `12` | Its comment provided instructions for examining another packet. |
| Packet `39765` | Wireshark displayed `HTTP/1.1 200 OK` and a JPEG File Interchange Format section. |
| Reassembled data | Wireshark showed a de-chunked entity body of `3324` bytes for the JPEG response. |

## Display Filters

### Select a specific packet

```text
frame.number == 39765
```

This displays packet `39765`.

### Display a specific TCP conversation

```text
tcp.stream eq 2
```

This displays packets belonging to TCP stream `2`.

Display filters change which packets are shown.
They do not delete packets from the capture file.

## Interpretation

- Expert Information highlights findings that need investigation.
- A malformed HTTP warning alone does not prove an attack.
- Binary image data can look unreadable in an ASCII view.
- Unreadable characters do not automatically mean encryption.
- Following a TCP stream provides context beyond one packet.
- The JPEG response in packet `39765` does not, by itself,
  explain the malformed warning in packet `9100`.

## Review Questions

### What are the three main panes in Wireshark?

Packet List, Packet Details, and Packet Bytes.

### Does a display filter delete packets?

No. It only changes which packets are displayed.

### Does an Expert Information warning prove an attack?

No. The finding needs further investigation and context.

### Do unreadable characters always mean encrypted data?

No. Binary data, such as JPEG image data, can also look
unreadable when displayed as text.

### Why follow a TCP stream?

It helps examine the data exchanged in a TCP conversation
rather than looking at one packet alone.

## Results

I completed the TryHackMe room and practised navigating
Wireshark, filtering packets, reading packet comments,
inspecting expert findings, and following TCP streams.

This report records selected observations from the exercise.
It does not establish that an attack occurred.

## Limitations

- This was a guided exercise using a provided capture file.
- I used AI assistance to understand findings and navigate some steps.
- I have not independently confirmed the cause of the malformed HTTP warning.
- Room completion does not mean I can explain every packet without assistance.
- This report does not record a verified image-export result or MD5 value.

## Next Step

Repeat a small packet analysis without step-by-step instructions.

Identify the source, destination, protocol, and relevant fields.
Explain what the evidence shows and what remains uncertain.
Practise exporting a transferred file and checking its hash.
