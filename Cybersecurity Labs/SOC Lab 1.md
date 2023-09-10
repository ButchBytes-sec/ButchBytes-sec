<h2>SOC Analyst Lab 1: Build a SOC Analyst home lab</h2>

_This lab was inspired by [Eric Capuano](https://www.sans.org/profiles/eric-capuano/), an instructor at the SANS Institute. As someone striving to establish a presence in the information security field, I regularly encounter a common question: 'What steps can I take to improve my chances of securing an entry-level SOC analyst position?' This is a question I have consistently asked myself. To address this and translate theoretical learning into practical experience, I delved into the exciting realm of Virtual Machines. This is just the beginning._

---

<h3>Table of Contents:</h3>

1. [Initial Resources to Download](#initial-resources-to-download)
2. [Setup Ubuntu Server VM](#setup-ubuntu-server-vm)
3. [Setup Windows VM](#setup-windows-vm)
4. [Install LimaCharlie EDR on Windows](#install-limacharlie-edr-on-windows)
5. [Setup Attack System](#setup-attack-system)

---
<h3>Initial Resources to Download</h3>

1. Download and install a free trial of VMware Workstation.
![001](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ede505b1-37f5-482e-8f47-863ee5c2c0fa)
2. Download and deploy a free Windows VM directly from Microsoft.
![002](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a0c91722-7064-4663-9fe8-b2ccd57ff33d)


<h3>Setup Ubuntu Server VM</h3>

1. Download and install [Ubuntu Server 22.04.1](https://releases.ubuntu.com/22.04.1/ubuntu-22.04.1-live-server-amd64.iso) into a new VM with the following specs:
    1. 2 CPU cores
    2. 2-4GB RAM
![003](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/18c198de-fd16-4273-8db6-7ce9dff6afe9)
2. During OS install, leave defaults unless otherwise specified > “Continue without updating” > When you get to “Network connections” section, we need to take a few steps to set a static IP address for this VM so that it doesn’t change throughout the lab or beyond it.
![004](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a4d347f1-9507-4956-ae86-5d78d0c51c13)
   1. Find the gateway IP of our VMware Workstation NAT network. “Edit” > “Virtual Network Editor” > “Type: NAT” > “NAT Settings…”
   2. Copy the “Subnet IP” & “Gateway IP” then close the NAT Settings
![005](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/782c2470-b566-4990-a878-8bcbdaf09dc7)
   3. Back in the Ubuntu installer, let’s change the interface from DHCPv4 to Manual.
![006](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a96de051-513c-44dc-ab49-eb59a0bea40d)
![007](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/93f5c9a0-c972-4d73-8699-f57840cd9ecd)
![008](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/2124ed46-6c6f-4705-bb8e-ac104c71f066)
![009](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/bc925373-3959-4674-86ce-e88cd34bda3b)
3. **NOTE**: Write down the Linux VM’s IP address because you will need it multiple times throughout this guide.
4. Set-up username and password. I will use these in this lab:
    1. Your name: user
    2. Your server’s name: attack
    3. Username: user
    4. Password: password
![010](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/0e19fddd-659a-46b2-abe4-80ebe13780b5)
5. Install OpenSSH server? click “yes” > continue installing OS until “Install complete!” > Hit Enter on [ Reboot Now ] > Login with username and password then > ping -c 2 google.com
![011](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/9b80129d-1da5-4bf7-84e4-7644362a026d)
![012](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/b08190cd-4c16-4b33-8d8a-4ed8a9a491a9)
![013](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/8655b6dc-8d8f-496b-a241-bf508aea3354)

<h3>Setup Windows VM</h3>

Initiate Windows VM<br>
1. Double-click the `WinDev####Eval.ovf` file to import the VM into VMware
2. Disable Virtualize Intel VT-x/EPT or AMD-V/RVI
![014](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/94b0dee9-e5ec-4e58-995f-9e5bcf665ed4)
3. Go ahead and “power on” your Windows VM for the first time.
    1. It will automatically log you in as “user”.
    2. Wait for the desktop to appear.

Disable Defender on Wwndows VM<br>
1. Disable Tamper Protection: “Start” > “Settings” > “Privacy & security” > “Windows Security” > “Virus & threat protection” > “Virus & threat protection settings” , click “Manage settings” > Toggle OFF the “Tamper Protection” switch, When prompted, click “Yes”
![015](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/3a3bd142-67c8-46bf-a3c1-74568e7ea0d9)
2. Permanently Disable Defender via Group Policy Editor: “Start”> Type “cmd” in the search bar > Run as administrator > Type and run “gpedit.msc” > Double-click “Turn off Microsoft Defender Antivirus”, select “Enabled” > Apply > OK
3. Permanently Disable Defender via Registry: run this in cmd - `REG ADD "hklm\software\policies\microsoft\windows defender" /v DisableAntiSpyware /t REG_DWORD /d 1 /f`
4. Prepare to boot into Safe Mode to disable all Defender services: “Start” > run “msconfig” in search > “Boot” > “Boot Options” > Check the box for “Safe boot” and “Minimal” > Click “Apply” and “OK” > System will restart into Safe Mode.
5. In Safe Mode, we’ll disable some services via the Registry: “Start” > run “regedit” in search bar > For each of the following registry locations, browse the key and find the “Start” value, and change it to 4. Once done, leave safe mode, same as step 4.
![016](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/1cdb5d6f-d926-461d-94c2-1049620c961b)
    1. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Sense`
    2. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdBoot`
    3. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinDefend`
    4. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisDrv`
    5. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisSvc`
    6. `Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdFilter`
       
Prevent the VM from going into standby<br>

    1. powercfg /change standby-timeout-ac 0
    2. powercfg /change standby-timeout-dc 0
    3. powercfg /change monitor-timeout-ac 0
    4. powercfg /change monitor-timeout-dc 0
    5. powercfg /change hibernate-timeout-ac 0
    6. powercfg /change hibernate-timeout-dc 0
<br>
Install Sysmon in Windows VM (Optional)<br><br>
While not a direct requirement for this guide, Sysmon can be an invaluable analyst tool for obtaining highly detailed telemetry from your Windows endpoint, providing insights into a wide range of intriguing activities. I strongly encourage its adoption, if only for the sake of becoming familiar with its capabilities.<br>
        1. Administrative PowerShell > type: “`Invoke-WebRequest -Uri https://download.sysinternals.com/files/Sysmon.zip -OutFile C:\Windows\Temp\Sysmon.zip`" > unzip: “`Expand-Archive -LiteralPath C:\Windows\Temp\Sysmon.zip -DestinationPath C:\Windows\Temp\Sysmon`"
        2. Download [SwiftOnSecurity](https://infosec.exchange/@SwiftOnSecurity)’s Sysmon config: “`Invoke-WebRequest -Uri https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml -OutFile C:\Windows\Temp\Sysmon\sysmonconfig.xml`"
        3. Install Sysmon with Swift’s config: "`C:\Windows\Temp\Sysmon\Sysmon64.exe -accepteula -i C:\Windows\Temp\Sysmon\sysmonconfig.xml`"
        4. Validate Sysmon64 service is installed and running: “`Get-Service sysmon64`"
        5. Check for the presence of Sysmon Event Logs: “`Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10`"

![017](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/2ce65e8c-b293-4c22-96bd-fbb07c9fecda)
![018](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5d271d32-6f25-4ec3-9bd9-41aa2f404f81)

<h3>Install LimaCharlie EDR on Windows</h3>

![019](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/fb6c8df3-f315-40ff-a80b-452325077b73)
![020](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c20081ed-c45e-4ca4-a837-673efb0211e9)
![021](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/1e884efd-1a68-4431-87bd-da3d0263d0aa)
![022](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/557db80c-ccc6-424f-8b42-c540260c0952)
![023](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/79b1a721-3b18-4120-b861-18f3fae80f28)
![024](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/7fab9652-91a3-4e38-a95a-f0dd97bb89eb)

<h3>Setup Attack System</h3>
