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

![alt text](https://i.ibb.co/gFhP6NQW/Screenshot-3-3-2025-105941-portal-azure-com.jpg)
<br />
<br />

Next, we will set up a Virtual Network.

<img src="https://i.ibb.co/qY0d7yF2/Screenshot-3-3-2025-11353-portal-azure-com.jpg" alt=""/>
<br />
<br />

Once the resource group and virtual network are created, we'll proceed with setting up a virtual machine running Windows Server 2022, which will function as the Domain Controller.

<img src="https://i.ibb.co/wrRnhRxB/Screenshot-3-3-2025-111112-portal-azure-com.jpg" alt=""/>

![Image](https://github.com/user-attachments/assets/62db806b-51c9-4755-9c36-08ab518d08c5)
<br />
<br />

In the Virtual Machine's Networking tab, make sure it is set to connect to the virtual network created earlier.

![Image](https://github.com/user-attachments/assets/4ffaf355-8f2b-4bee-ad6a-8fe67bc64605)
<br />
<br />

Next, we'll set up a second virtual machine to act as the client. This VM should use a Windows 10 image instead of Windows Server.

![Image](https://github.com/user-attachments/assets/4c0875f0-b90e-4bc2-a9e9-3c0dafecaaf1)

![Image](https://github.com/user-attachments/assets/8ba8ac87-827f-4771-a739-28350393c013)
<br />
<br />

In the Virtual Machine's Networking tab, ensure it is configured to connect to the previously created virtual network.

![Image](https://github.com/user-attachments/assets/b79d5bd4-0708-4574-a261-29d96403b814)
<br />
<br />

I need to set the Domain Controller's private IP address to static instead of dynamic, as it will also function as a DNS server. A static IP ensures the DNS configuration remains consistent for the client. I'll make this change in the DC's network settings.

<img src="https://i.ibb.co/nq4kj1gW/Screenshot-3-3-2025-114045-portal-azure-com.jpg" alt=""/>

![Image](https://github.com/user-attachments/assets/55507c04-e0af-4f60-bf76-fc7973c2af3d)
<br />
<br />

Next, we'll connect using Remote Desktop, utilizing the Domain Controller's public IP address and the login credentials created during the Virtual Machine setup.

![Image](https://github.com/user-attachments/assets/3fa3ffc7-2e06-42e9-b6d2-6a16aa5fa08a)
<br />
<br />

After logging into the Domain Controller, you should see the Server Manager screen displayed.

<img src="https://i.ibb.co/1t620tnm/Screenshot-2025-03-03-115311.png" alt=""/>
<br />
<br />

To disable the firewall, I’ll right-click the "Start" button and select "Run." Then, I'll type "wf.msc".

<img src="https://i.ibb.co/N2BzzD6z/Screenshot-2025-03-03-115530.png" alt=""/>
<br />
<br />

Click on "Windows Defender Firewall Properties," then disable the firewall state under the "Domain Profile," "Private Profile," and "Public Profile" tabs.

<img src="https://i.ibb.co/Q7P8sNM0/Screenshot-2025-03-03-115748.png" alt=""/>

![Image](https://github.com/user-attachments/assets/76c27b61-f299-45e8-932c-06661ae3f170)
<br />
<br />

Next, we need to configure the client's DNS settings to point to the Domain Controller. To do this, we’ll return to Microsoft Azure to obtain the Domain Controller's private IP address</p>

<img src="https://i.ibb.co/k63vzZ70/Screenshot-2025-03-03-120122.png" alt=""/>
<br />
<br />

Next, I’ll navigate to the client machine’s network settings, select the NIC (Network Interface Card), go to settings, and then DNS servers. I'll change the option from "Inherit from virtual network" to "Custom," enter the Domain Controller’s private IP, and save the changes.

<img src="https://i.ibb.co/MFMt3Xj/Screenshot-2025-03-03-120418.png" alt=""/>
<br />
<br />

After that is completed, we will restart the client Virtual Machine.

<img src="https://i.ibb.co/bMBLXHgr/Screenshot-2025-03-03-120629.png" alt=""/>
<br />
<br />

After the machine restarts, I’ll use Remote Desktop to connect to the client machine using its public IP and the login credentials I set up during its configuration

<img src="https://i.ibb.co/HpxH9KBc/Screenshot-2025-03-03-121802.png" alt=""/>
<br />
<br />

Now that I'm logged in, I'll open PowerShell and ping the Domain Controller using its private IP. If there's a timeout error, ensure both machines are on the same virtual network in Azure, as this could be the issue.

![Image](https://github.com/user-attachments/assets/455cb363-05d0-461c-a516-75b182160a6e)
<br />
<br />

I'll run "ipconfig /all" and check the "DNS Servers" section. It should point to our Domain Controller if everything is set up correctly.

![Image](https://github.com/user-attachments/assets/07701018-18d9-450f-9ce6-e05ad6d727cf)
<br />
<br />
