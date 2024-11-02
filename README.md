<p align="center">
<img src="https://i.imgur.com/RCXtBFx.png" alt="Microsoft Active Directory Logo" height="40%" width="40%" alt="Domain Controller"/>
</p>

<h1>On-Premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within two Azure virtual machines.<br/>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10

<h2>Deployment and Configuration Steps</h2>

<h3>Step 1: Setup Resources in Azure</h3>

- Create two virtual machines
	- The first vm will be you Domain Controller (DC-1), the second will be your Client VM (Client-1)
		- Ensure that DC-1 uses a Windows Server image and Client-1 uses Windows Pro
    - Set the Domain Controller's NIC Private IP address to be static.
 
      <img src="https://i.imgur.com/GtCutrq.png" height="70%" width="70%" alt="Domain Controller"/> <img src="https://i.imgur.com/oa951HY.png" height="70%" width="70%" alt="Domain Controller"/>
     
<h3>Step 2: Ensure Connectivity Between the Client and Domain Controller</h3>

- Login to Client-1 using Microsoft Remote Desktop
- Windows Powershell
- Ping DC-1's private IP Address 
	
<p align="center">
<img src="https://i.imgur.com/ZNf5tIh.png" height="50%" width="50%" alt="Domain Controller"/> <img src="https://i.imgur.com/tSjTYXb.png" height="50%" width="50%" alt="Domain Controller"/>

<h3>Step 3: Install Active Directory</h3>

- Ensure you are logged into DC-1
	- Open Server Manager from Start menu
	- Select "Add Roles and Features" click Next until you see Server Roles
	- At Server Roles, check "Active Directory Domain Services."
	- Select Add Features ---> select Next
   	- Select restart if required
	- Complete the installation

<p align="center">
<img src="https://i.imgur.com/dvLJ7EY.png="60%" width="60%" alt="Domain Controller"/> <img src="https://i.imgur.com/k5V4w78.png"50%" width="50%" alt="Domain Controller"/>
</p>

- At the top right of the Server Manager Dashboard, click on the flag
- Select "Promote This Server to a Domain Controller" 	
 - Select "Add a New Forest"
 	- Root domain name: mydomain.com
- Select Next
- Create a password
- Select Next and follow the prompts
- Select Install to complete the installation

<p align="center">
<img src="https://i.imgur.com/IjfUZ0a.png" height="70%" width="70%" alt="Domain Controller"/> 
	
- DC-1 will automatically restart
- Log back into DC-1 as user: mydomain.com\labuser85 (Insert the user name you created)               

<p align="center">
<img src="https://i.imgur.com/q59L4FG.png" height="40%" width="40%" alt="Domain Controller"/> 
	
<h3>Step 4: Create an Admin and Normal User Account in Active Directory </h3>
     
- On DC-1, Start Menu --> Windows Administartive Tools --> Active Directory Users and Computers
- Click Tools at the top-right of the screen
- Select Active Directory Users and Computers

<p align="center">
<img src="https://i.imgur.com/vcUGkP3.png" height="40%" width="40%" alt="Domain Controller"/> 
	
- Right-click mydomain.com --> New --> Select Oranizational Unit (OU)
- Create two OUs
	- Name the first "_EMPLOYEES"
	- Name the second "_ADMINS"
	
<p align="center">
<img src="https://i.imgur.com/fZMDHBr.png" height="40%" width="40%" alt="Domain Controller"/> 
		
- Right-click mydomain.com and click Referesh to sort the new organizational units to the top
- Go to the _ADMINS OU
- Right-click the name of the OU --> New --> User
	- First/Last name: Jane Doe
	- User login name: jane_admin
	- Select Next
	- Create a password
	- Uncheck all boxes
	- Select Next and then select Finish
<p align="center">
<img src="https://i.imgur.com/eSTQvrI.png" height="40%" width="40%" alt="Domain Controller"/> <img src="https://i.imgur.com/GQWbgao.png" height="40%" width="40%" alt="Domain Controller"/> 
	
- Go to the _ADMINS OU
- Right-click Jane Doe > select Properties
	- Click the tab named "Member of" > select Add
	- Type in the names of your domain administrators
	- Select "Check Names" > OK > Apply
- Log out of DC-1 as "labuser" and log back in as “mydomain.com\jane_admin”

<p align="center">
<img src="https://i.imgur.com/ViLtOhe.png" height="40%" width="40%" alt="Domain Controller"/> 
</p>
 
<h3>Step 5: Join Client-1 to your domain (mydomain.com)
</h3>

- Go back to the Azure portal
- Navigate to the Client-1 Virtual Machine
- On the left-hand side of the screen select Networking
- Select the link next to the NIC > select DNS Server > Custom
- Type in DC-1's private IP address
- Click Save
- After it is done updating, select Restart and select Yes

<p align="center">
<img src="https://i.imgur.com/z6UesO7.png" height="70%" width="70%" alt="Domain Controller"/> <img src="https://i.imgur.com/bt0yK17.png" height="70%" width="70%" alt="Domain Controller"/>  <img src="https://i.imgur.com/sB5edH5.png" height="70%" width="70%" alt="Domain Controller"/>
</p>

- Log back into Client-1 using Microsoft Remote Desktop as the original local admin (labuser)
- Right-click the Start menu and select System
- On right-hand side of the screen, select Rename This PC (Advanced) > Change
- Under "Member of," select Domain
- Type "mydomain.com" and select OK
	- Username: mydomain.com\jane_admin
	- Type in password and press OK
- Restart the computer 			


<p align="center">
<img src="https://i.imgur.com/3HxJLpe.png" height="80%" width="80%" alt="Domain Controller"/> <img src="https://i.imgur.com/J8M4zBU.png" height="50%" width="50%" alt="Domain Controller"/>
</p>

<h3>Step 6: Setup Remote Desktop for non-administrative users on Client-1
</h3>

- Log back into Client-1
- Use mydomain.com\jane_admin
- Right-click the Start menu and select System
- On the right-hand side of the screen, select Remote Desktop
- Under User Accounts, click "Select Users That Can Remotely Access This PC > select Add
- Type in the name of your domain users
- Select "Check Names" > OK > OK

 
 <p align="center">
<img src="https://i.imgur.com/HgAXVMX.png" height="70%" width="70%" alt="Domain Controller"/> <img src="https://i.imgur.com/0QDUk5l.png" height="60%" width="60%" alt="Domain Controller"/>
</p>

<h3>Step 7: Create as many additional users as you would like and attempt to log into Client-1 with one of the users' profiles
</h3>

- Log back into DC-1 as jane_admin
- Search for "Powershell_ise,"
- Right-click on Powershell_ise and open it as an administrator
- At the top-left of the screen select New Script and paste the contents of the following script into it
	- You can find the script [here](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

<p align="center">
<img src="https://i.imgur.com/MpvLIbB.png" height="70%" width="70%" alt="Domain Controller"/> <img src="https://i.imgur.com/V4vIvre.png" height="70%" width="70%" alt="Domain Controller"/>
</p>

- Click the green arrow button near the top-middle of the screen
	- This will run the script
- Once the users have been created, go back to Active Directory Users and Computers > mydomain.com > _EMPLOYEES
		- You will see all the accounts that were created
- You can now log into Client-1 with one of the accounts that were created
	- Try logging into Client-1 as user "base.milu" using the password "Password1"

<p align="center">
<img src="https://i.imgur.com/3HN1Nf4.png" height="80%" width="80%" alt="Domain Controller"/> <img src="https://i.imgur.com/CeE8LGh.png" height="50%" width="50%" alt="Domain Controller"/>  <img src="https://i.imgur.com/7ZVBp8a.png" height="70%" width="70%" alt="Domain Controller"/>
</p>

<p align="center">
<img src="https://i.imgur.com/EzgHWRs.png" height="70%" width="70%" alt="Domain Controller"/> <img src="https://i.imgur.com/hYFodxu.png" height="70%" width="70%" alt="Domain Controller"/>
</p>



🎉Congratulations! You have implementated on-premises Active Directory and created users within an Azure virtual machine!🎉
