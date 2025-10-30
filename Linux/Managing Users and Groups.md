### Managing Users and Groups
## Lab Overview

In this lab, I practiced managing users and groups on an Amazon Linux EC2 instance.
The goal was to create new users, assign them to groups, and test different permission levels by logging in as multiple users.

## Objectives

Create new users with default passwords

Create and manage Linux groups

Assign users to groups according to their job roles

Log in as different users to test access and permissions

Environment Setup

I launched the lab environment from the AWS re/Start portal and accessed the AWS Management Console.
From there, I opened an SSH session into my Amazon Linux EC2 instance using the provided credentials.
```bash
ssh ec2-user@<public_dns>
```

After connecting successfully, I confirmed my current directory:
```bash
[ec2-user@ip-10-0-10-217 ~]$ pwd
/home/ec2-user
```
# Task 1: Connect to the EC2 Instance

Once connected, I verified that I had shell access and sufficient permissions as the ec2-user.
This confirmed that I was ready to manage user accounts and groups on the system.

# Task 2: Create Users

I began by creating new users following the given table of names, user IDs, and roles.
Each user was assigned a default password: P@ssword1234!

Example commands I used:
```bash
sudo useradd arosalez
sudo passwd arosalez
```
I repeated these steps for all users:

| **First Name** | **Last Name** | **User ID** | **Job Role** | **Starting Password** |
|----------------|----------------|--------------|--------------------------|-----------------------|
| Alejandro | Rosalez | arosalez | Sales Manager | P@ssword1234! |
| Efua | Owusu | eowusu | Shipping | P@ssword1234! |
| Jane | Doe | jdoe | Shipping | P@ssword1234! |
| Li | Juan | ljuan | HR Manager | P@ssword1234! |
| Mary | Major | mmajor | Finance Manager | P@ssword1234! |
| Mateo | Jackson | mjackson | CEO | P@ssword1234! |
| Nikki | Wolf | nwolf | Sales Representative | P@ssword1234! |
| Paulo | Santos | psantos | Shipping | P@ssword1234! |
| Sofia | Martinez | smartinez | HR Specialist | P@ssword1234! |
| Saanvi | Sarkar | ssarkar | Finance Specialist | P@ssword1234! |


After creating all users, I verified them with:
```bash
sudo cat /etc/passwd | cut -d: -f1
```

Output snippet:
```bash
ec2-user
arosalez
eowusu
jdoe
ljuan
mmajor
mjackson
nwolf
psantos
smartinez
ssarkar
```
All users were successfully created.

# Task 3: Create and Manage Groups
Next, I created the necessary groups for each department and role.
```bash
sudo groupadd Sales
sudo groupadd HR
sudo groupadd Finance
sudo groupadd Shipping
sudo groupadd Managers
sudo groupadd CEO
```

I confirmed group creation:
```bash
cat /etc/group
```

Output:
```bash
Sales:x:1014:
HR:x:1015:
Finance:x:1016:
Shipping:x:1017:
Managers:x:1018:
CEO:x:1019:
```

Then I assigned users to their respective groups according to their job roles.
```bash
sudo usermod -a -G Sales arosalez
sudo usermod -a -G Sales nwolf
sudo usermod -a -G HR ljuan
sudo usermod -a -G HR smartinez
sudo usermod -a -G Finance mmajor
sudo usermod -a -G Finance ssarkar
sudo usermod -a -G Shipping eowusu
sudo usermod -a -G Shipping jdoe
sudo usermod -a -G Shipping psantos
sudo usermod -a -G Managers arosalez
sudo usermod -a -G Managers ljuan
sudo usermod -a -G Managers mmajor
sudo usermod -a -G CEO mjackson
```

I also added the ec2-user to all groups for administrative access.

To verify, I ran:
```bash
sudo cat /etc/group
```

Final output:
```bash
Sales:x:1014:arosalez,nwolf,ec2-user
HR:x:1015:ljuan,smartinez,ec2-user
Finance:x:1016:mmajor,ssarkar,ec2-user
Shipping:x:1017:eowusu,jdoe,psantos,ec2-user
Managers:x:1018:arosalez,ljuan,mmajor,ec2-user
CEO:x:1019:mjackson,ec2-user
```

Groups and memberships were correctly configured.

# Task 4: Test User Access and Permissions

To test access, I switched to one of the new users:
```bash
su arosalez
```

Entered the default password: P@ssword1234!

Once logged in, I confirmed my current directory:
```bash
pwd
```

Output:
```bash
/home/ec2-user
```

I attempted to create a file:
```bash
touch myFile.txt
```

Result:
```
touch: cannot touch ‘myFile.txt’: Permission denied
```

Then I tried using sudo:
```
sudo touch myFile.txt
```

Output:
```
arosalez is not in the sudoers file.  This incident will be reported.
```

This confirmed that the user arosalez does not have administrative (sudo) privileges.
I exited the user session:
```
exit
```

Back as ec2-user, I checked the security log:
```
sudo cat /var/log/secure | tail
```

I found the recorded failed sudo attempt:
```
Aug  9 14:45:55 ip-10-0-10-217 sudo: arosalez : user NOT in sudoers ; TTY=pts/1 ; PWD=/home/ec2-user ; USER=root ; COMMAND=/bin/touch myFile.txt
```

The unauthorized sudo attempt was logged successfully.

# What I Learned

I learned how to create, manage, and verify Linux users and groups using commands like useradd, groupadd, and usermod.
I understood the principle of least privilege, where users should only have the access necessary for their role.
I explored how sudo privileges and logging mechanisms enhance system security.
I practiced using /etc/passwd, /etc/group, and /var/log/secure for verification and auditing.
