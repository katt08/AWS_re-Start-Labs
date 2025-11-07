Activity - Optimize Utilization
Activity overview
In this activity, you will optimize the AWS resources that are used to run the Café web application. Specifically, you will:

Uninstall the decommissioned local database from the Café instance to decrease the instance’s storage requirements.

Change the instance type to T3 micro to reduce costs.

This diagram illustrates the topology of the Café web application runtime environment before and after the optimization.


Before and after resource optimization topology diagrams:

before and after optimization topology graphic

 

Activity objectives
After completing this activity, you will be able to:

Optimize an Amazon Elastic Compute Cloud (Amazon EC2) instance to reduce costs.
Use the AWS Pricing Calculator to estimate AWS service costs.
 

Business case relevance
A new business requirement for Café—Optimize resources to reduce AWS service costs

cafe scene

After the migration to Amazon Relational Database Service (Amazon RDS) was completed (an action taken in a prior activity), Sofîa identified a number of optimization opportunities that she could implement to reduce AWS service costs. First, she realized that the decommissioned local database could be uninstalled to reduce the storage requirements of the Café instance. She also realized that the Café instance could be downsized to a smaller instance type because the database process was no longer running on it.

In this activity, you will take on the role of Sofîa, and work on optimizing the Café instance to save costs.

 

Activity steps
Duration: This activity requires approximately 50 minutes to complete.

 

Accessing the AWS Management Console
At the top of these instructions, click Start Lab to launch your lab.

A Start Lab panel opens displaying the lab status.

Wait until you see the message "Lab status: ready", then click the X to close the Start Lab panel.

At the top of these instructions, click AWS

This will open the AWS Management Console in a new browser tab. The system will automatically log you in.

Tip: If a new browser tab does not open, there will typically be a banner or icon at the top of your browser indicating that your browser is preventing the site from opening pop-up windows. Click on the banner or icon and choose "Allow pop ups."

Arrange the AWS Management Console tab so that it displays along side these instructions. Ideally, you will be able to see both browser tabs at the same time, to make it easier to follow the lab steps.

 

Task 1: Optimize the website to reduce costs
Because the local database was migrated to Amazon RDS, you can reduce AWS service costs by performing the following actions on the Café EC2 instance:

Remove the local database from the instance. This action will reduce costs in both CPU and storage utilization.
Change the instance type from t3.small to t3.micro. Because the database process no longer runs on the instance, the smaller instance type will be both effective and also cheaper to run.
In this task, you use the AWS Command Line Interface (AWS CLI) to perform these actions. You begin by opening a Secure Shell (SSH) session to the Café instance and the CLI Host.

 

Task 1.1: Connect to the Café instance by using SSH
If you are a Windows user, follow the steps described in Task 1.1.1. Otherwise, if you are a macOS or Linux user, follow the steps in Task 1.1.2.

macOS/Linux users—Click here for login instructions

 

Task 1.1.1: Windows SSH
 These instructions are specifically for Windows users. If you are using macOS or Linux, skip to the next section.

Select the Details drop-down menu above these instructions you are currently reading, and then select Show. A Credentials window will be presented.

Select the Download PPK button and save the labsuser.ppk file.
Typically your browser will save it to the Downloads directory.

Make a note of the PublicIP address.

Then exit the Details panel by selecting the X.

Download  PuTTY to SSH into the Amazon EC2 instance. If you do not have PuTTY installed on your computer, download it here.

Open putty.exe

Configure your PuTTY session by following the directions in the following link: Connect to your Linux instance using PuTTY

Windows Users: Select here to skip ahead to the next task.


 

Task 1.1.2: macOS/Linux SSH
These instructions are for Mac/Linux users only. If you are a Windows user, skip ahead to the next task.

Read through the three bullet points in this step before you start to complete the actions, because you will not be able see these instructions when the Details panel is open.

Click on the Details drop down menu above these instructions you are currently reading, and then click Show. A Credentials window will open.
Click on the Download PEM button and save the labsuser.pem file.
Then exit the Details panel by clicking on the X.
Open a terminal window, and change directory cd to the directory where the labsuser.pem file was downloaded.

For example, run this command, if it was saved to your Downloads directory:

cd ~/Downloads
Change the permissions on the key to be read only, by running this command:

chmod 400 labsuser.pem
Return to the AWS Management Console, and in the EC2 service, click on Instances. Check the box next to the CafeInstance and click on the Details tab.

NOTE: The setup for this lab takes a few minutes, because an RDS database instance must first be created before the CafeInstance EC2 instance is created, and RDS instances take a few minutes to create. If you do not yet set the CafeInstance EC2 instance, you will need to wait until the lab setup completes before you can complete this step.
Copy the IPv4 Public IP value.

Return to the terminal window and run this command (replace <public-ip> with the actual public IP address you copied):

ssh -i labsuser.pem ec2-user@<public-ip>
Type yes when prompted to allow a first connection to this remote SSH server.

Because you are using a key pair for authentication, you will not be prompted for a password.


 

Task 1.1.3: Configure the AWS CLI
Before you can run AWS CLI commands on the instance, you must first configure the AWS CLI environment to define the AWS account credentials, Region name, and output format to use.

Discover the region in which the CLI Host instance is running:

curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region
You will use this region information in a moment.

Update the AWS CLI software with the credentials.

aws configure
At the prompts, enter the following information:

AWS Access Key ID: Click on the Details drop down menu above these instructions, and then click Show. Copy the AccessKey value and paste it into the terminal window.
AWS Access and Secret Keys

AWS Secret Access Key: Copy and paste the SecretKey value from the same Credentials screen.
Default region name: Type in the name of the region where your EC2 instances are running, which you just discovered a moment ago. For example, us-east-1 or eu-west-2.
Default output format: json
Leave this terminal window SSH session open. You will return to use it later in the activity.

 

Task 1.2: Connect to the CLI Host instance by using SSH
Follow the same instructions that you used in Task 1.1 to open an SSH session to a different EC2 instance—the CLI Host instance.

Do not close the connection to the CafeInstance, instead, create a connection to the CLI Host in a new window (using putty on Windows or using an additional terminal window on macOS/Linux).

You can find the CLI Host public IP address in the EC2 console, or by clicking on the Details drop down menu above these instructions, and then click Show.

After connecting, be sure to also configure the AWS CLI client software on the CLI Host EC2 instance, by running the aws configure command.

 

Task 1.3: Uninstall MariaDB and resize the instance
Stop the local database and uninstall it from the Café instance. In the SSH window for the CafeInstance, enter:

sudo systemctl stop mariadb
sudo yum -y remove mariadb-server
If the last command runs successfully, you will see a Complete! message in the output.

Close the SSH window for the CafeInstance because you no longer need it.

Determine the Instance ID of the CafeInstance. Switch to the SSH window for the CLI Host instance and enter:

aws ec2 describe-instances \
--filters "Name=tag:Name,Values= CafeInstance" \
--query "Reservations[*].Instances[*].InstanceId"
Record the value returned as:

CafeInstance Instance ID: i-nnnnnnnnnn
Stop the Café instance and change its instance type to t3.micro. In the SSH window for the CLI Host instance, enter:

aws ec2 stop-instances --instance-ids <CafeInstance Instance ID>
In the command, substitute <CafeInstance Instance ID> with the value that you recorder earlier.

Change the instance type to t3.micro. In the SSH window for the CLI Host instance, enter:

aws ec2 modify-instance-attribute \
--instance-id <CafeInstance Instance ID> \
--instance-type "{\"Value\": \"t3.micro\"}"
In the command, substitute <CafeInstance Instance ID> with the value that you recorder earlier.

If the command completes successfully, no output is returned.

Start the Café instance. In the SSH window for the CLI Host instance, enter:

aws ec2 start-instances --instance-ids <CafeInstance Instance ID>
In the command, substitute <CafeInstance Instance ID> with the value that you recorder earlier.

Check the current state of the instance, and wait until the status shows running. In the SSH window for the CLI Host instance, enter:

aws ec2 describe-instances \
--instance-ids <CafeInstance Instance ID> \
--query "Reservations[*].Instances[*].[InstanceType,PublicDnsName,PublicIpAddress,State.Name]"
In the command, substitute <CafeInstance Instance ID> with the value that you recorder earlier.

The instance might take a few moments to reach the running state. Periodically repeat the command until you can confirm that it is running. Also, record the PublicDnsName and PublicIPAddress values that are returned by the command by using the following format:

Downsized CafeInstance Public DNS Name: ec2-zzz-zzz-zzz-zzz.eu-west-2.compute.amazonaws.com
Downsized CafeInstance Public IP Address: nnn.nnn.nnn.nnn
Information: Because you restarted the instance, Amazon EC2 will assign a different Public DNS name and Public IP address to the instance than what it had before.

Test the Café website to make sure that it is functional. In a browser window, enter the following URL:

http://<Downsized CafeInstance Public DNS Name>/cafe
Substitute <Downsized CafeInstance Public DNS Name> with the value that you recorded.

Exercise the website's functions to verify that it works properly.

Great job! You have successfully uninstalled the decommissioned local database and downsized the Café instance.

 

Task 2: Use the AWS Pricing Calculator to estimate AWS service costs
AWS provides a tool that allows you to estimate the monthly costs of the AWS services that you use or are planning to use. In this task, you will use the AWS Pricing Calculator to estimate the cost of running the Café website on AWS before and after EC2 instance optimization. You will then calculate the projected cost savings.

NOTE: The values that you will enter into the AWS Pricing Calculator have been simplified to serve the purposes of this exercise. The intent is to show you the basic use of the calculator and highlight the functions that it provides.

The pricing examples shown in this activity were current as of time of publishing, April 2020, and is for informational purposes. Refer to the AWS website for current pricing by service.

 

Task 2.1: Calculate the costs before optimization
First, calculate the costs of running the website in its before optimization topology, that is, on a T3 small instance with a decommissioned local database still occupying storage space.

Specifically, you will use the following service list and configuration to describe the topology components:

Region: (the region where the CafeInstance EC2 instance is running)

Amazon EC2 instance:

Instance type: t3.small
Instance class: On-Demand
Utilization: 100% per month
Operating system: Linux
Amazon EBS volume: General Purpose SSD (gp2), 40 GB (including 20 GB occupied by the local database)
Amazon RDS instance:

Instance class: db.t3.micro
Engine: MariaDB
Allocated storage: 20 GB
 

Open the AWS Pricing Calculator. In a web browser, go to:

https://calculator.aws
Click on Create estimate.

Browse down and choose Configure in the Amazon EC2 service box.

In the Region menu at the top of the page, select the region where the CafeInstance EC2 instance is running.

For example, choose US East (N. Virginia) if your instances is running in us-east-1.

If you are prompted to confirm the region change, choose Change Region.

Choose the Advanced estimate option.

In the EC2 instance specifications area, for the Operating system, choose Linux

In the Workload area:

Choose Constant usage.
For Number of instances choose 1.
In the EC2 instances area, in the search box, search for and then select the t3.small instance type.

In the Pricing strategy area, set the pricing model to On-Demand

In the Amazon Elastic Block Storage (EBS) area:

Storage for each EC2 instance: General Purpose SSD (gp2)
Storage amount: 40 GB
Snapshot Frequency: No snapshot storage
Scroll to the bottom and select Add to my estimate.

Congratulations, you have now estimated the cost of the EC2 instance.

Next, you will add the RDS instance to your price estimate.

In the My Estimate page, click Add service.

In the Select service page, locate and click Configure in the Amazon RDS for MariaDB service panel.

Configure as follows:

Region: (choose the same Region you chose for the EC2 instance)
MariaDB instance specifications: Standard (single-AZ)
Instance type: search for and select db.t3.micro
Quantity: 1
Pricing model: On-Demand Instances
Storage volume: General Purpose SSD (gp2)
Storage amount: 20 GB per month
Choose Add to my estimate.

The My Estimate page shows a breakdown of the estimated monthly cost of the AWS services that you configured, and it provides a monthly total.

Choose Save and share.

If prompted, choose Agree and continue.

Choose Copy the public link and paste the link into another browser tab.

This is the estimated cost of your before optimization topology.

calculator services before optimization image

Export the estimate to a comma-separated values (CSV) file by choosing Action > Export estimate.

In the export dialog window, click OK and save the file to your local computer. You can optionally open it to see its contents.

Record the total estimated monthly cost (for example, $35.50) as:

AWS Services Before Optimization Estimated Monthly Cost: $35.50
 

Task 2.2: Calculate the costs after optimization
Next, you will calculate the costs of running the website after the Café instance was optimized. Specifically, you will modify the following entries in the calculator to reflect the effects of the optimization:

Amazon EC2 instance:

Instance type: t3.micro (Reduced size)
Amazon Elastic Block Store (Amazon EBS) General Purpose SSD (gp2), 20 GB (Reduced from 40 GB because the local database was removed)
In the https://calculator.aws/#/estimate browser tab, click Edit next to the Amazon EC2 entry.

In the EC2 Instances area, search for and select t3.micro as the instance type.

In the Amazon Elastic Block Storage (EBS) area, change the Storage amount  to 20 GB.

Scroll down and click Save to see the monthly cost estimate.

The My Estimate page should show the estimate of
your monthly costs for the after optimization topology.

calculator estimate after optimization image

Export the estimate to a comma-separated values (CSV) file by choosing Action > Export estimate.

In the export dialog window, click OK and save the file to your local computer. You can optionally open it to see its contents.

Record the total estimated monthly cost (for example, $25.18) as:

AWS Services After Optimization Estimated Monthly Cost: $25.18
 

Task 2.3: Estimate the projected cost savings for Café
Because you calculated the costs of the AWS services that are needed to run the Café website both before and after you optimize the instance, you can estimate the overall projected cost savings as follows:

Before optimization monthly costs:
    - Amazon RDS service         $14.71
    - Amazon EC2 service         $20.89
                                --------
    Total                        $35.60

After optimization monthly costs:
    - Amazon EC2 service         $10.47  
    - Amazon RDS service         $14.71
                                --------
    Total                        $25.18


Overall monthly cost savings     $10.42
Pricing is current as of time of publishing, April, 2020, and is for demonstration purposes only. Refer to the AWS website for current pricing by service.
Congratulations! By removing the decommissioned local database and downsizing the Café instance type, you will save more than $10 per month in AWS service costs.

 

Update from Café

Cafe Logo

Martha and Frank are very happy that Sofîa's initiative resulted in cost savings for the business. The amount of savings might be small, but it still helps the business!

 

Activity Complete
