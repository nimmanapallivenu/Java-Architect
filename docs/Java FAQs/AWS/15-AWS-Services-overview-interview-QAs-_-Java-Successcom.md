# 15+ AWS Services overview interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is the dif ference between CloudW atch & CloudT rail?](#q1)
- [Q2: What are the dif ferences among EBS, EFS & S3?](#q2)
- [Q3: What is the dif ference between SNS & SQS?](#q3)
- [Q4: What is Amazon SES?](#q4)

---

## Q1: What is the dif ference between CloudW atch & CloudT rail?

**Answer:**

Both provide logging capabilities.
CloudT rail is for logging “Who did what?” within your account for compliance, governance & auditing. For example, which user made the A WS S3 bucket public? Who shut down the EC2 instance? Which IP address logged into the A WS?
etc. It also allows you to centralize all the logs across regions & accounts into a S3 bucket.
CloudW atch is for creating logs, alarms, metrics & events for what’ s happening with A WS resources.
For example, sending your web & application server and application logs to CloudW atch. Lambda function logs.
For example, monitoring your EC2 instance metrics like CPU utilization, disk reads & writes, network in/out etc, Lambda function metrics like invocation counts, duration & errors.

---

## Q2: What are the dif ferences among EBS, EFS & S3?

**Answer:**

EBS & EFS are “ block level ” storages & A WS S3 is an “ object level ” storage.
Block Level VS. Object Level storages
EBS
1) EBS is suited as EC2 server disk.
2) High read & write performance as you can read & write in blocks. Suited for storing database & OS files.
3) Can be mounted to an EC2 instance in a same availability zone, and can be replicated within the same availability zone. Only one EC2 instance at a time in the same availability zone .

EFS
1) Can be mounted to multiple EC2 instances at the same time, and also can be replicated across multiple availability zones in the same region.
2) Can be mounted to an on premise server via VPN or Direct Connect.
3) Unlike EBS, no sizing need to be done.
S3
1) Suited for write once & read many times. E.g. storing the log files, archive files, application jars, data files for analytics, etc. Not suited for storing database or operating system files as fast read & writes are required.
2) Scales well, and unlike EBS, no sizing need to be done. Data also can be replicated across multiple availability zones.

---

## Q3: What is the dif ference between SNS & SQS?

**Answer:**

SNS – Simple Notification Service and SQS – Simple Queue Service.
SNS is a publish/subscribe system where A WS resources can publish events to a topic , and subscribers (E.g. SQS, Lambda, Email, SMS, HTTP , etc) will get notified of the events that are delivered to that topic.
AWS SNS publisher , topic & subscribers
SQS is a queuing service for message processing. A system (E.g. a service on EC2) must poll the queue to discover new events. If you want 2 EC2 services to poll and process, then you must 2 queues, and the sender should put the event to
both the queues.

AWS SQS – one to one

---

## Q4: What is Amazon SES?

**Answer:**

Amazon SES is Simple Email Service, which is a mail server that can both send and receive mail on your behalf. When you use Amazon SES to receive your mail, Amazon SES handles underlying mail-receiving operations, such as:
1) communicating with other mail servers.
2) scanning for spam and viruses.
3) rejecting mail from non trusted sources.
4) accepting mail for recipients in your domain.
You set up receipt rules to specify how to handle the mail when a condition is satisfied. A receipt rule consists of a condition and an ordered list of actions . For example, an action to invoke a Lambda function , which will detach the
attachment (e.g. .xlsx file), and convert it to a .csv and put it an A WS S3 bucket. The dif ferent types of actions are S3 action (E.g. delivers the email to S3 bucket), SNS action (E.g. to publish the email to a SNS topic), Lambda action, bounce
action, stop action, etc.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
