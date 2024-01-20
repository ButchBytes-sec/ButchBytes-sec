<h2>OSINT Course Capstone</h2>

_Disclaimer: The activities and information shared in relation to my pursuit of the Security Blue Team Junior Analyst Certification are intended solely for educational purposes. Any content or knowledge acquired during this certification attempt is not intended for malicious or unauthorized use. I strictly adhere to ethical standards and emphasize that this information is shared with the sole purpose of advancing my educational journey in cybersecurity._

---

This is my attempt on the Security Blue Team Junior Activity OSINT Course Capstone.

Here is the scenario:

*You work for a law enforcement organization, and you have been assigned to track a person-of-interest, that is believed to be associated with a hacking group that recently compromised a Managed Service Provider (MSP) and are trying to sell the stolen credentials on both the clear net and dark web. Another team is focusing on the dark web lead, so you have been tasked with using OSINT sources to build up a profile on the individual and attempt to locate any evidence that links them to the MSP breach and sale of account details. You have been provided with some information to start your investigation. You should use any of the sources or tools taught in this course, that you deem to be applicable and appropriate. We know that the email address used to register the Twitter account is fake, so do not include this in your report.*<br>

*This is a fictional scenario. Any affiliation between the scenario individual and any real person is strictly coincidental.*<br>

*Your manager has provided you with the following starting information:*<br>

Twitter handle used by actor: **@sp1ritfyre**<br>

![1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/e6bfc837-7021-48bd-be56-3afe22fd0c43)

---

Clue: Twitter Handle: @sp1ritfyre<br>

- This is the hacker’s Twitter profile:

![2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ab38a0af-58ac-41f4-bc8a-f24991ff3d83)

- Searched the hacker’s username, Sp1ritfyre, in a search engine and as a results, it provided a link to the hacker’s blog profile.

![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/41b0cd63-fdc7-4c95-89ee-c7e7174602be)

- This page provided 3 clues, random numbers & letters, email link to the hacker’s email address and blog post.

![4](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/b8d64e7b-2e90-4513-83ae-942a29946898)
![13](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c6f538e2-1da4-405f-a4c5-0f5eaee03d86)


- Now, we’ll try to convert the random numbers and letters from hex to clear text and this is what we got:

![5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/325237e5-294d-4ccc-b4e2-39867f132088)

- We got the hacker’s blog website, which is, https://sammiewoodsec.blogspot.com/. Now, we’ll view the about me page:

![6](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/cf107be1-2db7-4233-b130-7b0b60347ddb)
![7](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5e4d06a9-c589-41c8-a654-6ccd41181149)

- Here are the other information we got from her blog.

![8](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c11d3a8a-de3c-41a9-a621-7c3605364850)
![9](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/caa77746-57ff-4393-b2ab-09b0bd8dd0e6)

- Now, we’ll go back to the hacker’s Twitter account to check the last clue, the website link: cmVkaHVudC5uZXQK.xyz. If we try to click the link as is, it will point us to an invalid website. We’ll convert this from base64 to text:

![10](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/9cdb1bf2-cffb-47de-b860-60a56343beea)

- This is what we got: redhunt.net. We now got all we need to answer the questions.

![11](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/2cfd56e4-508d-447b-a2ac-5d6b235f4377)
![12](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/f018bacb-0a1e-4018-a1da-af4f06ccad21)


---

<h3>Answers:</h3>

1. First Name:  Sam
2. Last Name:  Woods
3. Age: 23
4. Country: United Kingdom
5. Interests (5 minimum): Security, Programming, Technology, Gaming, Photography, Camping, Malware Analysis
6. Hacker’s employer (company name): PhilmanSecurityInc
7. Hacker's position within company: Junior Pen Tester
8. Self-Owned Website (Hacker owns the domain): https://redhunt.net/
9. Other Websites (Person does not own the domain, such as blogs): 
    1. https://sp1ritfyrehackerstories.blogspot.com
    2. https://sammiewoodsec.blogspot.com
10. Email address of the Hacker: d1ved33p@gmail.com








