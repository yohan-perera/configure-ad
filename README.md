<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring On-Premises Active Directory Within The Cloud (Azure VMs)</h1>
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

- Using resources in Azure, create environment consisting of a domain controller (Windows Server 2022 Virtual Machine), a client (Windows 10 Virtual Machine), and a Virtual Network and Subnet.
- Establish and verify connectivity between client and domain controller by initiating perpetual ping and configuring local firewall settings if necessary.
- Install Active Directory on domain controller, create admin and user accounts, and setup appropriate domain.
- Join client to newly created domain on domain controller, and enable remote desktop capability for non-administrative users on client VM.
- Create and/or add user accounts and test to verify that it is possible to log in to client VM with any of these accounts.

<h2>Deployment and Configuration Steps</h2>
<br />
<p>
Create Domain Controller VM (DC-1) running Windows Server 2022.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/b4c00ef2-e6aa-440a-98bb-3d8096fe69a7)

<br />

<br />
<p>
Configure the Domain Controller’s NIC so that the Private IP Address is set to "static".
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/8336fba8-dff2-4894-80a8-db16ddf2b0c2)

<br />

<br />
<p>
Create Client VM (Client-1) running Windows 10, ensuring that it uses the name Resource Group and Virtual Network (Vnet) as DC-1.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/136a2e0c-fc62-4f8d-84c3-6b1a149d50c1)

<br />

<br />
<p>
Establish RDP (Remote Desktop Protocol) Connection to Client-1 and initiate perpetual ping to DC-1 private IP address.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/cbdfc8bc-ada2-4e6e-a201-3743e6b18b22)

<br />

<br />
<p>
Establish RDP (Remote Desktop Protocol) Connection to Domain Controller (DC-1) and configure local firewall settings to enable ICMPv4.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/9f7c577b-f9c6-4ed9-9b48-599095e6e249)

<br />

<br />
<p>
ICMP traffic should now be allowed and perpetual ping initiated on Client-1 should receive corresponding replies.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/77b91d51-3b29-4e74-929a-8b5f818ef6db)

<br />

<br />
<p>
Install Active Directory Services on DC-1.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/45b9070f-77d8-4ab9-a26f-9b89dee48a55)

<br />

<br />
<p>
Promote DC-1 as a domain controller and setup new forest as mydomain.com or any appropriate domain, then restart VM and login as mydomain.com\username.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/7edb43f3-f513-4b33-a27e-47dfe9c93324)

<br />

<br />
<p>
Under "Active Directory Users and Computers", create new Organization Units called "_EMPLOYEES" and "_ADMINS".
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/0e11841f-ed75-4f74-83e4-49052f65a092)

<br />

<br />
<p>
Create new employee within the "_ADMINS" Organizational Unit (OU).
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/a9fa0aa3-1e4d-420b-aef2-3523e756a6d0)

<br />

<br />
<p>
Add created employee to the "Domain Admins" Security Group, then restart Domain Controller and log in as newly created admin account.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/c0fdbf01-a122-438b-b46d-7fb01aaa955b)

<br />

<br />
<p>
Configure Client-1 network settings to set DNS server to DC-1 private IP address, then restart Client-1.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/c1c823a6-e9bd-4361-ae7b-82f0ebc6601f)

<br />

<br />
<p>
After restarting VM, login to Client-1, join to the domain, and restart again. Client-1 should now be joined to the Domain Controller.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/0105dc05-2543-45d5-8d27-ffd0918f07e6)
![image](https://github.com/yohan-perera/configure-ad/assets/156178441/43ce11e2-61dc-40de-945c-2f5e450fed57)

<br />

<br />
<p>
Configure Client-1 system properties and allow “Domain Users” to access Remote Desktop.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/c1b09b17-a58e-4bcb-ba4d-f0de6082c66b)

<br />

<br />
<p>
On DC-1, under "Active Directory Users and Computers" create and add user accounts. In this instance, a script was run in PowerShell ISE (as administrator) to create accounts under the "_EMPLOYEES" Organizational Unit.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/964f27c4-4275-45de-9612-59e5a9e27d98)
![image](https://github.com/yohan-perera/configure-ad/assets/156178441/7316d52f-4915-47b7-89e2-670f153641cd)

<br />

<br />
<p>
Testing login using appropriate credentials for one of the created accounts.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/441cedf0-846b-4feb-a389-c48e114f3c5f)

<br />

<br />
<p>
Resetting password and unlocking user account.
</p>
<br />

![image](https://github.com/yohan-perera/configure-ad/assets/156178441/99cdd4e2-a2d1-44e6-bb54-6b81505c62ec)
