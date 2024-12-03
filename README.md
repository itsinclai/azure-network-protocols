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

<b><p>Step 4: Filtering for ICMP Traffic</b></p>

![image](https://github.com/user-attachments/assets/906c2803-6afd-48ac-9836-4464372d7c0f)

In the filter bar at the top of the Wireshark interface, type icmp and press Enter. This command filters out all traffic except ICMP packets, making it easier to analyze the data we’re interested in.

At this point, Wireshark is prepared to capture and display ICMP traffic generated during connectivity tests.

<p><b>Step 5: Pinging the Linux VM from the Windows 10 VM</b></p>

![image](https://github.com/user-attachments/assets/1caf653d-056b-43a0-8e00-4450f8f68dec)

Next, we’ll test the connectivity between the two virtual machines:

- Open the Azure Portal and locate the private IP address of the Ubuntu VM.
- Inside the Windows 10 VM, open the Command Prompt or PowerShell.
- Use the ping command to send ICMP requests to the Linux VM. For example:
ping <Ubuntu VM private IP>

In Wireshark, observe the ICMP packets generated by this action. 

![image](https://github.com/user-attachments/assets/514107c4-5a4e-4d76-9c70-4f756178af58)

For each ping attempt:

- The Echo Request is sent from the Windows VM to the Linux VM.
- The Echo Reply is sent back from the Linux VM to the Windows VM, confirming that the packets were received.

This back-and-forth communication validates that the two VMs can communicate over the private network.

<p><b>Step 6: Exploring Packet Details</b></p>

![image](https://github.com/user-attachments/assets/20452e20-e4d5-46b5-8019-94e7a533ef0f)

Wireshark provides detailed information about each packet. By selecting an ICMP packet, you can view its breakdown across different layers of the OSI model:

- Layer 2 (Data Link): Examine the source and destination MAC addresses. These correspond to the virtual NICs of the Windows and Linux VMs.
- Layer 3 (Network): Analyze the IP headers, which include the source (Windows VM) and destination (Linux VM) private IP addresses.

These layers illustrate how the ICMP traffic is routed through the network, encapsulated in lower-layer protocols for delivery.

![image](https://github.com/user-attachments/assets/5e039679-d600-4350-8566-85fc891c9af9)

<p><b>Step 7: Pinging Google’s Public IP Address</b></p>

To analyze traffic beyond the private virtual network, we’ll test connectivity to a public server using its IP address. Specifically, we’ll ping Google’s public DNS server at 8.8.8.8. This avoids the need for DNS resolution and directly targets an accessible public IP.

![image](https://github.com/user-attachments/assets/387241aa-70a2-444c-b939-fc3429a2f1bc)

In the Command Prompt or PowerShell on the Windows 10 VM, type the following command:
Copy code ping 8.8.8.8

Observe the traffic in Wireshark:

- ICMP Echo Requests are sent from the Windows VM to Google’s server at 8.8.8.8.
- ICMP Echo Replies are received from Google’s server, confirming successful communication over the internet.

![image](https://github.com/user-attachments/assets/94c6612c-a78b-4ed8-a04e-58e801d6dff5)

Analyze the packet details in Wireshark to understand the flow:

- The Source Address corresponds to the private IP of your Windows VM.
- The Destination Address is 8.8.8.8.

<h2>Configuring a Firewall with Network Security Groups (NSG)</h2>

In this section, we will simulate how a firewall works by modifying the Network Security Group (NSG) associated with the Ubuntu VM. This will allow us to control the flow of ICMP traffic (ping requests) and observe the impact of these changes on network communication. By using Wireshark and the command line, we’ll see the effects in real time and develop a deeper understanding of how traffic rules shape connectivity in Azure environments.








