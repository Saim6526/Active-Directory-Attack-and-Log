This was my first attack from kali to the windows machine. This was mainly to test if the systems were working like intended and if the logs were being properly generated and forwarded.

This was very simple so this page wont be very long. I opened up my kali machine and did the usual "sudo apt update && sudo apt upgrade -y" 
after that I installed crowbar and moved my rockyou file from the "/usr/share/wordlists/" direcotry to the same direcotry as crowbar. once that was done I copied the first 10 passwords from rockyou and moved it to another file with a different name
Then I added a correct password at the end. This was to ensure that we get a "successful logon" message after multiple "failed logon". 


Crowbar sadly didnt work for me so I moved to hydra instead and it worked just fine.
