---
tags:
  - web-testing
  - owasp-zap
  - dvwa
  - python-web-server
  - lab-soc-core-skills
date: Feb 09, 2024
---

# 12. OWASP ZAP - Web Testing


### OWASP ZAP - Web Testing

> _Disclaimer: The content, procedures, and materials provided in this lab are strictly for educational purposes. Any information acquired is intended solely for advancing knowledge in cybersecurity. All procedures outlined in this lab, along with the VM utilized, were developed and acquired from the [SOC Core Skills](https://www.antisyphontraining.com/on-demand-courses/soc-core-skills-w-john-strand/) course instructed by [John Strand](https://www.sans.org/profiles/john-strand/). It is emphasized that the knowledge gained here will not be applied for any unauthorized or malicious activities._

> In this lab, we're setting up a basic Python Web Server alongside a vulnerable web server named DVWA (Damn Vulnerable Web Application). Both servers are intentionally created to educate individuals about various web application attacks. While a comprehensive introduction to web attacks isn't the primary focus of this class, we'll demonstrate how to utilize tools like ZAP (Zed Attack Proxy) to automatically scan for vulnerabilities. It's important to note that automated tools like ZAP may not always detect all vulnerabilities, underscoring the need for manual inspection and understanding of attack vectors. With DVWA now operational, participants can explore and learn about common web application vulnerabilities in a controlled environment, fostering a deeper understanding of web security principles and defense strategies.

---

First, you will need to start an Ubuntu prompt.  Please select that from the Windows Terminal drop-down.

Now, let's start the Python Web Server:

![](_attachments/Pasted%20image%2020240211033942.png)

![](_attachments/Pasted%20image%2020240211034000.png)

Now, let's start ZAP!

![](_attachments/Pasted%20image%2020240211034022.png)


![](_attachments/Pasted%20image%2020240211034045.png)

Let's do a quick test of the Python Web Server:

First, select Automated Scan

![](_attachments/Pasted%20image%2020240211034114.png)

Now, put in your Linux IP and port 65412 in as the URL to attack.

<pre>http://172.17.32.119:65412</pre>

![](_attachments/Pasted%20image%2020240211034233.png)

Then, select "Use traditional spider" and then select "Attack":

When it gets done crawling and scanning select Alerts:

This shows that ZAP does a pretty good job of finding the easy to identify vulnerabilites.

![](_attachments/Pasted%20image%2020240213155801.png)


### Additional Lab
DVWA LAB!

Let’s get started by opening a Terminal as Administrator
When you get the User Account Control Prompt, select Yes.

PS C:\Users\adhd> `docker run --rm -it -p 80:80 vulnerables/web-dvwa`

![](_attachments/Pasted%20image%2020240213152851.png)

In another Command Prompt window run ipconfig and record your IP address.  Remember, your IP address may be different from mine.

C:\Users\adhd>`ipconfig`

![](_attachments/Pasted%20image%2020240213160809.png)

Now, let's start Chrome and play with DVWA. Please note that our class has a track record of DoSSing the Docker download for this section.  I recomend doing this after class when less than 100 people are hitting it at the same time.

![](_attachments/Pasted%20image%2020240213153043.png)

When your browser runs, it usually connects to the Internet directly.  In this lab, however, we need it to connect to a local proxy (ZAP) to intercept and attack the web traffic.  To do this, we need to configure Chrome to use ZAP as the proxy.

Now, lets configure the proxy:

![](_attachments/Pasted%20image%2020240213160911.png)

Now, we will need to surf to your IP address.  You recorded it above with the ipconfig command. Simply put http://<YOUR_IP> into the browser.

You will get an error.  This is normal.  This is because the traffic is being intercepted by a proxy.  Normally, this would be very, very bad.   However, in this lab, we are proxying the traffic to test the app.  Go ahead and select Advanced. Then, select Proceed.:

![](_attachments/Pasted%20image%2020240213160840.png)

The credentials are admin:password

![](_attachments/Pasted%20image%2020240213161027.png)

Please log in.

For the first run, you will need to configure the database. 

Please select Create / Reset Database

![](_attachments/Pasted%20image%2020240213161216.png)

Now, log back in

IF you go back to ZAP you will see that it is capturing the site data.  We could do this manually.  Simply clicking every page.  But, that would take a long time.  We can have ZAP do this for us automatically.  This is called crawling or spidering a website.

Now, from ZAP lets spider the app:

![](_attachments/Pasted%20image%2020240213161629.png)

When the pop-up hits, select Start Scan

While scanning a site for links is cool.  We want to actively scan the site for vulnerabilities.   ZAP can do this as well.  This is called an active scan.

Now, let's start the active scan:

![](_attachments/Pasted%20image%2020240213161745.png)

When prompted, select Start Scan

Scan Running:

![](_attachments/Pasted%20image%2020240213161850.png)

When done, select Alerts

![](_attachments/Pasted%20image%2020240213163118.png)

Did it find anything interesting? Here lies the issue with solely relying on automated tools; they often overlook certain aspects. While they excel at detecting the 'easy' stuff, they fail to capture everything, not even close.

I'd like to take a moment to highlight a few things that the scanner may have overlooked. Let's see if it missed anything.

Here's just one example.

`dsds;ls -lrt;sdsd`

![](_attachments/Pasted%20image%2020240213163302.png)

`%' or '0'='0' union select user, password from dvwa.users #`

![](_attachments/Pasted%20image%2020240213163404.png)
