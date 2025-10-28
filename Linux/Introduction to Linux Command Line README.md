# AWS re/Start – Introduction to Linux Command Line

## Overview
In this lab, I worked with the **Linux command line interface (CLI)** on an **Amazon Linux EC2 instance**.  
The goal was to strengthen my familiarity with fundamental Linux commands, system information retrieval, and command history functionality.  

Through this hands-on activity, I learned how to identify system details, view logged-in users, manage time zones, explore calendar options, and use Bash history features to streamline workflow.


## Objectives
After completing this lab, I was able to:

- Use **SSH** to connect to an **Amazon Linux EC2 instance**
- Run various **Linux system commands**
- Identify **system and user information**
- Work with **time zones and calendar views**
- Use **command history and search** to improve CLI efficiency

---

## Lab Environment
This lab used a preconfigured **Amazon EC2 Command Host** running **Amazon Linux** within the AWS re/Start Vocareum environment.  
All actions were performed through an SSH session.

---

## Task 1: Connect to an Amazon Linux EC2 Instance
I began the lab by connecting to the EC2 instance via **SSH**.

### Steps:
1. Accessed the **Vocareum lab environment**.  
2. Retrieved the **Public IP address** and **labsuser credentials**.  
3. Connected using **PuTTY** (Windows) or the terminal (macOS/Linux) with:
   ```bash
   ssh ec2-user@<public-ip-address>
Successfully logged into the Amazon Linux terminal as the ec2-user.

 Result: I established a secure shell connection to the EC2 instance and confirmed access to the Linux CLI.

## Task 2: Run Familiar Linux Commands
In this task, I explored several commands to gain information about the system and current user session.

  User & System Information
bash
Copy code
whoami
hostname -s
uptime -p
whoami — displayed my current username (ec2-user)
hostname -s — showed a shortened host name like ip-10.x.x.x
uptime -p — showed how long the system had been running

These commands helped me verify my user identity, instance details, and uptime information.

User Sessions
bash
Copy code
who -H -a
Displayed all users logged into the system, along with session details such as:

Name, Line, Time, Idle, PID, and Exit

I learned how to view active user sessions and related process information.

Time Zones and Dates
bash
Copy code
TZ=America/New_York date
TZ=America/Los_Angeles date
Displayed the current date and time in New York and Los Angeles time zones.
Helped me understand how Linux handles time zone environments dynamically.
This was useful for seeing how different systems or regions reflect local times.

Calendar and Julian Dates
bash
Copy code
cal -j
cal -s
cal -m
cal -j — showed the calendar with Julian dates (consecutive day count)
cal -s — displayed a Sunday–Saturday view
cal -m — displayed a Monday–Sunday view
I learned that Julian dates are used in some industries for continuous day tracking.

User ID and Groups
bash
Copy code
id ec2-user
Displayed my User ID (UID), Group ID (GID), and associated groups.

Example output:

sql
Copy code
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user)
This helped me understand Linux’s user and group identification system.

## Task 3: Improve Workflow with Command History and Search
In this task, I explored how to reuse and search previously entered commands efficiently.

View Command History
bash
Copy code
history
Displayed all commands I had executed during the session.
Confirmed that every command from Task 2 was stored in Bash history.

Search Command History
Keyboard Shortcut:
Press CTRL + R to open the reverse search feature.
I typed TZ to search for previously used date commands and edited them inline using arrow keys and Tab for autocomplete.
Reverse history search saved time by allowing me to quickly recall and reuse old commands.

Repeat the Last Command
bash
Copy code
date
!!
!! repeats the most recent command executed (in this case, date).
This shortcut is extremely useful for rerunning commands without retyping them.

## Lab Completion
At the end of the lab, I successfully:

Connected to an EC2 instance using SSH
Executed basic Linux commands
Viewed system and user data
Worked with time zones and calendar utilities

Practiced efficient command recall with Bash history

I then ended the lab session and confirmed that the environment was deleted automatically.

🧠 Key Learnings
