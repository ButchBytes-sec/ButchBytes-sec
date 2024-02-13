---
tags: elastic-cloud siem sysmon elk elasticsearch kibana
date: Feb 10, 2024
---

## ELK

### Elk in Cloud

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, the ELK stack, comprised of Elasticsearch, Logstash, and Kibana, brings together three powerful tools for handling large amounts of data. By setting up SIEM rules, it helps us defenders get alerts when there are attacks on our organization. ELK makes it easier for us to find and respond to threats quickly.

---

**1. Set up an account.**

[Start your free Elastic Cloud Trial](https://cloud.elastic.co/registration?settings=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsZW5ndGgiOjE1MCwic2l6ZSI6NDA5NiwiZGVmYXVsdF9zaXplIjoxMDI0fQ.dS6xqdrcNBVkANlcS19AnsZmHVSqoPROLHprdeN-Qbc&source=education "https://cloud.elastic.co/registration?settings=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJsZW5ndGgiOjE1MCwic2l6ZSI6NDA5NiwiZGVmYXVsdF9zaXplIjoxMDI0fQ.dS6xqdrcNBVkANlcS19AnsZmHVSqoPROLHprdeN-Qbc&source=education")

1. Start a trial by signing up.
2. Watch your email for a confirmation. The email will look something similar to this.
3. Click "Verify and Accept."  You should be redirected to the cloud login page.  If you're not redirected, you can find it here.

---

After logging in, the page will look like this.

![](_attachments/Pasted%20image%2020240213175525.png)

Fill out the proper filed with the correct information pictured below and select the check boxes with red dots.

Once those fields are filled out click "Next"

---

**2. Start an ELK instance.**

Upon clicking next you will see the following page. For my instance I will be calling it "security-development. Make sure to enter the name of your deployment and click "Create Deployment".

![](_attachments/Pasted%20image%2020240213175550.png)

Next we will see this page.

Elastic will present the credentials for this ELK stack. There is the option to download a CSV of the credentials. However you decide to hold onto these credentials, don't lose them.

![](_attachments/Pasted%20image%2020240213175608.png)

Then we will need to wait for the continue button to turn blue, once that's done click continue

![](_attachments/Pasted%20image%2020240213175700.png)

Then at the top of the page we want to click search and type "kibana" and hit enter.

![](_attachments/Pasted%20image%2020240213181120.png)

Once the next page load we want to add kibana. Select "Add Kibana"

![](_attachments/Pasted%20image%2020240213181143.png)

We will next be prompted to "Install Elastic Agent" This is what we are going to put on our machine that monitors what's happening. Click "Install Elastic Agent"

![](_attachments/Pasted%20image%2020240213181244.png)

The next page we meet will have a wall of text. Select windows.

We will need to click the "Copy to Clipboard".

![](_attachments/Pasted%20image%2020240213181312.png)

Hold onto this command.  It is recommended to paste this command into some file where you won't lose it. In this example, I saved it to a file I called "agent.txt."  We will use this command later.

![](_attachments/Pasted%20image%2020240213181516.png)

The ELK stack is now configured and we have our connection information saved.
Part two will cover how to install and configure an Elastic Agent.

### Elastic Agent

In part one, we started an ELK instance in the Elastic Cloud.

The Elastic Agent software enables users to easily send logs to our ELK instance, a process typically called "ingesting."

---

**1. Download the Elastic Agent.**

Press the windows button and type powershell, make sure to click "Run as Admin"

Once the powershell instance opens, copy what you kept in the file in my case it was "Agent.txt" and paste it into the powershell and hit enter.

![](_attachments/Pasted%20image%2020240213182803.png)

Make sure you type `y` and hit enter when prompted by powershell.

---

Switch back over to your browser and you should see "1 Agent has been enrolled".

![](_attachments/Pasted%20image%2020240213182945.png)

Then Click "Add to Integration".

---

On the next page leave everything default and click "Confirm Incoming Data".

![](_attachments/Pasted%20image%2020240213183015.png)

The browser will take a few seconds to confirm the machine is connected, once thats finished click "View Assets"

![](_attachments/Pasted%20image%2020240213183049.png)

---

**2. Check The Fleet.**

Once thats done we should be connected and ready for the next part, But first lets make sure the device has successfully connected.

![](_attachments/Pasted%20image%2020240213183202.png)

click the hamburger at the top left of the window and scroll down almost all the way to the bottom. You should see the option "Fleet", select fleet.

![](_attachments/Pasted%20image%2020240213183258.png)

Our Elastic Agent is installed and configured to be connected to our ELK instance in the cloud.  Next part, we will cover how to configure Sysmon to submit logs to this Elastic Agent, which will ingest the logs to appear in Kibana.

---

### Sysmon Logs and Elastic Security

---

By default, Windows logs are not ideal.  To get logs that are more readable and useful, we can use Sysmon. 

**1. Download Sysmon**

Follow this link to download Sysmon.

[Download Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon "https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon")

Find the "Download Sysmon" link.

![](_attachments/Pasted%20image%2020240213184136.png)

In first part, Elk in Cloud, we reviewed several ways to find our download.  Repeat these steps to find find the Sysmon download in the Downloads folder.

Perform "Extract All" on the Sysmon Folder. Ensure the Sysmon folder is selected -- It will be highlighted blue.

![](_attachments/Pasted%20image%2020240213184157.png)

"Extract" to the Downloads folder.  Windows should auto-populate the Downloads path.

In your search bar, type "PowerShell."
The following options will be presented.  Click "Run as Administrator."

![](_attachments/Pasted%20image%2020240213184250.png)

In your PowerShell window, enter the following command. You will need to substitute [USER] for the user you are using on your local system.

```powershell
cd C:\Users\[USER]\Downloads\Sysmon\
```


The following command will install and start Sysmon as a service.

```powershell
.\Sysmon.exe -i -n -accepteula
```


Your following output should look similar to this.


![](_attachments/Pasted%20image%2020240213184331.png)


Now that Sysmon is running on our system, we need to configure our Elastic agent to gather these logs.  Sign into your cloud account.

Navigate to "Integrations" through the navigation menu.

![](_attachments/Pasted%20image%2020240213184356.png)

At the top of the page enter "windows" into the search bar.  Select the Windows option with the red square pictured below.

![](_attachments/Pasted%20image%2020240213184630.png)

Add this integration.

![](_attachments/Pasted%20image%2020240213184711.png)

By default, the Sysmon logs channel should be active.  This can be checked under the "Collect events from the following Windows event log channels:" section of the "Add integration" page.

Save the Integration.

![](_attachments/Pasted%20image%2020240213184910.png)

When prompted click "Add elastic agent to your hosts".

![](_attachments/Pasted%20image%2020240213185019.png)

In the Integrations menu, find the "Installed integrations" tab.

In first part, Elk in the Cloud we selected an Elastic Security configuration. In doing so, "Endpoint Security" and "System" are automatically installed in our Integrations.

![](_attachments/Pasted%20image%2020240213185314.png)

At this point, play around on the computer that has Elastic Agent installed.  Move files around, create files, start programs, make a few Google searches.  This will generate some logs to ensure that we have Sysmon logs reaching our cloud.

After you have created some log activity, navigate to "Discover" by accessing the hamburger menu on the top left.

Set a filter on your data to limit your results to sysmon data.  This can be done by searching the "data_stream.dataset" field for "windows.sysmon_operational" data. Then click "add filter". Your filter should now be set.

![](_attachments/Pasted%20image%2020240213185514.png)

![](_attachments/Pasted%20image%2020240213185610.png)

![](_attachments/Pasted%20image%2020240213185757.png)

If you have a result, and not an error, your Sysmon data is being collected and sent to Elastic.

![](_attachments/Pasted%20image%2020240213185843.png)
