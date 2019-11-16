#### Identity Access Management (IAM) ####
* Allows you to manage users and their level of access to the AWS console.
* What does IAM give you?
  * Centralized control of your AWS account
  * Shared Access to your AWS account
  * Granular Permissions
    * You can enable different levels of access to different users within your organization
  * Identity Federation
    * Active Directory, Facebook, Linkedin and etc
  * Multifactor Authentication
    * Multiple independent authentication mechanisms
    * username & password + software token site
  * Provides temporary access for users / devices and services, as necessary
  * Allows you to set up your own password rotation policy
  * Integrates with many different AWS services
  * Supports Payment Card Industry Data Security Standard (PCI DSS) compliance
* Critical Terms
<table>
    <tbody>
        <tr>
            <td>Users</td>
            <td>End Users</td>
        </tr>
        <tr>
            <td>Group</td>
            <td>A collection of users under one set of permissions</td>
        </tr>
        <tr>
            <td>Roles</td>
            <td>You create roles and can then assign them to AWS resources</td>
        </tr>
        <tr>
            <td>Policies</td>
            <td>A document that defines one (or more) permissions can be attacged to either a user group or role</td>
        </tr>
    </tbody>
</table>

---

#### EC2 ####
* Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud
* Allowing users to rent virtual computers on which to run their own computer applications in the cloud
<table>
    <tbody>
        <tr>
            <th>EC2 Option</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>On Demand</td>
            <td>Allows you to pay a <strong>fixed rate</strong> by the hour (or by the second) with no commitment</td>
        </tr>
        <tr>
            <td>Reserved</td>
            <td>Provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. 1 Year or 3 Year <strong>Terms</strong></td>
        </tr>
        <tr>
            <td>Spot</td>
            <td>Enables you to <strong>bid</strong> whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times</td>
        </tr>
        <tr>
            <td>Dedicated Hosts</td>
            <td><strong>Physical EC2 server dedicated</strong> for your use (or regulatory). Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses (SQL Server, VMware and etc)</td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>EC2 Option</th>
            <th>Good for</th>
        </tr>
        <tr>
            <td>On Demand</td>
            <td>
                <ul>
                    <li>Perfect for users that want the <strong>low cost and flexibility</strong> of Amazon EC2 <strong>without any up-front payment or long term commitment</strong></li>
                    <li>Applications with short term, spiky, or unpredictable workloads that cannot be interrupted</li>
                    <li>Applications being developed or tested on Amazon EC2 for the first time</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Reserved Instances</td>
            <td>
                <ul>
                    <li>Applications with steady state or <strong>predictable usage</strong></li>
                    <li>Application that <strong>require reserved capacity</strong></li>
                    <li>
                        Users can make up-front payments to reduce their total computing costs (DISCOUNT) even further
                        <ul>
                            <li>Standard RIs (Up to 75% off on-demand cost)</li>
                            <li>Convertible RIs (Up to 4% off on-demand cost) feature the capability to change the arributes of the RI as long as the exchange results in the creation of Reserved Instances of equal or greater value (could go from a very CPU intensive instance over to a very memory intensive instance)</li>
                            <li>Scheduled RIs are available to launch within the time window you reserve. This option allows you to match your capacity reservation to a predictable recurring schedule that only requires a fraction of a day, a week, or a month</li>
                        </ul>
                    </li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Sport Instances</td>
            <td>
                <ul>
                    <li>Applications that have <strong>flexible start and end times</strong></li>
                    <li>Applications that are only feasible at very low compute prices (Chemical companies use this to do huge amounts of computing (at 4 AM on a Sunday that will save they an awful lot of money)</li>
                    <li>
                        Users with an <strong>urgent need for large amounts of additional computing capacity</strong>
                    </li>
                    <li><strong></strong>If the instance is terminated by Amazon EC2, you will not be charged for a partial hour of usage. However, if you terminate the instance yourself, you will be charged for the complete hour in which the instance ran</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Dedicated Hosts</td>
            <td>
                <ul>
                    <li><strong>Useful for regulatory requirements that may not support multi-tenant virtualization</strong></li>
                    <li>Multi-tenancy is an architecture in which a single instance of a software application serves multiple customers</li>
                    <li>Great for licensing which does not support multi-tenancy or cloud deployments</li>
                    <li>Can be purchased On-Demand (hourly)</li>
                    <li>Can be purchased as a Reservation for up to 70% off the On-Demand price</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* EC2 Instance Types
<table>
    <tbody>
        <tr>
            <th>Family</th>
            <th>Speciality</th>
            <th>Use case</th>
        </tr>
        <tr>
            <td>F1</td>
            <td>Field Programmble Gate Array</td>
            <td>Genomics research, financial analytics, real-time video processing, big data etc</td>
        </tr>
        <tr>
            <td>I3</td>
            <td>High Speed Storage</td>
            <td>NoSQL DBs, Data Warehousing etc</td>
        </tr>
        <tr>
            <td>G3</td>
            <td>Graphics Intensive</td>
            <td>Video Encoding / 3D Application Streaming</td>
        </tr>
        <tr>
            <td>H1</td>
            <td>High Disk Throughput</td>
            <td>MapReduce-based workloads, distrubuted file systems such as HDFS and MapR-FS</td>
        </tr>
        <tr>
            <td>T2</td>
            <td>Lowest Cost, General Purpose</td>
            <td>Web Servers / Small DBs</td>
        </tr>
        <tr>
            <td>D2</td>
            <td>Dense Storage</td>
            <td>Fileservers / Data Warehousing / Hadoop</td>
        </tr>
        <tr>
            <td>R4</td>
            <td>Memory Optimized</td>
            <td>Memory Intensive Apps / DBs</td>
        </tr>
        <tr>
            <td>M5</td>
            <td>General Purpose</td>
            <td>Application Servers</td>
        </tr>
        <tr>
            <td>C5</td>
            <td>Compute Optimized</td>
            <td>CPU Intensive Apps / DBs</td>
        </tr>
        <tr>
            <td>P3</td>
            <td>Graphics / General Purpose GPU</td>
            <td>Machine Learning, BitCoin Mining etc</td>
        </tr>
        <tr>
            <td>X1</td>
            <td>Memory Optimized</td>
            <td>SAP HANA / Apache Spark etc</td>
        </tr>
    </tbody>
</table>

* What is EBS?
  * A virtual disk in the cloud
  * Elastic Block Store (EBS) allows you to create storage volumes and attach them to Amazon EC2 instances
* EBS Volume Types
<table>
    <tbody>
        <tr>
            <th>Volume Type</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>General Purpose SSD (GP2)</td>
            <td>
                <ul>
                    <li>General purpose, balances both price and performance</li>
                    <li>Ratio of 3 IOPS per GB with up to 10,000 IOPS and the ability to burst up to 3000 IOPS for extended periods of time for volumes at 3334 GiB and above</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Provisioned IOPS SSD (IO1)</td>
            <td>
                <ul>
                    <li>Designed for I/O intensive applications such as large relational or NoSQL databases</li>
                    <li>Use if you need more than 10,000 IOPS</li>
                    <li>Can provision up to 20,000 IOPS per volume</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Throughput Optimized HDD (ST1)</td>
            <td>
                <ul>
                    <li>Big data</li>
                    <li>Data warehouses</li>
                    <li>Log processing</li>
                    <li>Cannot be a boot volume</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Cold HDD (SC1)</td>
            <td>
                <ul>
                    <li>Lowest cost storage for infrequently accessed workloads</li>
                    <li>File Server</li>
                    <li>Cannot be a boot volume</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Magnetic (Standard)</td>
            <td>
                <ul>
                    <li>Lowest cost per gigabyte of all EBS volume types that is <strong>bootable</strong>. Magnetic volumes are ideal for workloads (test / dev environments) where data is accessed infrequently, and applications where the lowest storage cost is important</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* Use putty for SSH
  * use puTTY Key Generator generate new .ppk file
  * ec2-user@x.x.x.x as hostname

* Elastic Load Balancers
  * Helps us balance our load across multiple different servers
<table>
    <tbody>
        <tr>
            <th>Load Balancer Type</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Application Load Balancer</td>
            <td>
                <ul>
                    <li>Best suited for loading HTTP and HTTPS traffic</li>
                    <li>They operate at Layer 7 and are application aware. They are intelligent, and you can create advanced request routing, sending specified requests to specific web servers</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Network Load Balancer (Use for <strong>extreme performance</strong>)</td>
            <td>
                <ul>
                    <li>Best suited for load balancing TCP traffic where extreme performance is required</li>
                    <li>Operating at the connection level (Layer 4), Network Load Balancer are capable of handling millions of requests per second, while maintaining ultra-low latencies</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Classic Load Balancer</td>
            <td>
                <ul>
                    <li>Are the legacy Elastic Load Balancers</li>
                    <li>You can load balance HTTP/ HTTPS applications and use Layer 7 specific features such as X-Forwarded and sticky sessions. You can also use strict Layer 4 load balancing for applications that rely purely on the TCP protocol</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* Load Balancer Errors
  * <strong>504 error means the gateway has timed out</strong>. This means that the application not responding within the idle timeout period. Trouble shoot the application. Is is the Web Server or Database Server?
* If you <strong>need the IPv4 address of your end user</strong>, look for the <strong>X-Forwarded-For Header</strong>
* Route 53 (<strong>buy domain name</strong>) is Amazon's DNS service
 * Allows you to map your domain names to
   * EC2 instances
   * Load Balancers
   * S3 Buckets
* Prefer roles than secret access key id's and secret access keys

* Amazon Relational Database Service (RDS)
  * Relational Database Types
    * SQL Server
    * Oracle
    * MySQL Server
    * PostgreSQL
    * Aurora
    * MariaDB

  * Non Relational Databases
    * Database
    * Collection = Table
    * Document = Row
    * Key Value Pairs = Fields

  * What is Data Warehousing
    * Used for business intelligence. Tools like Cognos, Jaspersoft, SQL Server Reporting Services, Oracle Hyperion and SAP NetWeaver
    * Used to pull in very large and complex data sets. Usually used by management to do queries on data

  * OLTP VS OLAP
    * https://techdifferences.com/difference-between-oltp-and-olap.html

* What is Elasticache
  * The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.
  * Supports 2 open-source in-memory caching engines:
    * Memcached
    * Redis

* RDS - Back Ups, Multi-AZ & Read Replicas
<table>
    <tbody>
        <tr>
            <th>Backup Type</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Automated Backups</td>
            <td>
                <ul>
                    <li>Allow you to recover your database to any point in time within a "retention period" (can be between 1 and 35 days)</li>
                    <li>Will take a full daily snapshot and will also store transaction logs throughout the day</li>
                    <li>When you do a recovery, AWS will first choose the most recent daily backup, and then apply transaction logs relevant to that day</li>
                    <li>Enabled by default</li>
                    <li>The backup data is stored in S3 and you get free storage space equal to the size of your database</li>
                    <li>The backup data will be deleted when you delete the RDS instance</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Snapshots</td>
            <td>
                <ul>
                    <li>Are done manually</li>
                    <li>They are stored even after you delete the original RDS instance, unlike automated backups</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* Whenever you restore either an Automatic Backup or manual Snapshot, the restored version of the database will be a new RDS instance with a new DNS endpoint

* Encryption
  * Supported for MySQL, Oracle, SQL Server, PostgreSQL, MariaDB & Aurora
  * Is done using AWS Key Management Service (KMS) service
  * Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as are its automated backups, read replicas, and snapshots
  * At the present time, encrypting an existing DB instance is not supported. To use Amazon RDS encryption for an existing database, you must first create a snapshot, make a copy of that snapshot and encrypt the copy

<table>
    <tbody>
        <tr>
            <td>Multi-AZ RDS (for Disaster Recovery only)</td>
            <td>
                <ul>
                    <li>Allows you to have an exact copy of your production database in another availability zone</li>
                    <li>AWS handles the replication for you, so when your production database is written to, this write will automatically be synchronized to the stand by database</li>
                    <li>In the event of planned database maintenance, DB instance failure, or an Availability Zone failure, Amazon RDS will automatically failover (故障转移) to the standby so that database operations can resume quickly without administrative intervention</li>
                    <li>Supported for MySQL, Oracle, SQL Server, PostgreSQL, MariaDB</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Read Replica (for Scaling, Performance)</td>
            <td>
                <ul>
                    <li>Allow you to have a read-only copy of your production database (up to 5 copies)</li>
                    <li>Achieved by using Asynchronous replication from the primary RDS instance to the read replica</li>
                    <li>Use read replicas primarily for very read-heavy database workloads</li>
                    <li>Supported for MySQL, PostgreSQL, MariaDB, Aurora</li>
                    <li>Must have automatic backups turned on in order to deploy a read replica</li>
                    <li>Can have read replicas of read replicas (Watch out for latency)</li>
                    <li>Each read replica will have its own DNS end point</li>
                    <li>Can have read replicas that have Multi-AZ</li>
                    <li>Can create read replicas of Multi-AZ source databases</li>
                    <li>Can promoted to their own databases. This breaks the replication</li>
                    <li>Can have a read replica ina second region</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* What is Elasticache?
  * Is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases
    * E.g. Always querying the top 10 for sale items
  * Can be used to significantly improve latency and throughput for many read-heavy application workloads
    * social networking
    * gaming
    * media sharing
    * Q & A portals
    * compute intensive workloads (recommendation engine)
  * Caching improves application performance by storing critical pieces of data in memory for low-latency access. Cached information may include the results of I/O-intensive database quries or theresults of computationally intensive calculations

<table>
    <tbody>
        <tr>
            <th>Types of Elasticache</th>
            <th>Description</th>
            <th>Use cases</th>
        </tr>
        <tr>
            <td>Memcached</td>
            <td>
                <ul>
                    <li>ElastiCache is protocol compliant with Memcached</li>
                    <li>Designed as a pure caching solution with no persistence, ElastiCache manages Memcached nodes as a pool that can grow and shrink similar to an Amazon EC2 Auto Scaling Group. Individual nodes are expendable, and ElastiCache provides additional capabilities here, such as automatic node replacement and Auto Discovery</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Is object caching your primary goal, for example to offload your database?</li>
                    <li>Are you interested in as simple a caching model as possible?</li>
                    <li>Are you planning on running large cache nodes, and require multithreaded performance with utilization of multiple core?</li>
                    <li>Do you want the ability to scale your cache horizontally as you grow?</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Redis</td>
            <td>
                <ul>
                    <li>A popular open-source in-memory key-value store that supports data structures such as sorted sets and lists</li>
                    <li>ElastiCache supports Master / Slave replication and Multi-AZ which can be used to achieve cross AZ redundancy</li>
                    <li>The replication of persistence features of Redis, ElastiCache manages Redis more as a relational database. Redis ElastiCache clusters are amanged as stateful entities that include failover, similar to how Amazon RDS manages database failover</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Are you looking for more advanced data types, such as lists, hashes, and sets?</li>
                    <li>Does sorting and ranking datasets in memory help you, such as with leaderboards?</li>
                    <li>Is persistence of your key store important?</li>
                    <li>Do you want to run in multiple AWS Availability Zones (Multi-AZ) with failover?</li>
                    <li>Pub/Sub capabilities are needed</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

---

#### S3 ####
* Provides developers and IT teams with secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web services interface to store and retrieve any amount of data from anywhere on the web
  * Is a safe place to store your files (Can be from 0 bytes to 5TB)
  * It is object-based storage (E.g. allows you to upload files)
  * The data is spread across multiple devices and facilities
  * There is unlimited storage
  * Files are stored in Buckets (similar to a folder)
  * S3 is a universal namespace. That is, names must be unique globally
    * https://s3-eu-west-1.amazonaws.com/acloudguru (acloudguru is name of the bucket)
  * When you upload a file to S3, you will receive a HTTP 200 code if the upload was successful
* Data consistency model for S3
  * Read after write consistency for PUTS of new Objects
    * You <strong>can access your file / object as soon as you have uploaded / added it into S3</strong>
* Eventual consistency for overwrite PUTS and DELETES (<strong>can take some time to propagate</strong>)
* S3 is a object based / simple key value store
* Key (name of the object)
* Value (the data, which is made of a sequence of bytes)
* Version ID (Can use to rollback previous version)
* Medatadata (Can add your own message)
* Subresources (Bucket specific configuration)
* Bucket polices, access control lists
* Cross origin resources sharing (CORS)
* Transfer acceleration (加速) (Accelerate file transfer speeds when uploading lots of files into S3)

<table>
    <tbody>
        <tr>
            <th>Storage Tiers / Classes</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>S3</td>
            <td>Durable, immediately available, frequently accessed</td>
        </tr>
        <tr>
            <td>S3 - IA</td>
            <td>Durable, immediately available, infrequently accessed</td>
        </tr>
        <tr>
            <td>S3 - One Zone IA</td>
            <td>Same as IA. However, data is stored in a single availability zone only</td>
        </tr>
        <tr>
            <td>Reduced Redundancy Storage</td>
            <td>Data that is reproducible, such as thumbnails, etc</td>
        </tr>
        <tr>
            <td>Glacier</td>
            <td>Archived data, where you can wait 3 - 5 hours before accessing</td>
        </tr>
    </tbody>
</table>

* S3 Security
  * By default, all newly created buckets are PRIVATE
  * You can set up access control to your buckets using:
    * Bucket polices - Applied at a bucket level
    * Access control lists - Applied at an object level
  * S3 buckets can be configured to create access logs, which log all requests made to the S3 bucket. These logs can be written to another bucket

* S3 Encryption
  * If you want to enfore the use of encryption for your files stored in S3, use an S3 Bucket Policy to deny all PUT requests that don't include the x-amz-server-side-encryption.parameter in thr request header

<table>
    <tbody>
        <tr>
            <th>Encryption</th>
            <th colspan="2">Description</th>
        </tr>
        <tr>
            <td>Encryption In-Transit</td>
            <td colspan="2">
                SSl/TLS (HTTPS)
            </td>
        </tr>
        <tr>
            <td>Encryption At Rest</td>
            <td>
                Server Side Encryption
                <ul>
                    <li>SSE-S3</li>
                    <li>SSE-KMS</li>
                    <li>SSE-C</li>
                </ul>
            </td>
            <td>
                Client Side Encryption
            </td>
        </tr>
    </tbody>
</table>

* Use S3 to host static website
  * Need to configure CORS in order to load the page in another bucket

* CloudFront (CDN)
  * A content devliery network (CDN) is a system of distributed servers (network) that deliver webpages and other web content to a user based on the geographic locations of the user, the origin of the webpage, and content delivery server
    * Can be used into S3 (S3 transfer acceleration)

![edge-location](https://user-images.githubusercontent.com/5309726/67011357-ffd6e900-f121-11e9-82a1-36a29ad3b3a7.png)

<table>
    <tbody>
        <tr>
            <th>Key Terminology</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Edge location</td>
            <td>The location where content is cached and can also be written</td>
        </tr>
        <tr>
            <td>Origin</td>
            <td>The origin of all the files that the CDN will distribute. Origins can be an S3 Bucket, an EC2 Instance, an Elastic Load Balacer, or Route53</td>
        </tr>
        <tr>
            <td>Distribution</td>
            <td>The name given the CDN, which consist of a collection of Edge Locations</td>
        </tr>
        <tr>
            <td>Real time messaging protocol (RTMP)</td>
            <td>Used for media streaming</td>
        </tr>
    </tbody>
</table>

![s3-upload](https://user-images.githubusercontent.com/5309726/67396616-7c623f80-f5da-11e9-8a68-2e417542cec4.png)

* Restrict Viewer Access (Use Signed URLs or Signed Cookies)
  * E.g. restricting that only to users who have paid for the content

* S3 Performance Optimization
  * Is designed to support very high request rates.
  * However, if your S3 buckets are routinely receiving > 100 PUT/LIST/DELETE or > 300 GET requests per second, then there are some best pratice guidlines that will help optimize S3 performance

<table>
    <tbody>
        <tr>
            <th>Type of Workload</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>GET intensive workloads</td>
            <td>Use CloudFront CDN to get best performance. CloudFront will cache your most frequently accessed objects and will reduce latency for your GET requests</td>
        </tr>
        <tr>
            <td>Mixed request type workloads</td>
            <td>
                <ul>
                    <li>A mix of GET, PUT, DELETE, GET bucket, the key names you use for your objects can impact performance for intensive workloads</li>
                    <li>S3 uses the key name to determine which partition an object will be stored in</li>
                    <li>The use of sequential key names, E.g. names prefixed with a time stamp / alphabetical sequence increase the likelihood of having multiple objects stored on the same partition. For heavy workloads this can cause I/O issues and contention</li>
                    <li>By using a random prefix to key names, can force S3 to distribute your keys across multiple partitions, distributing the I/O workload</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* In July 2018, Amazon announced a massive increase in S3 performance,
  * 3500 put requests per second
  * 5500 get requests
* This means logical and sequential naming patterns can now be used without any performance implication

---

#### What is Lambda? ####
* Is a compute service where you can upload your code and create a Lambda function.
* AWS Lambda takes care of provisioning and managing the servers that you use to run the code. You don't have to worry about operating system, patching, scaling, etc.
* You can use Lambda in the following ways,

<table>
    <tbody>
        <tr>
            <th></th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Event driven compute service</td>
            <td>AWS Lambda runs your code in response to events. These events could be changes to data in an Amazon S3 bucket or an Amazon DynamoDB table</td>
        </tr>
        <tr>
            <td>Compute service</td>
            <td>Run your code in response to HTTP requests using AmazonAPI Gateway or API calls made using AWS SDKs</td>
        </tr>
    </tbody>
</table>

* How is Lambda priced?
<table>
    <tbody>
        <tr>
            <th></th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Number of requests</td>
            <td>First 1 million requests are free. $0.2 per 1 million requests thereafter</td>
        </tr>
        <tr>
            <td>Duration</td>
            <td>Is calculated from the time your code begins executing until it return or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB/second used</td>
        </tr>
    </tbody>
</table>

* Use AWS X-ray allows you debug what is happening

* What is API Gateway?
  * Is a fully managed service that makes it easy for developers to publish, maintain, monitor and secrure APIs at any scale.
  * Create an API that acts as a "front door" for applications to access data, business logic or functionality from your back-end services, such as applications running on ES2

* What can API Gateway do?
  * Expose HTTPS endpoints to define a RESTful API
  * Serverless-ly connect to services like Lambda & DynamoDB
  * Send each API endpoint to a different target
  * Run efficiently with low cost
  * Scale effortlessly
  * Track and control usage by API key
  * Throttle requests to prevent attack
  * Connect to CloudWatch to log all requests for monitoring
  * Maintain multiple versions of your API

* What is API Caching?
  * Can enable API caching in Amazon API Gateway to cache your endpoint's response. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of the requests to your API.
  * When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time to live (TTL) period, in seconds
  * API Gateway then responds to the request by looking up the endpoint response from the cache instead of making request to you endpoint

* Cross origin resource sharing (CORS)
  * If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
  * CORS is enforced by the client

* Version control with Lambda
  * Can have multiple versions of lambda functions
  * Latest version will use $latest
  * Qualified version will use $latest, unqualified will not have it
  * Versions are cannot be changed
  * Can split traffic using aliases to different versions
    * Cannot split traffic with $latest, instread create an alias to latest

* Step Functions (Workflows)
  * Great way to visualize your serverless application
  * Automatically triggers and tracks each step
  * Logs the state of each step so if something goes wrong you can track what went wrong and where

* What is X-Ray?
  * Is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization
  * Integrates with the following AWS services:
    * Elastic Load Balancing
    * AWS Lambda
    * Amazon API Gateway
    * Amazon Elastic Compute Cloud
    * AWS Elastic Beanstalk
  * Integrates with the following languages:
    * Java
    * Go
    * Node.js
    * Python
    * Ruby
    * .Net

* Advanced API Gateway
  * Can import Swagger v2.0 definition files

* API Throttling
  * By default, API Gateway limites the steady state request rate to 10,000 requests per second (rps)
  * The maximum concurrent requests is 5000 requests across all APIs within an AWS account
  * If you go over 10,000 reuqests per second or 5000 concurrent requests you will receive a 429 Too Many Request error response

<table>
    <tbody>
        <tr>
            <th>No</th>
            <th>Scenario</th>
        </tr>
        <tr>
            <td>1</td>
            <td>If a caller submits 10,000 requests in a one second period evenly (for example 10 requests every millisecond), API Gateway processes all requests without dropping any</td>
        </tr>
        <tr>
            <td>2</td>
            <td>If the caller sends 10,000 requests in the first millisecond, API Gateway serves 5,000 of those requests and throttles the rest in the one-second period</td>
        </tr>
        <tr>
            <td>3</td>
            <td>if the caller submits 5,000 requests in the first millisecond and then evenly spreads another 5,000 requests through the remaining 999 milliseconds (for example, about 5 requests in the one-second period without returning 429 Too Many Requests error responses)</td>
        </tr>
    </tbody>
</table>

  * Can configure API Gateway as a SOAP web service passthrough

---

#### Dynamo DB ####
* Is a low latency NoSQL database
  * Consists of tables items and attributes
  * Supports both document and key value data models
  * Supported document formats are JSON, HTML, XML
  * Stored on SSD storage
  * Spread across 3 geographically distinct data centers

<table>
    <tbody>
        <tr>
            <th>Consistency Model</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
        <tr>
            <td>Eventually (Best Read Performance)</td>
            <td>When you read data from a DynamoDB table, the response might not reflect the results of a recently completed write operation. The response might include some stale data. If you repeat your read request after a short time, the response should return the latest data</td>
            <td>If you are saving high score, if you use it fetch picture for caching website or if you are running any scenario where getting the former record value instead of the latest (remember we are speaking about 1 second delay) then eventual consistency will grant you scalability and better performance</td>
        </tr>
        <tr>
            <td>Strongly</td>
            <td>When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. A strongly consistent read might not be available if there is a network delay or outage</td>
            <td>If you run financial algorithm of life critical systems or booking system you should favor string consistency. </td>
        </tr>
    </tbody>
</table>

<table>
    <tbody>
        <tr>
            <th>Types of primary key</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Partition</td>
            <td>Value of the partition key ins input to an internal hash function which determines the partition or physical location on which the data is stored</td>
        </tr>
        <tr>
            <td>Composite</td>
            <td>Partition key + Sort Key in combination. Allows you to store multiple items with the same Partition Key. E.g. Same user (Partition Key) posting multiple items to a forum (Sort Key will be timestamp of the post)</td>
        </tr>
    </tbody>
</table>

![dynamo-table](https://user-images.githubusercontent.com/5309726/68082308-e9d16400-fe55-11e9-97bb-0417b6835cfd.png)

* DynamoDB access control
  * Is managed using AWS IAM
  * Can create an IAM user within your AWS account which has specific permissions to access and create DynomoDB tables
  * Can create an IAM role which enables you to obtain temporary access keys which can be used to access DynomoDB
  * Can also use a special <strong>IAM Condition</strong> to restrict user access to only their own records

![iam-condition](https://user-images.githubusercontent.com/5309726/68082283-bc84b600-fe55-11e9-82d1-77492900ea09.png)

* Indexes Deepdive
  * Enable fast queries on specific data columns
  * Give you a different view of your data, based on alternative Partition / Sort Keys

<table>
    <tbody>
        <tr>
            <th>Types of index</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Local Secondary</td>
            <td>
                <ul>
                    <li>Must be created at when you create your table</li>
                    <li>Same Partition Key as your table but different Sort Key</li>
                    <li>Gives you a different view of your data, organized according to an alternative Sort Key</li>
                    <li>E.g. Partition Key: User ID, Sort Key: Account Creation Date</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Global Secondary</td>
            <td>
                <ul>
                    <li>Can create any time at, at table creation or after</li>
                    <li>Different Partition Key and Sort Key</li>
                    <li>E.g. Partition Key: Email Address, Sort Key: Last Log-In Date</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* Scan vs Query API Call
  * Reduce the impact of a query or scan by setting a smaller page size which uses fewer read operations
  * Avoid using scan operations if you can, design tables in a way that you can use the Query, Get, or BatchGetItem APIs

<table>
    <tbody>
        <tr>
            <th></th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Query</td>
            <td>
                <ul>
                    <li>Find items in a table using only the Primary Key attribute</li>
                    <li>Same Partition Key as your table but different Sort Key</li>
                    <li>Gives you a different view of your data, organized according to an alternative Sort Key</li>
                    <li>E.g. Partition Key: User ID, Sort Key: Account Creation Date</li>
                    <li>Set ScanIndexForward parameter to false to reverse the order queries only</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Scan</td>
            <td>
                <ul>
                    <li>Examines every item in the table</li>
                    <li>By default returns all data attributes</li>
                    <li>Use the ProjectionExpression parameter to refine the results</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* DynamoDB Provisioned Throughput
  * DynamoDB Provisioned Throughput is measured in Capacity Units
  * When you create your table, you specify your requirements in terms of Read and Write Capacity Units
    * 1 x Write Capacity Unit = 1 x 1KB Write per second
    * 1 x Read Capacity Unit = 1 x Strongly Consistent Read of 4KB per second / 2 x Eventually Consistent Reads of 4KB per second (Default)

* Example Configuration
  * Table with 5 x Read and Write Capacity Units
  * This configuration will be able to perform
    * 5 x 4KB Strongly Consistent reads = 20KB per second
    * Twice as many Eventually Consistent = 40KB
    * 5 x 1KB Writes = 5KB per second
  * If your application reads / writes larger items it will consume more Capacity Units and will cost you more as well

<table>
    <tbody>
        <tr>
            <th>Calculation</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Strongly Consistent Reads Calculation</td>
            <td>
                <ul>
                    <li>Your application needs to read 80 items (table rows) per second</li>
                    <li>Each item 3KB in size</li>
                </ul>
                <ul>
                    <li>You need Strongly Consistent Reads</li>
                    <li>First, calculate how many Read Capacity Units needed for each read:</li>
                    <li>Size of reach item / 4KB</li>
                    <li>3KB / 4KB = 0.75</li>
                    <li>Rounded-up to the nearest whole number, reach read will need 1 x Read Capacity Unit per read operation</li>
                    <li>Multiplied by the number of reads per second = 80 Read Capacity</li>
                </ul>
                <ul>
                    <li>You need Eventually Consistent Reads</li>
                    <li>First, calculate how many Read Capacity Units needed for each read:</li>
                    <li>Size of reach item / 4KB</li>
                    <li>3KB / 4KB = 0.75</li>
                    <li>Rounded-up to the nearest whole number, reach read will need 1 x Read Capacity Unit per read operation</li>
                    <li>Multiplied by the number of reads per second = 80 Read Capacity</li>
                    <li>Divide 80 by 2 = 40 Read Capacity Units</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Write Capacity Units Calculation</td>
            <td>
                <ul>
                    <li>You want to write 100 items per second</li>
                    <li>Each item 512 bytes in size</li>
                </ul>
                <ul>
                    <li>First, calculate how many Capacity Units needed for each write:</li>
                    <li>Size of reach item / 1KB (for Write Capacity Units)</li>
                    <li>512 bytes / 1KB = 0.5</li>
                    <li>Rounded-up to the nearest whole number, reach read will need 1 x Write Capacity Unit per write operation</li>
                    <li>Multiplied by the number of writes per second = 100 Write Capacity Units required</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* DynamoDB on-demand capacity option
  * Charges apply for: reading, writing and storing data
  * With On-Demand, you don't need to specify your requirements
  * DynamoDB instantly scales up and down based on the activity of your application

<table>
    <tbody>
        <tr>
            <th>On-Demand Capacity</th>
            <th>Provisioned Capacity</th>
        </tr>
        <tr>
            <td>Unknown workloads</td>
            <td>You can forecast read and write capacity requirements</td>
        </tr>
        <tr>
            <td>Unpredictable application traffic</td>
            <td>Predictable application traffic</td>
        </tr>
        <tr>
            <td>You want a Pay-per-use model</td>
            <td>Application traffic is consistent or increases gradually</td>
        </tr>
        <tr>
            <td>Spiky, short lived peaks</td>
            <td></td>
        </tr>
    </tbody>
</table>

* DynamoDB Accellorator - DAX
  * Is a fully managed, clustered in-memory cache for DynamoDB
  * Delivers up to a 10x read performance improvement
  * Microsecond performance for millions of requests per second
  * Ideal for Read-Heavy and bursty workloads
  * E.g. Auction applications, gaming and retail sites during block Friday promotions

* How does it works?
  * DAX is a write-through caching service - this means data is written to the cache as well as the back end store at the same time
  * Allows you point your DynamoDB API calls at the DAX cluster
  * If the item you are querying is in the cache (cache hit), DAX returns the result to the application
  * If the item is not available (cache miss), then DAX performans an Eventually Consistent GetItem operation against DynamoDB
  * Retrieval of data from DAX reduces the read load on DynamoDB tables

* NOT suitable for?
  * Caters for Eventually Consistent reads only,so not suitable for applications that require Strongly Consistent reads
  * Write intensive applications
  * Applications that do not perform many read operations
  * Application sthat do not required microsecond response times

* Elasticache
  * Sits between your application and the database
  * E.g. an application frequently requesting specific product information for your best selling products
  * Takes the load off your database
  * Good if your database is particularly <strong>read-heavy</strong> and the data is not changing frequently

* Benefits and use cases
  * Improves performance for read-heavy workloads
    * E.g. Social networking, gaming media sharing, Q&A portals
  * Frequently accessed data is stored in memory for low latency access, improving the overall performance of your application
  * Also good for compute heavy worklaods
  * E.g. Recommendation engines
  * Can be used to store results of I/O intensive database quries or output of compute-ivensive calculations

* Caching strategies
  * Avoid stale data by implementing TTL
  * Lazy Loading treats an expired key as a cache miss and causes the application to retrieve the data from the database and subsequently write the data into the cache with a new TTL
  * Does not eliminate stale data, but helps to avoid it
  * Stale state is information in an object that does not reflect reality.

* Stale state, E.g. an object's members are filled with information from a database, but the underlying data in the database has changed since the object was filled

<table>
    <tbody>
        <tr>
            <th></th>
            <th>Lazy Loading</th>
            <th>Write Through</th>
        </tr>
        <tr>
            <td>Description</td>
            <td>
                <ul>
                    <li>Loads the data into the cache only when necessary</li>
                    <li>If requested data is in the cache, Elasticache returns the data to the application</li>
                    <li>If the data is not in the cache or has expired, Elasticache returns a null</li>
                    <li>Your application then fetches the data from the database and writes the data received into the cache so that it is available next time</li>
                </ul>
            </td>
            <td>Adds or updates data to the cache whenever data is written to the database</td>
        </tr>
        <tr>
            <td>Advantages</td>
            <td>
                <ul>
                    <li>Only requested data cache, avoids filling up cache with useless data</li>
                    <li>Node failures are not fatal, a new empty node will just have</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Data in the cache never stale</li>
                    <li>Users are generally more tolerant of additional latency when updating data than when retrieving it</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>Disadvantages</td>
            <td>
                <ul>
                    <li>Cache miss penalty, initial request-> query to DB -> writing of data to the cache</li>
                    <li>Stale data, if data is only updated when there is a cache miss, it can become stale. Doesn't automatically update if the data in the database changes</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>Write penalty, every write involves a write to the cache as well as a write to the database</li>
                    <li>If a node fails and a new one is spun up, data is missing until added or updated in the database. (Mitigate by implementing Lazy Loading in conjunction with write-through)</li>
                    <li>Wasted resources if most of the data is never read</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* DynamoDB Transactions
  * Provide developers atomicity, consistency, isolation, and durability (ACID) across one or more tables within a single AWS account and region
  * Check for a pre-requisite condition before writing to a table

* DynamoDB TTL
 * Time To Live attribute defines an expiry time for your data
 * Expired items marked for deletion
 * Great for removing irrelevant or old data:
   * Session data
   * Event logs
   * Temporary data
* Reduces cost by automatically removing data which is no longer relevant

![ttl-table](https://user-images.githubusercontent.com/5309726/68684170-ffa3ff00-05a2-11ea-82b8-1d7785ce2508.png)

* DynamoDB Streams
  * Capture time ordered sequence of item level modifications (insert, update, delete)
  * Logs are encrypted at rest and stored for 24 hours
  * Accessed using a dedicated endpoint (Endpoints for DynamoDB Streams AWS maintains separate endpoints for DynamoDB and DynamoDB Streams)
  * By default the Primary Key is recorded
  * Before and After images can be captured

![dynamodb-streams](https://user-images.githubusercontent.com/5309726/68770526-435d3e00-0661-11ea-83aa-775b4a9661cd.png)

* Processing DynamoDB Streams
  * Events are recorded in near real time
  * Applications can take actions based on contents
  * Event source for Lambda
  * Lambda pools the DynamoDB stream
  * Executes Lambda code based on a DynamoDB Streams event

* Provisioned Throughput Exceeded Exception
  * `ProvisionedThroughputExceededException`
  * Your request rate is too high for thr read / write capacity provisioned on your DynamoDB table
  * SDK will automatically retries the requests until successful
  * If you are not using the SDK you can,
    * Reduce request frequency
    * Use Exponential Backoff

* What is Exponential Backoff
  * Many components in a network can generate errors due to being overloaded
  * In addition to simple retries <strong>all AWS SDKs</strong> use Exponential Backoff
  * Progressively longer waits bettwen consecutive retries e.g. 50ms, 100ms, 200ms... for improved flow control
  * If after 1 minute this doesn't work, your request size may be exceeding the throughput for your read/write capacity

  ---

* Aws Key Management Service (KMS)
  * Is a managed service that makes it easy for you to create and control the encryption keys used to encryp your data.

* KMS API Calls
  * aws kms encrypt
  * aws kms decrypt
  * aws kms re-encrypt
  * aws kms enable-key-rotation

* Envelope Encryption
![kms-encrypt](https://user-images.githubusercontent.com/5309726/68776622-5ecd4680-066b-11ea-8674-3ba5445b8136.png)

![kms-decrypt](https://user-images.githubusercontent.com/5309726/68776644-67be1800-066b-11ea-88cc-55b8ed3e7d1c.png)

  ---

What is SQS?
  * Is a distributed message queueing system
  * Allows you to decouple the components of an application so that they are independent
  * Pull-based, not pushed-based
  * Messages are 256KB in size
  * Messages can be kept in the queue from 1 minute to 14 days
  * Default retention period is 4 days
  * Guarantees that your messages will be processed at least once

<table>
    <tbody>
        <tr>
            <th>Queue types</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Standard</td>
            <td>
                <ul>
                    <li>As the default queue type</li>
                    <li>Nearly unlimited number of transactions per second</li>
                    <li>Guarantee that a message is delivered at least once</li>
                    <li>Occasionally (because of the highly distributed architecture that allows high throughput), more that 1 copy of a message might be delivered out of order</li>
                    <li>Provide <strong>best-effort (not guarantee)</strong> ordering which ensures that messages are generally delivered in the same order as they are sent</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>FIFO</td>
            <td>
                <ul>
                    <li>FIFO delivery and exactly once processing</li>
                    <li>The order in which messages are sent and received is strictly preserved and a message is delivered once and remains available until a consumer processes and deletes it</li>
                    <li>Duplicates are not introduced into the queue</li>
                    <li>Support message groups that allow multiple ordered message groups within a single queue</li>
                    <li>Limited to 300 transactions per second (TPS), but have all the capabilities of standard queues</li>
                </ul>
            </td>
        </tr>
    </tbody>
</table>

* Visibility timeout
  * Is the amount of time that the message is invisible in the SQS queue after a reader picks up that message
  * default is 30 seconds, increase if your task tasks > 30 seconds to complete
  * Max 12 hours

<table>
    <tbody>
        <tr>
            <th>Polling</th>
            <th>Description</th>
        </tr>
        <tr>
            <td>Short</td>
            <td>
                Returned immediately even if not messages are in the queue
            </td>
        </tr>
        <tr>
            <td>Long</td>
            <td>
                Polls the queue periodically and only returns a response when a message is in the queue or the timeout is reached (can save your money)
            </td>
        </tr>
    </tbody>
</table>

* Simple Notification Service (SNS)
  * Is a scalable and highly available notification service which allows you to send push notifications (to Apple, Google, as well as Android devices) from the cloud
  * Variety of message formats supported, SMS text message, email Amazon Simple Queue Service (SQS) queues, any HTTP endpoint
  * Pub-sub model whereby users subscribe to topics
  * It is a push mecahnism

* SNS Topics
  * Is a logical access point and communication channel for allowing receipients to dynamically subscribe for identical copies of the same notification
  * Create a topic and control access to it by defining policies that determine which publishers and subscribers can communicate with the topic

* Simple Email Service (SES) vs SNS
<table>
    <tbody>
        <tr>
            <th>SES</th>
            <th>SNS</th>
        </tr>
        <tr>
            <td>Email messaging service</td>
            <td>Pub/sub messaging service, formats include SMS, HTTP, SQS, email</td>
        </tr>
        <tr>
            <td>Can trigger Lambda function or SNS notification</td>
            <td>Can be used to trigger Lambda function</td>
        </tr>
        <tr>
            <td>Can be used for both incoming and outgoing email</td>
            <td>Can fan out messages to large number of recipients (replicate and push messages to multiple endpoints and formats)</td>
        </tr>
        <tr>
            <td>An email address is all that is required to start sending messages to a user</td>
            <td>Consumers must subscribe to a topic to receive the notifications</td>
        </tr>
    </tbody>
</table>

* SES use cases
  * Automated emails
  * Purchase confirmations, shipping notifcations, order status updates
    * E.g. a mobile phone company that needs to send automated confirmation email every time a customer purchases pre-paid mobile phone minutes
  * Marketing communications, advertisements, newsletters, special offers
    * E.g. an online retail business that needs to let customers know about sales promotions and discounts

* Elastic Beanstalk
  * Platform as service
  * Brings together AWS services like EC2, Auto Scaling, and S3 for the purpose of deploying the elastic cloud applications
