# Blueprint
In this section I will go in depth a bit about the initial setup and a quick overview on what we will be doing

<img width="842" height="785" alt="Screenshot 2026-04-30 155845" src="https://github.com/user-attachments/assets/f7c55fdb-1398-4ffe-b2db-d0665bc833f7" />

Before we install the VMs (Virtual Machine) we have to install a hypervisor. I usually go with Oracle becaues its easier for me and that is also what I am used to.
After the hypervisor is installed we can move on to the installation of our VMs:

| System | Version | Link|
|----|----|----|
| Windows VM | Windows 10 | https://www.microsoft.com/en-ca/software-download/windows10 |
| Active Directory | Windows Server 2022 ISO | https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022|
| Linux | Kali Linux | https://www.kali.org/get-kali/#kali-virtual-machines|

> **Note on Kali Linux**: I used a pre-built virtual machine as we don't need to add our on specific configurations

For this project we will need 4 Virtual Machines. You don't need for as you can susitute the virtual machiens for a docker container if you lack the resources. I wanted to emulate a real world environment as close as possbile so I used my previous ELK stack setup instead of install a new one or installing a Splunk server.
Each virtual machine has its own purpose and function in this project. As I probably mentioned before this project is to help us understand how system logs work, how to detect those logs, how to understand / be able to read those logs and how to respond to them. Likewise potentially creating alerts, hardening systems or adding more security / automation tools.

I will list the reason for each virutal machine below:
Windows 10: This machine is mainly to act as a victim. This is where you can log onto as 2 users (We will create them using our Domain Controller: HR and IT). This is also where our **winlogbeat**, **Sysmon** and **Atomic Read Team** will sit.
Active Directory: We will set this machine as our **Domain Controller**. A domain controller is a active directory server which manages users allocation to resources, security policies and user authentication. It is important to have this setup properly as it will allow us to simulate a more realistic attack on an environmnet.
Kali Linux: Kali needs no introduction, its the attacking machine that will be used for reconnaciance, basic attacks and potentially payload creations. Dependning on your level you can do whatever with this machine or use another machine for attacking.

As for how they will all communicate, I went for a briged network. This connects multiple different virutal machine segments into one physical network, making them part of one logical network. In the context of our virtual machine, Oracle VirutalBox will take these machines appear as physical devices connected to the same network as the host. This way each machine act as stand alone devices, with their own IP Address on existing local network.

For how I have set these up you can refer to my Setup Folder where show through screenshot, how I have setup the machines and any tools that I used along with troubleshooting any errors that occured,
