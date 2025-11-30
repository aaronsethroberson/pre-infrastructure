<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<br />

<h1>Preparing Active Directory Infrastructure in Azure </h1>
For this project, I configured two virtual machines (VMs): one with Windows Server serving as a Domain Controller and another with Windows 10 as a client, which will be added to the domain.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services


<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Project Walkthrough</h2>
<br />
<br />

Begin by navigating to Microsoft Azure and setting up a resource group.
<br />

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/1.jpg" alt=""/>
<br />
<br />

Next, we will set up a Virtual Network.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/2.jpg" alt=""/>
<br />
<br />

Once the resource group and virtual network are created, we'll proceed with setting up a virtual machine running Windows Server 2022, which will function as the Domain Controller.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/3.jpg" alt=""/>

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/4.jpg" alt=""/>
<br />
<br />

In the Virtual Machine's Networking tab, make sure it is set to connect to the virtual network created earlier.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/5.jpg" alt=""/>
<br />
<br />

Next, we'll set up a second virtual machine to act as the client. This VM should use a Windows 10 image instead of Windows Server.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/6.jpg" alt=""/>

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/7.jpg" alt=""/>
<br />
<br />

In the Virtual Machine's Networking tab, ensure it is configured to connect to the previously created virtual network.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/8.jpg" alt=""/>
<br />
<br />

I need to set the Domain Controller's private IP address to static instead of dynamic, as it will also function as a DNS server. A static IP ensures the DNS configuration remains consistent for the client. I'll make this change in the DC's network settings.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/9.jpg" alt=""/>

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/10.jpg" alt=""/>
<br />
<br />

Next, we'll connect using Remote Desktop, utilizing the Domain Controller's public IP address and the login credentials created during the Virtual Machine setup.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/11.png" alt=""/>
<br /11
<br />

After logging into the Domain Controller, you should see the Server Manager screen displayed.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/12.png" alt=""/>
<br />
<br />

To disable the firewall, I’ll right-click the "Start" button and select "Run." Then, I'll type "wf.msc".

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/13.png" alt=""/>
<br />
<br />

Click on "Windows Defender Firewall Properties," then disable the firewall state under the "Domain Profile," "Private Profile," and "Public Profile" tabs.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/14.png" alt=""/>

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/15.png" alt=""/>
<br />
<br />

Next, we need to configure the client's DNS settings to point to the Domain Controller. To do this, we’ll return to Microsoft Azure to obtain the Domain Controller's private IP address</p>

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/16.png" alt=""/>
<br />
<br />

Next, I’ll navigate to the client machine’s network settings, select the NIC (Network Interface Card), go to settings, and then DNS servers. I'll change the option from "Inherit from virtual network" to "Custom," enter the Domain Controller’s private IP, and save the changes.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/17.png" alt=""/>
<br />
<br />

After that is completed, we will restart the client Virtual Machine.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/18.png" alt=""/>
<br />
<br />

After the machine restarts, I’ll use Remote Desktop to connect to the client machine using its public IP and the login credentials I set up during its configuration

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/19.png" alt=""/>
<br />
<br />

Now that I'm logged in, I'll open PowerShell and ping the Domain Controller using its private IP. If there's a timeout error, ensure both machines are on the same virtual network in Azure, as this could be the issue.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/20.png" alt=""/>
<br />
<br />

I'll run "ipconfig /all" and check the "DNS Servers" section. It should point to our Domain Controller if everything is set up correctly.

<img src="https://github.com/aaronsethroberson/pre-infrastructure/blob/main/images/21.jpg" alt=""/>
<br />
<br />
