<img width="436" height="373" alt="Password_Creation" src="https://github.com/user-attachments/assets/b37764e9-627a-4b74-9eff-91f003d6299f" />In this seciton I will do showcase how I setup 2 users (IT and HR), gave the server a static IP along with turning my Windows server into a Domain Controller. Lets start with the IP.

This is probably the simplest step as all you need to go to setting > network & internet > status > change adapter options > right click the interface > click on properties > double click Internet Protocol Version 4 > Click on "Use the following IP Address" and then add in the Satic IP, Subnet, Gateway IP, DNS.
Once that is done we can go onto promoting the server to a domain controller

1: <img width="1024" height="840" alt="Step_1_Add_Role_Features" src="https://github.com/user-attachments/assets/e12041b0-30c4-4b0a-aef8-6da4c01d8a91" />
When you open up your windows server you should have the server manager application open. if not you can just type it in the search tab then open. Once its open you should see 4 options on the top right, click on manage > add role and featuers

2: <img width="782" height="556" alt="Step_2_Role_Based" src="https://github.com/user-attachments/assets/e0336b4d-b0ec-4a01-942e-24a05d95dc67" />
Click next till you see this screen here we want to select "Role Based or Feature Based"

3: <img width="1019" height="777" alt="Step_3_Select_Server" src="https://github.com/user-attachments/assets/2b29acdf-ac9c-4b95-a3c9-f6811813ecbf" />
Then select the sever you want to promote. In my case its just one so I will select that, then click next.

4: <img width="1021" height="839" alt="Step_4_Select_Domain_Services" src="https://github.com/user-attachments/assets/51770b0b-203d-45e0-be5c-49db36577ae2" />
Here we want to select "Active Directory Domain Service" then next.

5: <img width="1019" height="843" alt="Step_5_Click_Next_Until_Install" src="https://github.com/user-attachments/assets/1b6be094-a9b8-465d-8578-cef825eb6de0" />
Then we keep clicking next till we see install.

6: <img width="1026" height="842" alt="Step_6_Promote_to_Domain" src="https://github.com/user-attachments/assets/083ac9f8-129b-403a-b83f-4731998d49d9" />
Once that is done, if we click the flag we should see "promote this server to domain controller" After that we should be given a setup up page like before

7: <img width="1023" height="841" alt="Step_7_New_Forest" src="https://github.com/user-attachments/assets/41478593-db82-4038-88d3-f71c965b066e" />
We want to select "New Forest" and then we need to give it a name with a domain. I used "oracle.local" you can use any.

8: <img width="1020" height="837" alt="Step_8_Next_for_All_Install" src="https://github.com/user-attachments/assets/200aece8-2ed6-494e-a61a-0b5674c5c6eb" />
Then keep clicking next till you see the install. After that we should be all done with the setup for the domain controller.

After that is done we will add 2 users one for IT and HR. This is so that if we ever try an attack simulation where we want to pivot and escalate our previllage, we will be able to simulate that.

1: <img width="1018" height="836" alt="Adding_Members" src="https://github.com/user-attachments/assets/20614aad-749f-4ec5-808c-a5c7e03b478d" />
To add a new member go to Tools > Active Directory Users and Computers. From there we will get a new page.

2: <img width="1023" height="835" alt="Organizational Unit" src="https://github.com/user-attachments/assets/bef848cb-cb9d-416c-af23-efa1c9b7e9b8" />
We want to go to our domain controllers name in my case its "MyOracle.local", its should drop down multiple folders. To create our users we will right click the "MyOracle.local" option > New > Organizational Unit
From there its pretty simple. Just like you would in windows, you need to create a username, email, password and everything should be setup. You can name this Unit as IT, then create another one with the name HR.

> **Note**: In the password section, make sure to untick the "User must change password at next logon option" since we want to keep the password the same. You can keep it on if that is what you want to do for you project.
<img width="436" height="373" alt="Password_Creation" src="https://github.com/user-attachments/assets/fec9dbdd-7994-4ec1-b9c0-012b311dbdad" />
