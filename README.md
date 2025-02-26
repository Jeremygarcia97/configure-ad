# -configure-ad<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

<h3>Step 1: Setup Resources in Azure</h3>

![image](https://github.com/user-attachments/assets/e6b89340-41cd-4043-b1c3-5e04b222ebc6)

<p>
Create Resource group, Name it and then hit review+create.
</p>
<br />

![image](https://github.com/user-attachments/assets/8bee7fa3-1508-448e-89fb-101053f5ed7a)

<p>
Create Virtual network, Put it in the same resource group we created, name it also and put same region as resource group. Then hit Review+Create.
</p>
<br />

![image](https://github.com/user-attachments/assets/880af017-2c42-4df1-af10-3860c57e499e)

<p>
Create First Vitural Machine-Name: DC-1
		- Image: Windows Server 2022, Hit Reveiw+Create at bottom
</p>
<br />

![image](https://github.com/user-attachments/assets/61224a2f-6dff-4388-8be0-075303f3e7c8)

<p>
Create Second VM-Name Client-1, Image: Windows 10 Pro, Hit Reveiw+create
</p>
<br />

![image](https://github.com/user-attachments/assets/04edb532-fff1-41d6-aba3-d769aa7825db)
![image](https://github.com/user-attachments/assets/bc891202-30d9-43a1-ac88-902f78a6d93c)
![image](https://github.com/user-attachments/assets/b7423fa9-40bb-48dd-938d-a1c63de14033)


<p>
- Set DC-1's Virtual Network Interface Card (vNIC) private IP address to be static
	- Go to DC-1's network settings
	- Select Networking
	- Select the link next to Network Interface
	- - Select IP Configurations > dc-1368-defaultIpConfiguration or whatever name pops up
	- Change the assignment from dynamic to static 
		- This ensures DC-1's IP address will not change
</p>
<br />

<h3>Step 2: Ensure Connectivity Between the Client and Domain Controller</h3>

![image](https://github.com/user-attachments/assets/74a8aeb0-da8e-49b7-943b-515fb4b38a29)
![image](https://github.com/user-attachments/assets/ace5159a-b275-4423-9b90-f7dc05bf0db7)


<p>
- Login to Dc-1 using Microsoft Remote Desktop- Once login Right click Start menu- Click Run- Type ms.msc. This will open windows Firewall- Click Firewall properties- Select domian,Private,public and switch to off-then hit apply and OK. 
</p>
<br />

![image](https://github.com/user-attachments/assets/0d17200b-d643-4e62-9444-53c3865a4c77)
![image](https://github.com/user-attachments/assets/84892fc1-9900-4a2d-82b4-76617c599511)


<p>
Now we are going to set Client-1 DNS setting to DC-1 Private IP Address. First Get the Private Ip address of DC-1- Click Client- 1 VM- Click Network then Network Settings- Click Network interface card- Select DNS Servers- Custom- Enter IP addres of DC-1- Then click Save.
</p>
<br />

![image](https://github.com/user-attachments/assets/e19fd5e4-40f0-4029-9fe5-05eec2c95cec)
![image](https://github.com/user-attachments/assets/bec853a0-71de-4a1b-8dea-e49372942280)


<p>
- Login to Client-1 using Microsoft Remote Desktop
- Search for Powershell and open it
-Ping DC-1's private IP Address (for example, ping 10.1.0.4)
-Enter ipcinfig /all to show DC-1 private IP Address
</p>
<br />

<h3>Step 3: Install Active Directory</h3>

![image](https://github.com/user-attachments/assets/4e3a8ae9-fe64-4a6c-8e63-66f9752aae2b)
![image](https://github.com/user-attachments/assets/e806e1dc-3f1b-4c8f-9856-a98a21775a9b)

<p>
Log back into DC-1
Open Server Manager
Select "Add Roles and Features" > Follow the prompts
At Server Roles, check "Active Directory Domain Services."
Select Add Features > select Next
Complete the installation.
</p>
<br />

![image](https://github.com/user-attachments/assets/7e93083d-0ba4-4852-a475-079c80e90f3f)

![image](https://github.com/user-attachments/assets/8f4f5b60-0a41-44ae-90fe-35ed97128f40)



<p>
At the top right of the Server Manager Dashboard, click on the flag>
Select "Promote This Server to a Domain Controller">
Select "Add a New Forest">
Root domain name: mydomain.com>
Select Next>
Create a password,
Select Next and follow the prompts,
Select Install to complete the installation.	
</p>
<br />

![image](https://github.com/user-attachments/assets/89628410-1cf5-41d4-abab-e0fdbdc0fe48)

<p>
DC-1 will automatically restart>
Log back into DC-1 as user: mydomain.com\labuser.
</p>
<br />

<h3>Step 4: Create an Admin and Normal User Account in Active Directory.</h3>

![image](https://github.com/user-attachments/assets/3d3267bc-5741-40a2-b45a-314ed33e668e)

<p>
On DC-1.click start menu> Select administrative tools> click Active Directory Users and Computers
</p>
<br />

![image](https://github.com/user-attachments/assets/d0a463b3-8e63-4da8-baaf-c14c863c0e17)

<p>
Right-click mydomain.com > New > Select Oranizational Unit>
Create two OUs>
Name the first "_EMPLOYEES">
Name the second "_ADMINS".
</p>
<br />

![image](https://github.com/user-attachments/assets/feee7813-0675-498a-af7d-9761043812cc)
![image](https://github.com/user-attachments/assets/fd139464-e33e-4e36-83f7-5bbca97b6261)
![image](https://github.com/user-attachments/assets/0ad4578e-92d5-4840-ba28-8679f1d05d6b)


<p>
Go to the _ADMINS OU
Right-click the name _ADMINS > New > User>
First/Last name: Jane Doe>
User login name: jane_admin>
Select Next>
Create a password>
Select Next and then select Finish.
</p>
<br />

![image](https://github.com/user-attachments/assets/cbc58e48-72bc-4cbe-af6d-6d18cafe4adb)
![image](https://github.com/user-attachments/assets/ee453884-f7b7-4e22-a0a0-26754bbb39be)

<p>
Right-click Jane Doe > select Properties
Click the tab named "Member of" > select Add
Type in the names of your domain administrators>
Select "Check Names" > OK > Apply
Log out of DC-1 as "labuser" and log back in as “mydomain.com\jane_admin”.
</p>
<br />

<h3>Step 5: Join Client-1 to your domain (mydomain.com)
</h3>

![image](https://github.com/user-attachments/assets/6cc47d37-7377-4bf6-8922-db658738f281)
![image](https://github.com/user-attachments/assets/7606d6ae-a402-4882-9bfc-c342c7bed28d)



<p>
Log back into Client-1 using Microsoft Remote Desktop as the original local admin (labuser)>
Right-click the Start menu and select System>
On right-hand side of the screen, select Rename This PC (Advanced) > Change
Under "Member of," select Domain>
Type "mydomain.com" and select OK>
Username: mydomain.com\jane_admin>
Type in password and press OK>
Restart the computer>
.
</p>
<br />


<h3>Step 6: Setup Remote Desktop for non-administrative users on Client-1
</h3>

![image](https://github.com/user-attachments/assets/865b64a7-118d-40a1-9fb0-cedf9afcc289)
![image](https://github.com/user-attachments/assets/33335b5e-f161-441d-8651-b430bd9d89cc)



<p>
- Log back into Client-1>
- Use mydomain.com\jane_admin>
- Right-click the Start menu and select System>
- right-hand side of the screen, select Remote Desktop>
- Under User Accounts, click "Select Users That Can Remotely Access This PC > select Add>
- Type in the name of your domain users>
- Select "Check Names" > OK.
</p>
<br />


<h3>Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users
</h3>

![image](https://github.com/user-attachments/assets/37df0ada-36c2-4556-8700-bf17a1df7736)
![image](https://github.com/user-attachments/assets/57de54bc-10c1-437c-a742-16c3a9ffdf5b)


<p>
 Log back into DC-1 as jane_admin
- Search for Powershell ISE>
- Right-click on Powershell ISE and open it as an administrator>
- At the top-left of the screen select New Script and paste the contents of the following script into it./
- You can find the script right here (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
