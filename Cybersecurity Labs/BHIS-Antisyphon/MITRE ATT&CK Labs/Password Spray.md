<h3>Password Spray</h3>

---

**Tools, Techniques & Technologies used:** Windows, Powershell, Powershell Script, cmd, Password Spray

---


_A Password Spraying Attack is a form of brute force attack in which an attacker repeatedly tries the same password across multiple accounts before moving to the next one. This method is effective because many users utilize easily guessable passwords like "password123."_

_Many organizations implement a security practice of locking out a user's account after a few consecutive failed login attempts, typically 3-5 tries within a short timeframe. However, due to the characteristic of password spraying attacks, malicious actors can evade detection and avoid being locked out of an account, distinguishing it from conventional brute force attacks._


---

To start, disable Defender by running the following command in an Administrator PowerShell prompt:

`Set-MpPreference -DisableRealtimeMonitoring $true`

This will deactivate Defender for this session. If you encounter red error messages, it indicates that Defender is already inactive. Now, proceed by opening a Terminal as an Administrator.

![1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5977653d-b126-45b2-81d0-b9722b6e5d87)
![1 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5d9970f7-efc9-4054-8fde-26b837a29e4d)

Now, let's open a command Prompt:

![2](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/f384aac9-3813-4403-8be7-05ad088368b9)

C:\Windows\system32> `cd \tools`

Run a simple .bat file that creates 200 users

![2 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/4500552d-20ea-4d3b-a185-6beeca1aefd8)

C:\Tools> `200-user-gen.bat`

It should look like this:

![3](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/f2ab7411-4b0d-48cf-b165-089a76094b1f)

Now, we will need to start PowerShell to run LocalPasswordSpray (Powershell script written by Beau Bullock

C:\Tools> `powershell`

PS C:\Tools> `Set-ExecutionPolicy Unrestricted`

PS C:\Tools> `Import-Module .\LocalPasswordSpray.ps1`

![3 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/de7bd801-598f-4880-a945-89f44c908141)

It should look like this:

![4](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/f3d5b934-cea0-4a3a-bbe0-b3abce8de98e)

Now, let’s try some password spraying against the local system!

PS C:\Tools> `Invoke-LocalPasswordSpray -Password Winter2020`

It should look like this:

![5](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/98e329bc-d5a5-4c5f-9cd9-edb438492fc1)
![5 1](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/6cae4137-87e7-45d0-a5f6-f47e1e1ef7ac)

Now we need to clean up and make sure the system is ready for the rest of the labs:

PS C:\Tools> `exit`

C:\Tools> `user-remove.bat`

![6](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/151e74c9-fadf-470a-a2c0-0e4a2ff55531)














