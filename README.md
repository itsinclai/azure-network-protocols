<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Actions and Observations</h2>

<b><p>Part 1: Create a Resource Group</p></b>

![image](https://github.com/user-attachments/assets/8746ede1-920f-4fd7-98e4-b73b833da12e)

<p>
Begin by creating a Resource Group in the Azure Portal. A Resource Group serves as a logical container to organize and manage all resources, including the virtual machines, networking components, and any associated configurations.
</p>

- Navigate to the Azure Portal and select Create a Resource Group.
- Name the Resource Group something meaningful, like Lab-NSG-Traffic, and assign it to an appropriate Azure region.

<b><p>Step 2: Provision a Windows 10 Virtual Machine</b></p>

![image](https://github.com/user-attachments/assets/6266eac8-ca08-466d-bf0c-fd731c79daf2)

<p>
Now, create the first VM running Windows 10. This will act as the workstation for observing and analyzing network traffic.
</p>

- In the Azure Portal, select Create a Virtual Machine and choose the Windows 10 operating system.
- Assign the VM to the previously created Resource Group (Lab-NSG-Traffic).
- Allow Azure to automatically create a Virtual Network (VNet) and a Subnet for this VM. These networking components will enable communication between resources.
- Configure basic settings like the VM size and authentication credentials.


<b><p>Step 3: Provision a Linux (Ubuntu) Virtual Machine</p></b>

![image](https://github.com/user-attachments/assets/28bc3c5f-8a82-4486-800d-64b74a8fa125)

<p>
Next, create a second VM, this time running Ubuntu Server 20.04, which will serve as the endpoint for network communication with the Windows VM.
</p>

- Again, select Create a Virtual Machine, but this time choose Ubuntu Server 20.04 as the operating system.
- Assign the VM to the same Resource Group (Lab-NSG-Traffic).
- Select the same Virtual Network and Subnet that was created for the Windows VM. This ensures both VMs can communicate without additional network configuration.
- Use a Username/Password for authentication, and configure any other required settings.


