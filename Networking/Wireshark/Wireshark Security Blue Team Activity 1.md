Overview:

This is my attempt to answer the Wireshark Challenge from one of the activities in Security Blue Team’s free course. Challengers were given a couple of .pcapng files and was asked some questions out of it. These were the questions in the first one:

**1. Which protocol was used over port 3942?**

**2. What is the IP address of the host that was pinged twice?**

**3. How many DNS query response packets were captured?**

**4. What is the IP address of the host which sent the most number of bytes?**
<br>

**1. Which protocol was used over port 3942?**

- Statistics > Endpoints > UDP tab.
- See port 3942 associated with IP 192.168.1.6.
- Right-click > Apply as Filter > Selected.
- The filter is applied in the display filter bar.


- The answer is SSDP


