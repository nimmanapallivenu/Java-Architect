# 01  AWS Q&As on VPC, Subnets, Availability Zones, VPN, Route tables, NACLs & Security Groups   Java Success.com

## Table of Contents

- [Q1: What is a VPC in A WS?](#q1)
- [Q2: What are the various components of VPC?](#q2)
- [Q3: What is a subnet?](#q3)
- [Q4: What is a NA T instance in A WS?](#q4)
- [Q5: What is a VPN in A WS?](#q5)
- [Q6: What is a route table?](#q6)
- [Q9: What is a CIDR block?](#q9)
- [Q10: What are VPC endpoints?](#q10)

---

## Q1: What is a VPC in A WS?

**Answer:**

A virtual private cloud (VPC) is a virtual network dedicated to your A WS account. It is logically isolated from other virtual networks in the A WS Cloud. Y ou can launch your A WS resources, such as Amazon EC2 instances, into your
VPC. Y ou can specify an IP address range for the VPC, add subnets (i.e. both public & private), associate NACLs (i.e. Network Access Contr ol Lists) & Security Gr oups and configure route tables .
You can create multiple VPCs within the same region or in dif ferent regions, in the same account or in dif ferent accounts. VPCs span multiple availability zones , which gives you High A vailability (i.e. HA) & Fault tolerance.

AWS regions, VPC and availability zones

---

## Q2: What are the various components of VPC?

**Answer:**

Various components of a VPC are shown below .
VPC components

---

## Q3: What is a subnet?

**Answer:**

A subnet is a range of IP addresses in your VPC. Y ou can launch A WS resources into a specified subnet. Use a public subnet for resources like a web server that must be connected to the internet, and a private subnet for resources
that won’ t be connected to the internet like application servers and database servers.

---

## Q4: What is a NA T instance in A WS?

**Answer:**

You can use a network address translation ( NAT) instance in a public subnet in your VPC to enable instances in the private subnet to initiate outbound IPv4 traf fic to the Internet or other A WS services, but prevent the instances fr om
receiving inbound traffic initiated by someone on the Internet .

Q. Why would a private subnet want to go out to the internet?
A. Apply patches to DB server , application server , etc.
As illustrated in the above diagram the main route table is associated with the private subnet and sends the traf fic from the instances in the private subnet to the NA T instance in the public subnet. The NA T instance sends the traf fic to the
Internet gateway for the VPC. The traf fic is attributed to the Elastic IP address of the NA T instance. The NA T instance specifies a high port number for the response; if a response comes back, the NA T instance sends it to an instance in the
private subnet based on the port number for the response.

---

## Q5: What is a VPN in A WS?

**Answer:**

You can connect your Amazon VPC to remote networks like your own on premise data centre over the public internet by using a VPN (i.e. V irtual Private Network) connection. On the A WS side of the VPN connection, a virtual private
gateway provides two VPN endpoints (tunnels) for automatic failover . You configure your customer gateway on the remote side of the VPN connection. The maximum throughput of a Site-to-Site VPN connection is typically less than 4.0
Gbps
You can also use AWS Dir ect Connect to create a dedicated private connection from a remote network to your VPC. A WS Direct Connect bypasses the public Internet and establishes a secure, dedicated connection from your infrastructure
into A WS. This dedicated connection occurs over a standard 1 GB or 10 GB Ethernet fiber -optic cable with one end of the cable connected to your router and the other to an A WS Direct Connect router .
AWS Direct Connect is a great option for businesses that are seeking secure, ultra-low latency connectivity into A WS. While provisioning A WS Direct Connect can sometimes be more involved, but it is worth the ef fort once the connectivity
is established as it has predictable network performance.

---

## Q6: What is a route table?

**Answer:**

A route table contains a set of rules, called routes, that are used to determine where network traf fic is directed. When you create a VPC, it automatically has a default main route table. Y our VPC can have route tables other than the default
table. One way to protect your VPC is to leave the main route table in its original default state (with only the local route), and explicitly associate each new subnet you create with one of the custom route tables you’ve created. This ensures
that you explicitly control how each subnet routes outbound traf fic.
Each route in a table specifies a destination CIDR (E.g. ) and a tar get.
Q7 What is A WS Security Groups?
A7. AWS of fers virtual firewalls via Security Gr oups . AWS security groups (SGs) are associated with EC2 instances and provide security at the protocol and port access level. Each security group – working much the same way as a
firewall – contains a set of rules that filter traf fic coming into and out of an EC2 instance.
Security groups do not deny traf fic, which means all the rules in security groups are positive, and allow traf fic. Whilst security group rules can be set to specify a traf fic source, or a destination, they cannot specify both on the same rule. A
single security group can be applied to multiple instances, or multiple security groups can be applied to a single instance.
The actual rule set that filters traf fic is made up of two tables: ‘Inbound’ and ‘Outbound’. A WS Security groups are stateful, meaning you do not need the same rules for both outbound traf fic and inbound. Therefore any rule that allows traf fic
into an EC2 instance, will allow responses to pass back out without an explicit rule in the Outbound rule set. Each rule is comprised of four fields: ‘T ype’, ‘Protocol’, ‘Port Range’, and ‘Source’.
Q8 What is a NACL (i.e. Network Access Control List)? How does it dif fer from the Security Groups?
A8. In addition to the security groups to further enrich its security filtering capabilities, A WS of fers a feature called Network Access Control Lists ( NACL s). Like security groups, each NACL is a list of rules, but there are two important
differences between NACLs and security groups. Security Groups are applied at the instance level.
Firstly , NACLs are not directly tied to EC2 instances, but are tied with the subnet within your A WS VPC. This means that the rules in a NACL apply to all of the EC2 instances within the subnet, in addition to all the rules from the security
groups.
Secondly , the NACLs support ‘ deny ’ rules to block traf fic from a particular set of IP addresses which are known to be compromised. The ability to write ‘deny’ actions is a crucial part of NACL functionality . When you have the ability to
write both ‘ allow ’ rules and ‘ deny ’ rules, the order of the rules becomes important. “Rule numbers’ are used within each NACLs to identify the correct order of the rules for your needs. Y ou can choose which traf fic you deny and which traf fic
you allow .

AWS inbound-outbound
Q. In what order are the rules applied?
A. For inbound traffic, A WS’s infrastructure first assesses the NACL rules. If traf fic gets through the NACL, then all the security groups that are associated with that specific instance are evaluated.
For outbound traffic , this order is reversed, where the traf fic is first evaluated against the security groups, and then finally against the NACL that is associated with the relevant subnet.

---

## Q9: What is a CIDR block?

**Answer:**

A system called Classless Inter -Domain Routing, or CIDR, was developed as an alternative to traditional subnetting. The idea is that you can add a specification in the IP address itself as to the number of significant bits that make up the
routing or networking portion.
The CIDR notation of 192.168.0.15/24 means that the first 24 bits (i.e. the first three octets) of the IP address are considered significant for the network routing. The last octet can have 255 ip addresses from 0 to 255.
There are various calculators and tools online that will help you understand some of these concepts and get the correct addresses and ranges that you need by typing in certain information.

---

## Q10: What are VPC endpoints?

**Answer:**

VPC endpoints enable you to create a private connection between your VPC and another A WS service that resides outside the VPC like Amazon S3 & DynamoDB without the need for Internet access.
If you wanted your EC2 instances in a VPC private subnet to be able to privately access resources outside VPC like S3 buckets & DynamoDB, you had to use an Internet Gateway , and potentially manage some NA T instances, which can be
expensive as you have to pay an hourly fee and a data processing fee.
VPC Endpoints simplify access to S3 resources from EC2 instances within a private subnet in VPC. These endpoints are easy to configure, highly reliable, and provide a secure connection to S3 that does not require a gateway or NA T
instances.
EC2 instances running in private subnets of a VPC can have controlled access to S3 buckets, objects, and API functions that are in the same region as the VPC using the endpoints. Y ou can use an S3 bucket policy to indicate which VPCs and
which VPC Endpoints have access to your S3 buckets.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
