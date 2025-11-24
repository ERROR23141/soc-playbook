# Wireshark Cheatsheet

## Basic display filters

- `ip.addr == 8.8.8.8` – traffic to/from 8.8.8.8 (Google DNS)
- `tcp.port == 80` – HTTP traffic on port 80
- `udp.port == 53` – DNS traffic
- `http` – all HTTP packets
- `icmp` – ping (echo request/reply) traffic
- `tcp.port == 22` – SSH traffic
- `ip.addr == 192.168.56.102` – all traffic to/from my Ubuntu VM  
  *(change IP when needed)*
