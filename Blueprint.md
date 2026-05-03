# Blueprint

In this section, I will go in-depth about the initial setup and provide a quick overview of the project architecture.

---

<img width="842" height="785" alt="Screenshot 2026-04-30 155845" src="https://github.com/user-attachments/assets/6d025800-2ae9-4372-a390-6e78045800fd" />

### **The Hypervisor**
Before installing the VMs (Virtual Machines), we have to install a hypervisor. I usually go with **Oracle VirtualBox** because it is easier for me and it is what I am used to.

### **Virtual Machine Inventory**
After the hypervisor is installed, we can move on to the installation of our machines:

| System | Version | Link |
| :--- | :--- | :--- |
| **Windows VM** | Windows 10 | [Download](https://www.microsoft.com/en-ca/software-download/windows10) |
| **Active Directory** | Windows Server 2022 ISO | [Download](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) |
| **Linux** | Kali Linux | [Download](https://www.kali.org/get-kali/#kali-virtual-machines) |

> **Note on Kali Linux**: I used a pre-built virtual machine as we don't need to add our own specific configurations for this phase.

### **Deployment Logic**
For this project, we need four Virtual Machines. If you lack hardware resources, you could substitute the SIEM for a docker container as that is what I did, but you don't have to. To do this, I integrated my **previous ELK stack setup** instead of installing a new one or using Splunk.

Each virtual machine has a specific purpose. As mentioned before, this project is designed to help us understand how system logs work, how to detect them, how to read them, and how to respond. This setup also allows for future hardening, alerting, and automation.

#### **Machine Roles:**
*   **Windows 10**: This acts as the victim machine. We will log on as two different users (HR and IT, created via the Domain Controller). This is where **Winlogbeat**, **Sysmon**, and **Atomic Red Team** are installed.
*   **Active Directory**: This is our **Domain Controller**. It manages user allocation, security policies, and authentication. Setting this up properly is vital for simulating a realistic corporate environment. We will create two users (IT and HR) for a simulation of lateral movement.
*   **Kali Linux**: Kali needs no introduction—it is the attacking machine used for reconnaissance and basic attacks. Depending on your skill level, you can use this for manual testing or payload creation.

### **Network Communication**
For communication, I chose a **Bridged Network**. This connects the virtual machines into one physical network, making them part of one logical network. Oracle VirtualBox makes these machines appear as physical devices connected to the same network as the host. Each machine acts as a standalone device with its own IP address on the existing local network.

---

**Next Steps**: For the specific configuration steps, refer to my **Setup** folder, where I show screenshots of the process, the tools used, and how I troubleshot any errors that occurred.
