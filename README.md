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

- Create an RG in Azure with two VMs in it, one running Windows and the other running Linux.
- Observe Network Topology within Azure.
- Download Wireshark.
- Observe and Send Traffic using Wireshark and Command Prompt.

<h2>Actions and Observations</h2>

<p>
<img src="https://imgur.com/By4NMzs.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Assuming you've done some of my other tutorials, navigate to your Azure account and create a Resource Group with two VMs in it. For the purposes of this lab, we'll make one VM a Windows machine and the other a Ubuntu Machine. Base them both in the same region, I'll be using US West 3. I'll also be making them both 2vcpu machines. Make sure you use a password for the Ubuntu machine, not an SSH Key. Also, make sure to put both machines on the RG's vnet.
</p>
<br />

<p>
<img src="https://imgur.com/vJkcWah.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once your RG is created with the two VMs deployed, navigate to the "Network Watcher" section of Azure, then click on topology. You should see something like the picture above.
</p>
<br />

<p>
<img src="https://imgur.com/1Tetelz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, log into your Windows machine in Remote Desktop. Navigate to wireshark.org in the Windows VM, and click "Download Now". Click on the Windows Installer option and wait for the file to download, then install the program. Click through all of the prompts. Once Wireshark is finished installing, open the program. 
</p>
<br />

<p>
<img src="https://imgur.com/o5mu6Th.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once you have the program open, click the blue shark fin and filter for ICMP traffic only, then open the Command Prompt and ping the Ubuntu Machine's private IP (ping [IP] -t), notice the requests and replies.
</p>
<br />

<p>
<img src="https://imgur.com/Z4ICmBZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Let's experiment a little. Go back to Azure and navigate to the "Networking" section of your Ubuntu Machine. Click on "Add Inbound Port Rule", then change the Protocol to ICMP, action to Deny, and Priority to 200. Click "Add". Go back to your Windows VM and observe the changes. Now enable the traffic from ICMP again. Notice the connection is back. Stop the ping with Ctrl + C.
</p>
<br />

<p>
<img src="https://imgur.com/5lg5lFV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Back in Wireshark, go ahead and refresh with the green shark fin, click continue without saving. Now, we're going to use ssh, so filter traffic in Wireshark by ssh. Open PowerShell on VM1 as an administrator, then type "ssh [VM2's Username]@" then VM2's private IP address. Then type yes, then type the password for VM2 (you won't be able to see the password, just trust that you're typing the right thing). Notice that just logging in has spammed some traffic in Wireshark. Feel free to play around with some commands, when you're done, type "exit" in PowerShell then press enter.
</p>
<br />

<p>
<img src="https://imgur.com/rXhd28a.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now go back to Wireshark and refresh it again. This time filter for DHCP traffic. Now, in PowerShell, attempt to give your Windows VM a new IP address using ipconfig /renew. Observe the DHCP traffic appearing in Wireshark.

</p>
<br />

<p>
<img src="https://imgur.com/LFYX4p0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, refresh Wireshark and filter for DNS. Using PowerShell, type "nslookup www.google.com". Observe the DNS traffic.
</p>
<br />

<p>
<img src="https://imgur.com/PT3wOzx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Refresh Wireshark once more and filter by tcp.port==3389. This is RDP traffic. Notice that it is a constant stream of information. This is because RDP acts as a "livestream" between two computers.
</p>
<br />


<p>
That's it for this one. If you'd like, continue playing with different protocols and port numbers, maybe some commands in PowerShell. When you're done, make sure to delete the RGs in Azure so you don't burn all your free credit. I'll see you soon!
</p>
<br />
