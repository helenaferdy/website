---
layout: post
title:  "Blue"
date:   2020-11-6 15:28:15 +0700
---
![Blue](https://i.imgur.com/e8i06nq.png)
&nbsp;  

Hey what's up!


This is the very first post on this blog/website thingy, so to start things off, we're gonna do some lightweight hacking.

The machine that we're gonna try to hack is Blue from tryhackme.com, it is a free room so anyone can spin it up and give it a go.

The first thing that we're gonna do is fire up our nmap scan to start our enumeration process.

![nmap1](https://i.imgur.com/ziEkURh.png)
&nbsp;  

Seeing the nmap scan results, there are quite a few open ports available, but we're only interested in the first 3 ports, so let's get deeper on our scan by running service enumeration and default scripts on those ports.

![nmap2](https://i.imgur.com/NlXidWJ.png)
&nbsp;  

Looking at our nmap scan result once again, we got some interesting information.

We know now that the machine is running on Windows 7 Profressional 7601 SP 1, and we also know that it has smb service (port 139 and 445),

so let's use google and try to find some exploits for that.

![google](https://i.imgur.com/mVOxxI4.png)
&nbsp;  

Quick google search reveals that this machine is likely vulnerable to MS17-010 exploit,

lets fire up metasploit to dive deeper.

![](https://i.imgur.com/k38J0OP.png)
&nbsp;  

We can run this auxiliary module to get a definitive confirmation wether the machine is vulnerable to MS17-010.

![](https://i.imgur.com/PbpiNgJ.png)
&nbsp;  

And the result shows that it is!

Lets run the real exploit and attemp to get easy win over this machine.

![](https://i.imgur.com/7C5vetR.png)
&nbsp;  

After setting up the appropriate values for the required fields, we run the exploit.

![](https://i.imgur.com/QcjrVCx.png)
&nbsp;  

And it succeeded! Easy win ladies and gentlemen.

But, we got a generic shell, it is far from ideal because it's pretty unstable and not very versatile.

We want to upgrade it to meterpreter shell.


We can use post exploitation module called "shell to meterpreter" to accomplish this.

![](https://i.imgur.com/lxtn8vr.png)
&nbsp;  

After running this exploit, we got another session popped up, and it is a meterpreter shell.

![](https://i.imgur.com/jhcV6Hg.png)
&nbsp;  

Let's interact with this new shell.

![](https://i.imgur.com/nxRhakz.png)
&nbsp;  

Everything looks great, except one problem.

The payload architecture doesn't match the OS architecture, as you can see that we have x86 meterpreter on an x64 machine.

This is not good because we wouldn't be able to run some modules and dump users credentials.

We can overcome this by migrating our shell to a different process.

Let's list all the running process on the machine by typing "ps".

![](https://i.imgur.com/MQJvilH.png)
&nbsp;  

Because we already have NT AUTHORITY\SYSTEM privilege, which is the highest privilege on windows system, we can migrate to any process as we please.

Here we're looking for an x64 process to match the OS architecture, and i'm choosing powershell.exe with the process id of 1948.

![](https://i.imgur.com/tveLFQJ.png)
&nbsp;  

And it looks like everything runs well. We have a matching OS and process architecture.

Now let's run hashdump to dump all users credentials.

![](https://i.imgur.com/NpNPtTv.png)
&nbsp;  

And then we can crack this hash to reveal Jon's password.

![](https://i.imgur.com/w6UKOAj.png)
&nbsp;  

We got the password.

Okay, now remember that we saw port 3389 was open, it is port for Remote Desktop Protocol.

Now that we have the username and password, we can just RDP in to the machine using xfreerdp.

![](https://i.imgur.com/KcmvY2y.png)
&nbsp;  

And now we have a complete control over the machine.
&nbsp;  