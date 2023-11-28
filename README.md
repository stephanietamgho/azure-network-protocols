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
<p><h3>Create a Ressource Group</h3></p>
<p>
<img src="https://i.imgur.com/AdO9wfj.png" height="80%" width="80%" alt="Resource Group creation"/>
</p>
<p>
In Microsoft Azure, create a Resource Group, give it a name and, assign it a server's location. Here, I chose West 2 Region but you can pick the one you want.
</p>
<br />


<p><h3>Create a Windows 10 VM</h3></p>
<p>
<img src="https://i.imgur.com/A2QIfPX.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
Create a Windows 10 Virtual Machine (VM). Make sure you select the previously created Resource Group. 
</p>
<br />

<p>
<img src="https://i.imgur.com/QZtYL1c.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
Choose a size (at least 2cpus) and set your username and password that will allow you to connect to your VM remotely.
</p>
<br />


<p>
<img src="https://i.imgur.com/yU3gWbX.png" height="80%" width="80%" alt="Win 10 VM creation"/>
</p>
<p>
Click on Networking (two tabs after the main Basic page) and notice how your new Virtual Network (Vnet) and Subnet. Keep everything else as is. 
</p>
<br />


<p>
<img src="https://i.imgur.com/wRP0TA2.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
Then click Create + Review. Once you pass the validation phase, you may eventually click on "Create".
</p>
<br />


<p>
<img src="https://i.imgur.com/7aBf8GC.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
You may check your Resource Group, a see a list of new resources your Windows VM is creating.
</p>
<br />

<p>
<img src="https://i.imgur.com/h7TpNKy.png" height="80%" width="80%" alt="Win 10 VM Creation"/>
</p>
<p>
The deployment of your Windows VM is now complete. You may create your Ubuntu VM.
</p>
<br />


<p><h3>Create a Linux (Ubuntu) VM</h3></p>
<p>
<img src="https://i.imgur.com/9ei7Jg3.png" height="80%" width="80%" alt="Linux Ubuntu VM Creation"/>
</p>
<p>
Create a Linux Ubuntu VM. Make sure to select the previously created Resource Group and Vnet.
</p>
<br />

<p>
<img src="https://i.imgur.com/V4KDbx3.png" height="80%" width="80%" alt="Linux Ubuntu VM Creation"/>
</p>
<p>
Under "Administrator Account", check "Password". Then, set your username and password. For convenience, use your credentials previously created during your Windows 10 VM set up.
</p>
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


<p>
<img src="https://i.imgur.com/6ZXl3yZ.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
You may also initiate perpetual ping request from your Windows to your Ubuntu, adding the "-t" to your command line, as an indication that you want to initiate perpetual ping.
</p>
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


<p>
<img src="https://i.imgur.com/41kbHes.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
On the set up page, we will keep the sources and destination port ranges to (*) which is another way to say Any, and click on "ICMP" under Protocol and "Deny" for the action we want our firewall to take. The priority is set to 200, the most important priority level. So the firewall will start with that rule first before moving on to the subsequent rules with higher numbered prioirity level. Name your rule.
</p>
<br />


<p>
<img src="https://i.imgur.com/RfNa5eZ.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
Back to your Windows 10 VM and command line, try to Ping your Ubuntu VM. Notice as the request timed out as your Windows VM didn't receive any replies from your Ubuntu. Basically all ping requests were lost since there was no location that intercepted the ping to reply back.
</p>
<br />

<p>
<img src="https://i.imgur.com/vrQe9aD.png" height="80%" width="80%" alt="Block ICMP Traffic"/>
</p>
<p>
Notice this interaction on Wireshark.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Observe ICMP Traffic"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
