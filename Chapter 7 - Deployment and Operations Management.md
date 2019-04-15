**Deployment and Operations Management Module**

- Types of Deployments
  - Software Deployment
    ![alt text](https://github.com/vforvarun/AWS-SA-PRO-PREP/blob/master/images/deployment-types.png)
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
