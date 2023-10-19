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

<h2>Configuration and Deployment Steps</h2>

Creating VMs in the Same VNET:
- Two virtual machines (VMs) will be set up in the same Virtual Network (VNET).
- One VM becomes a Domain Controller (DC), and the other a Client machine.
- The DC is assigned a static IP since it offers Active Directory services to the Client.
- The Client machine joins the domain, with its DNS settings pointing to the DC.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/49ac1ee4-6b51-442a-8b2e-a8880fc166dc)

Network Configuration:
- The DC needs a static private IP address.
- Initial ping from the Client to the DC fails due to ICMPv4 being blocked in the DC's firewall.
- Enabling ICMPv4 on the DC's firewall allows successful pinging from the Client.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/5e4e7ba2-5dd1-4cee-a101-ab34bdfaa463)
![image](https://github.com/buriostegui/configure-ad/assets/148411510/1a0308ee-3d00-494b-a325-e126045e79b2)

Active Directory Setup:
- On DC-1, install Active Directory Users & Computers.
- Promote the VM to a Domain Controller.
- Set up a new forest called "mydomain.com."
- After a restart, log in as "mydomain.com\labuser" to verify Active Directory functionality.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/b1a40c3a-0139-4a3e-9365-989979b1fb56)

Organizational Units (OUs):
- Create two OUs: "_EMPLOYEES" and "_ADMINS" within the domain.
- Add a new user named Jane Doe with the username "Jane_admin."
- Include Jane in the domain admins security group.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/29da5bbe-caaf-4e3f-96dc-db554fa5eb79)
![image](https://github.com/buriostegui/configure-ad/assets/148411510/c56a1622-bd3d-4f6f-9afd-bb6316df1785)

Client-Joining Domain:
- Change Client-1's DNS settings to the DC's private IP address in the Azure portal.
- Restart Client-1 to ensure it's using the correct DNS.
- Verify the configuration, showing Client-1 using DC-1 DNS.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/2f32e715-5466-4207-97d2-0aa0ab914e28)
![image](https://github.com/buriostegui/configure-ad/assets/148411510/1e15aadc-6eee-4286-9047-1d4d76101573)

Joining Client to the Domain:
- Access system settings on Client-1.
- Select "rename this PC (advanced)" and change the domain to "mydomain.com."
- Enter credentials from "mydomain.com\labuser."
- After a restart, Client-1 becomes part of "mydomain.com."

![image](https://github.com/buriostegui/configure-ad/assets/148411510/62e618f3-0568-4307-9c31-1fd5fbdbe847)

Setting Up Remote Desktop for Users:
- Log into Client-1 as an admin and open system properties.
- Click on "Remote Desktop" and allow "domain users" access to remote desktop.
- This enables normal users to log into Client-1.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/4711e215-31bd-4f84-8ed7-f3c2bf9b31ec)

Testing Remote Desktop Access:
- A script is used to generate thousands of users in the domain.
- PowerShell is used to input the script.
- After creating users, the goal is to verify that normal users can RDP into Client-1.

![image](https://github.com/buriostegui/configure-ad/assets/148411510/cee723b8-ff0f-4cbb-899d-c77c1a5d20b5)

Verified! 

![image](https://github.com/buriostegui/configure-ad/assets/148411510/1d0a93e3-d5d8-4283-b827-5b17eec498c2)
![image](https://github.com/buriostegui/configure-ad/assets/148411510/ba0851d9-38ee-4efc-969d-ec9be2854470)



These steps describe the setup and configuration of VMs, Active Directory, OUs, and remote desktop access within a domain environment.





