# azure-network-protocols<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<p>
</p>
<p>
Welcome to my tutorial on Network Security Groups and Network Protocol Inspection.

First, I will create two virtual machines in Azure: a Linux VM and a Windows 10 VM. Both machines will have two CPUs and will be deployed within the same Virtual Network (VNET).

Next, I will install Wireshark on the Windows machine using the following download link: https://www.wireshark.org/download.html

Once installed, I will use Wireshark to filter for ICMP traffic. ICMP is a network-layer protocol used to send diagnostic and error messages. The ping command uses ICMP to test connectivity between hosts.

By filtering for ICMP packets and pinging the private IP address of the Linux VM, I can capture and visually inspect the network traffic in Wireshark.
</p>
<br />
<p>
<img width="1500" height="857" alt="Screenshot 2026-11-09 005320" src="https://github.com/user-attachments/assets/bbb7103d-b1c0-4050-9f9b-f9516aa075c2" />

</p>
<p>
We can inspect individual packets to view the data transmitted during each ping. The image below demonstrates how Wireshark captures and displays this information.
</p>
<br />
<p>
<img width="1631" height="411" alt="Screenshot 2026-11-09 005751" src="https://github.com/user-attachments/assets/642a5983-b9de-4abb-b2e8-1bbaaabbfcfb" />

</p>
<p>
In the next portion of the lab, I will continuously ping the Linux machine using the `ping -t` command on Windows. While the ping is running, I will configure an Azure Network Security Group (NSG) to block inbound ICMP traffic to the Linux VM. As a result, the Windows machine will stop receiving ICMP Echo Replies.

Finally, I will modify the NSG to allow ICMP traffic, restoring connectivity between the two machines.
</p>
<br />
<img width="1407" height="836" alt="Screenshot 2026-11-09 011435" src="https://github.com/user-attachments/assets/192f6906-af26-4409-b106-b97a661a6ef4" />

</p>
<img width="1586" height="447" alt="Screenshot 2026-11-09 011217" src="https://github.com/user-attachments/assets/408b2ec6-5058-405e-b3f0-f2d1f75944e2" />

<p>
Next, I will use the Windows machine to establish an SSH connection to the Linux VM. SSH provides secure remote access to the Linux command-line interface (CLI) without a graphical interface.

I will configure Wireshark to capture SSH traffic only. Using the command `ssh labuser@10.0.0.5`, I will connect to the Linux machine and observe Wireshark capturing SSH packets in real time.
</p>
<br />
<img width="1503" height="883" alt="Screenshot 2026-11-09 160133" src="https://github.com/user-attachments/assets/b1a1e41b-f35b-4a79-a858-8a9c2d1c7df2" />

</p>
<p>
Next, I will use Wireshark to filter for DHCP traffic. DHCP (Dynamic Host Configuration Protocol) uses ports 67 and 68 to automatically assign IP addresses to devices.

I will run the `ipconfig /renew` command on the Windows machine to request a new IP address. Wireshark will capture the DHCP traffic, allowing me to inspect the packets exchanged during the process.
</p>
<br />
<img width="1442" height="850" alt="Screenshot 2026-11-09 163416" src="https://github.com/user-attachments/assets/c08f4418-fd7e-4f85-b64f-3141d2943a91" />

</p>
<p>
Next, I will filter for DNS traffic in Wireshark. DNS (Domain Name System) translates domain names into IP addresses.

I will generate DNS traffic by running the `nslookup www.google.com` command. This sends a query to the DNS server requesting Google's IP address. Wireshark will capture the DNS packets, allowing me to inspect the request and response.
</p>
<br />
<img width="1602" height="886" alt="Screenshot 2026-11-09 163933" src="https://github.com/user-attachments/assets/700fae0a-a140-4856-8cc9-a95feb9cd471" />

</p>
<p>
Lastly, I will filter for RDP traffic in Wireshark using the filter `tcp.port == 3389`. RDP (Remote Desktop Protocol) uses TCP port 3389 to provide remote access to Windows machines. Since I am connected to the VM through Remote Desktop, Wireshark continuously captures RDP packets, allowing me to inspect the traffic in real time.
</p>
<br />
<img width="1617" height="938" alt="Screenshot 2026-11-09 164322" src="https://github.com/user-attachments/assets/b8cbcdc5-226c-47bc-8d43-71629b3ea399" />

</p>
<p>
