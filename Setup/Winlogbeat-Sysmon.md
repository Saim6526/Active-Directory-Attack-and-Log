# Endpoint Logging Setup: Sysmon & Winlogbeat

Once you have your Windows and Windows Server 2022 setup, we will now move onto installing Sysmon and Winlogbeat. The process on Windows is the same on the Server and vice versa.

Before we begin the install, I recommend installing **Notepad++** for when we edit the configuration files later on. Now to set it up, we will first need to get the Winlogbeat and Sysmon files.

### Resources
*   **Winlogbeat**: [Download here](https://www.elastic.co/downloads/beats/winlogbeat)
*   **Sysmon**: [Download here](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
*   **Bonus Sysmon Config by SwiftOnSecurity**: [GitHub Link](https://github.com/SwiftOnSecurity/sysmon-config)

---

# Sysmon

Once Sysmon and the Sysmon config are installed, we will first move the config file to the Sysmon folder. After that, we will open up PowerShell as an Administrator and navigate to the Sysmon folder. Sysmon should be in your `Program Files` folder.

1.  In PowerShell, run: `cd "C:\Program Files\Sysmon"`.
2.  To install Sysmon with the config, run: `.\Sysmon64.exe -i .\sysmon_config.xml -accepteula`.
    *   *Note: If you named your config file something else, use that name in the command instead.*
3.  Once that is done, run `Get-Service sysmon` to see if it is active.

<img width="660" height="381" alt="Proper_Install_Sysmon" src="https://github.com/user-attachments/assets/6cc14d13-d88c-4f79-9d4f-6646050308a5" />

> [!CAUTION]
> If you are not in the right directory, you will get the `CommandNotFoundException` error.

<img width="824" height="237" alt="Wrong_Sysmon_Install_and_Directory" src="https://github.com/user-attachments/assets/052349a1-6d7e-4541-8e6c-f2d4886a3fd4" />

Another way to confirm it is active is by going to **Event Viewer** and following this path:
`Applications and Services Logs` > `Microsoft` > `Windows` > `Sysmon` > `Operational`

<img width="1014" height="713" alt="Sysmon Verfication" src="https://github.com/user-attachments/assets/e476e1d6-ff26-4e07-8da4-936af2bcfd57" />

---

# Winlogbeat

For Winlogbeat, the process is fairly similar to that of Sysmon. Once you have installed Winlogbeat, added it to a folder in `Program Files`, and extracted the content, we will move onto PowerShell.

1.  In PowerShell, `cd` to the Winlogbeat directory.
2.  Run this command: `.\install-service-winlogbeat.ps1`.

> [!TIP]
> If you get an **Execution Policy** error, run:
> `powershell.exe -ExecutionPolicy UnRestricted -File .\install-service-winlogbeat.ps1`.
> When the Security Warning appears, type `r` to run the script.

<img width="1003" height="634" alt="Winlogbeat_Install_with_Troubleshooting" src="https://github.com/user-attachments/assets/e65ce40f-73ca-4e46-a7f8-08b587cd7e6c" />

### Configuration
This step ensures you get the logs where you want them. Open your config file `winlogbeat.yml`:
*   Scroll down to **Logstash Output**.
*   Add your ELK Server or SIEM Server IP and Port.
*   Add any Username and/or password if required.
*   Make sure to **comment out** the `output.elasticsearch` section.

<img width="1018" height="749" alt="Winlogbeat_Config_File_Setup" src="https://github.com/user-attachments/assets/01115b51-1518-408c-8a24-76c4cccec113" />

### Testing and Starting
To test if your config file is working, run:
`.\winlogbeat.exe test config -c .\winlogbeat.yml -e`.

If you get a **"Config OK"** text near the bottom of the output, the config works. After that, you can start the service.

<img width="998" height="724" alt="Winlogbeat_Config_TestandStart" src="https://github.com/user-attachments/assets/0fca9108-bad6-443b-8686-05c969764a5d" />

> [!NOTE]
> If you need to see if logs are flowing, type:
> `.\winlogbeat.exe -e -c .\winlogbeat.yml -d "publish"`.

<img width="1016" height="722" alt="Winlogbeat_Logs_Working" src="https://github.com/user-attachments/assets/0c362b0c-b32a-48a5-b9f0-1bf1365a040b" />
