# Active Directory & SIEM Monitoring Lab

## Project Overview
The goal of this project was to build a comprehensive, end-to-end laboratory environment to master the relationship between **Active Directory (AD)**, **Windows telemetry**, and **SIEM (Security Information and Event Management)**. This setup provides a functional environment that mirrors a corporate SOC infrastructure to practice the "Defensive Lifecycle": Setup → Attack Simulation → Log Collection → Analysis.

### **Why I Built This**
*   **Log Visibility**: To fill a gap in understanding how Windows logs are generated and structured.
*   **Adversary Awareness**: To see exactly how **Active Directory** events look when an adversary moves through a network.
*   **Skill Demonstration**: To prove technical proficiency by building a functional environment that mirrors a corporate infrastructure.

---

## Architecture & Network Design
This lab is built on a distributed architecture to ensure realistic log flow and network behavior.

<img width="842" height="785" alt="Architecture Diagram" src="https://github.com/user-attachments/assets/f7c55fdb-1398-4ffe-b2db-d0665bc833f7" />

### **The Bridged Network Strategy**
A **Bridged Network** was chosen for all Virtual Machines to ensure real-world visibility. 

*   **Real-World Visibility**: Every VM acts as a distinct node on the physical network with its own IP address.
*   **Log Integrity**: This ensures the SIEM sees the actual source IP of the Windows machines and the Attacker, rather than a translated NAT address, making analysis more accurate.
*   **Infrastructure Testing**: It forced me to handle real-world networking hurdles, such as ensuring the Windows 10 client correctly utilized the Domain Controller for DNS resolution.

---

## Technology Stack
This project integrates industry-standard tools to ensure telemetry is as close as possible to a real-world attack.

*   **Directory Services**: Windows Server 2022 (Active Directory Domain Services).
*   **Endpoint Systems**: Windows 10 Pro (The primary target for simulations).
*   **Attacker Platform**: **Kali Linux** (Used for manual testing and initial foothold simulation).
*   **SIEM Environment**: **ELK Stack** (Elasticsearch, Logstash, Kibana) deployed via **Docker** on an Ubuntu host.
*   **Telemetry Tools**: **Sysmon** for granular process logging and **Winlogbeat** for shipping logs to the SIEM.
*   **Adversary Simulation**: **Atomic Red Team** (Invoke-AtomicRedTeam) to trigger MITRE ATT&CK mapped behaviors.

---

## Scaling & Future Improvements
This project serves as a foundational "Proof of Concept." It is fully functional but designed to be scaled:

*   **Security Hardening**: Implementing **Group Policy Objects (GPOs)** to enforce least privilege and monitoring how those changes affect attack success rates.
*   **Automated Response**: Integrating tools like **Wazuh** or custom **Python/Bash** scripts to automatically block IPs based on high-confidence alerts.
*   **Firewall Integration**: Implementing firewalls, IDPS, or EDR solutions to aid in handling threats.

---

## Hardware Requirements & Specs
Running a multi-VM lab with a SIEM stack is resource-intensive. Below are the recommended minimum specs for a smooth experience.

| Component | Recommended Specs |
| :--- | :--- |
| **RAM** | 16 GB |
| **Storage** | 250GB+ NVMe SSD |

> **Note on SIEM**: I have previously documented the ELK setup process in my "DVWA" project.
> **Note on RAM Allocation**: The ELK stack and Windows Server are the most resource-heavy; ensure your host machine has enough overhead to prevent system instability.

---

**Next Steps**: For specific configuration steps, tool setups, and troubleshooting, refer to the **`Setup/`** folder.
