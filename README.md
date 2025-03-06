# Azure-Network-Protocals
How to observe and interpret various network traffic.


# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines using Wireshark

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.


# Environments and Technologies Used

Microsoft Azure (Virtual Machines/Compute)
Remote Desktop
Various Command-Line Tools
Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
Wireshark (Protocol Analyzer)


# Operating Systems Used

Windows 10 (21H2)
Ubuntu Server 20.04


# High-Level Steps

Create a Resource Group
Create a Virtual Machine
Observe ICMP Traffic
Observe SSH Traffic
Observe DHCP Traffic
Observe DNS Traffic
Observe RDP Traffic


# Actions and Observations

Set up your virtual environment

First, let's create our Resource Group inside our Azure subscription.

![image](https://github.com/user-attachments/assets/d147bade-7305-48c8-b5fb-6c68d4587a7a)

Now create your Windows virtual machine. I typically create the VM in (US) East US.

While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the Administrator Account section:

![image](https://github.com/user-attachments/assets/5a2902cc-1f24-4ca0-99cb-f5df2336ebaa)


# Create an Ubuntu virtual machine.

While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet. Make sure to use the password option under the Administrator Account section (not seen in image):

![image](https://github.com/user-attachments/assets/49dc8a81-0063-4741-a546-e5d00d82e92a)

Observe Your Virtual Network within Network Watcher:

![image](https://github.com/user-attachments/assets/43d3851d-8c63-46eb-87c7-1b04061649e6)


# Now let's observe some ICMP traffic

Remote into your Windows 10 Virtual Machine, install Wireshark, open it and filter for ICMP traffic only.

![image](https://github.com/user-attachments/assets/043ff6da-f859-43f7-8a5c-d876a4a38259)

Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM. Observe ping requests and replies within WireShark:

![image](https://github.com/user-attachments/assets/c468d4c2-652f-4bc4-ade9-0e714d96e7ac)

![image](https://github.com/user-attachments/assets/4be90dd0-6a1f-4d0e-8353-76d554863314)

Attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark:

![image](https://github.com/user-attachments/assets/71aee6c0-58c0-49ad-a85b-1e84d7c99e4e)

Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM:

![image](https://github.com/user-attachments/assets/0805bc7b-e591-4e0e-88be-77c6d5c7520c)

Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic, while back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity:

![image](https://github.com/user-attachments/assets/004237b8-ddc2-4ee0-b564-ec6375de580e)

![image](https://github.com/user-attachments/assets/446c36de-5454-41e5-8b2a-a2d0ad44f8ab)

Re-enable ICMP traffic for the Network Security Group in your Ubuntu VM and back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line ping activity (should start working again).Finally, stop the ping activity:

![image](https://github.com/user-attachments/assets/68840445-1d30-4163-bf72-34c5c089ac9c)


# Time to observe SSH traffic

Back in Wireshark, filter for SSH traffic only and from your Windows 10 VM, “SSH into” your Ubuntu virtual machine (via its private IP address). Type commands (ls, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark.

Exit the SSH connection by typing ‘exit’ and pressing [return]:

![image](https://github.com/user-attachments/assets/e13a8900-a18e-4c58-9fb2-b14dc05b05b9)


# Next, we're going to observe DHCP Traffic

Back in Wireshark, filter for DHCP traffic only. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)

Observe the DHCP traffic appearing in WireShark:

![image](https://github.com/user-attachments/assets/fd20e030-9742-4e2c-b699-5e295354f7ac)


# Let's now observe our DNS traffic next

Back in Wireshark, filter for DNS traffic only.

From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are and observe the DNS traffic being shown in WireShark:

![image](https://github.com/user-attachments/assets/db3f5230-77e1-47d5-8e25-0af626108a8a)


# Finally, we will observe RDP traffic to finish up this tutorial

Back in Wireshark, filter for RDP traffic only using "tcp.port==3389".

You'll be obseving a non-stop stream of traffic. Do you know why there is constant traffic in our tcp.port==3389?

The answer is because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted:

![image](https://github.com/user-attachments/assets/658e8da9-0949-4647-999c-70d0ede8dcf2)

Now that we're finished observing the network, DON'T FORGET TO CLEAN UP YOUR AZURE ENVIRONMENT! This will prevent you from incurring additional charges and you won't be left surprised!

Close your Remote Desktop connection, delete the Resource Group(s) created at the beginning of this tutorial, and verify Resource Group deletion. You'll typically be notified or can click unde the bell notification just to make sure.



