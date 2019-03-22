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

- S3
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
        - Flowchart:
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

Amazon EBS:
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
