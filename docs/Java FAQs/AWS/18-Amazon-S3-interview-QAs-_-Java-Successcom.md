# 18 Amazon S3 interview Q&As   Java Success.com

## Table of Contents

- [Q01: What is Amazon S3 storage?](#q01)
- [Q02: What is the max size of an object that you can upload to S3?](#q02)
- [Q03: When you upload a file, what properties can you set?](#q03)
- [Q05: What is the dif ference between S3 & EBS (i.e. Elastic Block Storage)?](#q05)
- [Q06: What is “Consistency” and explain eventual consistency model used by S3?](#q06)
- [Q07: Is it possible to get the ARN of an S3 bucket via the A WS command line?](#q07)
- [Q08: What are the dif ferent S3 storage classes?](#q08)
- [Q09: What SLA ’s are provided by Amazon S3?](#q09)
- [Q10: What are the dif ferent ways to transfer objects to/from Amazon S3?](#q10)
- [Q11: How will you go about downloading folders in S3 using A WS CLI?](#q11)
- [Q12: What is S3 replication?](#q12)
- [Q13: How do you list the objects of the A WS bucket via CLI?](#q13)
- [Q14: How do you protect your Amazon S3 buckets?](#q14)
- [Q15: How do you monitor your Amazon S3 buckets?](#q15)
- [Q16: How will you go about periodically deleting the logs or archives from Amazon S3 ](#q16)

---

## Q01: What is Amazon S3 storage?

**Answer:**

S3 stands for Simple Storage Service, which provides storage through web service interfaces. Unlike the other storage systems like Unix file system, HDFS (i.e. Hadoop Distributed File System), etc which are based on having folders
and files, the S3 is based on a concept of a “ key” and a “ object “. Amazon S3 stores data as objects within a bucket , which is a logical unit of storage. An object consists of a file and optionally any metadata that describes that file.

---

## Q02: What is the max size of an object that you can upload to S3?

**Answer:**

You can upload a file up to 5TB using a REST API PUT or POST .

---

## Q03: When you upload a file, what properties can you set?

**Answer:**

When you upload a file, you can set permissions on the object, object creation date, privacy classification like protected, sensitive, public etc as metadata.Metadata cannot be modified after uploading, but you can make a copy of the
object and set the new metadata.
Buckets are the containers and you control access per bucket, view access logs for it and its objects, and choose the geographical region where Amazon S3 will store the bucket and its contents. Customers are not char ged for creating buckets,
but are char ged for storing objects in a bucket and for transferring objects in and out of buckets. Amazon S3 data model is a flat structure, and there is no hierarchy of sub-buckets or sub-folders. Y ou can infer logical hierarchy using key name
prefixes and delimiters as Amazon S3 console supports a concept of folders for example:

---

## Q05: What is the dif ference between S3 & EBS (i.e. Elastic Block Storage)?

**Answer:**

EBS is a virtualized SAN (i.e. Storage Area Network). This means it is a RAID storage with fault tolerance as you don’ t lose data if the disk dies. It is also virtualized, so you can provision and allocate storage, and attach it to your EC2
instance with various API calls.
You can think of Amazon S3 like a ftp storage, where you can move files to and from there, but not mount it like a filesystem.
EBS uses S3, but both cannot be used interchangeably . You can’ t install applications in S3 as it is an object store. Y ou can upload & store files to S3. In EBS you can install applications. Y ou can attach EBS volume to EC2 Linux Instances.
Performance wise A WS S3 is fast, but A WS EBS is super fast. S3 is accessed via internet, and EBS is accessed from within the VPC. So, EBS is more local to the data.
AWS S3 has better redundancy as the data is replicated across availability zones (i.e. across the data centres), and in A WS EBS the data is replicated within the availability zone (i.e. within the data centre).
S3 can be made public or private, but EBS can be accessed only attached to an EC2 instance.

---

## Q06: What is “Consistency” and explain eventual consistency model used by S3?

**Answer:**

Consistency is a key concept in data storage where when changes are committed to a system those changes must be visible to all participants. SQL transactional databases employ various levels of consistencies like read-after -write, read-
after-update, read-after -delete, etc. The typical standard is that after a transaction commits the changes are guaranteed to be visible to all participants without any time lag .
Unlike SQL, the NoSQL databases relax the rules a bit, allowing a time lag between the point the data is committed to storage and the point where it is visible to all others. This is known as “Eventual Consistency Model”. Like other NoSQL
models, S3 uses “ Eventual Consistency Model “. A change committed at “millisecond 1” might be visible to all immediately or might not be visible until “millisecond 200” or “millisecond 1000”. But eventually the data will be visible.

---

## Q07: Is it possible to get the ARN of an S3 bucket via the A WS command line?

**Answer:**

Yes. If you know the name of the bucket, you know the ARN. No need to ‘get’ it from anywhere. ARN stands for Amazon Resource Name, which takes the format
In the example below , service is “s3”, region is empty , and account-id is empty , and resource is the “NAME-OF-YOUR-S3-BUCKET”

---

## Q08: What are the dif ferent S3 storage classes?

**Answer:**

Amazon S3 comes in 3 storage classes
1) S3 Standard: Suitable for frequent access with low latency & high throughput delivery . Targets dynamic websites & Big Data workloads.
2) S3 Infr equent Access: offers lower storage cost for data that that is needed less often, but quickly accessible. T argets backups, disaster recovery , and long term storage.
3) Amazon Glacier: is the least expensive, and strictly designed for archival use cases as it takes longer to access the data. It of fers various retrieval rates from minutes to hours.
Amazon S3 also of fers capabilities to manage your data throughout its lifecycle. Once an S3 Lifecycle policy: is set, your data will automatically transfer to a dif ferent storage class without any changes to your application.

---

## Q09: What SLA ’s are provided by Amazon S3?

**Answer:**

SLAs like availability (i.e. system up time) & durability (i.e. data is never lost or compromised) are key to designing applications that rely on S3 as a storage mechanism. Durability guarantees that the data gets written will survivearn:partition :service :region :account -id:resource -id
arn:partition :service :region :account -id:resource -type/resource -id
arn:partition :service :region :account -id:resource -type:resource -id
arn:aws:s3:::NAME -OF-YOUR -S3-BUCKET

permanently . For example, if a customer spending habits reports are successfully written to the disk, then the data will remain in the disk even if the system crashes.
S3 claims 99.999999999% durability of objects across multiple Availability Zones and 99.99% availability .
S3’s cross-r egion r eplication feature can be used for disaster recovery & enhances its strong availability by withstanding the complete outage of an A WS region.
S3 has easy-to-configure audit logging and access control capabilities. These features along with multiple types of encryption makes S3 easy to meet regulatory compliance needs such as PCI (i.e. Payment Card Industry) or HIP AA (i.e. Health
Insurance Portability and Accountability Act) compliance.

---

## Q10: What are the dif ferent ways to transfer objects to/from Amazon S3?

**Answer:**

1) Public internet: via S3 APIs using A WS console, A WS SDK or A WS CLI. By default, the A WS CLI sends requests to A WS services by using HTTPS on TCP port 443.
If you are using an instance from a private subnet within a VPC, then use VPC endpoints for a private network between your instance and Amazon S3, which resides outside VPC, but within the same region. Here is an example where Athena
within VPC accessing S3 via a VPC Endpoint.
Instance in VPC to S3 via VPC Endpoints
2) Amazon S3 T ransfer Acceleration (S3T A): can speed up content transfers to and from Amazon S3 for long-distance transfer of lar ger objects. S3T A reduces the variability in Internet routing, congestion and speeds that can af fect
transfers, and logically shortens the distance to S3 for remote applications. S3T A improves transfer performance by routing traf fic through Amazon CloudFr ont’s globally distributed edge nodes and over A WS backbone networks, and by
using network protocol optimisations. Y ou can turn on S3T A with a few clicks in the S3 console, and you pay only for transfers that are accelerated.
3) AWS Dir ect Connect: for a private and consistent connection between S3 and your data centre.
4) AWS Snowball: is a petabyte scale data transport solution that uses devices designed to be secure to transfer lar ge amounts of data into and out of the A WS Cloud. Using Snowball addresses common challenges with lar ge-scale data
transfers including high network costs, long transfer times, and security concerns.
You don’ t need to write any code or purchase any hardware to transfer your data. Simply create a job in the A WS Management Console (“Console”) and a Snowball device will be automatically shipped to you.

---

## Q11: How will you go about downloading folders in S3 using A WS CLI?

**Answer:**

Using “aws s3 cp” or “aws s3 sync” as shown below .

OR
By default, the aws s3 sync command will copy a whole directory . It will only copy new & modified objets.

---

## Q12: What is S3 replication?

**Answer:**

Replication enables automatic, asynchronous copying of objects across Amazon S3 buckets. Buckets that are configured for object replication can be owned by the same A WS account or by dif ferent accounts. Y ou can copy objects
between dif ferent A WS Regions or within the same Region.

---

## Q13: How do you list the objects of the A WS bucket via CLI?

**Answer:**



---

## Q14: How do you protect your Amazon S3 buckets?

**Answer:**

1) IAM user policies specify user access to specific S3 buckets or objects. IAM policies provide a programmatic way to manage Amazon S3 permissions for multiple users.
2) Bucket policies specify access to specific buckets & objects. Y ou can use a deny statement in a bucket policy to restrict access to specific IAM users, even if the users are granted access in an IAM policy .
3) Contr ol Lists (ACLs) on your buckets and objects. If you need a programmatic way to manage permissions, use IAM policies or bucket policies instead of ACLs.
4) Encrypting the objects using HTTPS during transmission, and if your use case requires encryption for data at rest, Amazon S3 of fers server -side encryption (SSE). The SSE options include SSE-S3, SSE-KMS or SSE-C. Client-side
encryption is the act of encrypting data before sending it to Amazon S3 using A WS KMS (i.e. Key Management Service).

---

## Q15: How do you monitor your Amazon S3 buckets?

**Answer:**

1) AWS CloudT rail logs monitors your buckets by default. If you want to monitor the objects as well then configure Amazon S3 data events .
2) Enabling Amazon S3 server access logging .

---

## Q16: How will you go about periodically deleting the logs or archives from Amazon S3 bucket?

**Answer:**

You can define lifecycle configuration rules for objects that have a well-defined lifecycle. Y ou can also use lifecycle events for objects that are frequently accessed for only a limited time.$ aws s3 cp s3://myBucket/dir localdir --recursive
$ aws s3 sync s3://mybucket/dir localdir
$ aws s3 ls --recursive

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
