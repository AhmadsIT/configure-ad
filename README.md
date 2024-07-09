<p align="center">
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

<h2>Deployment and Configuration Steps</h2>

Step 1: Setup Resources in Azure

Create Domain Controller VM (DC-1)

- Use Windows Server 2022.
- Note the Resource Group and Virtual Network (Vnet).
- Set DC-1's NIC Private IP address to static.
  
![image](https://github.com/ahmadspain/configure-ad/assets/158358030/50b8179b-badf-4667-a64b-dcd187f44c42)

Step 2: Ensure Connectivity between Client and Domain Controller

Ping DC-1 from Client-1

- Login to Client-1 via Remote Desktop.
- Open Command Prompt and perpetual ping DC-1's private IP (ping -t <ip>).

Enable ICMPv4 on DC-1

- Login to DC-1.
- Enable ICMPv4 inbound rule in Windows Firewall.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/b9c22f02-9b31-48f7-b0c3-d7c59443ed3a)

Step 3: Install Active Directory

Install AD DS on DC-1

- Login to DC-1.
- Install Active Directory Domain Services.
- Promote DC-1 as a domain controller for "mydomain.com".
- Restart DC-1 and login as mydomain.com\labuser.
  
![image](https://github.com/ahmadspain/configure-ad/assets/158358030/d472e980-e705-4161-9a21-561191f81c6a)

Step 4: Create Admin and Normal User Account in AD

Create Users in ADUC

- Create an OU named "_EMPLOYEES".
- Create an OU named "_ADMINS".
- Create user "Jane Doe" as "jane_admin".
- Add "jane_admin" to "Domain Admins" group.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/6474e6e2-2680-416a-b8c1-43900daf624f)

Step 5: Join Client-1 to Domain

Join Client-1 to Domain

- Set Client-1's DNS to DC-1's Private IP.
- Restart Client-1.
- Login as local admin (labuser) and join to domain mydomain.com.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/39e753ac-a73f-48f6-9b5c-a80b596fa130)

Step 6: Setup Remote Desktop for Non-Admin Users on Client-1
Allow Remote Desktop for Non-Admin Users

- Login to Client-1 as mydomain.com\jane_admin.
- Open System Properties > Remote Desktop.
- Allow "domain users" access.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/9a65558a-ae33-4831-8e66-970167cc5068)

Step 7: Create Additional Users and Test Login

Create Users with PowerShell

- Login to DC-1 as jane_admin.
- Use PowerShell ISE as Administrator.
- Run script to create users.

Test Login with Additional Users

- Attempt to login to Client-1 with newly created user credentials.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/9dead55f-0313-420a-9ebc-ea3deb0d5c94)

Step 8: Finish

- Validate all users can login to Client-1 successfully.
- Ensure organizational units (like "_CLIENTS") are properly used for organizing objects in AD, if desired.
