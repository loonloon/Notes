#### Episode 2 ####
* Identity Access Management (IAM) 101
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

#### Episode 3 ####
* What is EC2?
  * Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud
  * Allowing users to rent virtual computers on which to run their own computer applications in the cloud
* EC2 Options
<table>
    <tbody>
        <tr>
            <th>Option</th>
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
            <th>Option</th>
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
                    <li><strong></strong>If the instance is terminated by Amazon EC2, you will not be charged for a partial hour of usage. However, if you terminate the instance yourself, you will be charged for the complete hour in which the instance ran.</li>
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
  * Elastic Block Store (EBS) allows you to create storage volumes and attach them to Amazon EC2 instances.
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
