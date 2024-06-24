<h1> Vulnerability Testing Siemens ICS devices and manipulation core processes using pentesting tools</h1>

<h2>Description</h2>
In this project I had the opportunity to attend an event at the University of Bristol where we were given our own ICS training box which we learnt to vulnerability test and eventually manipulate the processes it was carrying out. <br>

<h2>Software Used</h2>

- <b>Nmap:</b> Pinging the training boxes and identifying the services that are running and protocols that are open.
- <b>Wireshark:</b> Analyse packets shared from the ICS to our device.
- <b>Python:</b> Execute scripts on the ICS after gaining access to manipulate the processes.
- <b>Hydra:</b> Brute forcing the ICS password
- <b>VNCViewer:</b> Connection to Sm@rtServer service on the ICS.



<h2>Project walk-through:</h2>
We started out by learning about ICS and their fundamentals including the common vulnerabilities that plague them. The University of Bristol was kind enough as to provide us with our own training boxes that was equipped with Siemens' Simatic HMI (Human Machine Interface) and S7-1200 PLCs (Programmable Logic Controller).
<br /><br />
<p align="center">

<img src="https://i.imgur.com/n13WT5l.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<p align="center">

<img src="https://i.imgur.com/xBMG8aL.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
Next we run an Nmap scan on the training box to see all the ports that are currently open. Once we identify the ports and what service it is currently running, we run an Nmap script that gives us detailed information on the exact service and information about the actual PLC itself, take a look: 
<br /><br />
<p align="center">

<img src="https://i.imgur.com/BTpXXbd.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<p align="center">

<img src="https://i.imgur.com/ozBcFbY.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
Once we identify the IP addresses of the HMI and the PLC we run vncviewer on that specific IPs to get a connection on to the actual HMI itself. The reason we do this is so that in an actual attacker situation we need to ensure that the people controlling the ICS are not able to stop the attack by overriding the software commands that we are passing. Take a look at what the remote connection looks like: 
<br /><br />
<p align="center">

<img src="https://i.imgur.com/8KHKHiX.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<p align="center">

<img src="https://i.imgur.com/DpSzLoi.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
Now you might be asking how we used vncviewer to connect to the Sm@rtServer service and for that we used xHydra on Kali to run a brute force attack on the Sm@rtServer password as the service has a character limit of 8 characters on any password. Due to this reason most people setup very easily crackable passwords on their HMIs.
<br /><br />
<p align="center">

<img src="https://i.imgur.com/L2hcjuQ.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<p align="center">

<img src="https://i.imgur.com/vz2VsQi.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
Now that we have control over the HMI we move onto to trying to manipulate the PLC itself. To do that we first use wireshark to analyze the data that it is transmitting. The protocol that it uses is called S7COMM and once we filter the packets we can see all the relevant packets we need:
<br /><br />

<p align="center">

<img src="https://i.imgur.com/aRVLBO1.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
The reason we do this is so that we can pinpoint exactly where the variables that control the final output or input of the process is stored such that we can write a script with python that is able to manipulate the variable to cause havoc. To better understand the PLC we use Siemens' TIA v18 software that gives us the schmatics of the PLC itself. Take a look: 
<br /><br />

<p align="center">

<img src="https://i.imgur.com/xzfGLP6.jpeg" height="80%" width="80%" alt="Task 1"/>
</p>
<br /> 
Once we identify where the variables are stored and how to access them we can easily write a python script which i cannot share here to manipulate the output of the actual PLC causing the entire ICS to malfunction.
<br /><br />
Now there are some easy ways to mitigate such an attack and they are as follows: 

- Disable the Sm@rtServer utility unless absolutely necessary
- If not able to disable the atleast set the most secure password possible within the 8 character limit of the Sm@rtServer service.
- Ensure that different HMIs have different passwords.
- Properly configure firewalls to limit access to the outside world.
- Patch devices asap so as to not leave any common vulnerabilities available to exploit.
- Configure the intrusion detection system to look for any large volumes of failed VNC connections so that brute forcing becomes harder.

<br /> <br /> 
Finally I'd like to thank University of Bristol for holding this event and enabling us to get hands on experience of HMIs and PLCs and how to experiment with them. This was a great learning experience and I hope to further my own learning in my free time. 
Thank you for reading my report on this project. <br/>
-rikosxay
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
