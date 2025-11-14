# Subnet Cheatsheet

## Common CIDR → Netmask → Hosts

| CIDR | Netmask         | # of Hosts (usable) |
|------|-----------------|---------------------|
| /32  | 255.255.255.255 | 1 (single host)     |
| /30  | 255.255.255.252 | 2                   |
| /29  | 255.255.255.248 | 6                   |
| /28  | 255.255.255.240 | 14                  |
| /27  | 255.255.255.224 | 30                  |
| /26  | 255.255.255.192 | 62                  |
| /25  | 255.255.255.128 | 126                 |
| /24  | 255.255.255.0   | 254                 |
| /23  | 255.255.254.0   | 510                 |
| /22  | 255.255.252.0   | 1022                |

## Powers of 2 (for quick subnet math)

- 2⁰ = 1  
- 2¹ = 2  
- 2² = 4  
- 2³ = 8  
- 2⁴ = 16  
- 2⁵ = 32  
- 2⁶ = 64  
- 2⁷ = 128  
- 2⁸ = 256  

## Host calculation

- Number of host bits = 32 − CIDR  
- Number of hosts = `2^(host bits) - 2` (network + broadcast)

Example (for /24):

- 32 − 24 = 8 host bits  
- 2⁸ = 256  
- 256 − 2 = **254 usable hosts**

## Private IP ranges (IPv4)

- 10.0.0.0/8  
- 172.16.0.0/12  
- 192.168.0.0/16  

## Practice notes

- [ ] Split a /24 into 4 equal subnets  
- [ ] Find first and last usable IP in a subnet  
- [ ] Recognize if an IP is private or public  

