# Install and Configure the AWS CLI  
This documentation reflects the steps I performed to install, configure, and test the AWS Command Line Interface (AWS CLI) on a Red Hat Linux EC2 instance. I also connected the AWS CLI to an AWS account and interacted with AWS Identity and Access Management (IAM).

---

## **Overview**

The AWS CLI is a command-line tool that allows interaction with AWS services. In this lab, I installed the AWS CLI on a Red Hat Linux instance (which does not have it pre-installed), configured it using provided IAM credentials, and tested IAM access via CLI commands.
The completed environment consisted of:
- A Red Hat EC2 instance inside a VPC
- AWS CLI installed and configured
- IAM access established using an Access Key and Secret Key
- CLI interaction with IAM resources

---

## **Objectives**

After completing this lab, I was able to:

- Install the AWS CLI on a Linux instance  
- Configure the AWS CLI with AWS credentials  
- Use the AWS CLI to interact with IAM  

---

## **Task 1: Connect to the Red Hat EC2 Instance Using SSH**
I connected to the EC2 instance using SSH. (Connection method varies depending on OS — PuTTY for Windows, terminal/SSH for Mac/Linux.)
Example SSH command (Mac/Linux):

```bash
ssh -i labsuser.pem ec2-user@<PublicIP>
```
---

## **Task 2: Install the AWS CLI**
From the terminal on the EC2 instance, I installed the AWS CLI using the following commands:

To write the downloaded file to the current directory, run the following curl command with the -o option:
```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```
To unzip the installer, run the following unzip command with the -u option. In this command, the unzip command prompts you to overwrite any existing files. To skip these prompts, the command includes the -u option.
```bash
unzip -u awscliv2.zip
```
To run the install program, run the following command. This sudo command grants write permissions to the directory. The installation command in the code snippet uses a file named install in the unzipped aws directory to install the AWS CLI. 
```bash
sudo ./aws/install
```
To confirm the installation, run the following command:
```bash
aws --version
```
The following is an example of the output:
```bash
aws-cli/2.7.24 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.4.5
```
Note: The version numbers that are installed change overtime and might not reflect this example.

To verify that the AWS CLI is now working, run the following aws help command. The help command displays the information for the AWS CLI.
```
aws help
```
At the : prompt, enter q to exit.

---

## **Task 3: Review IAM Configuration in the Console**
In the AWS Console:
I navigated to IAM.
Opened Users → selected awsstudent.
Examined the permissions attached to the user.
Located the Access Key ID for use in AWS CLI configuration.
The Secret Access Key was retrieved from the Details panel provided with the lab.

---

## **Task 4: Configure the AWS CLI**
I configured the AWS CLI with the AWS credentials:

```bash
aws configure
```
I entered:
```
AWS Access Key ID	(Provided Access Key)
AWS Secret Access Key	(Provided Secret Key)
Default region name	us-west-2
Default output format	json

```
---

Task 5: Verify IAM Access Using the AWS CLI
I confirmed the CLI could communicate with IAM:

```bash
aws iam list-users
```
This returned a JSON list of user accounts, confirming CLI access was working.



Conclusion
I successfully:
Installed and verified the AWS CLI on a Linux instance
Configured the CLI to authenticate with an AWS account
Accessed IAM via the CLI and retrieved policy data
This lab reinforced how authentication works differently when accessing AWS via console (username/password) vs CLI (access key/secret key). It also improved my familiarity with IAM policy retrieval and command reference documentation.

Lab Complete
