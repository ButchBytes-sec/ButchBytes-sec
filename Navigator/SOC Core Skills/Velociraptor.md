---
tags:
  - velociraptor
  - edr
  - lab-soc-core-skills
date: Feb 10, 2024
---

## Velociraptor

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we will install and utilize Velociraptor to examine the various IR artifacts on your computer. Velociraptor, available for free, is a fantastic EDR tool that enhances our comprehension of computer functionalities. Additionally, it serves as an exemplary illustration of the commercial tools prevalent in the security profession. Furthermore, for those inclined to delve deeper, Velociraptor offers exceptional training resources. Explore more on their website: https://www.velocidex.com/ and for training: https://www.velocidex.com/training/

---


# Velociraptor 

Let's get started.

First, we will need to extract the executable from the 7zip archive. 

Within Windows File Explorer navigate to the C:\IntroLabs directory:

`cd \IntroLabs`

![](_attachments/Pasted%20image%2020240213164950.png)

Next, right click on the Velociraptor .7z file and select 7-Zip > Extract Here

![](_attachments/Pasted%20image%2020240213165040.png)

Now we will need to open a command prompt and change directories to the IntroLabs directory.

First, open a Windows Terminal as Administrator:

When you get the pop up, select Yes.

Next, let's open a Command Prompt:

Now, let's navigate to the IntroLabs directory:

![](_attachments/Pasted%20image%2020240213165213.png)

For this installation, we are going to set up Velociraptor as a standalone deployment.  This means the server and the client will be run on the same system.

Let’s get started:

`velociraptor-v0.5.5-1-windows-amd64.exe config generate -i`

When it asks about the OS, please choose windows.  It should be the default.

![](_attachments/Pasted%20image%2020240213165253.png)

When it asks about the Path to the datastore, just hit enter.  This will keep the default.

![](_attachments/Pasted%20image%2020240213165318.png)

When it asks about the SSL certs, just hit enter.  It will choose the default of Self Signed SSL.

![](_attachments/Pasted%20image%2020240213165334.png)

When it asks about the DNS name, just hit enter.  It will set the default to localhost.  This will work fine as we are just running this locally.

![](_attachments/Pasted%20image%2020240213165351.png)

For the default ports, once again, just hit enter to accept 8000 and 8889 as the defaults.

![](_attachments/Pasted%20image%2020240213165408.png)

When asked about Google Domains DynDNS, please enter `N`

![](_attachments/Pasted%20image%2020240213165521.png)

For the GUI username, please just hit enter to end.

![](_attachments/Pasted%20image%2020240213165444.png)

![](_attachments/Pasted%20image%2020240213165615.png)

When it asks about the logs directory, just hit enter to accept the default.

![](_attachments/Pasted%20image%2020240213165659.png)

When it asks where to write the server and client configs, just hit enter to accept the defaults.

![](_attachments/Pasted%20image%2020240213165722.png)

Now, let’s add a GUI user.

`velociraptor-v0.5.5-1-windows-amd64.exe --config server.config.yaml user add root --role administrator`

When it asks for the password, please choose a password you will remember.

When finished, it should look similar to this:

![](_attachments/Pasted%20image%2020240213165821.png)

Now, lets run the msi to load the proper files to the proper directories:

`velociraptor-v0.5.5-1-windows-amd64.msi`


Now, let's start the server.

`velociraptor-v0.5.5-1-windows-amd64.exe --config server.config.yaml frontend -v`

There will be some red.  Don’t panic.

![](_attachments/Pasted%20image%2020240213165928.png)

Next, let’s surf to the GUI and see if it worked!

`https://127.0.0.1:8889`

When you load the page, there will be an SSL error about the self-signed cert.  That is fine.

select Advanced then proceed to 127.0.0.1

![](_attachments/Pasted%20image%2020240213170101.png)

When it asks for the Username and Password, please enter root and the password you chose earlier.


Please select Inspect the server's state.

![](_attachments/Pasted%20image%2020240213170213.png)

Next, we need to start the client. Lucky for us, it is the same executable.

We will need to open another Windows Command Prompt.

Then Navigate to the IntroLabs directory.

`cd \IntroLabs`

![](_attachments/Pasted%20image%2020240213170321.png)

Next, we will need to start the client.  To do this will need to run the MSI first.

`velociraptor-v0.5.5-1-windows-amd64.msi`

When you get the pop up, select Run.  This will install the proper libraries and files.

Next, we will start the client.

`velociraptor-v0.5.5-1-windows-amd64.exe --config client.config.yaml client -v`

![](_attachments/Pasted%20image%2020240213170436.png)

Now, let’s go back to the GUI and select the Home button.

![](_attachments/Pasted%20image%2020240213170526.png)

You should see one connected client.

![](_attachments/Pasted%20image%2020240213170559.png)

Now let’s look at what we can do with this.

First things first, this is not necessarily a detection platform.  It is designed to allow you to dig when you get an alert on malware signatures or from suspicious traffic. 

So please, keep in mind, it is not a replacement for AV!

So that said, let’s look around.

First, let’s "Show All" Clients.

![](_attachments/Pasted%20image%2020240213170716.png)

As you can see below there will only be one client.

![](_attachments/Pasted%20image%2020240213170733.png)

If you select that client, you can get additional information about that system.

![](_attachments/Pasted%20image%2020240213170803.png)

Next, let’s "Show All" Clients again.

![](_attachments/Pasted%20image%2020240213170716.png)

Then select our only client.

![](_attachments/Pasted%20image%2020240213170733.png)

Now, select Shell.

![](_attachments/Pasted%20image%2020240213170908.png)

This allows us to run commands on the target system.  Think of the commands that we ran from the Windows CLI, we can run those here too.

Please select the PowerShell box and select Cmd.

![](_attachments/Pasted%20image%2020240213170949.png)

Now, enter netstat -naob in the Cmd box and select Launch.

![](_attachments/Pasted%20image%2020240213171014.png)

This will not display the results right away. To see the results, select the Eye icon with your netstat command below:

Now, let’s do a Hunt.   Please select the Hunt icon.

![](_attachments/Pasted%20image%2020240213171118.png)

To start a Hunt, please select the + icon.

![](_attachments/Pasted%20image%2020240213171149.png)

Please name your Hunt, then select "Select Artifacts" on the bottom.

![](_attachments/Pasted%20image%2020240213171234.png)

We are going to keep this simple for this lab. Please select Generic.System.Pstree.

![](_attachments/Pasted%20image%2020240213171326.png)

Then, Review on the bottom.

![](_attachments/Pasted%20image%2020240213171353.png)

We now have an overview of what is going to be run on all systems...  Which is only one.

Now select Launch.

Once you select Launch, it will start the Hunt and load it in the que.

![](_attachments/Pasted%20image%2020240213171438.png)

Please select our Hunt.  Now, we can run it.  Please press the Play button above.

![](_attachments/Pasted%20image%2020240213171515.png)

When you get the pop-up, select Run it!

This will take a few moments.

When done, you will see Total scheduled is 1 and Finished Clients is 1.

You can also download the results.

Please select Download Results.

![](_attachments/Pasted%20image%2020240213171705.png)

Then, CSV Only.

![](_attachments/Pasted%20image%2020240213171736.png)

This will create a zip with the output.

Please download that by clicking on the zip file.

![](_attachments/Pasted%20image%2020240213171801.png)

Go ahead and open the zip file.

![](_attachments/Pasted%20image%2020240213171826.png)

Then, open the csv file with WordPad:

![](_attachments/Pasted%20image%2020240213171859.png)

![](_attachments/Pasted%20image%2020240213171953.png)

Granted, this is not optimal.  We did not load Excel on this system because of licensing restrictions.  However, you can copy this over to your host system and open it there. 

However, if you want to see a simple HTML report you can click on the turn back time icon on the left side (Right above the binoculars) and then clock Download Results > Prepare Collection Report, then click on the HTML report that appears below it.

![](_attachments/Pasted%20image%2020240213172308.png)

![](_attachments/Pasted%20image%2020240213172347.png)

![](_attachments/Pasted%20image%2020240213172409.png)

We have not even begun to touch what we can do with this awesome tool.
