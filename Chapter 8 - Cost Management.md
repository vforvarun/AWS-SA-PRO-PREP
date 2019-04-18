**Cost Management Module**

- Concepts
  - Capital Expenses (CapEx)
    - Money spent on long-term assets like property, buildings and equipment etc
  - Operational Expenses (OpEx)
    - Money spent for on-going costs for running the business. Usually considered variable expenses
  - Total Cost of Ownership (TCO)
    - A compherhensive look at the entire cost model of a given decision or option, often including both hard costs and soft costs
  - Return on Investment (ROI)
    - The amount an entity can expect to receive back within a certain amount of time given an investment
  - TCO and ROI
    - Many times, organizations don't have a good handle on their full on-prem data ceontre costs (power, cooling, fire suppression etc)
    - Soft costs are rarely tracked or even understood as a tangible expense
    - Learning curve will be very different from person to person
    - Business plans usually include many assumptions which in turn require support organizations to create derivative assumptions - sometimes layers deep
- Cost Optimization Strategies
  - Appropriate Provisioning
    - Provision the resources needed and nothing more
    - Consolidate where possible for greater density and lower complexity (multi-database RDS, containers)
    - CloudWatch can help by monitoring utilization
  - Right-Sizing
    - Using lowest-cost resource that still meets the technical specifications
    - Architecting for most consistent use of resources is best versus spikes and valleys
    - Loosely coupled architectures using SNS, SQS, Lambda and DynamoDB can smooth demand and create more predictability and consistency
  - Purchase Options
    - For permanent applications or needs, Reserved Instances provide the best cost advantage
    - Spot instances are best for temporary horizontal scaling
    - EC2 Fleet lets users define target mix of On-Demand, Reserved and Spot Instances

    |Instance|OnDemand|Reserved 1 Year All Upfront|Spot Instance|
    |---|---|---|---|
    |m5.2xlarge|0.384|0.229 (40% less)|0.0798 (79% less)|

    - Geographical Selection
      - AWS Pricing can vary from region to region
      - Consider potential savings by locating resources in a remote region if local access is not required
      - Route 53 and CLoudFront can be used to reduce potential latency of a remote region

      |Region|S3 Standard Storage First 50 TB|
      |---|---|
      |us-west-2|0.023/GB|
      |us-west-1|0.026/GB|
      |ap-northeast-1|0.025/GB|
      |sa-east-1|0.0405 per GB|

    - Managed Services
      - Leverage managed services such as MySQL RDS over self-managed options such as MySQL on EC2
      - Cost savings gained through lower complexity and manual intervention
      - RDS, RedShift, Fargate and EMR are great examples of fully-managed services that replace traditionally complex and difficult installations with push-button ease.
    - Optimized Data Transfer
      - Data-in to AWS, no cost
      - Data going out and between AWS regions can become significant cost component
      - Direct Connect can be more cost-effective option given data volume and speed
- AWS Tagging
  - **The Number one best thing** that can be used to manage the AWS assets
  - Tags are just arbitrary name/value pairs that you can assign to virtually all AWS assets to serve as metadata
  - Tagging strategies can be used for Cost Allocation, Security, Automation, and many other uses
    - Example: A tag can be used in an IAM policy to implement access controls to certain resources
  - Enforcing standardized tagging can be done via AWS Config Rules or custom scripts
    - Example, EC2 instances not properly tagged are stopped or terminated
  - Most resources can have up to 50 Tags
- AWS Resources Groups
  - Resource Groups are groupings of AWS assets defined by tags
  - Create custom consoles to consolidate metrics, alarms and config details around given tags
  - Common Resources Groupings
    - Environments (DEV, QA, PRD)
    - Project Resources
    - Collection of resources supporting key business processes
    - Resources allocated to various departments or cost centres
- Spot Instances and Reserved Instances
  - Reserved Instances
    - Purchase (or agree to purchase) usage of EC2 instances in advance for a significant discount over On-Demand pricing
    - Provides capacity reservation when used in a specific AZ
    - AWS Billing automatically applies discounted rates when an instances is launched that matches purchased RI
    - EC2 has three RI types
      - Standard
      - Convertible
      - Scheduled
    - Can be shared across multiple accounts within Consolidated Billing
    - If RIs are not needed, then it can be sold on the Reserved Instance Marketplace
    - Standard vs Convertible

      ||Standard|Convertible|
      |---|---|---|
      |Terms|1 year, 3 year|1 year, 3 year|
      |Average Discount off On-Demand|40% - 60%|31% - 54%|
      |Change AZ, Instance Size, Networking Type|Yes via ModifyReservedInstance API or console|Yes via ExchangeReservedAPI or console|
      |Change instance family, OS, Tenancy, Payment Options|No|Yes|
      |Benefit from Price Reduction|No|Yes|
      |Sellable on Reserved Instances Marketplace|Yes (Sale proceeds must be deposited in a US bank account)|Coming soon|
    - RI Attributes
      - Instance Type - designates CPU, memory, networking capability
      - Platform - Linux, SUSE Linux, RHEL, Microsoft Windows, Microsoft SQL Server
      - Tenancy - Default (shared) tenancy or Dedicated tenancy
      - Availability Zone (Optional)
        - If AZ is selected, RI is reserved and discount applies to that AZ (Zonal RI).
        - If no AZ is specified, no reservation is created by the discount is applied to any instance in the family in any AZ in the region (Regional RI)
        - Zonal RI can be changed to Regional AI or vice versa via the console or ModifyReservedInstance API
  - Spot Instance
    - Excess EC2 capacity that AWS tries to sell on market exchange basis
    - Customer creates a Spot Request and specifies AMI, desired instance types and other key information
    - Customer defines highest price willing to pay for instance. If capacity is constrained and other are willing to pay more, the instance can be terminated or stopped
    - Spot request Types
      - Fill and Kill (One Time only)
        - Spins up a spot instance if the price is higher than the going rate
        - It will get rid of that request
        - When the instance is terminated ephemeral data is lost
      - Maintain
        - Will re provision a spot instance if by chance the price rises too high and it gets terminated.
        - When the price goes back down maintain will recreate another spot instance
        - Same applies if the instance is manually terminated
        - Instances can be configured to Terminated, Stop or Hibernate until price point can be met again
      - Duration
        - A finite period time can be specified
          - For example, a spot instance can be booked for 3 hours
          - After 3 hours, the instance is terminated and the request is removed
      ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/spot-instance-pricing.png)
  - Dedicated instances vs Dedicated Hosts
    - Dedicated instances
      - Virtualized instances on dedicated hardware
      - May share hardware with other non-dedicated instances in the same account
      - Available as On-Demand, Reserved Instances and Spot Instances
      - Cost additional $2 per hour per region
    - Dedicated Hosts
      - Physical servers dedicated a single customer
      - Complete control of which instances are deployed on that host
      - Available as On-Demand or with Dedicated Host Reservation
      - Useful if there is a server-bound software licenses that use metrics like per-core, per-socket or per-VM
      - Each dedicated host can only run one EC2 instances size and type
- Cost Management Tools
  - AWS Budgets
    - Allows to set pre-defined limits and notifications if nearing a budget or exceeding the budget
    - Can be based on Cost, Usage, Reserved Instance Utilization or Reserved Instance Coverage
    - Use as a method to distribute cost and usage awareness and responsibility to platform users
  - Consolidated billing
    - Enable a single payer account that's locked down to only those who need access
    - Economies of scale by bringing together resource consumption across accounts
  - Trust Advisor
    - Runs a series of checks on the resources and proposes suggested improvements
    - Can help recommend cost optimization adjustments like reserved instances or scaling adjustments
    - Core checks are available to all customers
    - Full Trusted advisor benefits require a business or enterprise support plan
- Exam Tips
  - Costing in General
    - Understand the difference between CapEx and OpEx models
    - Understand TCO, ROI and the challenges faced in these activities
  - Cost Optimization Strategies
    - Know conceptually the variety of ways customers can approach cost optimization on AWS
    - Full understand the Cost Optimization Pillar
  - Tagging and Resource Groups
    - Understand the various common uses for tagging and ways to implement/enforce a tagging strategy
    - Know when and how to create Resource Groups, and don't be tricked into thinking they are anything more than a logical grouping.
  - Spot and Reserved instances
    - Know the differences and limitations for the different types of Reserved Instances, including Zonal and Regional
    - Understand how Spot Instances work and when they are best used
    - Understand Dedicated Instances and Dedicated Hosts
  - Cost Management
    - Know how and when to use AWS Budgets
    - Understand the benefits of consolidated billing
    - Know how Trusted Advisor can help customers optimize and improve their AWS Landscapes
  - Whitepapers
    - Cost Optimization Pillar: AWS Well-Architected Framework
      - https://d1.awsstatic.com/whitepapers/architecture/AWS-Cost-Optimization-Pillar.pdf
    - Maximizing Value with AWS
      - https://d1.awsstatic.com/whitepapers/total-cost-of-operation-benefits-using-aws.pdf
    - Introduction to AWS Economics: Reducing Cost and Complexity
      - https://d1.awsstatic.com/whitepapers/introduction-to-aws-cloud-economics-final.pdf
  - Videos
    - Building a Solid Business case for Cloud Migration
      - https://www.youtube.com/watch?v=CcspJkc7zqg
    - Running Lean Architectures: How to Optimize for Cost Efficiency
      - https://www.youtube.com/watch?v=XQFweGjK_-o
    - How Hess has continued to Optimize the AWS Cloud After Migrating
      - https://www.youtube.com/watch?v=1Z4BfRj2FiU
- Pro Tips
  - Be extra careful around the TCO and ROI minefield.
    - Take advise from a finance person
    - Find ways to translate soft costs into hard costs. Refer to academic research or studies
    - Harvard Business Review and MIT Sloan Management Review have articles on Organizational Change Management quantifying soft costs.
    - Take care when looking at the case studies from a vendor   -
  - The real benefit of a Cloud Migration is in Agility and Flexibility. Cost alone is not the strongest business case.
    - Initial migration costs are going to raise because it is something different
    - Cost is a malleable, it can go up and down
    - The real benefit is the agility and flexibility. It translates to cost savings, customer happiness and ability to respond to external or internal threats. They also translate into innovation.
  - Think of cost optimization as a long-term effort - don't spend too much time on trying to micro-manage it up-front
  - Implement a tagging strategy out of the gate!
  - Be aggressive in formulating a pilot project: Large-scale benefits make for dramatic business cases, where small-scale winds can easily be ignored
