# Home Network Security Assessment – iPhone Exposure and Traffic Analysis (FULL RAW WALKTHROUGH)

## Step 1 – Network Context (Kali WSL2)

### Command
ip a

### Raw Output
inet 172.23.143.138/20 brd 172.23.143.255 scope global eth0

### Analysis
WSL2 uses a virtual network (172.x.x.x), not the real LAN (192.168.x.x).
This means Kali cannot directly observe Wi-Fi traffic from the iPhone.

---

## Step 2 – Identify Devices (Windows)

### Command
arp -a

### Raw Output
Interface: 192.168.1.158
192.168.1.1
192.168.1.160

### Analysis
Identified active LAN devices.
iPhone later confirmed manually as 192.168.1.153.

---

## Step 3 – Nmap Scan

### Command
nmap -sS -sV -O 192.168.1.153

### Raw Output
PORT      STATE SERVICE    VERSION
49152/tcp open  tcpwrapped
62078/tcp open  tcpwrapped

OS details: Apple iOS 15.x

### Analysis
- Only 2 open ports
- Both tcpwrapped → restricted access
- Minimal attack surface

---

## Step 4 – Active Connections

### Command
netstat -ano

### Raw Output
Multiple connections to :443 (HTTPS)

### Analysis
Normal encrypted traffic behavior.

---

## Step 5 – Packet Capture

### Commands
pktmon filter remove
pktmon filter add -i 192.168.1.153
pktmon start --capture --pkt-size 0

### Activity
YouTube, Safari browsing

### Stop
pktmon stop

### Convert
pktmon etl2txt PktMon.etl -o capture.txt

### Raw Output
Events formatted: 1403

### Analysis
Successful packet capture.

---

## Step 6 – Packet Analysis

### Raw Output
ARP request: who-has 192.168.1.153 tell 192.168.1.1

### Analysis
- Normal ARP communication
- No suspicious traffic patterns observed

---

## Final Conclusion

No evidence of spyware detected at network level.
Behavior consistent with normal iPhone usage.

---

## Key Technical Lessons

- WSL2 ≠ LAN visibility
- ARP confirms device presence
- Nmap reveals exposure, not compromise
- Packet capture requires correct interface

---

## Author
Tomasz (Tomason34)
