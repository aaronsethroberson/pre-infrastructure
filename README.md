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

![image](https://github.com/user-attachments/assets/ecd6b747-791a-4fab-b70a-060057ecebf0)

<p align="center">
Next, we will create a Virtual Network.
</p>

![image](https://github.com/user-attachments/assets/59c00f49-4853-488b-9d8c-bd7417c62aee)

<p>
After creating both the resource group and virtual network, we'll move on to creating and setting up our virtual machine, which will serve as the Domain Controller (running Windows Server 2022).
</p>

![image](https://github.com/user-attachments/assets/afab89dc-8e8e-45c0-9f55-3406fcd449e9)

![image](https://github.com/user-attachments/assets/fe7b7d05-701a-4ed7-92d4-15ed9293d81b)

<br />

</p>In the Virtual Machine's Networking tab, we need to ensure that it is configured to connect to the virtual network we set up earlier </p>

![image](https://github.com/user-attachments/assets/0d162eb8-200f-4a5a-8f6e-d8eeb4d1de3a)

<p>Next, we will create a second virtual machine to serve as the client. For this VM, the image should be Windows 10, not Windows Server</p>

![image](https://github.com/user-attachments/assets/b39c7903-30b7-4064-ab6b-fe4a7abdfdbf)

![image](https://github.com/user-attachments/assets/5a6d5e06-91a0-4e76-91e6-1945abb8ff2f)

<p>In the Virtual Machine's Networking tab, we need to ensure that it is configured to connect to the virtual network we set up earlier.</p>

![image](https://github.com/user-attachments/assets/b90bd423-583c-47b4-afb3-45267b1d45f6)

<p>I need to change the Domain Controller's private IP address from dynamic to static since it will also serve as a DNS server. A static IP ensures the DNS configuration remains valid for the client. I'll update this in the DC's network settings.</p>

![image](https://github.com/user-attachments/assets/a21af208-ca09-411e-ae32-cb65c3c6a910)
![image](https://github.com/user-attachments/assets/3fbf5935-ac8f-488c-b848-f3010613d620)

<p>Next, we'll use Remote Desktop along with the Domain Controller's public IP address and the login credentials we set up during the Virtual Machine configuration</p>

![image](https://github.com/user-attachments/assets/111958d5-2ce5-4da4-86f7-9a58823c977d)

<p align="center">
After logging into the Domain Controller, you should see the Server Manager screen displayed.
</p>

![Screenshot 2024-10-02 144622](https://github.com/user-attachments/assets/c3c871a4-1500-4715-a601-126531d6d652)

<p align="center">
To disable the firewall, I’ll right-click the "Start" button and select "Run." Then, I'll type "wf.msc".
</p>

![image](https://github.com/user-attachments/assets/ffa1b34c-a5a0-43c4-a02a-f712fd0757e0)

<p align="center">
Click on "Windows Defender Firewall Properties," then disable the firewall state under the "Domain Profile," "Private Profile," and "Public Profile" tabs.
</p>

![image](https://github.com/user-attachments/assets/a36dca60-fc80-4122-9cc5-fcefeaffb92c)
![image](https://github.com/user-attachments/assets/3c7b4f9e-f82b-4db5-acf4-f204df1f9ff8)

<p align="center">
Next, we need to configure the client's DNS settings to point to the Domain Controller. To do this, we’ll return to Microsoft Azure to obtain the Domain Controller's private IP address</p>

![image](https://github.com/user-attachments/assets/1151fa43-c461-4e5a-978d-63b9c8036599)

<p>Next, I’ll navigate to the client machine’s network settings, select the NIC (Network Interface Card), go to settings, and then DNS servers. I'll change the option from "Inherit from virtual network" to "Custom," enter the Domain Controller’s private IP, and save the changes.</p>

![image](https://github.com/user-attachments/assets/7d6a1744-a68c-47dc-96b6-7f2f96ee1afa)

<p align="center">
After that is completed, we will restart the client Virtual Machine</p>

![image](https://github.com/user-attachments/assets/c5b55841-f42f-4b06-97ad-11c08f886ffb)

<p align="center">
After the machine restarts, I’ll use Remote Desktop to connect to the client machine using its public IP and the login credentials I set up during its configuration
</p>

![image](https://github.com/user-attachments/assets/7f36ffe5-c802-4ea7-8f42-1f8857d51c5a)

<p>Now that I'm logged in, I'll open PowerShell and ping the Domain Controller using its private IP. If there's a timeout error, ensure both machines are on the same virtual network in Azure, as this could be the issue.</p>

![image](https://github.com/user-attachments/assets/70c23a03-a89a-4705-b0fb-287d9d70d3c2)

<p align="center">
I'll run "ipconfig /all" and check the "DNS Servers" section. It should point to our Domain Controller if everything is set up correctly.
</p>

![image](https://github.com/user-attachments/assets/9b7f2a23-2948-42cd-b81b-f170d76ef569)


