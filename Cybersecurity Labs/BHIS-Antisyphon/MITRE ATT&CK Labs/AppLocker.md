<h3>AppLocker</h3>

_NOTE: All the procedures in this lab were created and learned from: [Getting Started in Security with BHIS and MITRE ATT&CK](https://www.antisyphontraining.com/event/getting-started-in-security-with-bhis-and-mitre-attck/2023-09-18/) taught by [John Strand](https://www.sans.org/profiles/john-strand/)_

[Windows AppLocker](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/applocker/what-is-applocker) is a security feature in Windows that lets administrators control which applications and scripts can run on a computer or network by creating policies based on file attributes. It enhances security by allowing only authorized and trusted software to execute, reducing the risk of malware and unauthorized applications.<br>

---

Let's examine the scenario when AppLocker isn't active. Our objective is to establish a basic backdoor that connects to the Ubuntu system. It's important to note that our goal is not to demonstrate evasion of EDR or Endpoint products but rather to create a straightforward backdoor connection.

First, let's disable Defender for this session. Execute the following command from an Administrator PowerShell prompt:

```Set-MpPreference -DisableRealtimeMonitoring $true```









