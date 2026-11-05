# Wireshark-labs
Hands-on Wireshark labs where I investigate network captures as a SOC analyst, tracking attackers, hidden data, exposed passwords, and a scam call.
| Lab | Topic | Key skill |
|-----|-------|-----------|
| 03 | Identify the attacker's IP | Spotting a SYN port scan and open/closed ports |
| 04 | Follow TCP Stream | Reading a plain text chat on TCP port 5000 |
| 05 | Follow TCP Stream, part 2 | Finding a hidden note in an HTTP page |
| 06 | HTTP plain text credentials | Finding login POSTs in unencrypted traffic |
| 07 | FTP brute force catch | Spotting failed logins (530) and the success (230) |
| 08 | VoIP call replay | Decoding SIP and RTP and replaying a call |

---

# Lab 3: Identify the Attacker's IP

## Scenario
You are a SOC analyst. A staff member reported unusual traffic to the
company's web server. I captured the packets and analyzed them in Wireshark
to find the type of attack and the attacker's IP address.

## Findings
- Attacker: 192.168.10.66
- Target: 192.168.10.20
- Attack type: port scan (one source port, 52311, probing 40 destination ports)
- Open ports: 22, 80, 443 (SYN, ACK)
- Closed ports: 37 (RST, ACK)

## Key filter
tcp.flags.syn == 1 && tcp.flags.ack == 0

## Lesson
One source sending SYNs to many ports on one target is a port scan.
Defenses: firewall rules, rate limiting, and an IDS/IPS.

![](https://github.com/deemanuel2/Wireshark-labs/blob/8263a8e67f6421996504185ab52fd47c091a176f/Screenshot%202026-11-05%20202051.png)
