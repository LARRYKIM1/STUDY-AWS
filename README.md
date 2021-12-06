## 1 Setup

생략

## 2 On-Promese to AWS

### QUIZ

- What is the difference between an availability zone and a region? 
  - A region is a geographical group of several availability zones that can consist of several individual data centers.

## 3 IaaS Compute

- AWS EC2
  - 인스턴스 stop 후 start하면, Virtual Machine이 다른 물리적 서버로 이동해갈 수 있다. 
  - image = 스크린샷 (AMIs)

### QUIZ

- What is the difference between horizontal and vertical scaling?
  - **Vertical** scaling is increasing the physical hardware to a single server. **Horizontal** scaling spreads traffic across several identical servers.
- How does a security group protect your cloud infrastructure?
  - All traffic is blocked by default and a security group rule allows external traffic. A security group with no rules by default means all traffic is being blocked.
- When creating a new EC2 instance, how do you access it from the public internet?
  - Auto-assign a new public IP address.
- You will install a database engine on your EC2 instance that will continuously consume a **large amount of CPU** resources. What is the best EC2 instance type?
  - t3.xlarge
    - T class instances use CPU credits and can be throttled if they consistently use high amounts of CPU.
  - p3.2xlarge
    - P class instances are for graphics accelerated applications.
  - r5.xlarge
    - R class instances are memory optimized, but this use case would work better with an M class instance that doesn't use CPU credits.	
  - m5.xlarge (정답)
- If you switch computers and have lost your EC2 keypair, how do you connect to your EC2 existing instances?
  - Follow the instructions in the AWS documentation to generate a new keypair and recover your existing instances.

## 2 IaaS Storage

- EBS (Elastic Block Store)
  - 컴퓨터에 HDD 추가와 비슷 (떼서 다른데 붙일수도)
- EFS (Elastic File System)
<img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/core-aws-services/store-and-retrieve-data-with-aws/images/41882e622c8ba490b48ad64d30ac128c_b-934813-d-d-538-4167-9099-27569-a-28-b-024.png"  width="10" height="10"/>
  - ![title](https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/core-aws-services/store-and-retrieve-data-with-aws/images/41882e622c8ba490b48ad64d30ac128c_b-934813-d-d-538-4167-9099-27569-a-28-b-024.png){: width="10" height="10"}
  - AWS NAS (Network Attached Storage)
- S3 (Simple Storage Service)
  - 

<img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/core-aws-services/store-and-retrieve-data-with-aws/images/41882e622c8ba490b48ad64d30ac128c_b-934813-d-d-538-4167-9099-27569-a-28-b-024.png"  width="10" height="10"/>











