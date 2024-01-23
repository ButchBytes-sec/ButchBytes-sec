<h2>Network Analysis Course Capstone</h2>

_Disclaimer: The activities and information shared in relation to my pursuit of the Security Blue Team Junior Analyst Certification are intended solely for educational purposes. Any content or knowledge acquired during this certification attempt is not intended for malicious or unauthorized use. I strictly adhere to ethical standards and emphasize that this information is shared with the sole purpose of advancing my educational journey in cybersecurity._

This is my attempt to answer Security Blue Team Junion Analyst course which is focused on Network Analysis. Students were given a .pcap file where we should analyse and answer the following question:<br>

1. What is the MAC address of the attacker?
2. What is the type of attack which is taking place that allows the attacker to listen in on conversations between the central server and another host?
3. What is the file which was downloaded from the central server?
4. What department does Borden Danilevich work at?
5. What is the SSH password of the Domain Administrator?

---

1. What is the MAC address of the attacker? Answer: 08:00:27:3d:27:5d
    - To find this we need to look for the suspicious IP address > Check the MAC address from the source IP address:
      ![1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a7f7703c-92cf-475e-a4d7-ebad0f092ebf)

---

2. What is the type of attack which is taking place that allows the attacker to listen in on conversations between the central server and another host? Answer: Man in the middle
    - A Man-in-the-Middle (MitM) attack is a covert cyberattack where an unauthorized party intercepts and potentially alters communication between two parties, often with the goal of eavesdropping, data manipulation, or impersonation without the knowledge of the legitimate parties involved.
  
---

3. What is the file which was downloaded from the central server? Answer: Alevis_Employee_Information_Chart.csv
    - Next, we will be following the TPC stream on the suspicious IP address, from there, we will be seeing that file that we are looking for.
      ![2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/eeb293c8-4b6f-4a22-9666-4d2748d0a3fb)
      ![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/cbb19a91-e95c-42e2-ae4b-95432953afb0)

---

4. What department does Borden Danilevich work at? Answer: Sales
    - Hit the up arrow on the “Stream” section at the bottom right to view the file and search the name “Danilevich”, you’ll see that that person is with the Sales team
      ![4](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8afa45a8-bd55-4b9a-8a9f-65f932a916d4)

---

5. What is the SSH password of the Domain Administrator? Answer: gMR<4eXf]e6W
    - Search ‘Admin’ and it will show the password, which is “gMR<4eXf]e6W”
      ![5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/6da36dd7-387f-4cb4-aff8-d5c2c30dab62)

      
