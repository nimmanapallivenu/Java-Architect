# 02  AWS Identity & Access Management (i.e. IAM) interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is IAM?](#q1)
- [Q2: What are the key components of IAM?](#q2)
- [Q3: What are some of the IAM best practices?](#q3)
- [Q4: If an EC2 resource wants to have access to an A WS S3 bucket, can you assign pol](#q4)
- [Q5: What is the purpose of groups?](#q5)
- [Q6: If you need to have a S3 bucket to serve static assets for public-facing web app](#q6)
- [Q7: If you want to leverage your A WS S3 with “Glacier” class for archival with a th](#q7)
- [Q8: If you engaged an external consultancy to set up your A WS infrastructure, and t](#q8)

---

## Q1: What is IAM?

**Answer:**

IAM stands for Identity & Access Management. A WS Identity and Access Management (IAM) is a web service that helps you securely control access to A WS resources. Y ou use IAM to control who is authenticated (i.e. signed in)
and authorized (i.e. has permissions) to access resources like EC2, S3, RDS etc.

AWS – IAM

---

## Q2: What are the key components of IAM?

**Answer:**

IAM User : is a person or service that will interact with A WS. A user can sign into A WS Management Console for performing tasks in A WS.
IAM Gr oup: is a collection of IAM users. Y ou can specify permissions to an IAM Group.
IAM Role : is an identity to which you give permissions. A Role does not have any credentials as in password or access keys. Y ou can temporarily give an IAM Role to an IAM User to perform certain tasks in A WS.
IAM Permission : to access or perform an action on an A WS Resource and assign it to a User , Role or Group. Y ou can create two types of Permissions – 1) Identity based and 2) Resource based. Y ou can also create Permissions on resources
like S3 bucket, Glacier vault, etc to specify who can access the resources.
IAM Policy : is a JSON document in which you list permissions to specify Actions, Resources and Ef fects. Y ou can attach a policy to an IAM User , Group or Role.

---

## Q3: What are some of the IAM best practices?

**Answer:**

Firstly , root user who creates an A WS account with an email address & credit card details needs to enable the “ MFA” (i.e. Multi-Factor Authentication) for the added security . You can use the “Google Authenticator” for the MF A, which
generates a 6 digit code to be entered in addition to the username/password.
Secondly , don’ t use the root user to access A WS resources like EC2, S3, RDS, etc. Create new users and assign policies (aka permissions) to access the resources as depicted above.

---

## Q4: If an EC2 resource wants to have access to an A WS S3 bucket, can you assign policies to a resource?

**Answer:**

No. Y ou cannot assign policies directly to any resources like EC2, etc. Y ou need to firstly create a “ Role “, and attach that role a resource. Finally , you can assign policies to that role.
Policies can only be assigned to Users , Groups & Roles .

---

## Q5: What is the purpose of groups?

**Answer:**

If your or ganisation has say 100+ users, it will be cumbersome to add and maintain individual policies to each user . You can create a new group say “Dev”, and assign policies to that group. When new users are join or leave the
organisation, they can be easily added to or removed from that group.

---

## Q6: If you need to have a S3 bucket to serve static assets for public-facing web application, which approach will ensure that all objects uploaded to the bucket are set to public read?
a) Configure the bucket policy to set all objects to public read.
b) Use A WS IAM roles to set the bucket to public read.

**Answer:**

The answers is a). Rather than making changes to every object, its better to set the policy for the whole bucket to public read. IAM is used to give more granular permissions .

---

## Q7: If you want to leverage your A WS S3 with “Glacier” class for archival with a third-party software, what approach will you use to limit the access of the third party software to only the Amazon S3 bucket named “myor g-backup”?

**Answer:**

Create a custom IAM user policy limited to the Amazon S3 API in “myor g-backup”. This is a typical use case to grant granular access.

---

## Q8: If you engaged an external consultancy to set up your A WS infrastructure, and they created an IAM group called “devops” and add their team to that group. After the team finishes setting your infrastructure up, they leave your project.
What actions should you take?

**Answer:**

Just remove the users from the “devops” group and delete the user accounts, but keep the IAM group “devops”. In the future, you may want to hire permanent staf f to modify & maintain your infrastructure, and you can add the new users
to this “devops” group, which already has policies attached to be reused.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
