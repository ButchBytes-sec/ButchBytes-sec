---
tags:
  - rita
  - ac-hunter
  - network-threat-hunting
  - lab-soc-core-skills
date: Feb 08, 2024
---

## R.I.T.A.

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we'll focus on detecting command and control traffic on a network. We'll utilize Real Intelligence Threat Analytics ([RITA](https://www.activecountermeasures.com/free-tools/rita/)), an open-source framework designed for detecting command and control communication through network traffic analysis. RITA ingests [Zeek](https://zeek.org/) logs or PCAPs converted to Zeek logs for analysis. Additionally, we'll use [AC-Hunter™](https://www.activecountermeasures.com/ac-hunter/), a software solution dedicated to continuous threat hunting within your network, specifically identifying compromised systems communicating with their command and control servers.

---

To start we first need to open Windows File Explorer and navigate to the tools directory.

First, open File Explorer:
Then, select the tools directory:

![](_attachments/Pasted%20image%2020240211030258.png)

Then, select rita-html-report:

![](_attachments/Pasted%20image%2020240211030518.png)

Then, select index.html:

![](_attachments/Pasted%20image%2020240211030539.png)



Let’s select VSAGENT-2017-3-15.

Review Options

The tabs across the top allow you to review the output for all the different analysis modules of RITA.
For VSAgent we will be focusing on Beacons, Blacklisted and User Agents.

Please select Beacons now.

![](_attachments/Pasted%20image%2020240211030606.png)

Beacons

Some backdoors have a very strong “heartbeat”. This is where a backdoor will constantly reconnect to get commands from an attacker at a specific interval. The interval consistency of the “heartbeat” is the TS score where a value of 1 is perfect. The top value in this set is the VSAgent communication. We will talk about the other connections in a few moments.

We also have the number of connections. While some beacons have a “strong” heartbeat, they are very short in nature. Our VSAgent connection had a very large number of connections which had very strong intervals, while some of the others (e.g. the 64.4.54.253 addresses) had a strong heartbeat, but not as many connections. We will also talk about TS Duration. This is detecting how consistent each connection duration is. For example, if every connection is 2 seconds and there are 8000+ it would have a very strong TS Duration score.

The other fields are statistical analysis fields showing things like mode range and skew.
DNSCat2

Now, select RITA and then select DNSCat-2017-03-21. We are going to review a backdoor which does not quite fit the same mold as VSAgent.

![](_attachments/Pasted%20image%2020240211030705.png)

![](_attachments/Pasted%20image%2020240211030725.png)

This does not beacon back to a specific IP address, but rather it beacons through a DNS server. It is very crafty and will highlight how we can review the RAR compressed Bro logs used to generate the RITA data.

For this one, we are going to jump right to the DNS tab. It gives us the clearest look at this backdoor.

![](_attachments/Pasted%20image%2020240211030745.png)

![](_attachments/Pasted%20image%2020240211030800.png)


A couple of things should jump out at an investigator straight away. First, there were over 40K requests for cat.nanobotninjas.com. This is an absurd number for a specific domain. Sure, there are lots of requests for com and org and net and uk, but that is to be expect

Now, let's play with AC Hunter!

Please go to https://training.aihhosted.com/

The creds are:

ID= training@blackhillsinfosec.com
PW = gotbeacons?


When logged in, please select the house in the lower left corner and then the gear in the upper right.

![](_attachments/Pasted%20image%2020240211030833.png)

This will open the dataset selection screen

![](_attachments/Pasted%20image%2020240211030850.png)

Please select vsagent then Confirm.

This will open the overall scoring screen.  This is the screen that allows you to see the systems that have the top scores across all areas from beacons to cyber deception.

Please select 10.55.100.111, then click on Beason Score on the right.

This will open the beacon score for this system.

![](_attachments/Pasted%20image%2020240211030906.png)

Notice the histogram on the bottom and the scoring criteria in the middle. 

Notice how on the bottom you can see multiple aspects of this systems connections.  For example, you can see if there are any connections that had a threat intel hit, or if there are any connections that have beacons to a fully qualified domain.

Now, using AC Hunter, answer the following questions:

In the winlab-agent dataset, what is the connection interval for 10.10.98.30?

![](_attachments/Pasted%20image%2020240211030925.png)

In the gcat dataset, what is the historic fqdn for the beacon on 10.55.100.111?

![](_attachments/Pasted%20image%2020240211030943.png)

For the dnscat2-ja3-strobe-agent dataset, what domain has the highest lookup count?

![](_attachments/Pasted%20image%2020240211031008.png)
