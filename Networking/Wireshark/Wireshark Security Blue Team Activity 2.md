This is part 2 of 2 of the Wireshark Challenge from one of the activities in Security Blue Team’s free course. Here are the questions:

1. What is the WebAdmin password?
2. What is the version number of the attacker’s FTP server?
3. Which port was used to gain access to the victim Windows host?
4. What is the name of a confidential file on the Windows host?
5. What is the name of the log file that was created at 4:51 AM on the Windows host?
---
1. What is the WebAdmin password?
- Edit > Find Packet or simply click the magnifying glass. Then, Packet Details > String > ”WebAdmin”, Find.
![8](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/96e3e0f7-6d63-4987-8247-8af2253b990d)
- Check out the HTML code that includes the text “webadmin”.
[9]
- Find the password.txt file and find the webadmin password.
- In the Find Packet options, change the first option to Packet list, and type in the string “password.txt” ,then click Find.
- An HTTP GET request is highlighted in the packet list which contains the string “password.txt”.
- Right-click the packet and Follow the HTTP stream to see the contents of the password.txt file.
[10]
- The WebAdmin password and the answer is: “sbt123”
[11]
---
2. What is the version number of the attacker’s FTP server?
- Right-click the source IP address in the GET request packet and apply it as a selected filter.
- Add and ftp to the current display filter to find all ftp traffic from the IP address in question.
- FTP info shows version and the answer is: 1.5.5.
[12]
---
3. Which port was used to gain access to the victim Windows host?
- Change the display filter so it only shows packets where 192.168.56.1 is the source IP.
- A series of ACKs indicates an established connection; The port and answer is: 8081
[13]
---
4. What is the name of a confidential file on the Windows host?
- Follow the TCP stream of TCP Retransmission packet 4258 which will drop at stream 2079.
[14]
- Use the arrows at the bottom right of the TCP stream window to move between streams.
- Find, stream 2075 which shows commands being used to list the contents of the desktop. The confidential file is listed below and the answer is: Employee_Information_CONFIDENTIAL.txt
[15]
---
5. What is the name of the log file that was created at 4:51 AM on the Windows host?
- In stream 2075 the desktop contents also show a LogFile.log file created at and the answer is: 4:51 AM.
[16]





