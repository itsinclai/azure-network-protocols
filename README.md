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

- Create and configure Azure Virtual Machines (Windows and Linux).
- Observe ICMP traffic using Wireshark.
- Configure a Network Security Group to block ICMP traffic and observe changes.

<h2>Observing ICMP Traffic Between Virtual Machines with Wireshark</h2>

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

<h2>Observing ICMP Traffic Between Virtual Machines with Wireshark</h2>

<p>In this section, we’ll analyze ICMP (Internet Control Message Protocol) traffic between two Azure Virtual Machines using Wireshark, a powerful network protocol analyzer. 
ICMP is often used for diagnostic purposes, such as verifying network connectivity via the ping command. By observing ICMP traffic, we can better understand the communication flow between our VMs and identify the underlying protocols and layers involved in data transmission.</p>

<b><p>Step 1: Connecting to the Windows 10 Virtual Machine</b></p>

![image](https://github.com/user-attachments/assets/f47d115f-a1de-48d0-99e2-b69ee4547fd4)

<p>To begin, connect to the Windows 10 VM. If you’re using a Mac, download and install Microsoft Remote Desktop from the App Store. Once installed:</p>

- Open the Remote Desktop application and input the public IP address of the Windows VM, along with the credentials you created during setup.
- Establish the connection, and you’ll be presented with the Windows desktop environment inside the VM.

<p>This environment serves as our base for running Wireshark and conducting the traffic analysis.</p>

<b><p>Step 2: Installing Wireshark on the Windows 10 VM</b></p>

![image](https://github.com/user-attachments/assets/232a8c65-ddea-4a65-aa5f-13dc4b4d60b9)

<p>Inside the Windows 10 VM, we need to set up Wireshark to capture and analyze packets:</p>

- Open a web browser and navigate to the official Wireshark website. Download and install the software.
- Once installed, launch Wireshark. The first screen displays a list of network interfaces. Select the interface associated with your VM’s virtual network.

<p>Wireshark is now ready to begin capturing live network traffic, showing all the data packets sent to and from the VM.</p>

<b><p>Step 3: Starting a Packet Capture</p></b>

Click the Start button in Wireshark to begin capturing network traffic. You’ll immediately see a stream of packets in the main window, each representing a chunk of data traveling across the network. Every packet contains detailed information, including the source and destination addresses, protocols, and data payload.

Initially, this data might seem overwhelming because it includes all types of traffic, such as HTTP, DNS, and ICMP. To focus on specific types of traffic, we’ll use filters.




