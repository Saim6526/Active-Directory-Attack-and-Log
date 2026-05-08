# Attack Simulation: RDP Brute Force & Log Analysis

This was my first attack from Kali Linux to the Windows machine. The main goal was to test if the systems were working as intended and ensure that logs were being properly generated and forwarded to the SIEM.

## 1. Enabling Remote Desktop Protocol (RDP)

Before we can begin the attack, we need to enable **Remote Desktop Protocol (RDP)**. This allows our Kali machine to attempt a connection and move around on the Windows workstation and server.

1.  Search for **View advanced system settings** in the taskbar.
2.  In the **System Properties** window, navigate to the **Remote** tab.
3.  Select **Allow remote connections to this computer**.

<img width="1018" height="841" alt="Advance_System_Settings" src="https://github.com/user-attachments/assets/14f21414-9b48-47ad-aaf9-f70270a93a3a" />

<img width="1022" height="841" alt="image" src="https://github.com/user-attachments/assets/5365fb85-4dbf-4e00-b37d-a6ea7eb2d2cc" />

Next, add the specific users you want to have remote access. (You can add Active Directory groups here as well, though I skipped that for this initial test).

<img width="1021" height="842" alt="image" src="https://github.com/user-attachments/assets/bb17c7b2-9fb5-423c-8ac5-fd5b778f1d28" />

---

## 2. Attacker Preparation (Kali Linux)

The preparation on the Kali side is straightforward. First, I ran the usual updates:
`sudo apt update && sudo apt upgrade -y`

I installed **Crowbar** and moved the `rockyou.txt` wordlist from the `/usr/share/wordlists/` directory to the same directory as Crowbar. To keep the test quick, I copied the first 10 passwords from `rockyou.txt` into a new file.

<img width="643" height="510" alt="image" src="https://github.com/user-attachments/assets/db4c10d1-58b3-4547-b5e5-97d408e7632b" />

I manually added the **correct password** to the very end of this new list. This ensures that the logs will show multiple "Failed Logon" messages followed by a single "Successful Logon," simulating a successful brute force.

<img width="621" height="135" alt="image" src="https://github.com/user-attachments/assets/412ec12a-e08d-41c6-9d06-c82c4f385ed4" />

---

## 3. Executing the Attack

Crowbar unfortunately didn't work as expected for this specific setup, so I switched to **Hydra**. 

<img width="644" height="529" alt="image" src="https://github.com/user-attachments/assets/206cd11b-c8e8-4b2f-851e-3d12305e446f" />

Using Hydra, the attack was successful, and I was able to identify the correct password after several failed attempts.

---

## 4. Log Analysis (SIEM/ELK)

Once the attack was finished, I checked the SIEM to see how the telemetry looked. In the logs, we can clearly see the pattern of the attack:
* **Event ID 4625**: An account failed to log on (Multiple entries).
* **Event ID 4624**: An account was successfully logged on (Final entry).

This sequence is a clear indicator of a successful brute force attempt.

<img width="1600" height="833" alt="image" src="https://github.com/user-attachments/assets/008da5b9-0c12-439e-8560-fed74c85eabf" />

---

## Next Steps

Now that we have confirmed that the attack is visible in our SIEM, the next steps could involve preventing the attacks:

1. **Account Lockout Policies**: I haven't tired it yet, but you can implement a GPO (Group Policy Object) its a collection of settings that define how a system behaves and what is looks like for the users and computer. This could be something like locking the user account after a certain number of failed login (e.g., 5 attempts in 30 mins)
2.  **Alerting**: Create a specific alert in ELK/Kibana that triggers when more than 10 `4625` events occur from a single source IP within a short window.
3.  **Firewall Rules**: Configure the Windows Firewall to only allow RDP traffic from specific authorized management IPs, rather than the entire network.
