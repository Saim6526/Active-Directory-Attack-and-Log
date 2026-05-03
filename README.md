

# Active Directory Project

## Project Overview
The goal of this project was to build a comprehensive, end-to-end laboratory environment to master the relationship between **Active Directory (AD)**, **Windows telemetry**, and **SIEM (Security Information and Event Management)**. 

### **Why I Built This**
While learning about security operations, I realized that understanding "how to defend" requires a deep understanding of "how to see." I noticed a gap in my knowledge regarding how Windows logs are generated and structured. 

Instead of just reading about attacks, I wanted to:
1.  See exactly how **Active Directory** events look when an adversary moves through a network.
2.  Practice the "Defensive Lifecycle": Setup → Attack Simulation → Log Collection → Analysis.
3.  Prove my technical proficiency by building a functional environment that mirrors a corporate SOC infrastructure.

---

## Architecture & Network Design

This lab is built on a distributed architecture to ensure realistic log flow and network behavior. 

<img width="842" height="785" alt="Screenshot 2026-04-30 155845" src="https://github.com/user-attachments/assets/f7c55fdb-1398-4ffe-b2db-d0665bc833f7" />


### **The Bridged Network Strategy**
A key design choice was using a **Bridged Network** for all Virtual Machines rather than a standard NAT or Host-Only setup.

*   **Real-World Visibility**: In a bridged setup, every VM acts as a distinct node on the physical network with its own IP address.
*   **Log Integrity**: This ensures that the SIEM (ELK Stack) sees the actual source IP of the Windows machines and the Attacker (Kali), rather than a translated NAT gateway address, making log analysis much more accurate.
*   **Infrastructure Testing**: It forced me to handle real-world networking hurdles, such as ensuring the Windows 10 client correctly utilized the Domain Controller for DNS resolution to join the domain successfully.

---

> **Note on SIEM**: I have already talked about the setup and the setup process in my previous project on "DVWA".

## 🛠️ Technology Stack
This project integrates multiple industry-standard tools to ensure its create telemetry as close as it can be to a real attack:

*   **Directory Services**: Windows Server 2022 (Active Directory Domain Services).
*   **Endpoint Systems**: Windows 10 Pro (The primary target for simulations).
*   **Attacker Platform**: **Kali Linux** (Used for manual testing and initial foothold simulation).
*   **SIEM Environment**: **ELK Stack** (Elasticsearch, Logstash, Kibana) deployed via **Docker** on an Ubuntu host.
*   **Telemetry Tools**: 
    *   **Sysmon**: For granular process and network event logging.
    *   **Winlogbeat**: For shipping Windows Event and Sysmon logs to the SIEM.
*   **Adversary Simulation**: **Atomic Red Team** (Invoke-AtomicRedTeam) to trigger MITRE ATT&CK mapped behaviors.

---

## Scaling & Future Improvements
This project serves as a foundational "Proof of Concept." It is fully functional, but designed to be scaled:

*   **Security Hardening**: The current setup is a "base" configuration. In the future you could implement **Group Policy Objects (GPOs)** to enforce least privilege and monitoring how those changes affect attack success rates.
*   **Automated Response**: Integrating tools like **Wazuh** or custom **Python/Bash** scripts to automation, e.g. automatically block IPs (via iptables or Windows Firewall) based on high-confidence alerts from the SIEM.
*   **Firewalls Integration**: Later on you could implement firewalls, IDPS or EDR solutions as well. This would help in understanding how those tools work and aid in handleing attacks and threats 

---

## 💻 Hardware Requirements & Specs
Running a multi-VM lab with a SIEM stack is resource-intensive. Below are the specs used for this build and the recommended minimum for a smooth experience.

| Component | Recommended Specs |
| :--- | :--- |
| **RAM** |  16 GB |
| **Storage** | 250GB+ NVMe SSD |

> **Note on Specs**: We won't be using all of it, but its good to have just enough to ensure a smooth experience
> **Note on RAM Allocation**: The ELK stack (Elasticsearch) and Windows Server are the most resource-heavy. Ensure your host machine has enough overhead to prevent system instability during simulation.
