# PROJECT 2 - DETECT AND ALERT FAILED SSH LOGIN ATTEMPTS USING AWS CLOUDWATCH


## Real-Time Monitoring of Failed SSH Login Attempts on EC2 Using CloudWatch Metric Filters and Alarms


## Project Overview
In this project, I implemented a security monitoring pipeline that detects failed SSH login attempts on an Amazon EC2 instance. Had some errors but I was able to troubleshoot and solve the underlying problems that constituted the error.
The solution uses CloudWatch Logs, Metric Filters, and Alarms to generate notifications whenever there is a login attempt failure 3 times in 5 minutes.

## Skills Demonstrated :
-	CloudWatch Agent configuration
-	Log ingestion and metric extraction
-	CloudWatch alarms & SNS notifications
-	AWS IAM roles and permissions
-	Threat detection & incident monitoring

## Technologies or services Used
-	Amazon EC2 (LINUX 2)
-	Amazon CloudWatch Logs
-	Amazon CloudWatch Metrics
-	CloudWatch Metric Filters
-	Amazon SNS
-	IAM Roles & Policies

## Architecture Summary
-	EC2 Instance generates Linux security logs (/var/log/audit/audit.log).
-	CloudWatch Agent pushes these logs to CloudWatch Log Group.
-	Metric Filter extracts failed SSH login events.
-	Metric increments every time a failed login occurs.
-	CloudWatch Alarm triggers when metric ≥ 3 within 5 minutes.
-	SNS Topic sends email notification.

<br>
<br>

# <p align="center">  **STEPS INVOLVED**</p>

## STEP 1 - CHECKING THE STATUS OF THE ALREADY AVAILABLE SERVICES FROM PROJECT 1
-	At the end of my last project I stopped the Ec2 instance in which my CloudWatch agent was installed and logging to CloudWatch.
-	I decided to start the instance once again
-	I SSH’ed into the Ec2 instance and confirmed the status of my CW agent and it was still working.
-	I checked the CloudWatch console to ensure that the log group was still collecting logs from /var/log/audit/audit.log
-	I checked the logs collected to ensure that the audit.log logs in the EC2WATCH log group collects the user login logs which appears as ‘type = USER_LOGIN’



![SSH](LM%20IMGS/SSH.jpg)


![AGENT-CHK](LM%20IMGS/AGENT-CHK.jpg)


![SSH](LM%20IMGS/USERLOGIN-CHK.jpg)


## STEP 2 - CREATE A METRIC FILTER FOR FAILED SSH LOGINS
-	In the process of creating a metric filter, I created a filter pattern using REGEX expressions ({ "$.type = "USER_LOGIN" && $.msg ~= "res=failed" "}).
-	I then simulated a failed login event by trying to ssh into my instance with wrong credentials and no key. (This was done to create a failed login log to be used to text my filter pattern.)
-	The failed login log appeared in the EC2WATCH audit log group
-	I went ahead to test the pattern and I got my first **ERROR**.

![LOGINFAIL](LM%20IMGS/LOGINFAIL-SIM.jpg)

![LOG](LM%20IMGS/LOGINFAIL-LOG.jpg)



## FIRST ERROR – INVALID CHARACTER ERROR
-	I realized that filter patterns do not accept regular expressions and characters just like I thought. Plain text is all that is needed


![FIRST ERROR](LM%20IMGS/FIRSTERROR.jpg)



## STEP 3 – (SOLUTION)
-	I removed all the expressions and left only the plain text (type=USER_LOGIN res=failed) 
-	I tested the pattern once again and it gave back a few results of failed login logs
-	I then completed the creation of the failed login metric filter


![NEW PATTERN](LM%20IMGS/WORKING-PATTERN.jpg)


## STEP 4 – CREATING AN SNS TOPIC AND SUBSCRIPTION
-	I opened my SNS console and created a topic named “FAILEDLOGIN”
-	I subscribed my emailed to it
-	I got a confirmation email which I confirmed.
-	An SNS topic with my email subscribed was ready to receive my alerts


![SNS](LM%20IMGS/SNS-SUB.jpg)
