# AWS re/Start – Working with the File System

## Overview
In this lab, I practiced managing files and directories in a Linux environment by creating, organizing, and modifying a structured folder hierarchy.  
This exercise combined all prior knowledge from earlier labs to reinforce file system navigation, organization, and command-line proficiency.

---

## Duration
Approximately 30 minutes

---

## Objectives
After completing this lab, I was able to:

- Create a folder structure in Linux
- Create and manage files
- Copy, move, and reorganize directories
- Delete files and folders safely
- Verify directory contents using navigation commands

---

## Lab Environment
This lab used a preconfigured **Amazon Linux EC2 instance** within the **AWS re/Start Vocareum environment**.

- Instance: Command Host (Amazon Linux)
- Access Method: SSH
- Tools Used: Bash shell commands (`mkdir`, `touch`, `cp`, `mv`, `rm`, `rmdir`, `ls`, `pwd`)

---

## AWS Service Restrictions
In this lab environment, access to AWS services and actions was limited to those required for the lab.  
Attempting to access or modify other services could result in permission errors.

---

## Accessing the AWS Management Console
1. Select **Start Lab** to launch the environment.  
2. Wait for the message **Lab status: ready**, then close the panel.  
3. Choose **AWS** to open the AWS Management Console in a new tab.  
   - If blocked, enable pop-ups in your browser settings.
4. Arrange the console and lab instructions side by side for convenience.

This lab automatically launched an **Amazon EC2 Command Host**, which I accessed via SSH to perform all file system tasks.

---

## Task 1: Connect to the Amazon Linux EC2 Instance
To begin, I connected to the EC2 instance using an SSH client (PuTTY on Windows or Terminal on macOS/Linux).

## Task 2: Create a Folder Structure
In this task, I recreated a given folder hierarchy under the /home/ec2-user directory.

Folder and File Structure to Create
```bash
/home/ec2-user/CompanyA/
/home/ec2-user/CompanyA/Finance/
/home/ec2-user/CompanyA/Finance/ProfitAndLossStatements.csv
/home/ec2-user/CompanyA/Finance/Salary.csv
/home/ec2-user/CompanyA/HR/
/home/ec2-user/CompanyA/HR/Assessments.csvv
/home/ec2-user/CompanyA/HR/TrialPeriod.csv
/home/ec2-user/CompanyA/Management/
/home/ec2-user/CompanyA/Management/Managers.csv
/home/ec2-user/CompanyA/Management/Schedule.csv
```
Steps Performed
Confirmed current location:
```
pwd
```
If not in home directory:

```
cd /home/ec2-user
```
Created main folder:

```
mkdir CompanyA
cd CompanyA
```
Created subfolders:

```
mkdir Finance HR Management
```
Added files:

HR folder:

```
cd HR
touch Assessments.csv TrialPeriod.csv
cd ..
```
Finance folder:

```
cd Finance
touch ProfitAndLossStatements.csv Salary.csv
cd ..
```
Management folder:
```bash
touch Management/Managers.csv Management/Schedule.csv
```
Verified structure recursively:

```bash

ls -laR
[ec2-user@ CompanyA]$ ls -laR
.:
total 0
drwxr-xr-x 5 ec2-user root     49 Aug 10 13:36 .
drwx------ 4 ec2-user ec2-user 90 Aug 10 13:25 ..
drwxrwxr-x 2 ec2-user ec2-user 59 Aug 10 13:39 Finance
drwxrwxr-x 2 ec2-user ec2-user 52 Aug 10 13:37 HR
drwxrwxr-x 2 ec2-user ec2-user 46 Aug 10 13:39 Management

./Finance:
total 0
drwxrwxr-x 2 ec2-user ec2-user 59 Aug 10 13:39 .
drwxr-xr-x 5 ec2-user root     49 Aug 10 13:36 ..
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:39 ProfitAndLossStatements.csv
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:39 Salary.csv

./HR:
total 0
drwxrwxr-x 2 ec2-user ec2-user 52 Aug 10 13:37 .
drwxr-xr-x 5 ec2-user root     49 Aug 10 13:36 ..
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:37 Assessments.cvs
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:37 TrialPeriod.csv

./Management:
total 0
drwxrwxr-x 2 ec2-user ec2-user 46 Aug 10 13:39 .
drwxr-xr-x 5 ec2-user root     49 Aug 10 13:36 ..
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:39 Managers.csv
-rw-rw-r-- 1 ec2-user ec2-user  0 Aug 10 13:39 Schedule.csv

[ec2-user@ CompanyA]$
```
Result: All files and directories were created successfully.

Task 3: Delete and Reorganize Folders
A few weeks later, I was asked to reorganize the structure into a new layout.

New Folder Structure
swift
```
/home/ec2-user/CompanyA/
└── HR/
    ├── Finance/
    │   ├── ProfitAndLossStatements.csv
    │   └── Salary.csv
    ├── Employees/
    │   ├── Assessments.csv
    │   └── TrialPeriod.csv
    └── Management/
        ├── Managers.csv
        └── Schedule.csv
```
Steps Performed
Verified current directory:

```bash
pwd
```
Copied the Finance folder (and its contents) into HR:

```
cp -r Finance HR
ls HR/Finance
```
Removed the old Finance folder:
```
rm -r Finance
```
Moved Management inside HR:
```
mv Management HR
ls HR/Management
```
Created a new Employees folder inside HR:
```
cd HR
mkdir Employees
```
Moved Assessments.csv and TrialPeriod.csv into Employees:
```
mv Assessments.csv TrialPeriod.csv Employees
ls Employees
```
Verified the final structure:
```
ls -laR /home/ec2-user/CompanyA
```
The directory tree was successfully reorganized with Finance, Management, and Employees nested inside HR.

Lab Completion
- By the end of the lab, I successfully:
- Created and organized multiple directories and files
- Moved and copied entire folders
- Deleted directories using safe removal methods
- Validated the structure using recursive listing
- All commands executed correctly, and the final layout matched the required structure.

Key Learnings
- mkdir, touch, and ls are foundational tools for file and directory management.
- Relative and absolute paths can be used to create or reference files efficiently.
- The cp -r and mv commands are essential for reorganizing directories.
- rm -r allows recursive deletion, but must be used with caution.
- Regularly verifying file structures with ls -laR helps ensure accuracy.

Reflection
This lab demonstrated the importance of understanding Linux directory management for maintaining organized file systems.
By practicing creation, deletion, and reorganization commands, I gained hands-on experience in file operations that are vital for cloud system administration and automation tasks.

