# Firewalls, VPNs and Security Zones

## Goal

Understand how firewalls, VPNs and network segmentation help protect a network.

## Key Concepts

### Firewalls

Firewalls allow or block traffic based on rules.

- Stateless firewalls check each packet independently.
- Stateful firewalls also track network connections.
- Firewalls can run on physical devices, computers or cloud systems.

### VPNs

A VPN creates an encrypted tunnel between a device and a VPN server.

Encapsulation places the original packet inside another packet.
HTTPS provides separate encryption between the browser and the website.
A VPN does not guarantee complete anonymity.

### Security Zones

| Zone | Purpose | Example |
|---|---|---|
| Uncontrolled zone | Networks outside the company's control | Internet |
| DMZ | Separates public-facing services from private systems | Public web server |
| Internal network | Contains private company systems | Employee computers |
| Restricted zone | Limits access to sensitive resources | Financial records |

## Example Access Rules

This is a learning example, not a configuration I deployed.

| Source | Destination | Decision |
|---|---|---|
| Internet | DMZ web server, TCP port 443 | Allow HTTPS |
| Internet | Private file server | Block direct access |
| DMZ web server | Internal systems | Allow only required connections |

The goal is to limit access and reduce the spread of an attack.

## Practice Completed

I completed the Networking Core Protocols room on TryHackMe.
I reviewed DNS, WHOIS, HTTP/HTTPS, FTP, SMTP, POP3 and IMAP.

## Limitations

I have not configured a firewall or deployed a DMZ.
This document records my learning and an example access policy.

## Next Step

Practise reading firewall rules and explaining why traffic is allowed or blocked.
