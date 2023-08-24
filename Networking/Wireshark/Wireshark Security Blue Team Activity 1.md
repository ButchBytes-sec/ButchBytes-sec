Overview:

This is my response to the Wireshark Challenge, one of the activities in Security Blue Team's free course. Participants were provided with a couple of .pcapng files and were presented with questions based on the content. Here are the questions for the first file:

1. Which protocol was used over port 3942?
2. What is the IP address of the host that was pinged twice?
3. How many DNS query response packets were captured?
4. What is the IP address of the host which sent the most number of bytes?
---

**1. Which protocol was used over port 3942?**

- Statistics > Endpoints > UDP tab.
- See port 3942 associated with IP 192.168.1.6.
- Right-click > Apply as Filter > Selected.
- The filter is applied in the display filter bar.
![1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/b682b957-2cf8-4198-971d-aa7956a08a5f)
![2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/3d6e2342-90db-43f6-95fe-cd61774ca46d)
- The answer is SSDP
---
**2. What is the IP address of the host that was pinged twice?**
- Type ICMP in filter.
- IP address 192.168.1.7 pinged 8.8.4.4 twice. The answer is 8.8.4.4
 ![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/875d9b2d-aa8a-4ea0-8ccf-4e6654d8b303)

---
**3. How many DNS query response packets were captured?**
- Type DNS in filter.
- Select a packet from the packet list with the "Info" column displaying "Standard query response." Next, navigate to the packet details pane and access the information regarding the "Flags" section.
- 
- The first flag says “Message is a response”; right-click this and select Apply as Filter > Selected.
- 
- At the bottom, you will see the count of packets displayed with the applied filter. The answer is:
- 
---
**4. What is the IP address of the host which sent the most number of bytes?**
- tatistics > Endpoints > IPv4 tab > Click the “Tx Bytes” column and sort to largest to smallest
- The IP address of the host which sent the most number of bytes and the answer is 115.178.9.18
- 
- 


