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
    
