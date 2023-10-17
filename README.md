<p align="center">
<img src="https://i.imgur.com/w7amRI0.png" height="40%" width="60%"/>
</p>

<h1>Configuring On-premises Active Directory within Azure Virtual Machines</h1>In this project, I configured on-premises Active Directory and created user accounts with PowerShell within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Steps</h2>

- Create 2 virtual machines, one with Windows 10 (Client VM) and the other with Windows Server 2022 (DC/Domain Controller VM)
-	Set the DC’s NIC private IP address from Dynamic to Static
-	Connect to both VMs using Remote Desktop
-	Initiate a perpetual ping from the Client to the DC; if there is no reply, enable Core Networking Diagnostics in the DC’s firewall
-	Install Active Directory Domain Services on the DC and promote it to a domain controller
-	Create an admin account and Organizational Units (OU) in Active Directory Users and Computers (ADUC), then log back in using the admin account
-	Set the Client’s DNS settings to the DC’s Private IP address, then join the Client to the DC
-	Enable Remote Desktop for domain users to access the Client
-	Create user accounts using a PowerShell script (run PowerShell ISE as administrator)
-	Connect to the Client with Remote Desktop using one of the newly created user accounts


<h2>Synopsis</h2>

<p>
<img src="https://i.imgur.com/ti0b95l.png" height="70%" width="70%"/>
</p>
<p>
I started by creating two virtual machines in Azure, one with Windows Server 2022 and the other with Windows 10. The Windows Server 2022 VM would serve as the Domain Controller (DC) and the Windows 10 VM would serve as the Client machine. I also set the DC’s NIC private IP address from Dynamic to Static, so that later in the lab when I configured the Client’s DNS settings (DC’s private IP address), the IP address would not change. Once the virtual machines were set up, I viewed the topology in Azure’s Network Watcher to make sure both machines were in the same network and subnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/YvrBTgj.png" height="70%" width="70%"/>
</p>
<p>
<img src="https://i.imgur.com/ym3Ld63.png" height="70%" width="70%"/>
</p>
<p>
<img src="https://i.imgur.com/LccuYWf.png" height="70%" width="70%"/>
</p>
<p>
After connecting to both VMs using Remote Desktop, I initiated a perpetual ping from the Client to the DC to ensure connectivity. The requests were timing out, so I opened Windows Defender Firewall in the DC and enabled Core Networking Diagnostics (ICMPv4 protocol). This allowed the DC to reply to the requests as shown in the command-line interface (CLI).
</p>
<br />

<p>
<img src="https://i.imgur.com/AayPHkb.png" height="70%" width="70%"/>
</p>
<p>
<img src="https://i.imgur.com/XqzeZYs.png" height="70%" width="70%"/>
</p>
<p>
In the DC, I installed Active Directory Domain Services (AD DS) from the Server Manager Dashboard. Once AD DS was installed, I promoted the machine to a Domain Controller so that it could manage devices and accounts on the domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/jbCeryh.png" height="70%" width="70%"/>
</p>
<p>
Now that Active Directory was all set up, I added two organizational units (OU) and a user. The OUs were _ADMINS and _EMPLOYEES, and the new user was “Jane Doe.” Jane was going to be an administrator, so I created her account inside the _ADMINS OU and added her as a member of Domain Admins. I logged out of the default account that I started with and logged back in as Jane.
</p>
<br />

<p>
<img src="https://i.imgur.com/afGXoqY.png" height="70%" width="70%"/>
</p>
<p>
<img src="https://i.imgur.com/31ZSBHk.png" height="70%" width="70%"/>
</p>
<p>
To continue with setting up my domain, I went back to Azure and set the DC’s private IP address as the DNS server for the Client. After this, I was able to join the Client to my domain. This was confirmed by the “Welcome” box that popped up, confirming that the Client was now part of the domain.
</p>
<br />

<p>
<img src="https://i.imgur.com/EWzPnOh.png" height="70%" width="70%"/>
</p>
<p>
I enabled Domain Users to user Remote Desktop to access the Client. Remote Desktop is the only way to log into the VM, so enabling this for Domain Users would allow for any user accounts in the domain to be able to log into this machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/lzUMtp1.png" height="70%" width="70%"/>
</p>
<p>
<img src="https://i.imgur.com/WF5lwhY.png" height="70%" width="70%"/>
</p>
<p>
Lastly, I created 10,000 user accounts using a PowerShell script. All of the accounts were placed into the _EMPLOYEES OU, so I selected a random account and used it to log into the Client. I used the commands “hostname” and “whoami” in the command-line, which confirmed that this was successful.
</p>
<br />

<p>
✨ This lab showed me a lot about how Active Directory works and was really interesting, plus it was cool creating thousands of user accounts using a PowerShell script!
</p>
<br />
