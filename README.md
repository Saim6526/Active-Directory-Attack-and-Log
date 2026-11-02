# Active Directory Attack & Detection Lab

I built a small corporate network (a Windows domain, a domain-joined workstation, a Kali attacker, and an ELK SIEM), attacked it, and then went looking for my own activity in the logs. The point was to see what an attack looks like from the defender's chair.

![Kibana showing repeated failed logons (4625) followed by a successful logon (4624)](https://github.com/user-attachments/assets/008da5b9-0c12-439e-8560-fed74c85eabf)

*Kibana view of the RDP brute force from Kali: a run of failed logons, then one success.*

## Results at a glance

| Attack | Tool | MITRE ATT&CK | What the logs showed | Status |
|---|---|---|---|---|
| RDP brute force, Kali → Windows 10 | Hydra | T1110.001 Password Guessing | Many Event ID 4625 (failed logon), then a single 4624 (successful logon) | Visible in Kibana. No alert rule yet. |

**Scope, honestly:** this first test was deliberately small. I used 10 passwords from `rockyou.txt` and added the real password to the end of the list so the logs would show failures followed by a success. It proves the pipeline works end to end (attack → Windows telemetry → SIEM). It is not a realistic attacker simulation.

## How it played out

**1. Telemetry flowing.** Sysmon and Winlogbeat on the Windows machines ship events to Logstash. Before attacking anything, I confirmed logs were actually reaching the SIEM.

![Winlogbeat publishing events to the SIEM](https://github.com/user-attachments/assets/0c362b0c-b32a-48a5-b9f0-1bf1365a040b)

**2. The attack.** Crowbar did not work against RDP in this setup, so I switched to Hydra, which found the password after several failed attempts.

![Hydra finding the RDP password](https://github.com/user-attachments/assets/206cd11b-c8e8-4b2f-851e-3d12305e446f)

**3. The detection.** In Kibana the attack is a clear pattern: a burst of Event ID 4625 followed by one 4624 (see the screenshot at the top). A single failed logon means nothing. The burst is the signal.

## What I learned

- **The pattern is the detection.** One 4625 is noise. Many of them from one source, ending in a 4624, is worth alerting on.
- **DNS is the first thing that breaks in AD.** The Windows 10 client could not join the domain until its preferred DNS pointed at the Domain Controller.

  ![Domain join error when DNS does not point to the DC](https://github.com/user-attachments/assets/87e1569e-74d9-4d55-add5-2db5d8961ceb)

- **Bridged networking was worth the extra work.** Every VM has its own real IP, so the SIEM sees the true attacker address instead of a NAT-translated one.

## Architecture

![Lab architecture diagram](https://github.com/user-attachments/assets/f7c55fdb-1398-4ffe-b2db-d0665bc833f7)

| Machine | Role |
|---|---|
| Windows Server 2022 | Domain Controller (AD DS), static IP, domain `oracle.local`, with IT and HR users |
| Windows 10 | Domain-joined victim. Sysmon (SwiftOnSecurity config) and Winlogbeat ship logs to the SIEM |
| Kali Linux | Attacker |
| ELK Stack (Docker, Ubuntu host) | Logstash → Elasticsearch → Kibana. Reused from my [web attack detection project](https://github.com/Saim6526/Web-Attack-Detection-Using-ELK) |

All VMs run in VirtualBox on a bridged network.

## Go deeper

- [`Blueprint.md`](Blueprint.md): architecture and design decisions
- [`Setup/Active Directory.md`](Setup/Active%20Directory.md): domain controller, users, domain join, and the DNS error
- [`Setup/Winlogbeat-Sysmon.md`](Setup/Winlogbeat-Sysmon.md): endpoint logging pipeline
- [`Attack-and-Log/Brute-Force-Attack.md`](Attack-and-Log/Brute-Force-Attack.md): the attack, step by step, with log analysis

## Next up (not done yet)

1. **Detection rule:** a Kibana alert that fires on more than 10 Event ID 4625 from one source IP in a short window.
2. **Mitigation test:** apply an account lockout GPO, rerun the attack, and compare what the logs show.
3. **More attack coverage:** Atomic Red Team tests mapped to ATT&CK, and an IT/HR lateral movement scenario.
4. **Network control:** restrict RDP to authorized management IPs with Windows Firewall.

<details>
<summary>Hardware used</summary>

Running four VMs plus a SIEM is resource-heavy. This lab ran on 16 GB RAM and a 250 GB+ NVMe SSD. ELK and Windows Server are the heaviest parts.

</details>
