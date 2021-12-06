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

## 4 IaaS Storage

- EBS (Elastic Block Store)
  - 컴퓨터에 HDD 추가와 비슷 (떼서 다른데 붙일수도)
- EFS (Elastic File System)
  - <img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70/learn/modules/core-aws-services/store-and-retrieve-data-with-aws/images/41882e622c8ba490b48ad64d30ac128c_b-934813-d-d-538-4167-9099-27569-a-28-b-024.png"  width="40" height="40"/>
  - AWS NAS (Network Attached Storage)
  - EC2에 mount 하려고 하면, 보안 때문에 막힘. inbound rule
  - 특정 scurity group id를 사용하면, 어디에서 오든 열어주는 방법 사용 가능. (그룹단위로 ACL)
    - IP 범위 정해주고 바꾸고 할 필요 없음..
- S3 (Simple Storage Service)
  - EBS, EFS 보단 느리지만, 따로 서버가 필요없다. 
  - 고객 사진이 있을수 있으니, 접근권한을 잘 
  -  공개용 비공개용S3 버킷 따로 만드는거 추천
  - 최악의 S3 breach 사례 https://businessinsights.bitdefender.com/worst-amazon-breaches
- CLI (Command Line INterface)
  - 로컬에 설치하기
- AWS SDK
  - S3 소스코드로 접근 가능 
- IAM Roles
  - 비밀번호 서버에 위험하게 넣지 않고도 접근 제어 가능 
- S3 Glacier
  - S3에 있는 오래된 데이터 옮겨오는 곳. 저렴하다.
- CloudFront
  - 정적, 동적 웹 컨테츠들을 빠르게 불러올수있게 해준다.

### QUIZ

- What is a **valid use case** for a public S3 bucket?
  - storing database backups
  - sharing server log files within your team
  - serving web content with CloudFront (정답)
  - sharing reports with external vendors
- Which **S3 storage class** is best for storing the only copy of your latest backups for your databases? 
  - S3 Glacier
  - S3 Databases
  - S3 Standard (정답)
    - *S3 Standard*-IA is for data that is accessed less frequently, but requires rapid access when needed. 
  - S3 One Zone-IA
    - 재현 가능한 infrequently accessed data 에 사용
    - 예, storing secondary backup copies of on-premises data or for storage that is already replicated in another AWS Region for compliance or disaster recovery purposes.
- Why is using a **IAM role** better security than using access keys?
  - IAM roles grant permissions without leaving a key on the server that can be compromised.
- The AWS SDK requires that you always hard code it with an access key to access AWS services.
  - FALSE
    - You can use IAM roles to grant access to AWS resources without risking your access keys.
- What CLI command will list all of the S3 buckets you have access to? 명령어
  - aws s3 ls
- What is the order of the speeds of the AWS storage services by fastest (first in the list) to slowest (last in the list)? 속도순서
  - EBS, EFS, S3
- If you are unable to mount an EFS volume, what is the most likely problem?
  - A security group or firewall is blocking the network traffic to the EFS endpoint.
- What is the difference between an Amazon Machine Image (AMI) and an EBS snapshot?
  - AMIs include everything needed to relaunch the instance, whereas an EBS snapshot is only a backup of the data volume.

## 5 IaaS Networking

- [VPC](https://docs.aws.amazon.com/vpc/index.html) (Virtual Private Cloud)
- [NAT gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) and [Internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)
  - NAT는 한쪽으로만 
  - Internet는 양쪽으로 
- Elastic IPs
  - AWS 하드웨어가 죽으면, 새로운 IP를 계속 할당받아, 코드의 IP 주소를 계속 바꿔야하는 상황이 올수 있따.  
  - 이것도 다달이 돈내야되니 사용안하면 멈추기.
- Using VPNs to access private subnets 
  - AWS에 있는 DB에 바로 접근 가능한가봄
  - site-to-site VPNs: on-premese 데이터 센터와 VPC와 연결가능 
  - 터널을 통해 데이터를 안전하게 옮길 수 있음.
- Scaling with Elatic Load Balancer (ELB)
  - 종류
    - NLB (Network): 빠름/기능적음/OSI Layer 4 Routing
    - ALB (Aplication): HTTP Routing rules/가장흔히사용/OSI Layer 7 Routing
- DNS with Route53

### QUIZ

- If you want to use Route53, do you have to transfer your domain name to Route53?
  - No, you only need to point your domain's DNS servers to the Route53 servers.
- When choosing an Elastic Load Balancer (ELB), what is the difference between an Application Load Balancer (ALB) and a Network Load Balancer (NLB)?
  - An ALB is used for HTTP traffic, and an NLB is used for traffic that requires speed, like low-latency streaming services.
- What is a trade off of placing your database server within a private subnet within your VPC?
  - You cannot access your database server with a database client tool unless you use a Client VPN connection.
- Can you lose the public IP address associated with your EC2 instance?
  - Yes, if you stop and start the instance.
- What is the difference between an Internet gateway and a NAT gateway?
  - A NAT gateway will not allow public originating internet traffic to pass to a server, but an Internet gateway will allow it.
- Why would you create a private subnet within a VPC?
  - to isolate database and file servers from public internet traffic








