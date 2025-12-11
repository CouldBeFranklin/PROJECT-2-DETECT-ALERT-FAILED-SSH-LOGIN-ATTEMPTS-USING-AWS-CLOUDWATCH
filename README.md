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



## **STEPS INVOLVED**

# STEP 1 - CHECKING THE STATUS OF THE ALREADY AVAILABLE SERVICES FROM PROJECT 1
-	At the end of my last project I stopped the Ec2 instance in which my CloudWatch agent was installed and logging to CloudWatch.
-	I decided to start the instance once again
-	I SSH’ed into the Ec2 instance and confirmed the status of my CW agent and it was still working.
-	I checked the CloudWatch console to ensure that the log group was still collecting logs from /var/log/audit/audit.log
-	I checked the logs collected to ensure that the audit.log logs in the EC2WATCH log group collects the user login logs which appears as ‘type = USER_LOGIN’

![SSH](LM%20IMGS/SSH.jpg)


![AGENT-CHK](LM%20IMGS/AGENT-CHK.jpg)


![SSH](LM%20IMGS/USERLOGIN-CHK.jpg)


