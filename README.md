<p align="center">

  ![geORXiQ](https://github.com/user-attachments/assets/6e191feb-e704-486a-b7c5-65fc890dda60)

</p>

<h1>Performing Network Activities and Configuring a Firewall - Prerequisites and Installation</h1>
This tutorial outlines the creations of Azure VMs and installing Wireshark within the VMs. We also perform some basic network activities between the VMs and observe the results in Wireshark. <br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Wireshark (Within the VMs)
- Windows Powershell

<h2>Operating Systems Used </h2>

- Windows 10 pro</b> (22H2)
- Linux (Ubuntu)


<h2>Installation Steps</h2>

<p>
<img src="https://imgur.com/7bQ1DAk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We start off at the home page in Azure and we navigate to create a resource group that I have labeled as Wireshark-Networking and that will contain our Virtual Machines.
</p>
<br />

<p>
  
 ![Win10 vm](https://github.com/user-attachments/assets/69bcee0d-6890-48cf-b171-43a319cdffa0)
</p>
<p>
After the resource group is created, we then proceed to make the Windows 10 virtual machine. When creating the VM I have to make sure the Resource group it uses is the one I created in the previous step (Wireshark-Networking) and it will also create a new Virtual Network and Subnet. Also, creating a username and password for the VM is required.
</p>
<br />

<p>
  
![Linux vm](https://github.com/user-attachments/assets/4fa36756-cf4a-4a7c-8f41-fddf17a4bd12)
</p>
<p>
We then repeat the last step but in this case we are making a Linux (Ubuntu) VM, making sure that it uses the same Resource group and Virtual Network as the Windows 10 VM (circled in yellow).
</p>
<br />

<p>
  
![Connecting to Win 10 VM](https://github.com/user-attachments/assets/31ddab6f-10c8-4770-b37a-60ab95df32e8)
</p>
<p>
Now that both VMs are created, we can now connect to the Windows 10 VM using Microsoft Remote Desktop. We do this by taking the Windows 10 VM's Public IP Address (Circled in red) and pasting into remote desktop. When clicking connect, it will ask for a username and password which is what we entered when creating the VM in Azure.
</p>
<br />

<p>
</p>
Once we are connected to the Windows 10 VM, we can now navigate to Microsoft edge and install wireshark.
</p>
<br />

<p>

![Wireshark Packet Capture](https://github.com/user-attachments/assets/033702bf-c4d6-4ed6-af17-8dc7727156be)
</p>
Once Wireshark is installed, we can begin packet capture (Circled in red).
</p>
<br />

<p>

![Network Traffic](https://github.com/user-attachments/assets/a4dfa8c2-ce07-4da8-a121-eb8d0c16d020)
</p>
After starting the packet capture in wireshark, it will start showing the constant network traffic within our network which is why we set a filter to display the icmp traffic only (circled in red). When we use the ping command in Windows Powershell to ping the Linux VM or google.com as another example, we can see when the Windows VM is sending a request to the Linux VM and receiving a reply (circled in yellow). When pinging within powershell, it sends 4 pings and at the end it will show how many were received indicating the ping was successful. I repeat this step pinging to google.com and we get the same result (circled in green). The private IP addresses for the VMs and for google.com are listed below which allows you to see how they interacted with each other when pinging.

- Windows VM- 10.1.0.4
- Linux VM- 10.1.0.5
- google.com- 142.251.33.164

</p>
<br />

<p>

![nonstop ping to linux vm](https://github.com/user-attachments/assets/6355b135-4d9e-4a42-9b58-6080c79a56d3)
</p>
We can now initiate a continuous ping from the Windows 10 VM to the Linux VM. We do this by entering the ping -t command in powershell in the Windows 10 VM (Circled in red). You can now see the constant pings displaying in Wireshark as they happen in Powershell.
</p>
<br />

<p>

![adding rule](https://github.com/user-attachments/assets/71b9745e-2f2d-4164-8815-295b968eac4d)
</p>
Next, we navigate back to Azure and select the Linux VM and go to its network settings. We are going to configure the Firewall for the Linux VM by disabling incoming ICMP traffic. After selecting the firewall for the Linux VM, I add a rule that allows us to disable the incoming ICMP traffic (shown above).
</p>
<br />

<p>

![No response](https://github.com/user-attachments/assets/6958ee85-9c8d-4026-9fcd-c04d42be7854)
</p>
After adding that rule for the firewall of the Linux VM, it is not allowing any ICMP traffic. Since we did this while the Windows VM was pinging the Linux VM, it is showing that the ping request are no longer getting a response from the Linux VM as a result from this (Displayed back on the Windows 10 VM on Wireshark and Powershell.
</p>
<br />
