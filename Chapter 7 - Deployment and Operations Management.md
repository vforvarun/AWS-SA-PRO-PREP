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
-
