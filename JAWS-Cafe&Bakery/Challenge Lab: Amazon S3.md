Challenge Lab: Amazon S3
 

Lab overview
In this challenge lab, you create an Amazon Simple Storage Service (Amazon S3) bucket and perform some routine tasks, such as uploading objects and configuring permissions to make those objects publicly accessible through a browser.

Objectives
By the end of this challenge, you should be able to do the following:

Create an S3 bucket. 

Upload an object into this bucket. 

Access the object by using a web browser. 

List the contents of the S3 bucket by using the AWS Command Line Interface (AWS CLI). 

Duration
This lab requires approximately 45 minutes to complete.

 

Accessing the AWS Management Console
At the top of these instructions, choose Start Lab to launch your lab.

A Start Lab panel opens displaying the lab status.

Wait until the message "Lab status: ready" appears, and then choose X to close the Start Lab panel.

At the top of these instructions, choose AWS to open the AWS Management Console on a new browser tab. The system automatically signs you in.

Tip If a new browser tab does not open, a banner or icon at the top of your browser will indicate that your browser is preventing the site from opening pop-up windows. Choose the banner or icon, and choose Allow pop-ups.

Arrange the AWS Management Console so that it appears alongside these instructions. Ideally, you should be able to see both browser tabs at the same time to follow the lab steps.

At the top of these instructions, choose Details, and then choose Show. 

Copy the values of the SecretKey, PublicIP, and AccessKey, and paste them into a text editor. You use this information while working through this challenge.

 

   Note: The PublicIP value is the public IPv4 address of the Linux instance that has been provisioned for you for this challenge.

 

Task 1: Connecting to the CLI Host instance
To start the challenge, you connect to the CLI Host instance that is already provisioned for you.

On the AWS Management Console, in the Search bar, enter and choose EC2 to open the EC2 Management Console.

In the navigation pane, choose Instances.

From the list of instances, select the CLI Host instance.

Choose Connect.

On the EC2 Instance Connect tab, choose Connect.

Note: If you prefer to use an SSH client to connect to the EC2 instance, see the guidance to Connect to Your Linux Instance.

Now that you are connected to the CLI Host instance, you can configure and use the AWS CLI to call AWS services.

 

Task 2: Configuring the AWS CLI
To set up the AWS CLI profile with credentials, run the following command in the EC2 Instance Connect terminal:

aws configure
At the prompts, copy the following values that you pasted into your text editor, and paste them into the terminal window as directed.

AWS Access Key ID: Enter the value for AccessKey.

AWS Secret Access Key: Enter the value for SecretKey.

Default region name: Enter us-west-2.

Default output format: Enter json.

Now you are ready to run AWS CLI commands to interact with AWS services.

 

Task 3: Finishing the challenge
To finish the challenge, do the following:

Create an S3 bucket. 

Upload an object into this bucket.

Try to access the object by using a web browser. 

Make the object (not the bucket) publicly accessible.

Access the object by using a web browser. 

List the contents of the S3 bucket by using the AWS CLI. 

Hint: You may refer to the relevant sections of the course which teaches S3. Additional documentation links are provided in the references section.

Note: As you complete these challenges, capture screenshots to submit to your instructor.

 

Conclusion
Congratulations! You now have successfully done the following:

Created an S3 bucket

Uploaded an object into this bucket

Accessed the object by using a web browser

Listed the contents of the S3 bucket by using AWS CLI

Lab complete
