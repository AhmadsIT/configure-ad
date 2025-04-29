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

<b>Set Up the Azure Network and Domain Controller (DC-1)</b>

We begin by laying the foundation for the Active Directory environment.
  - In the Azure Portal, create a Resource Group, a Virtual Network, and a Subnet to isolate our internal domain.
  - Next, deploy a Windows Server 2022 VM named “DC-1”. This VM will act as our Domain Controller.
  - Set DC-1’s private IP address to static.
  - Finally, for initial testing and setup flexibility, disable the Windows Firewall on DC-1.
  
![image](https://github.com/user-attachments/assets/5c013db9-2245-4381-b957-aede42945b7b)

<b>Deploy and Configure the Client VM (Client-1)</b>

Now, we add a Windows 10 VM to act as the domain-joined client.
  - Deploy a new Windows 10 VM named “Client-1” into the same Virtual Network.
  - Set Client-1's DNS server to the private IP of DC-1, so it can locate the domain controller.
  - After restarting Client-1, perform a ping test to DC-1 to confirm network connectivity.

![image](https://github.com/user-attachments/assets/69da26ee-0917-41a5-b72b-fd4db0d8a30a)

<b>Install and Configure Active Directory Domain Services</b>

With DC-1 running, we install and configure Active Directory:
  - Log into DC-1 and install Active Directory Domain Services (ADDS).
  - After installation, promote DC-1 to a Domain Controller by creating a new forest (mydomain.com).
  - Within Active Directory Users and Computers (ADUC), create your Organizational Units (OUs) and a Domain Admin user for administrative access.
  - Restart DC-1 and log in with the newly created domain administrator account.
  
![image](https://github.com/user-attachments/assets/33088606-6a6d-40d1-a25c-4e2cdf3fbb5b)

<b>Join Client-1 to the Domain</b>

On Client-1, go to <b>System Settings → Rename this PC (advanced) → Domain</b>, and join it to mydomain.com.
Once the device reboots and rejoins the network, verify that Client-1 appears in Active Directory.
Use ADUC to move Client-1 into the appropriate OU for better organization.

![image](https://github.com/user-attachments/assets/a1bbe43d-1538-427c-99dd-d8a2ffc5430b)

<b>Configure Remote Desktop Access</b>

To allow support staff or general users to use RDP:
  - Log into Client-1 using the Domain Admin credentials.
  - Open <b>System Properties → Remote Desktop</b>, and add <b>“Domain Users”</b> to the list of allowed remote desktop users.

![image](https://github.com/user-attachments/assets/fa89358b-83d7-4a18-8205-95cd3f9163f5)

<b>Bulk Create User Accounts</b>

Now that our domain is functioning, we can populate it with users:
  - Use PowerShell on DC-1 to run a script that creates multiple users in bulk under a specified OU.
  - After creation, test logins on Client-1 using one of the newly created user accounts.

![image](https://github.com/ahmadspain/configure-ad/assets/158358030/9a65558a-ae33-4831-8e66-970167cc5068)

<b>Account Lockout Policies and Testing</b>

To simulate and understand account security mechanisms:
Configure Group Policy for account lockout under: <b>Group Policy Management Editor → Computer Configuration → Policies → Windows Settings → Security Settings → Account Lockout Policy</b>.

Adjust settings like:

  - Account lockout threshold

  - Lockout duration

  - Reset account lockout counter

Attempt multiple failed logins on Client-1 to trigger a lockout, then observe and reset the account status from ADUC.

![image](https://github.com/user-attachments/assets/892720ff-7c9a-4e19-a660-c196f0b271ff)
