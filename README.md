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
Welcome to my tutorial on Network Security Groups and Inspecting Network Protocols. First you will need to create two VMs on Azure. One machine will be a Linux machine and the other will be a Windows 10 machine. Both will have two cpus and they must be on the same VNET. Once that is done go on the Windows machine and download Wireshark. I will attatch a link to the wireshark download. https://www.wireshark.org/download.html Once installed open Wireshark and filter for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol, ping tests connectivity between hosts. When we filter wirehsark to only capture ICMP packets and ping the private IP address of our linux machine we can visually see the packets on wireshark. 
</p>
<br />
<p>
<img width="1500" height="857" alt="Screenshot 2026-11-09 005320" src="https://github.com/user-attachments/assets/bbb7103d-b1c0-4050-9f9b-f9516aa075c2" />

</p>
<p>
We can inspect each individual packet and see the actual data that is being sent in each ping. the picture below demonstrates just that. 
</p>
<br />
<p>
<img width="1631" height="411" alt="Screenshot 2026-11-09 005751" src="https://github.com/user-attachments/assets/642a5983-b9de-4abb-b2e8-1bbaaabbfcfb" />

</p>
<p>
In the next portion of the lab we will perpetually ping the Linux machine with the command ping -t. This will continually ping the machine until we decide to stop it, while the Windows machine is pinging the Linux machine we will go to the Linux machine and block inbound ICMP traffic on it's firewall. Once we do that we will stop recieving echo replys from the Linux machine. We will block ICMP by creating a new Network Security Group on the Linux machine that will be set to block ICMP. We can allow the traffic by allowing ICMP on the Linux Network Security Groups page on Azure. 
</p>
<br />
<img width="1407" height="836" alt="Screenshot 2026-11-09 011435" src="https://github.com/user-attachments/assets/192f6906-af26-4409-b106-b97a661a6ef4" />

</p>
<img width="1586" height="447" alt="Screenshot 2026-11-09 011217" src="https://github.com/user-attachments/assets/408b2ec6-5058-405e-b3f0-f2d1f75944e2" />

<p>
Next we will use our Windows machine to SSH to the Linux machine. SSH has no GUI it just gives the user access to the machines CLI. We will set the wireshark filter to capture SSH packets only. When we ssh into the Linux machine with the command prompt "ssh labuser@10.0.0.5" we can see that wireshark starts to immediately capture SSH packets.
</p>
<br />
<img width="1503" height="883" alt="Screenshot 2026-11-09 160133" src="https://github.com/user-attachments/assets/b1a1e41b-f35b-4a79-a858-8a9c2d1c7df2" />

</p>
<p>
Now we will use wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol this works on ports 67/68. It is used to assign IP addresses to machines. We will request a new ip address with the command "ipconfig /renew". Once we enter the command wireshark will capture DHCP traffic.
</p>
<br />
<img width="1442" height="850" alt="Screenshot 2026-11-09 163416" src="https://github.com/user-attachments/assets/c08f4418-fd7e-4f85-b64f-3141d2943a91" />

</p>
<p>
Time to filter DNS traffic. We will set wireshark to filter DNS traffic. We will initiate DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks our DNS server what is google's IP address.
</p>
<br />
<img width="1602" height="886" alt="Screenshot 2026-11-09 163933" src="https://github.com/user-attachments/assets/700fae0a-a140-4856-8cc9-a95feb9cd471" />

</p>
<p>
Lastly we will filter for RDP traffic. When we enter tcp.port==3389 traffic is spammed non stop because we are using Remote Desktop Protocol to connect to our Virtual Machine. 
</p>
<br />
<img width="1617" height="938" alt="Screenshot 2026-11-09 164322" src="https://github.com/user-attachments/assets/b8cbcdc5-226c-47bc-8d43-71629b3ea399" />

</p>
<p>
