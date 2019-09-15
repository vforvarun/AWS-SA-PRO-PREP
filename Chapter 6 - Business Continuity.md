*Business Continuity Module*

- Terminologies
  - Business Continuity
    - Seeks to minimize business activity disruption when something unexpected happens
  - Disaster Recovery
    - Act of responding to events that threatens Business Continuity
  - High Availability
    - Designing in redundancies to reduce the chance of impacting service levels
  - Fault Tolerance
    - Designing the ability to absorb problems without impacting service levels
  - Service Level Agreement
    - An agreed goal or target or a given service on its performance or availability
  - Recovery Time Objective (RTO)
    - Time that takes after a disruption to restore business processes to their service levels
  - Recovery Point Objective (RPO)
    - Acceptable amount of data loss measured in time
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/RTO-RPO.png)
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/BCP-RTO.png)
- Disaster Categories
  - Hardware Failure
    - Network switch power supply fails and brings down the LAN
  - Deployment Failure
    - Deploying a patch that breaks a key ERP business process
  - Load Induced
    - DDoS attack on a website
  - Data Induced
    - Ariane 5 rocket explosion on June 4, 1996
  - Credential Expiration
    - An SSL/TLS certificate expires on eCommerce site
  - Dependency
    - S3 subsystem failure cause numerous other AWS service failures
  - Infrastructure
    - A construction crew accidentally cuts a fiber optic data line
  - Identifier Exhaustion
    - "We currently do not have sufficient capacity in the AZ you requested"
  - Human Error
- DR Architecture
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/DR-Failure.png)
  - Backup and Restore
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/backup-restore.png)
    - Pros
      - Very common entry point into AWS
      - Minimal effort to configure
    - Cons
      - Least flexibility
      - Analogous to off-site backup storage
  - Pilot Light
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/pilot-light.png)
    - Pros
      - Cost effective way to maintain a "hot site" concept
      - Suitable for a variety of landscapes and applications
    - Cons
      - Usually requires manual intervention for failover
      - Spinning up cloud environments will take minutes or hours
      - Must keep AMIs up-to-date with on-prem counterparts
  - Warm Standby: More response than Pilot Light
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/warm-standby.png)
    - Pros
      - All services are up and ready to accept failover faster within minutes or seconds
      - Can be used as a "shadow environment" for testing or production staging
    - Cons
      - Resources would need to be scaled to accept production load
      - Still requires some environment adjustments but could be scripted
  - Multi Site
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/multi-site.png)
    - Pros
      - Ready all the time to take full production load - effectively a mirrored data centre
      - Fails over in seconds or less
      - No or little intervention required to failover
    - Cons
      - Most expensive DR Option
      - Can be perceived as wasteful as there are resources just standing around waiting for the primary to fail
- Storage Options
  - EBS Volumes
    - Annual failure rate of < 0.2% compared to commodity level hard drive at 4% (Given 1000 EBS volumes, expect around 2 to fail per year)
    - Availability target of 99.999%
    - Replicated automatically *within a single AZ*
    - Vulnerable to AZ failure. Plan accordingly.
    - Easy to snapshot, which is stored on S3 and multi-AZ durable.
    - Copy snapshots to other regions as well
    - Supports RAID configurations
    - RAID Configurations
      - RAID 0: Striping
        - Redundancy: None
        - Reads: **** (fastest)
        - Writes: **** (fastest)
        - Capacity: 100%
        - Disk Layout: Striping
          A1  A2
          A3  A4
          A5  A6
          A7  A8
      - RAID 1: Mirroring
        - Redundancy: 1 drive can fail
        - Reads: ***
        - Writes: ***
        - Capacity: 50%
        - Disk Layout: Mirroring
          A1  A1
          A2  A2
          A3  A3
          A4  A4
      - RAID 2, RAID 3 and RAID 4 are not used these days
      - RAID 5: Very popular option
        - Requires minimum of 3 drives, 2 drives store the data and one drive stores the parity bit
        - Parity bit allows information recovery if one of the drives is removed or fails
        - In case drive fails, an empty drive can be added and the parity drive can calculate and recreate the data
        - Redundancy: 1 drive can fail
        - Reads: ****
        - Writes: **
        - Capacity: (n-1)/n
        - Disk Layout:
          A1  A2  Ap
          B1  Bp  B2
          Cp  C1  C2
          D1  D2  Dp
      - RAID 6:
        - Requires minimum of 4 disks, 2 drives store the data and two drives stores parity bit
        - A level above RAID 5, has two methods of parity
          - Primary Parity
          - Secondary Parity
        - Redundancy: 2 drives can fail
        - Reads: ****
        - Writes: *
        - Capacity: (n-2)/n
        - Disk Layout:
          A1  A2  Ap1 Ap2
          B1  Bp1 Bp2 B2
          Cp1 Cp2 C1  C2
          Dp2 D1  D2  Dp1
      - Nested RAIDs are also available; RAID 0 + RAID 1 etc; very closely tied to OS capabilities.
      - AWS doesn't recommend using RAID 5 and RAID 6 because
        - EBS volumes are accessed over a network
        - Writing parity bits sucks up a lot of I/O, consumes almost 20% to 30% of available IOPS
        - Stick to RAID 0 or RAID 1 on AWS.
      - RAID levels and I/O throughputs: Volume Type - ESB Provisioned IOPS SSD (lo1)
        - No RAID
          - Volume Size: 1000 GB (1 Disk)
          - Provisioned IOPS: 4000
          - Total Volume IOPS: 4000
          - Usable Space: 1000 GB
          - Throughput: 500 MB/s
        - RAID 0
          - Volume Size: 500 GB (2 Disk)
          - Provisioned IOPS: 4000
          - Total Volume IOPS: 8000
          - Usable Space: 1000 GB
          - Throughput: 1000 MB/s (doubled, because we are spreading the load across multiple drives)
        - RAID 1
          - Volume Size: 500 GB (1 Disk)
          - Provisioned IOPS: 4000
          - Total Volume IOPS: 4000
          - Usable Space: 500 GB
          - Throughput: 500 MB/s
  - S3 Storage
    - Standard Storage Class (99.99% availability = 52.6 minutes / year)
    - Standard Infrequent Access (99.9% availability = 8.76 hours / year)
    - One-Zone Infrequent Access (99.5% availability = 1.83 days / year)
    - Eleven 9s of durability (99.999999999%) - All storage classes
    - Standard and Standard-IA have multi-AZ durability; One-Zone only as single AZ durability
    - Backing service for EBS snapshots and many other AWS Services
  - Amazon EFS
    - Implementation of the NFS filesystem
    - True filesystem as opposed to block storage (EBS) or object storage (s3)
    - File locking, strong consistency, concurrency accessible
    - Each file object and metadata is stored across multiple AZs
    - Can be accessed from all AZs concurrently
    - Mount targets are highly available
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/EFS-Architecture.png)
  - Amazon Storage Gateway
    - Good way to migrate on-prem data to AWS for offsite backup
    - Best for continuous sync needs
  - Snowball
    - Various options for migrating data to AWS based on volume
    - Only for batch transfers of data
  - Glacier
    - Safe offsite archive storage
    - Long-term storage with rare retrieval needs
- HA approaches for Compute
  - Up-to-date AMIs are critical for rapid fail-over
  - AMIs can be copied to other regions for safety or DR staging purposes
  - Horizontally scalable architectures are preferred because risks can be spread across multiple smaller machines versus one large machine
  - Reserved instances is the only way to guarantee that resources will be available when needed
  - Auto-scaling and Elastic Load Balancing work together to provide automated recovery by maintaining minimum instances
  - Route 53's health checks also provide "Self-Healing" redirection of traffic
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/route-53.png)
- HA approaches for Database
  - If possible, choose DynamoDB over RDS because of inherent fault tolerance feature of DynamoDB
  - If DynamoDB can't be used, choose Aurora because of redundancy and automatic recovery features
  - If Aurora can't be used, then choose Multi-AZ RDS
  - Frequent RDS snapshots can protect against data corruption failure and then won't impact performance of multi-AZ deployment
  - Regional replication is also an option, but it is not strongly consistent. It will be slightly out-of-date from the master
  - If Database is on EC2, then HA is in the hands of the user.
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rds-ha.png)
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rds-ha-az-failure.png)
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rds-ha-region-failure.png)
  - Redshift
    - Currently Redshift doesn't support multi-AZ deployments
    - Best HA option is to use a multi-node cluster which supports data replication and recovery
    - A single-node Redshift cluster doesn't support data replication, if it fails then manual it should restore from a snapshot on S3.
  - Memcached
    - Because Memcached doesn't support replication, a node failure will result in data loss
    - Use multiple nodes in each shard to minimize data loss on node failure
    - Launch multiple nodes across available AZs to minimize data loss on AZ failure
  - Redis
    - Use multiple nodes in each shard and distribute the nodes across multiple AZs
    - Enable multi-AZ on the replication group to permit automatic failover if the primary node fails
    - Schedule regular backups of the Redis cluster
- HA approaches for Networking
    - By creating subnets in the available AZs, create multi-AZ presence for the VPC
    - Best practice is to create at least two VPN tunnels into Virtual Private Gateway
    - Direct Connect is no HA by default, so establish a secondary connection via another Direct Connect (ideally with another provider) or use a VPN
    - Route 53's (100% availability SLA) Checks provide basic level of redirecting DNS resolutions
    - Elastic IPs allow flexibility to change out baking assets without impacting name resolution
    - For multi-AZ redundancy of NAT Gateways, create gateways in each AZ with routes for private subnets to use the local Gateway
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/redundant-arch.png)
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/route-53-hcs.png)
- Exam Tips
  - General Concepts
    - Know the difference between Business Continuity, Disaster Recovery and Service Levels
    - Know the difference between High Availability and Fault Tolerance
    - Understand the inter-relationships and how AWS uses the terms
    - Know the difference between TRO and RPO
    - Know the four general types of DR architectures and trade-offs of each
  - Storage Options
    - Understand the HA Capabilities and limitations of AWS storage options
    - Know when to use each storage option to achieve the required level of recovery capability
    - Understand RAID and the potential benefits and limitations
  - Compute Options
    - Understand why horizontal scaling is preferred from an HA perspective
    - Know that compute resources are finite in an AZ and know how to guarantee their availability
    - Understand how Auto Scaling and ELB can contribute to HA
  - Database Options
    - Know the HA attributes of the various Database services
    - Understand the different HA approaches and risks for Memcached and Redis
    - Know which RDS options require manual failover and which are automatic
  - Network Options
    - Know which networking components are not redundant across AZs and how to architect for them to be redundant
    - Understand the capabilities of Route 53 and Elastic IP in context of HA
  - White Papers
    - Backup and Recovery Approaches Using AWS
      - https://d1.awsstatic.com/whitepapers/Storage/Backup_and_Recovery_Approaches_Using_AWS.pdf
    - Getting Started with Amazon Aurora
      - https://d1.awsstatic.com/whitepapers/getting-started-with-amazon-aurora.pdf
    - AWS Reliability Pillar: AWS Well-Architected Framework
      - https://d1.awsstatic.com/whitepapers/architecture/AWS-Reliability-Pillar.pdf
  - Videos
    - Modes of Availability
      - https://www.youtube.com/watch?v=xc_PZ5OPXcc
    - How to Design a Multi-Region Active-Active Architecture
      - https://www.youtube.com/watch?v=RMrfzR4zyM4
    - Disaster Recovery with AWS: Riered Approaches to Balance Cost with Recovery Objectives
      - https://www.youtube.com/watch?v=a7EMou07hRc
- Pro Tips
  - Failure Mode and Effects Analysis (FMEA): Created in 1950s by the ARMY
    - A systematic process to examine
      - What could go wrong
      - What impact it might have
      - What is the likelihood of it occurring
      - What is our ability to detect and react
    - Formula
      - Severity * Probability * Detection = Risk Priority Number
    - Case Study #1
      - Invoicing
      - Step 1: Round up Possible Failures
      | Failure Mode  | Cause | Current Controls  |
      | ------------  | ----- | ----------------  |
      | Pricing Unavailable  |  Retail price incorrect in ERP system | Master data maintenance audit report  |
      | Pricing Incorrect | Retail price not assigned in ERP system | None  |
      | Slow to build Invoice | Invoicing System is slow  | None  |
      | Unable to build invoice | Invoicing System is offline | Uptime monitor  |

      - Step 2: Assign Scores
      | Failure Mode  | Customer Impact | Likelihood  | Detect and React  | Risk Priority Number  |
      | ------------  | --------------- | ----------- | ----------------  | --------------------  |
      | Pricing Unavailable  |  7 | 3 | 2 | 42  |
      | Pricing Incorrect | 8 | 3 | 9 | 216  |
      | Slow to build Invoice | 5 | 2 | 9 | 90  |
      | Unable to build invoice | 8 | 3 | 2 | 48  |

      - Step 3: Prioritize on Risk
      | Failure Mode  | Customer Impact | Likelihood  | Detect and React  | Risk Priority Number  |
      | ------------  | --------------- | ----------- | ----------------  | --------------------  |
      | Pricing Unavailable  |  7 | 3 | 2 | 42  |
      | Pricing Incorrect | 8 | 3 | 9 | **`216`**  |
      | Slow to build Invoice | 5 | 2 | 9 | 90  |
      | Unable to build invoice | 8 | 3 | 2 | 48  |

    - FMEA Across Diaster Categories
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/case-study-2.png)
