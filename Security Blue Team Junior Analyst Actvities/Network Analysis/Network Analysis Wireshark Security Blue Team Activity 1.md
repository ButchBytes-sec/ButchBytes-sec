## Network Analysis Wireshark Security Blue Team Activity 1/2

Overview:

_Disclaimer: The activities and information shared in relation to my pursuit of the Security Blue Team Junior Analyst Certification are intended solely for educational purposes. Any content or knowledge acquired during this certification attempt is not intended for malicious or unauthorized use. I strictly adhere to ethical standards and emphasize that this information is shared with the sole purpose of advancing my educational journey in cybersecurity._

This is my response to the Wireshark Challenge, one of the activities in Security Blue Team's free course. Participants were provided with a couple of .pcapng files and were presented with questions based on the content. Here are the questions for the part 1 of 2:

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
- IP address 192.168.1.7 pinged 8.8.4.4 twice. The answer is: 8.8.4.4
 ![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/875d9b2d-aa8a-4ea0-8ccf-4e6654d8b303)

---
**3. How many DNS query response packets were captured?**
- Type DNS in filter.
- Select a packet from the packet list with the "Info" column displaying "Standard query response." Next, navigate to the packet details pane and access the information regarding the "Flags" section.
![4](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5db28fd8-8835-4c0e-9a2a-474897235170)


- The first flag says “Message is a response”; right-click this and select Apply as Filter > Selected.
![5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/9fa0f0b4-c623-4204-9b61-a535cdc4bfc2)


- At the bottom, you will see the count of packets displayed with the applied filter. The answer is: 90
![6](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/03ff4c44-cdb4-4d5e-bdef-7fdd51793efa)


---
**4. What is the IP address of the host which sent the most number of bytes?**
- tatistics > Endpoints > IPv4 tab > Click the “Tx Bytes” column and sort to largest to smallest
- The IP address of the host which sent the most number of bytes and the answer is: 115.178.9.18
![7](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/fe80f1af-b7d8-412c-b35c-c89941be9893)
---
_Was inspired by this post: https://www.linkedin.com/pulse/btja-wireshark-challenge-pcap-1-walkthrough-octavious-williams/_




