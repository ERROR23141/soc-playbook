# Networking Basics

## IP addresses

- An IP address identifies a device on a network.
- Example private IP: '192.168.1.10'
- Example public IP: '8.8.8.8' (Google DNS)

## Ports

- Ports are like "doors" on a host that services use.
- One IP can have many ports open at the same time.

Common ports to remember:
- 22 - SSH
- 80 - HTTP (web, uncrypted)
- 443 - HTTPS (web, encrypted)
- 53 - DNS
- 3389 - RDP

## Protocols: TCP vs UDP

**TCP**
- Connection oriented, reliable.
- Used for: web (HTTP/HTTPS), SSH, email (SMTP/IMAP/POP3).

**UDP**
- Connectionless, "fire and forget".
- Used for: DNS, streaming, some VPNs, games.

## DNS (Domain Name System)

- Translates domain names (e.g. 'example.com') into IP addresses.
- You type a domain -> DNS resolver asks servers -> gets an IP -> your browser connects.

## HTTP vs HTTPS

**HTTP**
- HyperText Transfer Protocol.
- Data is **cleartext** on the network (can be sniffed).

**HTTPS**
- HTTP + TLS encryption.
- Protects data in transit (e.g. logins, cookies).
- Uses port 443 by default.

## Quick questions for myself

- What is an IP address?
- What is a port?
- Which port does HTTPS use by default?
- What does DNS do?
- Give one example of a TCP protocol and one UDP protocol.
