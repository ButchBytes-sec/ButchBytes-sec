<h2>Basic Concepts of Operating Systems</h2>
**https://samples.jbpub.com/9781449626341/26341_CH01_Garrido.pdf**

---

Here are the questions located at the end of the book designed to confirm your understanding of the Basic Concepts of Operating Systems:
1. Explain the main facilities provided by an operating system. Give examples.
2. What type of computing do you normally use with your personal computer: batch or interactive? Explain and give examples.
3. Explain when you would prefer time-sharing processing instead of batch processing.
4. Describe the general type of work that you regularly do with Windows. Explain the type or level of operating system interface that you normally use. Describe any other possible interface level that you could use with Windows.
5. Get a Unix/Linux account and log in to the system; start experimenting with a small set of commands. Get acquainted with the system's prompt, which is normally a dollar sign. Investigate the use and purpose of the following commands: who, cd, pwd, Is, finger, mkdir, cp, more, cat, and pico.
6. From the user point of view, list and explain the main differences between Windows and Linux.
7. From your point of view, what are the main limitations or disadvantages of Windows?
8. From your point of view, what are the main limitations or disadvantages of Linux?
9. Search the Internet and, from the user point of view, list and explain the main differences between Unix and Linux.
10. Explain the main advantages and disadvantages of using internal and external views for the operations of a computer system, such as the one presented in this chapter. Give good arguments.
11. Log in to Linux. At the command prompt, type man ps to read the online manual documentation for the ps command. After reading this documentation, type ps. How many programs are active? How are they identified? Are these all the programs in the OS? Investigate with the options in the ps command.
12. Search the Internet to find data on the commercial support and commercial applications developed for Linux.
---

<h3>Answers:</h3>
1. Main Facilities Provided by an Operating System:
Examples include process management, memory management, file system management, user interface, and networking. For instance, an OS manages multiple programs running simultaneously (process management), allocates memory to programs (memory management), provides a way to organize files (file system management), offers a graphical interface (user interface), and enables devices to communicate over a network (networking).
2. Type of Computing with Personal Computer:
Most personal computers use interactive computing. Interactive computing allows users to interact directly with the computer through a graphical user interface (GUI) or command line. Examples include using a web browser, text editor, or file explorer.
3. Time-Sharing Processing vs. Batch Processing:
Time-sharing processing is preferred when multiple users need simultaneous access to a computer system. For example, a server where users are performing tasks concurrently. Batch processing is useful when a large number of similar tasks need to be executed without user intervention, like processing payroll.
In a nutshell:
- Time-sharing processing is like everyone cooking in a shared kitchen, taking turns to use the computer for different tasks.
- Batch processing is like a factory making many similar things on a conveyor belt, handling repetitive tasks efficiently without needing constant supervision.
4. Work with Windows and Interface Levels:
Most users work with the graphical interface of Windows, using icons, windows, and a mouse. There's also a command-line interface (Command Prompt) that offers text-based interaction.
5. Linux Commands:
    - `who`: Shows users currently logged in.
    - `cd`: Changes the current directory.
    - `pwd`: Prints the current working directory.
    - `ls`: Lists files and directories.
    - `finger`: Displays information about users.
    - `mkdir`: Creates a new directory.
    - `cp`: Copies files or directories.
    - `more`: Displays content one screen at a time.
    - `cat`: Concatenates and displays file content.
    - `pico`: A text editor.
6. Differences Between Windows and Linux:
Differences include licensing (Windows is proprietary, Linux is open-source), file system structure, user interface, package management, and security model.
7. Limitations/Disadvantages of Windows:
Proprietary nature, susceptibility to viruses/malware, resource-intensive, and costs associated with licensing.
8. Limitations/Disadvantages of Linux:
Learning curve for new users, limited availability of certain proprietary software, and hardware compatibility issues (though this has improved).
9. Differences Between Unix and Linux:
Similarities due to Linux being inspired by Unix, but differences include licensing (Unix was initially proprietary), system architecture, and command variations.
10. Advantages/Disadvantages of Internal and External Views:
    - **Internal View:** Provides low-level details and control over system operations but can be complex for non-experts.
    - **External View:** Offers a user-friendly interface and abstracts complexities, but users have limited control over system internals.
11. Linux ps Command:
The `ps` command shows currently running processes. The number of processes and their details are displayed. Additional options provide more detailed information.
12. Commercial Support and Applications for Linux:
Search for companies that offer paid support for Linux distributions, such as Red Hat or SUSE. There are various commercial applications available for Linux, including office suites (LibreOffice), graphic design software (GIMP), and more.
---

<h3>Supplementary:</h3>

1. What is the WebAdmin password?
- Edit > Find Packet or simply click the magnifying glass. Then, Packet Details > String > ”WebAdmin”, Find.
[8]
- Check out the HTML code that includes the text “webadmin”.
[9]
- Let’s find the password.txt file and find the webadmin password.
- In the Find Packet options, change the first option to Packet list, and type in the string “password.txt” ,then click Find.
- An HTTP GET request is highlighted in the packet list which contains the string “password.txt”.
[10]
- Right-click the packet and Follow the HTTP stream to see the contents of the password.txt file.
[11]
- The WebAdmin password and the answer is: “sbt123”
[12]
________________________
2. What is the version number of the attacker’s FTP server?
- Right-click the source IP address in the GET request packet and apply it as a selected filter.
- Add and ftp to the current display filter to find all ftp traffic from the IP address in question.
[13]
- FTP info shows version and the answer is: 1.5.5.
[14]
________________________
3. Which port was used to gain access to the victim Windows host?
- Change the display filter so it only shows packets where 192.168.56.1 is the source IP.
- A series of ACKs indicates an established connection; The port and answer is:8081
[15]
________________________
4. What is the name of a confidential file on the Windows host?
- I followed the TCP stream of TCP Retransmission packet 4258 which dropped me in at stream 2079.
[16]
- I used the arrows at the bottom right of the TCP stream window to move between streams.
- I eventually found stream 2075 which shows commands being used to list the contents of the desktop. The confidential file is listed below and the answer is: Employee_Information_CONFIDENTIAL.txt
[17]

________________________
5. What is the name of the log file that was created at 4:51 AM on the Windows host?
In stream 2075 the desktop contents also show a LogFile.log file created at and the answer is: 4:51 AM.
[18]

I am seeking an opportunity within my own community as a Messenger Personnel to apply my computer literacy and physical fitness. I aim to contribute to a team and acquire new skills in the process.

In a nutshell:
- Time-sharing processing is like everyone cooking in a shared kitchen, taking turns to use the computer for different tasks.
- Batch processing is like a factory making many similar things on a conveyor belt, handling repetitive tasks efficiently without needing constant supervision.
_________


1. Imagine you're organizing your computer's files into folders. How is this similar to a file system's organization?

Concept 1: File System:
A file system organizes and manages files on storage devices, like hard drives or SSDs.
Practical Implication 1:
It allows users to store, access, and organize their data efficiently.

2. Think about how you use your computer. How does multitasking enhance your productivity?

Concept 2: Multitasking:
Multitasking allows multiple applications or processes to run concurrently, sharing the CPU's processing time.
Practical Implication 2:
It enables users to switch between different applications seamlessly and increases overall system efficiency.


3. Imagine you're using several apps on your smartphone. How does the operating system manage the memory for these apps to ensure smooth performance?

Concept 3: Memory Management:
Memory management controls the allocation and use of computer memory (RAM) by various programs and processes.
Practical Implication 3:
Efficient memory management ensures optimal usage of available memory, preventing system slowdowns due to memory shortages.


4. Consider your email account. How does your email service ensure that only you can access your account?

Concept 4: User Authentication:
User authentication ensures that users are who they claim to be before granting access to a system.
Practical Implication 4:
It safeguards data and resources by preventing unauthorized access to the system.


5. Think about a busy cafeteria. How might the concept of process scheduling be similar to serving customers efficiently?

Concept 5: Process Scheduling:
Process scheduling decides the order in which processes run on the CPU, balancing fairness and efficiency.
Practical Implication 5:
It prevents any single process from monopolizing the CPU, ensuring all tasks get a fair share of processing time.


6. Imagine your computer's keyboard. How does it work with the operating system, and why do you need a device driver for it?

Concept 6: Device Drivers:
Device drivers are software components that enable the operating system to communicate with hardware devices.
Practical Implication 6:
They allow devices like printers, graphics cards, and network adapters to work seamlessly with the operating system.


7. Think about opening multiple browser tabs. How might virtual memory help prevent your computer from slowing down?

Concept 7: Virtual Memory:
Virtual memory uses a portion of the hard drive as an extension of RAM, allowing the system to handle more applications than the physical RAM can accommodate.
Practical Implication 7:
It prevents programs from crashing due to running out of memory and allows for smoother multitasking.

