Once you have your Windows and Windows Server 2022 setup we will now move onto installing Sysmon and Winlogbeat. The process on windows is the same on the server and vice versa.
Before we being the install I reccommend installing notepad++ for when we edit the configuration files later on. Now to set it up we will first need to get the winlogbeat and sysmon file.
Here are the links:
Winlogbeat: https://www.elastic.co/downloads/beats/winlogbeat
Sysmon: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
Bounus Sysmon Config by SwiftOnSecurity: https://github.com/SwiftOnSecurity/sysmon-config


# Sysmon
Once sysmon and the sysmon config is installed. We will first move the config file to the sysmon folder, after that we will open up powershell and go the sysmon folder there as well. Sysmon should be in your Program Files folder. In powershell paste "cd C:\Program File\Sysmon" then to install sysmon paste ".\Sysmon64.exe -i .\sysmon_config.xml -accepteula" for the config file, depending on what you named it, add that. Once that is done we can do "Get-Service sysmon" to see if its active.

<img width="660" height="381" alt="Proper_Install_Sysmon" src="https://github.com/user-attachments/assets/6cc14d13-d88c-4f79-9d4f-6646050308a5" />

> **Note**: if you are not in the right directory you will get the "CommandNotFoundExpectation" Error
<img width="824" height="237" alt="Wrong_Sysmon_Install_and_Directory" src="https://github.com/user-attachments/assets/052349a1-6d7e-4541-8e6c-f2d4886a3fd4" />


Another way to confirm if its active is by going to event viewer and going to sysmon (I don't remember the path, just figure it out).
<img width="1014" height="713" alt="Sysmon Verfication" src="https://github.com/user-attachments/assets/e476e1d6-ff26-4e07-8da4-936af2bcfd57" />

# Winlogbeat
For winlogbeat the process if fairly similar to that of sysmon. Once you have installed winlogbeat, added it to a folder in Program File and Extracted the content, we will then move onto powershell. In powershell cd to the winlogbeat directory and run this command ".\install-service-winlogbeat.ps1" while doing this you might get a "Execution Policy" Error. To fix that we can simply type "powershell.exe -ExecutionPolicy UnRestricted -File .\install-service-winlogbeat.ps1". That should cause it run, then we will get a Security Warning from Winlogbeat where we have to pick between running, not running and suspending. We will run so type "r ".

<img width="1003" height="634" alt="Winlogbeat_Install_with_Troubleshooting" src="https://github.com/user-attachments/assets/e65ce40f-73ca-4e46-a7f8-08b587cd7e6c" />

For the step im going to talk about, you can do this before, after or during the test. This is to ensure you get the logs where you want them. Open your config file "winlogbeat.yml". Scroll down to "Logstash Output" and add your ELK Server or SIEM Server IP, Port, any Username and/ or password that you need and make sure to comment out the "output.elasticsearch".
<img width="1018" height="749" alt="Winlogbeat_Config_File_Setup" src="https://github.com/user-attachments/assets/01115b51-1518-408c-8a24-76c4cccec113" />

To test if your config file is working we will go to powershell paste ".\winlogbeat.exe test config -c .\winlogbeat.yml -e" if we get a "Config OK" text near the bottom of the output, it means the config works. After that we start the service.
<img width="998" height="724" alt="Winlogbeat_Config_TestandStart" src="https://github.com/user-attachments/assets/0fca9108-bad6-443b-8686-05c969764a5d" />

> **Note**: If you need to see if logs are flowing, type ".\winlogbeat. exe -e -c .\winlogbeat.yml -d "publish" "
<img width="1016" height="722" alt="Winlogbeat_Logs_Working" src="https://github.com/user-attachments/assets/0c362b0c-b32a-48a5-b9f0-1bf1365a040b" />

