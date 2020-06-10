we choose NAT Instance over a NAT Gateway because we need following: 
1. We want to ability to detach elastic IP;
2. We want to use Security group for the instance;
3. We want to allow connections initiated from public internet to our private instances. 


Deployment types: 
1. Rolling Deployment: New launch configuration
2.A/B Testing: Route53  90% 10%
3. Canary Release: Route53 - ALB
4. Blue-Green Deployment: Route53-- ALB current and ALB New 
    Update DNS with Route53 to point to a new ELB or instance
    Swap Auto-scaling group already primed with new version instances behind ELB
    Change Auto-scaling group launch configuration to use new AMI version and terminate old instances
    Swap environment URL of Elastic Beanstalk
    Clone stack in AWS OpsWorks and update DNS

Blue-Green Contraindication: 
    Data store schema is too tightly coupled to the code changes
    The upgrade requires special upgrade routines to be run during deployment
    Off-the-shelf products might not be blue-green friendly
    
Elastic Beanstalk Deployment Options: 
1. All at once: All old version instances stopped and all new ones started, shortest time, with Downtime, manual Rollback;
2. Rolling: One by one, more time, no downtime, manual rollback;
3. Rolling with Additional batch: Lanuch new version instances before taking any old version instances out of service, 
    longer time, no downtime, manual rollback
4. Immutable: Launch a full set of new version instances in a separate auto-scaling group and only cuts over when
    health check is passed, longest deployment time, no downtime, rollback via termination of new instances
5. Blue/Green: CNAME DNS entry changed when new version is fully up, leaving old version in place util new is 
    fully verified, longest deployment time, no downtime, rollback via swap URL
              
    
    
    



