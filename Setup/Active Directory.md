# Active Directory Configuration

In this section, I will showcase how I setup 2 users (IT and HR), gave the server a static IP, and turned my Windows server into a Domain Controller.

## Static IP Setup

This is probably the simplest step as all you need to do is go to **Settings > Network & Internet > Status > Change adapter options**. From there, right-click the interface and click on **Properties**. Double-click **Internet Protocol Version 4 (TCP/IPv4)**, click on **Use the following IP address**, and then add in the Static IP, Subnet, Gateway IP, and DNS.

Once that is done, we can move on to promoting the server to a domain controller.

---

## Promoting the Server to a Domain Controller

### 1. Add Roles and Features
<img width="1024" height="840" alt="Step_1_Add_Role_Features" src="https://github.com/user-attachments/assets/e12041b0-30c4-4b0a-aef8-6da4c01d8a91" />

When you open up your Windows server, you should have the Server Manager application open. If not, you can just type it in the search tab to open it. Once it is open, you should see options on the top right; click on **Manage > Add Roles and Features**.

### 2. Installation Type
<img width="782" height="556" alt="Step_2_Role_Based" src="https://github.com/user-attachments/assets/e0336b4d-b0ec-4a01-942e-24a05d95dc67" />

Click next until you see this screen. Here, we want to select **Role-based or feature-based installation**.

### 3. Server Selection
<img width="1019" height="777" alt="Step_3_Select_Server" src="https://github.com/user-attachments/assets/2b29acdf-ac9c-4b95-a3c9-f6811813ecbf" />

Then select the server you want to promote. In my case, there is just one, so I will select that and click next.

### 4. Server Roles
<img width="1021" height="839" alt="Step_4_Select_Domain_Services" src="https://github.com/user-attachments/assets/51770b0b-203d-45e0-be5c-49db36577ae2" />

Here, we want to select **Active Directory Domain Services** and then click next.

### 5. Confirmation
<img width="1019" height="843" alt="Step_5_Click_Next_Until_Install" src="https://github.com/user-attachments/assets/1b6be094-a9b8-465d-8578-cef825eb6de0" />

Then we keep clicking next until we see the **Install** button.

### 6. Post-Deployment Configuration
<img width="1026" height="842" alt="Step_6_Promote_to_Domain" src="https://github.com/user-attachments/assets/083ac9f8-129b-403a-b83f-4731998d49d9" />

Once that is done, click the flag icon in the top right and you should see **Promote this server to a domain controller**. After clicking that, you will be given a setup page.

### 7. Deployment Configuration
<img width="1023" height="841" alt="Step_7_New_Forest" src="https://github.com/user-attachments/assets/41478593-db82-4038-88d3-f71c965b066e" />

We want to select **Add a new forest** and then we need to give it a name with a domain. I used **oracle.local**, but you can use any name you prefer.

### 8. Installation
<img width="1020" height="837" alt="Step_8_Next_for_All_Install" src="https://github.com/user-attachments/assets/200aece8-2ed6-494e-a61a-0b5674c5c6eb" />

Keep clicking next through the defaults until you see the **Install** option. After that, the server will finish the setup for the domain controller and usually perform a restart.

---

## Adding Users (IT and HR)

After the promotion is finished, we will add two users: one for IT and one for HR. This is so that if we ever try an attack simulation where we want to pivot and escalate our privilege, we will be able to simulate that realistically.

### 1. Active Directory Users and Computers
<img width="1018" height="836" alt="Adding_Members" src="https://github.com/user-attachments/assets/20614aad-749f-4ec5-808c-a5c7e03b478d" />

To add a new member, go to **Tools > Active Directory Users and Computers**. This will open up a new management window.

### 2. Organizational Units
<img width="1023" height="835" alt="Organizational Unit" src="https://github.com/user-attachments/assets/bef848cb-cb9d-416c-af23-efa1c9b7e9b8" />

Navigate to your domain name (in my case, it is **MyOracle.local**) and it should drop down multiple folders. To create our users, we will right-click the **MyOracle.local** option and select **New > Organizational Unit**. 

From there, it is pretty simple. Just like you would in standard Windows, you need to create a username, email, and password. You can name this Unit as **IT**, then repeat the process to create another one with the name **HR**.

<img width="436" height="373" alt="Password_Creation" src="https://github.com/user-attachments/assets/fec9dbdd-7994-4ec1-b9c0-012b311dbdad" />

> **Note**: In the password section, make sure to untick the **User must change password at next logon** option since we want to keep the password the same for the lab. You can keep it on if that is what you specifically want to do for your project.
