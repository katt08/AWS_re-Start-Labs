AWS Lambda Exercise (Challenge)
Lab Overview

In this challenge lab, I created an AWS Lambda function that counts the number of words in a text file. The function is triggered automatically when a file is uploaded to an Amazon S3 bucket, and the resulting word count is sent out using Amazon SNS.

Objectives

- After completing this lab, I was able to:

- Create a Lambda function that counts the number of words in a text file.

- Configure an Amazon S3 bucket to automatically invoke the Lambda function when a text file is uploaded.

- Create an Amazon SNS topic that sends the resulting word count to an email recipient.


Your challenge
Create a Lambda function to count the number of words in a text file. The general steps are as follows:

Use the AWS Management Console to develop a Lambda function in Python and create the function's required resources.

Report the word count in an email by using an SNS topic. Optionally, also send the result in an SMS (text) message.

Format the response message as follows:

The word count in the <textFileName> file is nnn. 
Replace <textFileName> with the name of the file.

Enter the following text as the email subject: Word Count Result

Automatically invoke the function when the text file is uploaded to an S3 bucket.

Test the function by uploading a few sample text files with different word counts to the S3 bucket.

Forward the email that one of your tests produces and a screenshot of your Lambda function to your instructor.

 

Hints 

Create all of your resources in the same AWS Region.

You need an AWS Identity and Access Management (IAM) role for the Lambda function to access other AWS services. Because the lab policy does not permit the creation of an IAM role, use the LambdaAccessRole role. The LambdaAccessRole role provides the following permissions:

AWSLambdaBasicExecutionRole

This is an AWS managed policy that provides write permissions to Amazon CloudWatch Logs.

AmazonSNSFullAccess

This is an AWS managed policy that provides full access to Amazon SNS via the AWS Management Console.

AmazonS3FullAccess

This is an AWS managed policy that provides full access to all buckets via the AWS Management Console.

CloudWatchFullAccess

This is an AWS managed policy that provides full access to Amazon CloudWatch.

Refer to the following lab for additional guidance:

Working with AWS Lambda

 

Conclusion
Congratulations! You now have successfully done the following:

Created a Lambda function to count the number of words in a text file

Configured an S3 bucket to invoke a Lambda function when a text file is uploaded to the S3 bucket

Created an Amazon SNS topic to report the word count in an email

 

Lab complete
