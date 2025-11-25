# Homelab Notes

## Windows Firewall and Ping

- Windows Firewall blocks inbound ping (ICMP) by default.
- To allow ping to Windows VM I enabled the **"File and Printer Sharing (Echo Request - ICMPv4-In)"** inbound rule.
  - Open: **Windows Defender Firewall with Advanced Security** → **Inbound Rules**
  - Find rules named **“File and Printer Sharing (Echo Request – ICMPv4-In)”**
  - Enable the rule for the profile in use (Private/Public).

## VirtualBox Lab Setup 

- VMs:
  - **Kali**
  - **Ubuntu**
  - **Windows 11**
- Network layout:
  - **Adapter 1: NAT** - internet access
  - **Adapter 2: Host-only** - `192.168.56.0/24` for VM to VM traffic
- Example host-only IPs
  - Kali: `192.168.56.101`
  - Ubuntu: `192.168.56.102`
  - Windows 11: `192.168.103`

## Checking IP Addresses

- **Kali / Ubuntu (Linux)**
  - hostname -I - quick list of IPs
  - `ip a` - detailed view of interfaces and addresses
- **Windows 11**
  - `ipconfig` - shows IPv4 addresses per adapter

## Services Enabled

### Windows 11

- Allow ICMP ping via te "Echo Request (ICMPv4-In)" firewall rule on the active profile.
- Used as Windows endpoint device for basic connectivity test from Kali/Ubuntu.

### Ubuntu

- SSH server installed:
  ```bash
  sudo apt install openssh-server
- SSH enabled and started:
  ```bash
  sudo systemctl enable --now ssh
- Tested from Kali using:
  ```bash
  ssh <ubuntu_user>@192.168.56.102
- Used as a simple web server for labs with:
  ```bash
  python3 -m http.server 8080

  
