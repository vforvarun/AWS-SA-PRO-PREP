**Deployment and Operations Management Module**

- Types of Deployments
  - Software Deployment
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/software-deployments.png)
    - Big Bang
      - Fastest and High Risk Deployment
    - Phased Rollout
      - Slower and Low Risk Deployment
      - Rolling Deployment
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rolling-deployment-1.png)
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rolling-deployment-2.png)
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/rolling-deployment-3.png)
      - A/B Testing
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/a-b-testing-1.png)
        - Used mostly in web-site or web-application deployments to test new features
        - A percentage of traffic can be sent using Route 53 weighted routing scheme to new deployment.
        - If all OK, then deploy all traffic to new deployment.
      - Canary Deployment
        - The methodology comes from mining times.
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/canary-release.png)      
      - Blue-Green Deployment
        - The goal of blue/green deployments is to achieve immutable infrastructure, where you don't make changes to your application after it's deployed, but redeploy altogether
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/blue-green.png)
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/blue-green-1.png)
        - Blue Green Methods
          - Update DNS with Route 53 to point to new ELB or instance
          - Swap Auto-scalaing Group already primed with new version instances behind the ELB
          - Change Auto-Scaling Group Launch Configuration to use new AMI version and terminate old instances.
          - Swap environment URL of Elastic Beanstalk
          - Clone stack in AWS Opsworks and update DNS
        - Blue Green Contraindication
          - Data store schema is too tightly coupled to the code changes
          - The upgrade requires special upgrade routines to be run during deployment.
          - Off-the-shelf products might not be blue-green friendly
    - Parallel Adoption
      - Slowest and Low Risk Deployment
- Continuous Integration and Deployment
  - Continuous Integration
    - Merge code changes back to main branch as frequently as possible with automated testing as you go.
  - Continuous Delivery
    - Automate release process to the point it can be deployed at the click of a button
  - Continuous Deployment
    - Each code change that passes all stages of the release process is released to production with no human intervention required
- Typical Development Lifecycle
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/development-lifecycle.png)
  - Test Automation
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/test-automation.png)
  - Continuous Integration
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/CI.png)
  - Continuous Delivery
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/continuous-delivery.png)
  - Continous Deployment
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/continuous-deployment.png)
- CI/CD Considerations
  - Objective is to create smaller, incremental compartmentalized improvements and features
  - Lowers deployment risk and tries to limit negative impact
  - Test Automation game must be STRONG
  - Feature toggle patterns useful for dealing with in-progress features not ready for release (versus more traditional branching strategies)
  - Microservice architectures lend themselves well to CI/CD practices
- AWS Development Lifecycle Tools
  - AWS CodeCommit
    - Hosted Git Repository
  - AWS CodeBuild
    - Compile code, Run Test and create deployment packages
  - AWS CodeDeploy
    - Deploys the deployment packages to EC2, Lambda, Elastic Beanstalk or ECS or even on-prem systems
  - AWS CodePipeline
    - Orchestration mechanism to do all the above together
  - AWS X-Ray
    - Helps with debugging
  - AWS CodeStar
    - Helps standardise the CI/CD using templates
- Elastic Beanstalk
  - Orchestration service to make it push-button easy to deploy scalable landscapes
  - Wide range of supported platforms - from Docker to PHP to Java to Node.js
  - Multiple Environments within Application (DEV, QA, PRD, etc)
  - Great for ease of deployment, but not great if you there is a need for control and flexibility
  - Layers of Elastic Beanstalk
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/elastic-beanstalk-components.png)
    - Application Layer
      - This is the management layer
      - Can comprise of multiple environments, these are instances, load balancers, auto-scaling groups, monitoring and so forth
      - Can have multiple application versions
  - Elastic Beanstalk Deployment Options
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/ebs-deployment-options.png)
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/blue-green-swap-url.png)
- CloudFormation
  - Infrastructure as code
  - Using JSON or YAML, model and provision entire landscapes
  - Releatable, automatic deployments and rollbacks
  - Nest common components for reuasability
  - Supports over 300 Resource Types (components of AWS Services). Custom Resource types are also possible.
  - Concepts
    - Templates
      - JSON or YAML text file that contains the instructions for building-out the AWS environment
      - Stacks
        - The entire envrionemnt described by the template and created, upldated and deleted as a single unit
      - Change Sets
        - A summary of proposed changes to the stack that will show those changes might impact existing resources before implementing them
      - The only required parameter in a template "Resources"
      - Stack Policies
        - Protect specific resources within the stack from being unintentionally deleted or updated.
        - Add a Stack Policy via the console or CLI when creating a stack.
        - Adding a Stack Policy to an existing stack can only be done via CLI
        - Once applied, a Stack Policy cannot be renamed but it can be modified only via CLI.
  - CloudFormation Best Practices
    - AWS provides Python "helper scripts" which can help you install software and start services on EC2 instances
    - Use CloudFormation to make changes to the landscape rather than going directly into the resources
    - Make use of Change Sets to identify potential trouble spots in the updates.
    - Use Stack Policies to explicitly protect sensitive portions of the stack.
    - Use a version control system such as COdeCommit or GitHub to track changes.
- Elastic Container Service
  - Managed, highly available, highly scalable container platform

  | Amazon ECS  | Amazon EKS  |
  |  ---------- | ----------  |
  | AWS-specific platform that supports Docker containers | Compatible with Upstreak K8s so its easy to lift and shift from other K8s |
  | Considered simpler to learn and use | Considered more feature-rich and complex wiht a steep learning curve  |
  | Leverages AWS services like Route 53, ALB and CloudWatch  | A hosted K8s platform than handles many things internally |
  | "Tasks" are instances of containers that run on underlying compute but more or less isolated  | "Pods" are containers co-located with one another and can have shared access to each other  |
  | Limited extensibility | Extensible via a wide variety of third-party and community add-ons |

  - Launch Types

  | Amazon EC2 Launch Type  | Amazon Fargate Launch Type  |
  | ----------------------  | --------------------------  |
  | Explicitly provision EC2 instances  | The control plane asked for resources and Fargate automatically provisions  |
  | Client responsible for upgrading, patching, care of EC2 pool  | Fargate provisions compute as needed |
  | Client must handle cluster optimization | Fargate handles cluster optimization |
  | More granular control over infrastructure | Limited control, as infrastructure is automated |

- API Gateway
  - Managed, highly available service to front-end REST APIs
  - Backed with customer code via Lambda, as a proxy for another AWS service or any other HTTP API on AWS or elsewhere
  - Regionally based, private or edge optimized 9deployed via CloudFront)
  - Supports API Keys and Usage Plans for user identification, throttling or quota management
  - Using CloudFront behind the scenes and customer domains and SNI are supported
  - Can be published as products and monetized on AWS Marketplace
  - Example of using API gateway
  ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/api-gateway.png)
    - API Gateway can cache responses
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/api-gateway-cache.png)
- AWS Config
  - Allows you to assess, audit and evaluate configurations of AWS resources
  - Very useful for Configuraiton Management as part of an ITIL program
  - Creates a baseline of various configuraiton settings and files then can track variations against that baseline.
  - AWS Config Rules can check resources for certain desired conditions and if violations are found, the resources is flagged as "non-compliant"
  - Examples of Config Rules
    - Is Backup enabled on RDS?
    - Is CloudTrial enabled on the AWS account?
    - Are EBS volumes encrypted?
- AWS OpsWorks
  - Managed instance of Chef and Puppet - two very popular automation platforms
  - Provide configuration management to deploy code, automate tasks, configure instances, perform upgrades etc
  - Has three offerings
    - OpsWorks for Chef Automate
    - OpsWorks for Puppet Enterprise
    - OpsWorks Stacks
  - OpsWorks for Chef Automate and Puppet Enterprise are fully managed implementation of each respective platform
  - OpsWorks Stacks is an AWS creation and uses an embedded Chef solo client installed on EC2 instances to run Chef recipes
  - OpsWorks Stacks support EC2 instances and on-prem servers as well with an agent
  - Stacks are collections of resources needed to support a service or application
  - Layers represent different components of the application delivery hierarchy
  - EC2 instances, RDS instances, and ELBs are examples of Layers
  - Stacks can be cloned - but only within the same region
  - OpsWorks is a global service. But when a stack is created, a region must be specified and that stack can only control resources in that region
    - For example, EC2 instance created in EU-CENTRAL-1 cannot be managed from OpsWorks stack created for US-EAST-2
- AWS System Manager
  - Centralized console and toolset for a wide variety of system management tasks
  - Designed for managing a large fleet of systems - tens or hundreds
  - SSM Agent enables System Manager features and support all OSs supported by OS as well as back to Windows Server 2003 and Raspbian (Raspberry Pi deployment of Debian)
  - SSM Agent installed by default on recent AWS-provided base AMIs for Linux and Windows
  - Manages AWS-based and on-prem based systems via the agent
  - Components of AWS System Manager
    - Inventory Service
      - Collects OS, application and instances metadata about instances
      - Which instances have Apache HTTP Server 2.2.x or earlier?
    - State Manager
      - Create states that represent a certain configuration is applied to instances
      - Keep track of which instances have been updated to the current stble version of Apache HTTP server.
    - Logging
      - CloudWatch Log agent and stream logs directly to CloudWatch from instances
      - Stream logs of our web servers directly to CloudWatch for monitoring and notification
    - Parameter Store
      - Shared secure storage for config data, connection strings, passwords etc
      - Store and treieve RDS credentials to append to a config file upon boot
    - Insight Dashboard
      - Account-level view of CloudTrail, Config, Trust Advisor
      - Single viewport for any exceptions on config compliance
    - Resource Groups
      - Group resource through tagging for organisation
      - Create a dashboard for all assets belonging to our Production ERP landscape
    - Maintenance Windows
      - Define schedules for instances to patch, update apps, run scripts and more
      - Define hours of 00:00 to 02:00 as maintenance windows for Patch Manager
    - Automation
      - Automating routine maintenance tasks and scripts
      - Stop DEV and QA instances every Friday and restart Monday Morning
    - Run Command
      - Run commands and scripts without logging in via SSH or RDP
      - Run a sheel script on 53 different instances at the same time
    - Patch Manager
      - Automates process of patching instances for updates
      - Keep a fleet at the same patch level by applying new security patches during next Maintenance window
      - Patch Baselines
        - AWS provides default patch baselines for all OSs
        ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/pm-baselines.png)
        - It is a set of rules to qualify patches for patching.
        - Custom patch baselines can be created
        - Wait for x number days before deploying
    - AWS System Manager Documents
      - JSON or YML files to perform a specific function
      - Types
        - Command Document
          - Used with Run Command State Manager
          - Run command uses command documents to execute commands. State Manager uses command documents to apply a configuration. These actions can be run on one or more targets at any point during the lifecycle of an instance.
        - Policy Document
          - Used with State Manager
          - Policy documents enforce a policy on your targets. If the policy document is removed, the policy action (for example, collecting inventory) no longer happens
        - Automation Document
          - Used with Automation
          - Use automation documents when performing common maintenance and deployment tasks such as creating or updating an Amazon Machine Image (AMI)
- Exam Tips
  - Types of Deployments
    - Understand the types of deployments and when each might be preferred in a given situation
    - Know the various ways AWS can support Blue/Green deployments and when Blue/Green is not recommended
  - Continuous Integration and Continuous Deployment
    - Understand conceptually Continuous Integration, Continuous Delivery and Continuous Deployment and their considerations
    - Know what AWS tools can be used to facilitate these methods of deployment.
  - Elastic Beanstalk
    - Know the components of Elastic Beanstalk and the platforms supported
    - Understand the deployment options with Elastic Beanstalk and the tradeoffs for each
  - CloudFormation
    - Understand how CloudFormation delivers infrastructure as code and the benefits of that
    - Go through other CloudFormation trainings availabe in acloud.guru
  - Elastic Container Service
    - Know the difference between ECS and EKS - as well as the uniqueness of each
    - Understand the difference between EC2 Launch Types and Fargate Launch Types
  - API Gateway
    - Understand what (and how) an API is deployed on API Gateway
    - Remember that API Gateway is designed to serve up REST APIs
    - Remember the Caching Feature
  - Management Tools
    - Know when and what one can expect when using AWS Config and understand the purpose of Config Rule
    - Know the different flavours of AWS OpsWorks and, conceptually, what Chef and Puppet offer
    - Understand the difference between AWS OpsWorks Stacks and AWS OpsWOrks for Chef Automate
    - Remember that OpsWorks is a global service, but you can only manage resources in the region in which OpsWorks stack was created
  - System Manager
    - Know the various services under the System Manager heading and how they help simplify management of large landscapes
    - Can manage both AWS-based and on-prem system so long as they are supported OSs (No IBM AIX for example)
    - Understand Patch Manager pre-defined baselines and that they act as a pre-approval gatekeeper
    - Understand the various SSM document types and their purposes
  - Whitepapers
    - Infrastructure as Code
      - https://d1.awsstatic.com/whitepapers/DevOps/infrastructure-as-code.pdf
    - Practicing Continuous Integration and Continuous Delivery on AWS
      - https://d1.awsstatic.com/whitepapers/DevOps/practicing-continuous-integration-continuous-delivery-on-AWS.pdf
    - Overview of Deployment Options on AWS
      - https://d1.awsstatic.com/whitepapers/overview-of-deployment-options-on-aws.pdf
  - Videos
    - Deep Dive on AWS CloudFormation
      - https://www.youtube.com/watch?v=01hy48R9Kr8
    - Moving to Containers: Building with Docker and Amazon ECS
      - https://www.youtube.com/watch?v=Qik9LBktjgs
    - Continuous Integration Best Practices for Software Development Teams
      - https://www.youtube.com/watch?v=GEPJ7Lo346A
- Pro Tips
  - The Cloud will open a whole new world of possibilities to landscape management and project deployment
  - Blue/Green is not just for nimble web apps in the cloud
  - It will be a hard transition to move to Infrastructure as Code. Training is key
  - API Gateway + Lambda create loads of opportunity for agility and efficiency in the way of serverless platform
