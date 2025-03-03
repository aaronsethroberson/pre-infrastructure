<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

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


<p align="center">
Start by heading to Microsoft Azure and creating a resource group.
</p>

<img src="https://i.ibb.co/C9Mjwwp/1.jpg" alt=""/>
<p align="center">
Next, we will create a Virtual Network.
</p>

<img src="https://i.ibb.co/qY0d7yF2/Screenshot-3-3-2025-11353-portal-azure-com.jpg" alt=""/>

<p>
After creating both the resource group and virtual network, we'll move on to creating and setting up our virtual machine, which will serve as the Domain Controller (running Windows Server 2022).
</p>

<img src="https://i.ibb.co/wrRnhRxB/Screenshot-3-3-2025-111112-portal-azure-com.jpg" alt=""/>

<img src="https://i.ibb.co/9mmktzdw/Screenshot-3-3-2025-111353-portal-azure-com.jpg" alt=""/>

<br />

</p>In the Virtual Machine's Networking tab, we need to ensure that it is configured to connect to the virtual network we set up earlier </p>

<img src="https://i.ibb.co/DfKMgBJP/Screenshot-3-3-2025-111539-portal-azure-com.jpg" alt=""/>

<p>Next, we will create a second virtual machine to serve as the client. For this VM, the image should be Windows 10, not Windows Server</p>

<img src="https://i.ibb.co/SDfZ921R/Screenshot-3-3-2025-112823-portal-azure-com.jpg" alt=""/>

<img src="https://i.ibb.co/7tHJ4p2b/Screenshot-3-3-2025-112939-portal-azure-com.jpg" alt=""/>

<p>In the Virtual Machine's Networking tab, we need to ensure that it is configured to connect to the virtual network we set up earlier.</p>

<img src="https://i.ibb.co/7JG7Zp31/Screenshot-3-3-2025-113018-portal-azure-com.jpg" alt=""/>

<p>I need to change the Domain Controller's private IP address from dynamic to static since it will also serve as a DNS server. A static IP ensures the DNS configuration remains valid for the client. I'll update this in the DC's network settings.</p>

<img src="https://i.ibb.co/nq4kj1gW/Screenshot-3-3-2025-114045-portal-azure-com.jpg" alt=""/>
<img src="https://i.ibb.co/Q7jnMj1p/Screenshot-3-3-2025-114247-portal-azure-com.jpg" alt=""/>

<p>Next, we'll use Remote Desktop along with the Domain Controller's public IP address and the login credentials we set up during the Virtual Machine configuration</p>

<img src="https://i.ibb.co/TxmxtM6j/Screenshot-2025-03-03-114924.png" alt=""/>

<p align="center">
After logging into the Domain Controller, you should see the Server Manager screen displayed.
</p>

<img src="https://i.ibb.co/1t620tnm/Screenshot-2025-03-03-115311.png" alt=""/>

<p align="center">
To disable the firewall, I’ll right-click the "Start" button and select "Run." Then, I'll type "wf.msc".
</p>

<img src="https://i.ibb.co/N2BzzD6z/Screenshot-2025-03-03-115530.png" alt=""/>

<p align="center">
Click on "Windows Defender Firewall Properties," then disable the firewall state under the "Domain Profile," "Private Profile," and "Public Profile" tabs.
</p>

<img src="https://i.ibb.co/Q7P8sNM0/Screenshot-2025-03-03-115748.png" alt=""/>
<img src="https://i.ibb.co/xTTpJm3/Screenshot-2025-03-03-115935.png" alt=""/>

<p align="center">
Next, we need to configure the client's DNS settings to point to the Domain Controller. To do this, we’ll return to Microsoft Azure to obtain the Domain Controller's private IP address</p>

<img src="https://i.ibb.co/k63vzZ70/Screenshot-2025-03-03-120122.png" alt=""/>

<p>Next, I’ll navigate to the client machine’s network settings, select the NIC (Network Interface Card), go to settings, and then DNS servers. I'll change the option from "Inherit from virtual network" to "Custom," enter the Domain Controller’s private IP, and save the changes.</p>

<img src="https://i.ibb.co/MFMt3Xj/Screenshot-2025-03-03-120418.png" alt=""/>

<p align="center">
After that is completed, we will restart the client Virtual Machine</p>

<img src="https://i.ibb.co/bMBLXHgr/Screenshot-2025-03-03-120629.png" alt=""/>

<p align="center">
After the machine restarts, I’ll use Remote Desktop to connect to the client machine using its public IP and the login credentials I set up during its configuration
</p>

<img src="https://i.ibb.co/HpxH9KBc/Screenshot-2025-03-03-121802.png" alt=""/>

<p>Now that I'm logged in, I'll open PowerShell and ping the Domain Controller using its private IP. If there's a timeout error, ensure both machines are on the same virtual network in Azure, as this could be the issue.</p>

<img src="https://i.ibb.co/ynzX4XcD/Screenshot-2025-03-03-123309.png" alt=""/>

<p align="center">
I'll run "ipconfig /all" and check the "DNS Servers" section. It should point to our Domain Controller if everything is set up correctly.
</p>

<img src="https://i.ibb.co/W4sQ497z/Screenshot-2025-03-03-123350.png" alt=""/>


