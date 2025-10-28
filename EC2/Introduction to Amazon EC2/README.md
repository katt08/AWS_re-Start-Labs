# My Hands-on Lab: Intro to Amazon EC2
## Overview
In this lab, I explored the fundamentals of **Amazon Elastic Compute Cloud (Amazon EC2)** by launching, configuring, monitoring, resizing, and terminating an EC2 instance.

**Amazon EC2** provides resizable compute capacity in the cloud and allows developers to scale resources up or down within minutes. Through this lab, I learned how to deploy a web server, manage instance settings, modify security groups, and test termination protection.

## Learning Objectives
By completing this lab, I was able to:

- Launch an Amazon EC2 instance with **termination protection enabled**
- Monitor and view **instance metrics** using **Amazon CloudWatch**
- Modify a **security group** to allow inbound HTTP traffic
- **Resize** an EC2 instance and its **EBS volume**
- Test and disable **termination protection**
- **Terminate** the EC2 instance properly

## Task 1: Launching My EC2 Instance
I started by accessing the **AWS Management Console** through the provided lab environment. Once the resources were provisioned, I proceeded to launch a new EC2 instance.

### Steps I Completed:
1. **Named my instance:** `Web Server`
2. **Selected the AMI:** Amazon Linux 2023 (default)
3. **Chose the instance type:** `t3.micro` (2 vCPUs, 1 GiB memory)
4. **Key pair:** Proceeded without a key pair (as per lab instructions)
5. **Network settings:**
   - VPC: *Lab VPC*
   - Security group: *Web Server security group* (with no SSH access)
6. **Storage:** Kept the default 8 GiB root volume
7. **Enabled termination protection**
8. **Added user data script** to automatically deploy a web server:
   ```bash
   #!/bin/bash
   yum -y install httpd
   systemctl enable httpd
   systemctl start httpd
   echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html

9. Launched the instance and confirmed it reached the Running state with 2/2 status checks passed.
Result: My EC2 instance successfully started, and the web server was automatically installed and running.

## Task 2: Monitoring My Instance
After my instance was active, I reviewed its performance and health checks.

1. Verified System reachability and Instance reachability (both passed ✅)
2. Viewed CloudWatch metrics such as CPU utilization and network activity
3. Captured an instance screenshot for visual status confirmation

Result: Monitoring verified that my instance was healthy and performing correctly.

## Task 3: Updating Security Group and Accessing the Web Server
Initially, I couldn’t access my web page because HTTP (port 80) wasn’t allowed in the security group.

To fix this:
1. Navigated to Security Groups → Web Server security group
2. Edited Inbound rules
3. Added a new rule:
    Type: HTTP
    Source: Anywhere-IPv4 (0.0.0.0/0)
4. After saving, I refreshed the public IPv4 address in my browser and successfully accessed my web page displaying:
    Hello From Your Web Server!
Result: The web server was live and publicly accessible through port 80.

## Task 4: Resizing My Instance
To simulate scaling resources, I modified both the instance type and disk size.

Steps:
1. Stopped the EC2 instance.
2. Changed instance type: t3.micro → t3.small
3. Modified EBS volume size: 8 GiB → 10 GiB
4. Restarted the instance.
Result: My instance relaunched successfully with upgraded compute power and larger storage capacity.

## Task 5: Testing Termination Protection
To test termination protection:

Attempted to terminate the instance — received an error because termination protection was enabled.
Navigated to Actions → Instance Settings → Change termination protection
Disabled it and saved the change.
Successfully terminated the instance.
Result: Termination protection worked as expected, preventing accidental deletion until explicitly disabled.

## Conclusion
This lab provided me with practical experience in deploying and managing EC2 instances. I learned how to:
  Launch, monitor, and secure an EC2 instance
  Configure networking and security groups
  Automate web server deployment via User Data
  Scale resources dynamically
  Safeguard resources using termination protection
  Through this exercise, I gained a solid understanding of the foundational skills required to operate compute resources in AWS.
