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
    - Pros
      - Cost effective way to maintain a "hot site" concept
      - Suitable for a variety of landscapes and applications
    - Cons
      - Usually requires manual intervention for failover
      - Spinning up cloud environments will take minutes or hours
      - Must keep AMIs up-to-date with on-prem counterparts
  
