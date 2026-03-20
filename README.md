# Home Network Security Assessment – iPhone Exposure and Traffic Analysis

## Project Goal

The purpose of this walkthrough was to assess whether an iPhone on a home network showed:

- unnecessary network exposure
- suspicious listening services
- abnormal traffic behavior
- possible signs of spyware-like activity at the network level

---

## Tools Used
- Kali Linux (WSL2)
- Windows PowerShell
- Nmap
- PktMon

---

## Step 1 – Identify Network Context

Command:
ip a

Result:
WSL2 interface detected (172.x.x.x) → NOT same as home network (192.168.x.x)

Conclusion:
WSL2 is isolated → cannot fully monitor LAN traffic

---

## Step 2 – Identify Devices on Network

Command:
arp -a

Key finding:
192.168.1.153 → iPhone

---

## Step 3 – Scan iPhone

Command:
nmap -sS -sV -O 192.168.1.153

Result:
- 998 closed ports
- 49152/tcp open (tcpwrapped)
- 62078/tcp open (tcpwrapped)
- OS: iOS 15.x (estimated)

Conclusion:
Minimal exposure, typical iPhone behavior

---

## Step 4 – Monitor Connections (Windows)

Command:
netstat -ano

Result:
- Multiple HTTPS connections (:443)
- Normal outbound traffic

Conclusion:
No obvious suspicious connections

---

## Step 5 – Packet Capture (PktMon)

Commands:
pktmon filter remove
pktmon filter add -i 192.168.1.153
pktmon start --capture --pkt-size 0

User activity:
- YouTube
- Safari
- Google browsing

Stop capture:
pktmon stop

Convert:
pktmon etl2txt PktMon.etl -o capture.txt

Result:
~1400 events captured

---

## Step 6 – Packet Analysis

Observed:
- ARP requests (router → iPhone)
- Normal LAN communication
- No suspicious outbound patterns

Example:
Request who-has 192.168.1.153 tell 192.168.1.1

Conclusion:
Normal network behavior

---

## Final Conclusion

The iPhone showed:

- minimal exposed services
- normal ARP communication
- no visible suspicious traffic patterns

No evidence of spyware was observed at the network level during this assessment.

---

## Key Lessons

- Always verify network context (WSL vs LAN)
- ARP is critical for device identification
- Nmap shows exposure, not infection
- Packet capture depends on correct interface
- Tool limitations are part of real-world analysis

---

## Author

Tomasz (Tomason34)
