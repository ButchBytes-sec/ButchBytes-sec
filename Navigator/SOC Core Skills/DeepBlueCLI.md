---
tags:
  - deepbluecli
  - ueba
  - lab-soc-core-skills
date: Feb 07, 2024
---

## DeepBlueCLI

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> [DeepBlueCLI](https://github.com/sans-blue-team/DeepBlueCLI), a free tool developed by [Eric Conrad](https://www.sans.org/profiles/eric-conrad/), is indispensable for SOC analysts due to its impressive detection capabilities and effective demonstration of UEBA-style techniques. This open-source tool enables analysts to identify potential threats, understand user behaviors, and enhance their incident response strategies. With strong community support and hands-on experience opportunities, DeepBlueCLI serves as a valuable asset for SOC teams aiming to bolster their cybersecurity defenses and mitigate risks effectively.

---

Let's get started by opening a Terminal as Administrator

![](_attachments/Pasted%20image%2020240211024636.png)

Now, let's open a command Prompt:

![](_attachments/Pasted%20image%2020240211024652.png)

Next, we need to navigate to the tools directory on your VM:

C:\tools>`cd \tools\DeepBlueCLI-master`
C:\tools\DeepBlueCLI-master>`powershell`

PS C:\tools\DeepBlueCLI-master> `Set-ExecutionPolicy unrestricted`

It should look like this:

![](_attachments/Pasted%20image%2020240211024710.png)

It is very common for attackers to add additional users on to a system they have compromised.  This gives them a level of persistence that they otherwise would not gain with malware.  Why?  There are lots and lots of tools to detect malware.  By creating an extra user account it allows them to blend in.  

Now, let’s run a check in the .evtx files for adding a new user:

PS C:\tools\DeepBlueCLI-master>`.\DeepBlue.ps1 .\evtx\new-user-security.evtx`

![](_attachments/Pasted%20image%2020240211024726.png)

Choose R

![](_attachments/Pasted%20image%2020240211024743.png)

Another attack that very few SIEMs detect is password spraying.  This is where an attacker takes a user list from a domain, and sprays it with the same password, think Summer2020.  This is effective because it keeps the lockout threshold below the lockout policy and many times flies under the radar simply because accounts are not getting locked out. 

But, this is the exact behavior that UEBA should be able to detect.

Now, let's look at an event log with a password spray attack.  This is very much part of what a full UEBA solution does:

PS C:\tools\DeepBlueCLI-master>`.\DeepBlue.ps1 .\evtx\smb-password-guessing-security.evtx`

![](_attachments/Pasted%20image%2020240211024801.png)

Same thing with detecting a password spraying attack:

PS C:\tools\DeepBlueCLI-master>`.\DeepBlue.ps1 .\evtx\password-spray.evtx`

![](_attachments/Pasted%20image%2020240211024818.png)

Finally, for fun, let’s look at how DeepBlueCLI detects various encoding tactics that attackers use to obfuscate their attacks.  It is very common for attackers to use a number of encoding techniques to bypass signature detection.  However, it is not something that normally happens with standard scripts.

PS C:\tools\DeepBlueCLI-master>`.\DeepBlue.ps1 .\evtx\Powershell-Invoke-Obfuscation-encoding-menu.evtx`

![](_attachments/Pasted%20image%2020240211024836.png)
