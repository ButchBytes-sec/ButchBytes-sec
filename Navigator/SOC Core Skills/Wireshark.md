---
tags:
  - lab-soc-core-skills
  - wireshark
  - packet-sniffer
  - tool
  - networking
  - core-networking-skills
date: Feb 06, 2024
---

## Wireshark

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we'll cover some basic Wireshark skills essential for every SOC and security analyst. Now that we've had some experience with tcpdump, let's explore Wireshark. Firstly, it's important to clarify that Wireshark isn't necessarily superior to tcpdump; each has its own strengths and weaknesses. Tcpdump is quick, lightweight, and scriptable, but lacks visualizations. Conversely, Wireshark provides helpful visualizations, which can be crucial when dealing with large datasets, although it may freeze with very large files. Sometimes, we extract data with tcpdump and analyze it in Wireshark. Ultimately, mastering both tools is essential.

---

First, select the Windows icon and type in wireshark:
Now select the Wireshark icon:
Now, select File > Open
Then, select magnitude_1hr in the Open Capture File box.  You may need to scroll down, it is at the bottom:

![](_attachments/Pasted%20image%2020240210183445.png)

When Wireshark opens, you will see packets represented in three different windows:

![](_attachments/Pasted%20image%2020240210183551.png)

The top window shows each individual packet in order:

![](_attachments/Pasted%20image%2020240210183630.png)

The second window shows a "decode" of any packet that is selected:

![](_attachments/Pasted%20image%2020240210183701.png)

Any of the lines with a > can be expanded:

![](_attachments/Pasted%20image%2020240210183728.png)

This means you do not have to memorize every possible packet and protocol value in hex...  Unless that is your thing.  If it is....  You must be Judy Novak, Mike Poor or Jonathan Ham. 

The last window is the hex for the packet:

![](_attachments/Pasted%20image%2020240210183756.png)

Click some hex in the bottom window:

![](_attachments/Pasted%20image%2020240210183823.png)

Notice that when you select any of the hex in the bottom window it opens and highlights the corresponding data in the second window.

This is very, very cool.  This means Wireshark can decode the hex on the fly and automatically highlight the relevant data instantly.

Ok, now, lets play with some statistics.

Please select Statistics > HTTP > Requests:

![](_attachments/Pasted%20image%2020240210183852.png)

This will show us the various HTTP requests for the capture:

![](_attachments/Pasted%20image%2020240210183919.png)


Now, let's look at Statistics > Conversations:

![](_attachments/Pasted%20image%2020240210183943.png)

This will give us a breakdown of who was talking to whom:

![](_attachments/Pasted%20image%2020240210184015.png)

Please select IPv4:
Then click on the top of the packets column twice:

![](_attachments/Pasted%20image%2020240210184048.png)

This gives us a breakdown of who was chatting with what system the most.  Click it again and it will sort the opposite direction and show you the least:

![](_attachments/Pasted%20image%2020240210184112.png)

Really want to know what those systems were saying to each other?  Right click on a conversation and select Apply as Filter > Selected > A<->B

![](_attachments/Pasted%20image%2020240210184133.png)

You should see the main Wireshark screen change

Then, close the Conversations window:

Notice the following in the filter bar

`ip.addr==68.183.138.51 && ip.addr==192.168.99.52`

![](_attachments/Pasted%20image%2020240210184159.png)

This is saying:

"IP address equals 68.183.138.51 And IP address equals 192.168.99.52"

If a packet meets both of those critiera it is displayed:

![](_attachments/Pasted%20image%2020240210184220.png)

Now, right-click on any of the packets and select Follow > TCP Stream:

![](_attachments/Pasted%20image%2020240210184242.png)

This is showing the request (in red) and the response (in blue) between our two systems:

![](_attachments/Pasted%20image%2020240210184312.png)

Anything look strange there?  If you look closely, there is a lot of encoded PowerShell.

Now, let's play with some basic filters in the filter bar.  We have already seen how Wireshark can filter on IP addresses.  But we can also filter on protocols.

To start, just type l.

Notice how Wireshark tries to help you with possible completion options as you type.

Now finish typing llmnr.

Then hit enter.

![](_attachments/Pasted%20image%2020240210184346.png)

Notice that when you type llmnr and hit enter, Wireshark shows you all packets that are that protocol

Now try ipv6 and hit enter:

![](_attachments/Pasted%20image%2020240210184419.png)

This allows you to very quickly drill in on any specific protocols you are reviewing in a packet capture.

Remember the PowerShell from tcpdump?  It had the string New-Object? Well, we can search though all the http traffic looking for that specific string:

Put the following into the filter bar:

http contains "New-Object"

![](_attachments/Pasted%20image%2020240210184439.png)

With Wireshark, we can search through all our packets looking for specific strings and data.
