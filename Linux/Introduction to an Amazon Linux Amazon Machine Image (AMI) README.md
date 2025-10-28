# AWS re/Start – Introduction to an Amazon Linux Amazon Machine Image (AMI)

## Overview
In this lab, I explored the basics of connecting to an **Amazon Linux Amazon Machine Image (AMI)** using **Secure Shell (SSH)** and practiced navigating the **Linux command line interface (CLI)**.  
The main goal of this exercise was to reinforce fundamental Linux skills, understand how to use the **man command** (manual pages), and learn how to explore command documentation within the Linux shell.

## AWS Service Restrictions
This lab environment was preconfigured and restricted to only allow the AWS services necessary for the lab.  
Any attempt to access additional AWS services or actions outside of the instructions resulted in restricted access errors. This helped ensure focus on EC2 and Linux shell usage only.

---

## Scenario
In this lab, I connected to an **Amazon Linux AMI** hosted on an **Amazon EC2 instance** within the **Vocareum lab environment** using **SSH**.  
Once connected, I explored the **man command** and its various sections to better understand how to access and navigate Linux documentation from the terminal.

---

## Objectives
After completing this lab, I was able to:

- Use **SSH** to access an **Amazon Linux AMI** in the Vocareum lab environment  
- Understand the purpose and usage of the **man (manual) command**  
- Demonstrate the **search feature** within man pages  
- Identify and examine key **man page headers**

---

## Lab Environment Components
The following components were provided automatically as part of the lab environment:

- **Amazon EC2 – Command Host** (in a public subnet): Used to run commands and access the Linux CLI  
- **Amazon VPC**: The network environment containing the EC2 instance  
- **Public Subnet**: Allowed SSH access to the command host instance  

---

## Task 1: Use SSH to Connect to an Amazon Linux EC2 Instance
I began by accessing the AWS Management Console through the lab environment and retrieving the SSH credentials.

### Steps I Completed (Windows):
1. Selected **Details → Show** to view lab credentials.  
2. Downloaded the provided **labsuser.ppk** key file.  
3. Noted the **Public IP address** of the EC2 instance.  
4. Opened **PuTTY**, configured the session with:
   - **Host Name (or IP address):** *[Public IP from the lab]*
   - **Connection Type:** SSH  
   - **Port:** 22  
   - **Private Key:** Loaded the `labsuser.ppk` file under *Connection → SSH → Auth*  
5. Connected successfully to the **Amazon Linux AMI** command host.

*Result:* I was able to securely access my EC2 instance through SSH and was greeted with a Linux command line prompt.

---

## Task 2: Explore the Linux man Pages
Once connected, I used the **man command** to explore Linux’s built-in help documentation.

### Commands I Executed:
```bash
man man
This opened the manual page for the man command, which provides documentation on how to use the man utility itself.

What I Observed:
The man page displayed several important headers that structure the command documentation, including:
NAME – Displays the name and a short description of the command
SYNOPSIS – Shows the syntax and options for using the command
DESCRIPTION – Provides a detailed overview of what the command does
EXAMPLES – Demonstrates how to use the command
FILES, OPTIONS, SEE ALSO – Offer related details and references

I scrolled through the man page using the arrow keys and exited by pressing q.

 Result: I successfully viewed, navigated, and exited the man page, understanding its purpose and structure.

 Lab Completion
After completing all tasks:
I verified that I could connect to a Linux instance using SSH
I navigated the Linux help system using man
I identified the key headers within a typical man page
I exited the session and ended the lab via the End Lab button
The lab environment automatically deprovisioned all resources after confirmation.

Key Learnings
SSH (Secure Shell) is the standard method for securely accessing remote Linux servers.
The man command is essential for learning and troubleshooting in Linux—it provides quick access to built-in documentation.
Understanding man page sections helps quickly locate command syntax, options, and examples.
This lab strengthened my confidence in using the Linux terminal for cloud operations.
