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
- Windows 10 (22H2)

<h2>Deployment and Configuration Steps</h2>

<b>1. Setup Domain Controller in Azure</b>

- Create a Resource Group and a Virtual Network with a Subnet.
- Create the Domain Controller VM (Windows Server 2022) named "DC-1".
- Set DC-1’s NIC private IP to static.
- Disable the Windows Firewall for testing.
  
![image](https://github.com/user-attachments/assets/5c013db9-2245-4381-b957-aede42945b7b)

<b>2. Setup Client VM</b>

- Create Client VM (Windows 10) named "Client-1".
- Set Client-1’s DNS settings to DC-1’s private IP.
- Restart Client-1 and ping DC-1 to verify connectivity.

![image](https://github.com/user-attachments/assets/69da26ee-0917-41a5-b72b-fd4db0d8a30a)

<b>3. Install Active Directory</b>

- Log into DC-1 and install Active Directory Domain Services.
- Promote as a DC and set up a new forest (e.g., mydomain.com).
- Create Organizational Units (OUs) and a Domain Admin user.
- Restart DC-1 and login as admin user.
  
![image](https://github.com/user-attachments/assets/33088606-6a6d-40d1-a25c-4e2cdf3fbb5b)

<b>4. Join Client-1 to Domain</b>

- Join Client-1 to the domain (mydomain.com).
- Verify that Client-1 appears in ADUC and organize it into the new OU.

![image](https://github.com/user-attachments/assets/a1bbe43d-1538-427c-99dd-d8a2ffc5430b)

<b>5. Remote Desktop Setup</b>

- Log into Client-1 as admin user.
- Allow "domain users" access to Remote Desktop.

![image](https://github.com/user-attachments/assets/fa89358b-83d7-4a18-8205-95cd3f9163f5)

<b>6. Create Additional Users</b>

- Use PowerShell to create multiple users in ADUC.
- Attempt to log into Client-1 with one of the new accounts.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/9a65558a-ae33-4831-8e66-970167cc5068)

<b>7. Account Lockouts</b>

- Lock out an account after failed login attempts and observe in ADUC.
- Reset the password and unlock the account.

![image](https://github.com/user-attachments/assets/892720ff-7c9a-4e19-a660-c196f0b271ff)

<b>8. DNS Configuration</b>

- Create an A-record and a CNAME record in DC-1.
- Test the DNS resolution from Client-1.
  
![image](https://github.com/user-attachments/assets/6ea3ad05-6f1e-48b4-aff7-db70e5b5d3ce)

<b>9. File Shares Setup</b>
- Create shared folders with different permissions on DC-1.
- Test access from Client-1 as a normal user.

![image](https://github.com/user-attachments/assets/2531a0e8-3242-45df-aa46-cb048733f094)

<b>10. Create Security Groups</b>
- Create a security group for ACCOUNTANTS and set permissions for the accounting folder.
- Test access for users in and out of the group.

![image](https://github.com/user-attachments/assets/1075fe74-91f1-4ad4-b865-0ca0c7d3655c)
