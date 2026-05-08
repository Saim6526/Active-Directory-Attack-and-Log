This was my first attack from kali to the windows machine. This was mainly to test if the systems were working like intended and if the logs were being properly generated and forwarded.

Before we begin with any attack, we want to enable remote desktop protocol (RDP) this will allow our kail machine to connect and move around on our windows macine and server.
In order to do that we go to advance system settings
<img width="1018" height="841" alt="Advance_System_Settings" src="https://github.com/user-attachments/assets/14f21414-9b48-47ad-aaf9-f70270a93a3a" />

From there we go to "Remote" then click on "Allow remote access"
<img width="1022" height="841" alt="image" src="https://github.com/user-attachments/assets/5365fb85-4dbf-4e00-b37d-a6ea7eb2d2cc" />

Then we add the users we want to have remote access (You can add the active directory as well, I didnt for now).
<img width="1021" height="842" alt="image" src="https://github.com/user-attachments/assets/bb17c7b2-9fb5-423c-8ac5-fd5b778f1d28" />


This was very simple so this page wont be very long. I opened up my kali machine and did the usual "sudo apt update && sudo apt upgrade -y" 
after that I installed crowbar and moved my rockyou file from the "/usr/share/wordlists/" direcotry to the same direcotry as crowbar. once that was done I copied the first 10 passwords from rockyou and moved it to another file with a different name.
<img width="643" height="510" alt="image" src="https://github.com/user-attachments/assets/db4c10d1-58b3-4547-b5e5-97d408e7632b" />


Then I added a correct password at the end. This was to ensure that we get a "successful logon" message after multiple "failed logon". 
<img width="621" height="135" alt="image" src="https://github.com/user-attachments/assets/412ec12a-e08d-41c6-9d06-c82c4f385ed4" />


<img width="644" height="529" alt="image" src="https://github.com/user-attachments/assets/206cd11b-c8e8-4b2f-851e-3d12305e446f" />
Crowbar sadly didnt work for me so I moved to hydra instead and it worked just fine.

Logs
Here you can see the multiple event code 4625s followed by a 4624. Indicating that the attacker had a successful brute force attempt. 
<img width="1600" height="833" alt="image" src="https://github.com/user-attachments/assets/008da5b9-0c12-439e-8560-fed74c85eabf" />


What should be our next actions?

