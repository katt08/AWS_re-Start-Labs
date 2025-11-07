# Amazon Route 53 Failover Routing
## Lab Overview

In this activity, I configured failover routing for a simple web application using Amazon Route 53.

The lab environment began with two Amazon EC2 instances already deployed. Each instance had the full LAMP stack installed along with a running copy of the café website. These EC2 instances were deployed across two different Availability Zones for high availability. For example, if I was working in the us-west-2 Region, one web server was located in us-west-2a and the other in us-west-2b.

My goal in this lab was to configure my domain so that if the web server in the primary Availability Zone became unavailable, Route 53 would automatically fail over traffic to the web server in the secondary Availability Zone.

When I completed the activity, my architecture provided automatic failover and increased reliability by using multiple Availability Zones.

Objectives
After completing this activity, you should be able to do the following:

- Configure a Route 53 health check that sends emails when the health of an HTTP endpoint becomes unhealthy.

- Configure failover routing in Route 53.


  ## Task 1: Confirming the Café Websites

I began by reviewing the resources that AWS CloudFormation automatically created for me. At the top of the page, I selected Details and then chose Show under AWS to open the Credentials panel.

I copied the following values and saved them in a text editor for later:
```
CafeInstance1IPAddress

PrimaryWebSiteURL

SecondaryWebsiteURL

CafeInstance2IPAddress
```

After copying the values, I closed the Credentials panel.

Then, I switched to the AWS Management Console and searched for EC2 to open the EC2 Management Console. I navigated to Instances and saw two EC2 instances already running: CafeInstance1 in us-west-2a and CafeInstance2 in us-west-2b. The URLs I copied earlier corresponded to the café application running on each instance.

To verify this, I opened a new browser tab and pasted the PrimaryWebSiteURL. The café application webpage opened successfully, displaying café information and a Server Information section showing the EC2 instance and Availability Zone it was running in.

Next, I opened another browser tab and pasted the SecondaryWebsiteURL, confirming that the second instance had the same configuration.

To ensure the application was functioning, I selected Menu, ordered an item, and submitted the order. The confirmation page showed the time of the order based on the server’s time zone.

At this point, I confirmed that both café application instances were running in separate Availability Zones for high availability.

## Task 2: Configuring a Route 53 Health Check

I opened Route 53 from the AWS Management Console and ignored any IAM-related warnings.

From the left navigation pane, I chose Health checks, then selected Create health check and configured:

Name: ```Primary-Website-Health```

What to monitor: Endpoint

Specify endpoint by: IP address

IP address: I entered the public IPv4 address of CafeInstance1

Path: cafe

Then I expanded Advanced configuration and set:

Request interval: Fast (10 seconds)

Failure threshold: 2

I clicked Next and created an SNS notification:

Create alarm: Yes

Send notification to: New SNS topic

Topic name: Primary-Website-Health

Recipient email address: I entered my own email

After creating the health check, Route 53 began checking the endpoint. I refreshed the page until I saw it change to Healthy.

I checked my email and confirmed the SNS subscription.

## Task 3: Configuring Route 53 Records
##Task 3.1: Creating the Primary A Record

I navigated to Hosted zones and selected my unique domain ending in vocareum.training.

I chose Create record and configured:

Record name: www

Record type: A

Value: CafeInstance1IPAddress

TTL: 15

Routing policy: Failover

Failover record type: Primary

Health check ID: Primary-Website-Health

Record ID: FailoverPrimary

I created the record.

## Task 3.2: Creating the Secondary A Record

I created another record:

Record name: www

Record type: A

Value: CafeInstance2IPAddress

TTL: 15

Routing policy: Failover

Failover record type: Secondary

Record ID: FailoverSecondary

This completed the failover configuration.

## Task 4: Verifying DNS Resolution

I copied the Record name from the A record and opened it in a new browser tab, adding /cafe to the end of the URL.

The primary café website loaded correctly and displayed the server’s Availability Zone.

## Task 5: Verifying Failover Functionality

To simulate a failure, I stopped CafeInstance1 in the EC2 console.

I returned to Route 53 and monitored the Primary-Website-Health status until it turned Unhealthy.

Then, I refreshed the café webpage. The Server Information now showed a different Availability Zone—confirming that the site had failed over to CafeInstance2.

I also received an email alert from AWS Notifications confirming the health check alarm.

## Conclusion

I successfully:

- Configured a Route 53 health check and SNS email alerts

- Created primary and secondary failover A records

- Verified that Route 53 automatically fails over to the secondary instance when the primary instance becomes unavailable

Lab Complete.
