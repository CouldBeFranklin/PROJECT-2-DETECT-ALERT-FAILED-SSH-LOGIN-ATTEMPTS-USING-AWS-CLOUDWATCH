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

![CONFIRM EMAIL](LM%20IMGS/CONFIRM-SUBEMAIL.jpg)


## STEP 5 – CREATING A CLOUDWATCH ALARM

-	I opened my CW console and navigated to alarms
-	I created an alarm with the attributes

| Setting	|	Value |
|:---------:|:----------:|
| Threshold type	|	Static | 
| Condition	|	Greater/Equal | 
| Threshold value	|	3 | 
| Evaluation period	|	5 minutes | 

-	I selected and attached the initially created SNS topic and named the alarm “FAILEDSSHLOGIN”
-	The alarm triggers when there is 3 recorded failed ssh login in 5 minutes. This sends an alert to the SNS topic attached to the alarm which in turn sends a message to the email subscribed to it.


![ALARM](LM%20IMGS/ALARM-CREATE.jpg)



## STEP 6 – TESTING THE WORKFLOW

-	I simulated a failed login attempt by trying to ssh into my instance using the wrong user and no key.
-	BUT THERE WAS A PROBLEM


![SIM](LM%20IMGS/LOGIN-SIM.jpg)


## SECOND ERROR – IAM PERMISSION ERROR

-	The failed logins where logged in my EC2WATCH audit log group in the CloudWatch console but it never triggered the alarm.
-	I also realized there was no spike on the metric filter graph despite a lot of failed logins being logged.
-	After minutes of troubleshooting and trying to narrow down the problem, I realized the role associated with the ec2 instance only had the permission for the CloudWatch agent to put logs in CloudWatch
-	It had no CloudWatch log permission to use the metric filter.




## STEP 7 - (SOLUTION)
-	I opened the IAM console and created a new policy and named it “PUTCLOUDWATCHMETRIC”, which had the permissions to put metric filter and filter log groups
-	I attached this policy to the EC2WATCH role attached to the ec2 instance.
-	I simulated the failed login once again and the metric filter graph spiked and BOOOOOOM I received an email alert from the SNS topic I created. 


![POLICY](LM%20IMGS/NEW-POLICY.jpg)




![ROLE](LM%20IMGS/CW-ROLE.jpg)




![ALERT](LM%20IMGS/EMAIL-ALERT.jpg)



## FINAL RESULT
The monitoring pipeline is fully functional:
-	CloudWatch agent ships audit.log to CloudWatch Logs
-	Metric filter extracts failed SSH login attempts
-	CloudWatch metric counts failures
-	Alarm triggers instantly
-	SNS sends email alerts
This provides real-time security monitoring for unauthorized login attempts on EC2.



## What I Learned
-	How Linux logs represent authentication events 
-	How CloudWatch Agent ingests logs
-	Building security metrics from log patterns
-	Filter pattern uses only plain text and no need for special characters
-	Creating actionable CloudWatch alarms
-	Always check the IAM permissions
-	Security monitoring




## <p align="center"> ROUGH SHEET GUIDE ON THE PROJECT


![ROUGH](LM%20IMGS/ROUGH1.jpg)


![ROUGH](LM%20IMGS/ROUGH2.jpg)
