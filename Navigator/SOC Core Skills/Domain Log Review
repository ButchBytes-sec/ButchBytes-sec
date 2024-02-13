---
tags:
  - domain-log-review
  - active-directory-analysis
  - lab-soc-core-skills
date: Feb 09, 2024
---

## Domain Log Review

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we'll examine logs generated during a domain password spray attack. We'll begin by utilizing DeepBlueCLI, then transition to directly inspecting the event logs themselves. This process mirrors active directory analysis and provides valuable insights into security threats.

---

First, we will need to extract the event logs for a domain attack.  To do this, simply navigate to 
the C:\IntroLabs directory:

![](_attachments/Pasted%20image%2020240211031345.png)

Right Click on The EntLogs directory and select 7-Zip > Extract Files
Then put `C:\tools\DeepBlueCLI-master` in the Extract To: field

It should look like this:

![](_attachments/Pasted%20image%2020240211031401.png)

Now, click OK

Now, we are going to use DeepBlueCLI to see if there are any odd logon patterns in the domain logs.

Let's start by opening a Terminal as Administrator:

Then, navigate to the \tools\DeepBlueCLI-master directory

C:\> `cd C:\tools\DeepBlueCLI-master\`

![](_attachments/Pasted%20image%2020240211031423.png)

Now, let's start looking at the DC2 Password spray file:

PS C:\> `.\DeepBlue.ps1 .\EntLogs\DC2-secLogs-3-26-DomainPasswordSpray.evtx`

When the warning pops up, press R.  This will start the script by running it:

![](_attachments/Pasted%20image%2020240211031439.png)

When this runs, there is an alert that catches our attention right away:

![](_attachments/Pasted%20image%2020240211031455.png)

We have 240 logon failures.  That...  is a lot for this small org.

Lets dig into the actual logs and see if we can see a pattern.

To do this, open File Explorer and navigate to the C:\tools\DeepBlueCLI-master\EntLogs directory:

![](_attachments/Pasted%20image%2020240211031513.png)

Once in this directory, double click on DC2-secLogs-3-26-DomainPasswordSpray.evtx:

![](_attachments/Pasted%20image%2020240211031531.png)

This will open Windows Event Viewer.  Note, it will open in Sysmon Operational.  This is not what we want.  Please scroll down to the DC2-secLogs-3-26-DomainPasswordSpray.evtx file under Saved Logs:

![](_attachments/Pasted%20image%2020240211031557.png)

Then click it.  

It will open the DC logs with the attack.

Now, please click on the header column called Event ID.  This will sort the logs by ID number we are doing this because we want to quickly get to the event IDs of 4776:

Now, scroll all the way to the bottom:

![](_attachments/Pasted%20image%2020240211031620.png)

Specifically, we are looking for Event ID 4776.  This is the Credential Validation Event log.

Select one, then press the up arrow key a bunch of times.  Watch the Logon Account Name in the General tab:

![](_attachments/Pasted%20image%2020240211031707.png)

Notice the large number of login attempts from a single system:

![](_attachments/Pasted%20image%2020240211031826.png)

![](_attachments/Pasted%20image%2020240211031843.png)

![](_attachments/Pasted%20image%2020240211031914.png)

Also, notice at the bottom of the General tab, these are predominantly Audit Failures:

![](_attachments/Pasted%20image%2020240211031931.png)

We now know that the workstation WINLABV2WKSRL-9 was attempting to authenticate to a large number of Logon Accounts in a very short period of time.
