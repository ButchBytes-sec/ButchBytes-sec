---
tags:
  - memory-forensics
  - volatility
  - lab-soc-core-skills
date: Feb 08, 2024
---

## Memory Analysis

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, memory forensics, demonstrated through tools like Volatility, is crucial for SOC analysts to dissect compromised systems through memory dumps. Despite limitations, it uncovers vital insights such as network connections and process data linked to malware, aiding swift incident response and strengthening defenses against cyber threats by enhancing analysts' understanding of attacker tactics.

---

To get started, we first need to extract the memory dump using 7zip.

To do this, first open file explorer and navigate to the memory dump and extract it.

First, click on the file explorer icon:

![](_attachments/Pasted%20image%2020240211025704.png)

Next, select the tools folder:

![](_attachments/Pasted%20image%2020240211025718.png)

Now, select the volatility_2.6_win64_standalone directory:

![](_attachments/Pasted%20image%2020240211025734.png)

Next, right click on the memdump file and then select 7-Zip and then Extract Here.  Please note that the file name does not end with 7z on the left display column.  However, on the far right column you can see it is a 7Z file.:

![](_attachments/Pasted%20image%2020240211025837.png)

Now we have it extracted!  Let's open a command prompt and look at it with Volatility!

Please note this memory dump was created from VMWare snapshot feature. There are multiple tools like winpmem and FTK Imager that can also create memory dumps.

To start, we will be working with the Ubuntu-18.04 Prompt in Windows Terminal.   This is on your desktop and can be opened by right-clicking it and selecting Run as administrator then selecting Ubuntu-18.04 from the down carrot menu:

![](_attachments/Pasted%20image%2020240211025858.png)

Now we need to extract Volatility:

<pre>
cd /mnt/c/IntroLabs/
tar xvfz ./volatility3-1.0.0.tar.gz
cd volatility3-1.0.0/
</pre>

![](_attachments/Pasted%20image%2020240211025919.png)

![](_attachments/Pasted%20image%2020240211025934.png)

Let's start by looking at the network connections:

<pre>
python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.netscan
</pre>

![](_attachments/Pasted%20image%2020240211025950.png)

![](_attachments/Pasted%20image%2020240211030008.png)


The above screenshot is... Concerning. We would want to look further into this because it is a SMB (port 445) connection to another computer. We know it is compromised (because it is a lab) but any time a "suspect" computer has another open connection to an internal system is, without question, a cause for concern.

Now, let's look at the processes on this system:

<pre>
python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.pslist
</pre>

![](_attachments/Pasted%20image%2020240211030028.png)

The cmd.exe should catch your attention. Generally, users and day to day usage of a system does not spawn a cmd.exe session. We may see it briefly as part of some sysadmin scripts. However, it is not seen all that often in normal day-to-day user interactions.

Let's look at pstree to see a bit more detail on what spawned what.

<pre>
python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.pstree
</pre>

![](_attachments/Pasted%20image%2020240211030047.png)

Here you can see that we traced back the parent process for one of the cmd.exe files back to TrustMe.exe. When hunting down these processes it helps to track the parent processes. It can help create a sort of timeline for the actions on the system.

In the above example we can also see that the parent process for TrustMe was Explorer.exe. This means it was invoked by the user on the system, as Explorer.exe is the GUI process for Windows 10.

Let's now dive into the TrustMe.exe process a bit further with dlllist:

<pre>
python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem dlllist --pid 5452
</pre>

![](_attachments/Pasted%20image%2020240211030111.png)

Here you can see the dll's associated with the TrustMe process.

We can also see the command line invocation of this process. This is great as it tells us any flags used to start the process and it can tell us where on the system it was executed from.

Finally, let’s look at the easy button with malfind.  This module will look at the processes for any suspicious activities. 

<pre>
python3 vol.py -f /mnt/c/tools/volatility_2.6_win64_standalone/memdump.vmem windows.malfind.Malfind
</pre>

![](_attachments/Pasted%20image%2020240211030131.png)
