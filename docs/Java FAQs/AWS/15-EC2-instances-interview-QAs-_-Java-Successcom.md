# 15 EC2 instances interview Q&As   Java Success.com

## Table of Contents

- [Q01: What is an Amazon EC2 Service ?](#q01)
- [Q02: What is an Elastic IP Address feature of EC2 instances?](#q02)
- [Q03: What is an AMI?](#q03)
- [Q04: What is an Instance type?](#q04)
- [Q05: What is the dif ference between stopping & terminating an instance?](#q05)
- [Q06: What is an EC2 Key Pairs?](#q06)
- [Q07: How do you set up V irtual Firewalls to your EC2 instances?](#q07)
- [Q08: What is an EC2 tagging?](#q08)
- [Q09: What are Regions and A vailability Zones EC2?](#q09)
- [Q10: What is an on-demand instance?](#q10)
- [Q11: What is a Spot instance?](#q11)
- [Q12: You have an application running on your Amazon EC2 instance. Y ou want to reduce](#q12)
- [Q13: When you create an EC2 instance in the subnet with default settings, which of th](#q13)
- [Q14: When should you use a classic load balancer Vs. an application load balancer?](#q14)
- [Q15: If you have your website using multiple EC2 instances and RDS MySQL across multi](#q15)

---

## Q01: What is an Amazon EC2 Service ?

**Answer:**

EC2 stands for Elastic Cloud Computing that provides resizable (i.e. scalable) computing capacity in the cloud. Y ou can use Amazon EC2 to launch as many virtual servers as you need. In EC2 you can configure security and networking
as well as manage storage by mounting an EBS volume.

---

## Q02: What is an Elastic IP Address feature of EC2 instances?

**Answer:**

Within an A WS infrastructure, you will have VPCs, within the VPCs, you have instances. When you launch an EC2 instance, you receive a Public IP address by which that instance is reachable from internet. Once you stop that
instance and r estart the instance you get a new Public IP for the same instance . This is a problem to connect your instance from internet for not having a static IP . To overcome this problem, you need to attach an Elastic IP to an Instance
which doesn’ t change after you stop & start the instance. In short, an Elastic IP is a permanent IP for your instance.

---

## Q03: What is an AMI?

**Answer:**

An Amazon Machine Image (i.e. AMI) provides the information required to launch an EC2 instance. Y ou can launch multiple instances from a single AMI when you need multiple instances with the same configuration. Y ou can
launch an instance from an existing AMI, customize the instance, and then save this updated configuration as a custom AMI.

---

## Q04: What is an Instance type?

**Answer:**

Instance types comprise varying combinations of CPU, memory , storage, and networking capacity and give you the flexibility to choose the appropriate mix of resources for your applications.
EC2 Instance types example

---

## Q05: What is the dif ference between stopping & terminating an instance?

**Answer:**

You cannot restart a terminated instance, but you can restart a stopped instance, but the data stored on the instance will be lost. Y ou can stop an Amazon EBS backed instance, but not an s3-backed instance. When the instance is stopped,
you’re not char ged for any instance hours, only for the storage volume.
When an instance is terminated, the instance performs a normal shutdown, then the attached Amazon EBS volumes are deleted unless the volume’ s deleteOnT ermination attribute is set to false.

---

## Q06: What is an EC2 Key Pairs?

**Answer:**

The public and private keys are known as a key pair . Public-key cryptography enables you to securely access your instances using a private key instead of a password.
You can use Amazon EC2 to create your key pair . Each key pair requires a name. Be sure to choose a name that is easy to remember . Amazon EC2 stores the public key only , and you store the private key . Anyone who possesses your private
key can decrypt your login information, so it’ s important that you store your private keys in a secure place.
REFER: SSH to A WS EC2 instances via PuTTY & between other EC2 instances

---

## Q07: How do you set up V irtual Firewalls to your EC2 instances?

**Answer:**

EC2 Security Gr oups acts as a virtual firewall that controls the traf fic for one or more instances. When you launch an instance, you can specify one or more security groups. If you don’ t a default security group will be used.
You can add rules in terms of protocols, ports, and source IP ranges in each security group that allow traf fic to or from its associated instances. Y ou can modify the rules for a security group at any time.
When you launch an instance in a VPC, you must specify a security group that’ s created for that VPC. After you launch an instance, you can change its security groups. Security groups are associated with network interfaces . Changing an
instance’ s security groups changes the security groups associated with the primary network interface (i.e. eth0).

---

## Q08: What is an EC2 tagging?

**Answer:**

AWS allows customers to assign metadata to their A WS resources in the form of tags. Each tag is a simple label consisting of a customer -defined key and an optional value that can make it easier to manage, search for , and filter
resources.

Tags enable you to categorize your A WS resources in dif ferent ways, for example, by purpose, owner , or environment. This is useful when you have many resources of the same type—you can quickly identify a specific resource based on the
tags you’ve assigned to it.
For example, you could define a set of tags for your account’ s Amazon EC2 instances that helps you track each instance’ s OWNER (E.g. DBAdmin, User , etc) and ENV (E.g. TEST , PROD, etc).

---

## Q09: What are Regions and A vailability Zones EC2?

**Answer:**

Amazon EC2 is hosted in multiple locations world-wide. These locations are composed of regions and A vailability Zones. Each region is a separate geographic area. Each region has multiple, isolated locations known as A vailability
Zones.
Each region is completely independent. Each A vailability Zone is isolated, but the A vailability Zones in a region are connected through low-latency links. AZ-1a, AZ-1b, and AZ-1c are availability zones in the region.
AWS regions, VPC and availability zones
When you launch an EC2 instance, you must select an AMI that’ s in the same region (if the AMI is in another region then you can copy the AMI to the region you are using). Now select an A vailability Zone or let A WS choose for you. After
creating the EC2 instance, it will show up in selected A vailability Zone.

---

## Q10: What is an on-demand instance?

**Answer:**

On-Demand instances are servers that run in EC2 or A WS Relational Database Service (RDS) and are purchased at a fixed rate per hour . They are also suitable for use during testing and development of applications on EC2.
This is generally the most expensive purchasing option for A WS instances, and the hourly prices vary depending on the operating system and size of the instance.

---

## Q11: What is a Spot instance?

**Answer:**

Spot instances are spare EC2 capacity that can save you up 90% of f of On-Demand prices that A WS can interrupt with a 2-minute notification. Spot uses the same underlying EC2 instances as On-Demand and Reserved Instances, and is
best suited for fault-tolerant , flexible workloads. Spot instances provides an additional option for obtaining compute capacity and can be used along with On-Demand and Reserved Instances

---

## Q12: You have an application running on your Amazon EC2 instance. Y ou want to reduce the load on your instance as soon as the CPU utilization reaches 100 percent. How will you do that?

**Answer:**

It can be done by creating an autoscaling gr oup to deploy more instances when the CPU utilization exceeds 100 percent and distributing traf fic among instances by creating a load balancer and registering the Amazon EC2 instances
with it.

---

## Q13: When you create an EC2 instance in the subnet with default settings, which of the IP addresses will be created as soon as it is launched?

**Answer:**

Private IP . Private IP is automatically assigned to the instance as soon as it is launched.
Public IP needs an Internet Gateway which again has to be created since it’ s a new VPC, and Elastic IP has to be set manually .

---

## Q14: When should you use a classic load balancer Vs. an application load balancer?

**Answer:**

AWS Elastic Load Balancing (i.e. ELB) supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers. A load balancer distributes incoming application traf fic across multiple
EC2 instances in multiple A vailability Zones. This increases the fault tolerance of your applications. Elastic Load Balancing detects unhealthy instances and routes traf fic only to healthy instances.
AWS ELB against EC2 instances
The classic load balancer is used for simple load balancing of traf fic across multiple EC2 instances. While, the application load balancing is used for more intelligent load balancing, based on the multi-tier architecture or container -based
architecture of the application. Application load balancing is mostly used when there is a need to route traf fic to multiple services.

---

## Q15: If you have your website using multiple EC2 instances and RDS MySQL across multiple availability zones, and you notice READ contention issues on your RDS MySQL. How will you go about fixing this?

**Answer:**

ElastiCache to the rescue, which is an in-memory cache running in every availability zone for creating a cached version of the website for faster access. Y ou can also add RDS MySQL read replica in each availability zone that can help
in efficient and better performance for read operations by reducing the workloads.
EC2 T utorials
AWS EC2 T utorials Step by step .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
