# Editing Files (Linux Text Editors)

## Overview
In this lab, I explored **Linux text editors**—specifically `vim` and `nano`—to learn how to view, edit, and save files within a command-line environment.  
This exercise focused on building comfort with **file editing tools**, understanding **vim basics** through the `vimtutor` executable, and performing file manipulation on the `/var/log/secure` system log file using **nano**.

## Objectives
After completing this lab, I was able to:

- Use the **`vimtutor`** executable to complete Vim tasks 1–4  
- Copy and edit content from the **`/var/log/secure`** file using **nano**
- Practice text editing and navigation skills in both editors

---

## Lab Environment
This lab utilized an **Amazon Linux EC2 instance** provided through the **AWS re/Start Vocareum environment**.

- **Instance:** Command Host (Amazon Linux)
- **Access:** Secure Shell (SSH)
- **Tools:** `vim`, `nano`
- **Log file:** `/var/log/secure`

---

## AWS Service Restrictions
The lab environment restricts AWS access to only the services required for completion.  
You may encounter permission errors if you attempt to use unrelated AWS services or actions.

---

## Accessing the AWS Management Console
1. Click **Start Lab** to launch the environment.  
   A *Start Lab* panel will open showing **Lab status: starting**.
2. Wait for the message **Lab status: ready**.
3. Close the panel by selecting **X**.
4. Click **AWS** at the top of the instructions.  
   This opens the AWS Management Console in a new browser tab and logs you in automatically.
5. If pop-ups are blocked, enable them and refresh.
6. Arrange your **AWS Console** and **lab instructions** side-by-side for convenience.

---

## Task 1: Connect to the Amazon Linux EC2 Instance via SSH
In this first task, I established a connection to my Amazon Linux EC2 instance to begin editing tasks.



## Task 2: Use vimtutor to Learn Vim Basics
The Vim Tutor is an interactive guide that teaches the essentials of the Vim editor.

Commands:
```bash
vimtutor
```
This launched a training session where I practiced:
- Navigating text using the h, j, k, and l keys
- Entering Insert mode with i and returning to Normal mode with Esc
- Saving and quitting using :wq
- Deleting words and lines with dw and dd

Result: I became comfortable with Vim’s modal editing workflow and core navigation commands.

## Task 3: Edit the /var/log/secure File Using nano
In this task, I learned to use nano, a simpler terminal text editor, to view and modify log data.

Steps:
Copied a system log file to my home directory to safely edit:

```bash
sudo cp /var/log/secure ~/secure.log
```
Opened the copy in nano:

```bash
nano ~/secure.log
```
Practiced editing actions such as:
-Moving the cursor using arrow keys
-Searching text with Ctrl + W
-Deleting lines with Ctrl + K
-Saving changes with Ctrl + O and pressing Enter
-Exiting nano with Ctrl + X

Result: I successfully opened, navigated, and edited a system file using nano.

Lab Completion
- At the end of the lab, I had:
- Completed vimtutor tasks 1–4
- Used nano to copy, view, and edit system logs
- Practiced text editing, file management, and command-line navigation
- Finally, I ended the lab by selecting End Lab → Yes, confirming the deletion of temporary resources.

## Key Learnings
vimtutor provides an excellent foundation for learning Vim’s modal editing style.
nano is an intuitive and lightweight text editor ideal for quick file edits.
Editing log files like /var/log/secure can help troubleshoot authentication or security events.
Understanding both Vim and Nano improves flexibility when managing Linux systems remotely.

## Reflection
This lab helped me gain confidence working with text editors directly in the terminal.
While Vim offers powerful, advanced capabilities, Nano provides a simpler alternative for quick edits.
Mastering both editors is essential for effective system administration in Linux-based environments.
