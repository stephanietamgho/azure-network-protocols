<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Creation of our resources (Windows 10 and Ubuntu virtual machines + a Resource Group)
- Observe ICMP Traffic (when allowed and denied)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>


<p><h2> 1. CREATION OF OUR RESOURCES (WINDOWS AND LINUX VM + A RESOURCE GROUP) </h2></p>
<br />
<p><h3>Create a Ressource Group</h3></p>
<p>
<img src="https://i.imgur.com/AdO9wfj.png" height="50%" width="50%" alt="Resource Group creation"/>
</p>
<p>
In Microsoft Azure, create a Resource Group, give it a name and, assign it a server's location. Here, I chose West 2 Region but you can pick the one you want.
</p>
<br />
<br />
<br />

<p><h3>Create a Windows 10 VM</h3></p>
<p>
<img src="https://i.imgur.com/A2QIfPX.png" height="50%" width="50%" alt="Win 10 VM Creation"/>
</p>
<p>
Create a Windows 10 Virtual Machine (VM). Make sure you select the previously created Resource Group. 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/QZtYL1c.png" height="50%" width="50%" alt="Win 10 VM Creation"/>
</p>
<p>
Choose a size (at least 2cpus) and set your username and password that will allow you to connect to your VM remotely.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/yU3gWbX.png" height="80%" width="80%" alt="Win 10 VM creation"/>
</p>
<p>
Click on Networking (two tabs after the main Basic page) and notice how your new Virtual Network (Vnet) and Subnet. Keep everything else as is. 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/wRP0TA2.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
Then click Create + Review. Once you pass the validation phase, you may eventually click on "Create".
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/7aBf8GC.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
You may check your Resource Group, a see a list of new resources your Windows VM is creating.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/h7TpNKy.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
The deployment of your Windows VM is now complete. You may create your Ubuntu VM.
</p>
<br />
<br />
<br />

<p><h3>Create a Linux (Ubuntu) VM</h3></p>
<p>
<img src="https://i.imgur.com/9ei7Jg3.png" height="80%" width="80%" alt="Linux Ubuntu VM Creation"/>
</p>
<p>
Create a Linux Ubuntu VM. Make sure to select the previously created Resource Group and Vnet.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/V4KDbx3.png" height="80%" width="80%" alt="Linux Ubuntu VM Creation"/>
</p>
<p>
Under "Administrator Account", check "Password". Then, set your username and password. For convenience, use your credentials previously created during your Windows 10 VM set up.
</p>
<br />
<br />
<br />

<p><h2> 2. OBSERVE ICMP TRAFFIC </h2></p>
<p>
<img src="https://i.imgur.com/wh9FaI6.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
 <p>
<img src="https://i.imgur.com/6UZZqEz.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Use Microsoft Remote Desktop to connect to your Windows 10 Virtual Machine.
</p>
<br />


<p>
<img src="https://i.imgur.com/PQCMGmu.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Within your Windows 10 VM, install Wireshark. Use their default setup.
</p>
<br />


<p>
<img src="https://i.imgur.com/Kiw73AK.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
 </p>
<img src="https://i.imgur.com/2QkdJbf.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Open Wireshark, click "Ethernet" and on the search bar write "ICMP". Your Wireshark will filter for ICMP traffic only.
</p>
<br />


<p>
<img src="https://i.imgur.com/59kbE9C.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Back to Microft Azure, retrieve the private IP address of the Ubuntu VM, we will attempt to ping it within the the Windows 10 VM.
</p>
<br />
<br />



<p>
<img src="https://i.imgur.com/mHCOZyM.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
<img src="https://i.imgur.com/OdfZFgh.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
<img src="https://i.imgur.com/16UDFjJh.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Back to your Windows VM, open "Powershell". Then ping your Ubuntu VM using the "ping" command and your Ubuntu private IP address. Here my private Linux IP is 10.0.0.5. You may notice in Wireshark (the pink screen) my private Windows 10 IP address (10.0.0.4) is sending ping requests to my Linux VM (10.0.0.5), and the latter replying.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/6ZXl3yZ.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
You may also initiate perpetual ping request from your Windows to your Ubuntu, adding the "-t" to your command line, as an indication that you want to initiate perpetual ping.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/D17OoKh.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Now, observe your Wireshark screen to witness the exchanges. When ready, press "Ctrl+c" on your command line to stop the ping requests.
</p>
<br />
<br />
<br />


<p><h3>Blocking and Re-allowing ICMP Traffic</h3></p>

<p>
<img src="https://i.imgur.com/rswZPDG.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
Back to Azure, go to your Ubuntu machine setting, and go to "Networking". This is your Network Security Group (nsg) a type of virtual firewall. To your right, you will notice that you can add new rules. Click on it. We will add a rule to block inbound ICMP traffic.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/41kbHes.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
On the set up page, we will keep the sources and destination port ranges to (*) which is another way to say Any, and click on "ICMP" under Protocol and "Deny" for the action we want our firewall to take. The priority is set to 200, the most important priority level. So the firewall will start with that rule first before moving on to the subsequent rules with higher numbered prioirity level. Name your rule.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/RfNa5eZ.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
Back to your Windows 10 VM and command line, try to Ping your Ubuntu VM. Notice as the request timed out as your Windows VM didn't receive any replies from your Ubuntu. Basically all ping requests were lost since there was no location that intercepted the ping to reply back.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/vrQe9aD.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
Notice this interaction on Wireshark.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/9bRHbdx.png" height="80%" width="80%" alt="Re-allowing ICMP Traffic"/>
 </p>
 <p>
<img src="https://i.imgur.com/BmCEEgh.png" height="80%" width="80%" alt="Re-allowing ICMP Traffic"/>
</p>
<p>
Back to your Ubuntu VM settings in Azure, go to your Networking session. We will re-authorize IMCP traffic. You can click directly on the Deny ICMP rule to delete it. Or edit the rule and click "Allow".
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/16UDFjJ.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Let's ping Ubuntu again and observe the ICMP traffic in Wireshark. 
</p>
<br />
<br />
<br />


<p><h2> 2. OBSERVE SSH TRAFFIC </h2></p>

<p>
<img src="https://i.imgur.com/qAZP859.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
Back to Wireshark on your Windows 10 VM, filter for SSH traffic only.
</p>
<br />
<br />
<p>
<img src="https://i.imgur.com/UdqpvOH.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
Back to Powershell on your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address). Press enter.
</p>
<br />
<br />


<p>
<img src="https://i.imgur.com/si5A3oN.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
Since its our first try, a warning message will be displayed. Write yes. Then, enter your password. Note: You will not see the letters or keys on the screen so make sure you write the correct password. :) Once you're in, you have full command your Linux VM!
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/5fgacre.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
Back to Wireshark, you can observe our first connections with Ubuntu VM. 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/puaboKW.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
<img src="https://i.imgur.com/NT98qlj.png" height="80%" width="80%" alt="Observe SSH Traffic"/>
</p>
<p>
Add a few commands on powershell to observe the traffic on Wireshark. Then exit the SSH connection by typing "Exit" and pressing Enter.
</p>
<br />
<br />



<p><h2> 3. OBSERVE DNS TRAFFIC </h2></p>
<p>
<img src="https://i.imgur.com/YO7RwHC.png" height="80%" width="80%" alt="Observe DNS Traffic"/>
</p>
<p>
Back to Wireshark on your Windows 10 VM, filter for DNS traffic only.
</p>
<br />
<br />


<p>
<img src="https://i.imgur.com/PCQq5Bw.png" height="80%" width="80%" alt="Observe DNS Traffic"/>
</p>
<p>
Back to Powershell on your Windows 10 VM, within the command line, use nslookup to see what disney.com's IP address is. 
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/kUSuunI.png" height="80%" width="80%" alt="Observe DNS Traffic"/>
</p>
<p>
Observe the DNS traffic being shown in Wireshark.
</p>
<br />
<br />
<br />


<p><h2> 4. OBSERVE RDP TRAFFIC </h2></p>
<p>
<img src="https://i.imgur.com/fqH3MYS.png" height="80%" width="80%" alt="Observe RDP Traffic"/>
</p>
<p>
Back to Wireshark on your Windows 10 VM, filter for RDP (or, tcp.port ==3389) traffic only. Notice how our Windows 10 VM is constantly in traffic. RDP protocol is constantly showing a live stream from one computer to another.
</p>
<br />
<br />
<br />



<p><h2> 5. OBSERVE DHCP TRAFFIC </h2></p>
<p>
<img src="https://i.imgur.com/9gotxtg.png" height="80%" width="80%" alt="Observe DHCP Traffic"/>
</p>
<p>
Back to Wireshark on your Windows 10 VM, filter for DHCP traffic only.
</p>
<br />
<br />

<p>
<img src="https://i.imgur.com/JIhQFjE.png" height="80%" width="80%" alt="Observe DHCP Traffic"/><p/>
<p> Within the command line, use ipconfig /renew to attempt to issue your VM a new IP address.
</p>
<br />
<br />


<p>
<img src="https://i.imgur.com/.png" height="80%" width="80%" alt="Observe DHCP Traffic"/>
</p>
<p>
Observe the DHCP traffic appearing in Wireshark.
</p>
<br />
<br />


<p><h2>VOILA! 🤓<h/2><p/>
