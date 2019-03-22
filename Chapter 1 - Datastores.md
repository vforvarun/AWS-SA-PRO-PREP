Concepts

  - Data Persistence
      - Persistent Data Store
          - Data is Durable and sticks around after reboots, restarts and power cycles
          - Examples: Glacier, RDS, EBS
      - Transient Data Store
          - Data is just temporarily stored and passed along to another process or persistent store
          - Examples: SQS and SNS
      - Ephemeral (Temporary) Data Store
          - Data is lost when stopped
          - Examples: EC2 Instance Store, Memcached
  - Data Performance
      - IOPS
        - Input/Output Operations per second. Measures how fast we can read or write to a device
        - Analogy: A small compact sports car that goes really fast with small amount of data.
      - Throughput
        - Measure of how much data can be moved at one time
        - Analogy A garbage truck moves slowly with lot of data
  - Consistency
      - A set a ground rules to ensure that a datastore plays by so that we can sure that when we hand the data it behaves in certain way.
  - Consistency Models
      - ACID
          - Atomic: Transactions are "all or nothing". No half transactions.
          - Consistent: Transactions must be valid. They can't be corrupted.
          - Isolated: Transactions can't mess with one another.
          - Durable: Completed transactions must stick around.
          - Examples: RDS
      - BASE
          - Basic Availability: Values available even if it is stale.
          - Soft State: Might not be instantly consistent across stores.
          - Eventual Consistency: Will achieve consistency at some point.
          - Examples: S3 and DynamoDB
      - ACID vs Base:
          - ACID is rigid not flexible, where as BASE allows flexibility
          - ACID is good for serial tasks, where as BASE is suitable for parallel tasks
          - ACID is not scalable, where as BASE is highly scalable

  S3
    - One of the first services AWS introduced back in 2006.
    - Used in other AWS Services - directly or behind the scenes
    - Maximum object size is 5TB; largest object in a single PUT is 5GB
    - Recommended to use multi-part uploads if larger than 100MB
    - S3 is an object store
      - S3 Key is looks like: s3://bucket/finance/april/16/invoice.pdf
      - It is called KEY not a file PATH
      - S3 is more common with a DB rather than a file system
      - The KEY is the name of record that uniquely identifies the document
    - S3 Consistency
        - Read-after-write consistency for PUTs of new objects:
          - In layman terms
            - Cool, I've never seen this object and no one has asked about it before.
            - Welcome aboard, new object - and you can read it immediately.
        - HEAD or GET requests of the KEY before an object exists will result in eventual consistency:
          - In layman terms
              - Wait a second, someone has already asked about this key and I told them "never saw it".
              - I remember that, and need to honor that response until I completely write this new object and fully replicate it.
              - So, I'll let you read it eventually.
        - S3 offers eventual consistency for overwrite PUTs and DELETEs:
          - In layman terms
              - Ok, so you want to update or delete an object.
              - Let's make sure we get that update or delete completely locally
              - Then we can replicate it to other places.
              - Until then, I have to serve up the current file
              - I'll serve up the update/delete once it is fully replicated - eventually.
        - Updates to a single is ATOMIC
          - In layman terms
              - Whoa there. Only person can update an object at a time.
              - If I get two requests, I'll process them in the order of their timestamp
              - You'll see updates as soon as I replicate else where.
      - S3 Security
          - Resource ACLs: Examples: Object ACL, Bucket Policy
          - User based: Examples: IAM Policies
          - MFA (optional): Before DELETE of objects.
          - Authentication Flowchart:
            - Is the user account allowed to access me?
              - No: Get Lost
              - Yes:
                  - Does the bucket policy allow access to you?
                  - No: Get Lost
                  - Yes:
                    - Does the object ACL allow you to access it?
                      - No: Get Lost
                      - Yes: Here you go..!!
        - S3 Versioning:
          - New version with each write

        - S3 Data Protection:

        - S3 Cross Region Replication:
          - Security
          - Compliance
          - Latency

        S3 Lifecycle Management:
          - Optimize Storage Costs
          - Adhere to Data Retention Policies
          - Keep S3 volumes well-maintained

        S3 Analytics:
          - Data Lake: Athena, Redshift, Spectrum, QuickSight
          - IoT Streaming Data Repository: Kinesis Firehose
          - Machine Learning and AI Storage: Rekognition, Lex, MXNet
          - Storage Class Analysis: S3 Management Analytics

        S3 Encryption at Rest:
          - SSE-S3: Server Side Encryption, S3's existing built-in AWS provided AES-256 KEY
          - SSE-C: Customer supplied Encryption Key.
          - SSE-KMS: Use a key generated and managed by AWS KMS
          - Client-Side: Encrypt objects using your own local encryption before uploading to S3 (PGP, GPG etc)

        Nifty Tricks:
          - Transfer Acceleration: Setup data uploads using CloudFront in reverse
          - Requestor plays
          - Tags
          - Events: Trigger notifications to SQS, SNS and Lambda
          - Static Web Hosting
          - BitTorrent

  Glacier:
    - Cheap, slow to respond, seldom accessed
    - Cold Storage
    - Used by Storage Gateway Virtual Tape Library
    - Integrated with S3 via Lifecycle Management
    - Faster retrieval speed options for an additional fee
    - Security:
      - Access control via IAM
        Archive:
          - File, zip, tar etc
          - Max Size 40 TB
          - Immutable: Can delete or overwrite but can't change once uploaded to Vault
        Glacier Vault Lock
          - Different than vault access Policy
          - Enforce rules like no deletes or MFA
          - Immutable: Can delete or overwrite but can't change once uploaded to Vault
          - Once the Vault Lock has been initiated have only 24 hours to change the mind, if not it becomes permanent.

  Amazon EBS: Elastic Block Storage
    - Think virtual hard drives
    - Can only be used with AZ
    - Tied to a single AZ: no multi AZ support
    - Variety of Optimized choices for IOPS, Throughput and Cost
    - Snapshots
    - EBS vs Instance Storage:
      - Instance Storage:
        - Temporary
        - Ideal for caches
        - Data goes away when EC2 instance stops or gets terminated
      - EBS:
        - Not locked; Attach or detach
        - Snapshotted
        - EC2 Writes over a network to EBS
    - EBS Snapshots
      - Cost-effective and easy backup strategy
      - Share data sets with other users or accounts
      - Migrate EC2 instances from one AZ or Region to another.
      - Convert unencrypted volume to encrypted
      - Pay for the entire block rather than pay for what you use.
      - Snapshots can be created, deleted or restored to point in time.
      - First snapshot is full, and incremental in subsequent snapshots.
      - All incremental snapshots contain only the blocks that changed.
      - If an incremental snapshot is deleted, we lose the ability to restore to that point in time.
      - If the first full snapshot is deleted, the EBS can still be fully restored using the incremental snapshots.

  Amazon EFS: Elastic File System
    - Sun Microsystems created Network File System in 1984
    - EFS is AWS's implementation of NFSv4
    - Elastic storage capacity, and pay for what you use (in contrast to EBS)
    - EFS is
      - 3 times more expensive than EBS
      - 20 times more expensive than S3
    - Multi-AZ metadata and data storage
    - Configure mount points in one or many AZs and mount from EC2 in any AZ.
    - Can be mounted from on-premises systems only if using Direct Connect.
      - EFS is very network intensive, works best over high speed connections.
    - Alternatively, use EFS File sync agent locally on-premises and in the background to sync to EFS

  Amazon Storage Gateway
    - Virtual Machine that can be run on-premises with VMWare of HyperV
    - Provides local storage resources backed by AWS S3 and Glacier
    - Often used in DR preparedness to sync to AWS
    - Useful in Cloud Migrations
    - Modes of Operation
      - File Gateway
        - Old Name: None
        - Interface: NFS, SMB
        - Allows on-prem servers or EC2 instances to store objects in S3 via NFS or SMB mount point
      -  Volume Gateway Stored Mode
        - Old Name: Gateway-Stored Volumes
        - Interface: iSCSI
        - Async replication of on-prem data to S3
        - All data is stored on-prem but copy of that is replicated and synced to S3 in the background.
      - Volume Gateway Cached Mode
        - Old Name: Gateway-Cached Volumes
        - Interface: iSCSI
        - Primary data stored in S3 with frequent access data cached locally on-prem.
        - In a migration scenario, start with Stored Mode, sync all files to S3 and then switch to Cached mode to use S3 as primary source of data.
      - Tape Gateway
        - Old Name: Gateway-Virtual Tape Library
        - Interface: iSCSI
        - Virtual media changer and tape library for use with existing backup software
    - Supports bandwidth throttling.

  Amazon Workdocs
    - AWS's version of Google Drive or Dropbox
    - Secure, fully managed file collaboration service
    - Can integrate with AD for SSO
    - Web, mobile and native Windows and Mac Clients (no linux support yet)
    - HIPPA, PCI DSS and ISO compliant
    - Available SDK for creating complementary apps.

  EC2 Databases
    - Run any database with full control and flexibility
    - Must manage everything like backups, redundancy, patching and scaling
    - Good option
      - if you require a database not yet supported by RDS, such as IBM DB2 or SAP HANA
      - if is not feasible to migrated to AWS-managed DB

  Amazon RDS
    - Managed database option for
      - MySQL
      - Maria
      - PostgreSQL
      - Oracle
      - Microsoft SQL Server
      - MySQL-compatible Aurora
    - Best suited for structured, relational data store needs.
    - Aims to be drop-in replacement for existing on-prem instances of same databases.
    - Automated backups and patching in customer-defined maintenance windows.
    - Push-button scaling, replication and redundancy
    - Amazon RDS Anti-Patterns: RDS is not suitable for and alternatives
      - Use S3 to store lots of Large Binary Objects (BLOBs)
      - Use DynamoDB if
        - Automated Scalablity is required
        - Name/Value Data Structure
        - Data is not well structured or unpredictable
      - Use EC2 if
        - Other DB plaforms like IBM DB2 or SAP HANA
        - Complete control over the database
    - Replication
      - MultiAZ feature replicates the DB to different AZs for redundancy
      - NOTE for MySQL: non-transactional storage engines like MyISAM don't support replication; use InnoDB (or Xtra DB on Maria)
      - Read Replicas in the same region or a different region can be created to minimise the read loads on the RDS.
      - Types
        - Sync: in multiAZ RDS scenario, replication between Master and Standby DB is synchronous. Any change to the master DB is immediately synced to standby DB.
          - In case of failure of the Master DB, standby can take over and also serve as master for read replicas
        - Async: in read replica scenario, replication between master and read replica is asynchronous.
          - In case of a region failure, a read replica in another region can be promoted to Master and continue to serve other read replicas.

  DynamoDB:
