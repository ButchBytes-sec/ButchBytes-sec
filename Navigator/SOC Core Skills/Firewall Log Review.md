---
tags:
  - firewall
  - log-review
  - firewall-log-review
  - log-analysis
  - network-threat-hunting
  - lab-soc-core-skills
date: Feb 08, 2024
---

## Firewall Log Review

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

>   In this lab, we'll examine logs from a Cisco ASA firewall, known for their complexity. With Bash scripting, we can efficiently extract valuable insights. This benefits SOC analysts by enabling them to parse logs effectively, identify security threats, and improve incident response.

---

Let’s get started by opening a Terminal as Administrator

![](_attachments/Pasted%20image%2020240211025218.png)

When you get the User Account Control Prompt, select Yes.

And, open a Ubuntu command prompt:

![](_attachments/Pasted%20image%2020240211025237.png)

When we first get the logs to the right directory.

On your Linux system, please run the following command:

<pre>cd /mnt/c/IntroLabs</pre>sudo apt install r-base-core

Next, let's get your Linux system to do some math!

<pre>sudo apt-get update</pre>

<pre>sudo apt install r-base-core --fix-missing</pre>

![](_attachments/Pasted%20image%2020240211025255.png)

The password is adhd and when it finds the correct package, press Y for yes.  This is a big package. It will take a while.

Now, let's look into the logs.  We are just going to start by using less to view the logs.  No magic.  Just look at the logs.  

<pre> less ASA-syslogs.txt </pre>

![](_attachments/Pasted%20image%2020240211025320.png)

That is a nightmare....

The first thing we can see is that 24.230.56.6 is getting a lot of traffic.  This is just a local gateway and we are not interested in it.  Let's clear it out:


<pre> grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | less</pre>

![](_attachments/Pasted%20image%2020240211025343.png)

Now, let's focus on the closed connections (FIN) and pull just specific fields out of the data to clean it up.   We use cut with the -d switch to tell cut the delimiter is a space.  Then, we tell it what fields we are interested in.


<pre>grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | cut -d ' ' -f 1,3,4,5,7,8,9,10,11,12,13,14</pre>

![](_attachments/Pasted%20image%2020240211025401.png)

There are a lot of connections from 13.107.237.38.  Let's drill down and see just data from that IP address.


<pre>grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 13.107.237.38 | cut -d ' ' -f 1,3,4,5,7,8,9,10,11,12,13,14</pre>

![](_attachments/Pasted%20image%2020240211025417.png)

Wow! There are also connections from 18.160.185.174.  Here, let's also zoom in on that IP as well:


<pre>grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 18.160.185.174 | cut -d ' ' -f 1,3,4,5,7,8,9,10,11,12,13,14</pre>

![](_attachments/Pasted%20image%2020240211025433.png)

Look at the last field.  See a pattern?  Is there one?  Let's see just that field!

<pre>grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 18.160.185.174 | cut -d ' ' -f 14</pre>

![](_attachments/Pasted%20image%2020240211025450.png)


Next, let's some math at that field! 

<pre>grep 192.168.1.6 ASA-syslogs.txt | grep -v 24.230.56.6 | grep FIN | grep 18.160.185.174 | cut -d ' ' -f 8,14 | tr : ' ' | tr / ' '  | cut -d ' ' -f 4 | Rscript -e 'y <-scan("stdin", quiet=TRUE)' -e 'cat(min(y), max(y), mean(y), sd(y), var(y), sep="\n")'</pre>
