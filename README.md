## Course Details

In this course, Jeremy Villeneuve breaks down key AWS services, giving developers a high-level look at the different ways they can host applications within AWS, as well as how to decide which services will fit their use case.

## Learning objectives

- Proper security for the AWS root account
- Identity and Access Management (IAM)
- Regions and availability zones
- Creating an EC2 instance web server
- Editing security groups
- Storing and serving files from AWS
- Scaling with Elastic Load Balancer (ELB)
- Hosting databases within AWS
- Running containers on AWS
- Machine learning services within AWS
- DevOps with AWS
- Security on AWS

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

## 6 Database as a Service (DBaaS)

- 클라우드 제공자가 DB와 백업을 관리해준다.
- Database Migration Service
  - 온프로미스를 클라우드 환경으로 가져오게 해준다. 
- RDS (Relational Database Service)
  - 지원 가능: PostgreSQL, MySQL, SQL Server, Oracle
  - PostgreSQL에서 single-AZ가 있고 multi-AZ가 있는데, 인스턴스 여러개 돌릴 거면 멀티 
- Amazon Aurora 
  - MySQL과 PostgreSQL와 비슷한데 AWS에서 편하게 관리해준다. 
  - 비용이 1/10
- NoSQL
  - DynamoDB
- DocumentDB
  - MongoDB랑 비슷 
- ElasticCache
  - 인메모리 캐시를 관리해준다. 
  - Redis, Memcachsed 
- 빅데이터 DB 
  - Data Lake 
    - 분석, 머신러닝, 실시간 데이터 모든게 들어감 
  - RedShift 
    - 클라우드에서 완벽하게 관리되는 페타바이트급 데이터 웨어하우스 서비스입니다. 작게는 수백 기가바이트부터 시작하여 페타바이트 이상까지 ~
  - [EMR](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=kbh3983&logNo=221084176372) (Elastic MapReduce)
    -  Hadoop, Spark 
  - Athena
    - S3에 있는 데이터를 직접 간편하게 분석할 수 있는 대화형 쿼리 서비스
- 메시징 서비스 
  - Kinesis
    - 실시간 데이터 
  - SQS (Simple Queue Serivce) 
  - SNS (Simple Notification Service)
  - 3개 중에 어떤걸 골라야 할까? 는 올바른 질문이 아니고, 각각 언제 사용해야될까로 질문
- 더 공부를 위해 Designing Data-Intensive Applications 책 추천 

### QUIZ

- You have 10,000 Internet of Things devices that are sending in real-time telemetry on vehicle movement at 5-second intervals. What is the best service to capture this data?
  - SNS
  - MQ
  - SQS
  - Kinesis (정답)
- If you need to store large streams of user activity data coming from a web and mobile application and generate aggregated reports on usage patterns, which database is the best choice?
  - Aurora
  - RDS for SQL Server
  - Neptune
  - Redshift (정답)
- What in-memory caching server is NOT supported by ElastiCache?
  - Memcached
  - Redis 3
  - Redis 5
  - Elasticsearch (정답)
- Which database is a NoSQL database type that can quickly store and retrieve key value pairs?
  - DynamoDB
- You are going to host an application that uses a MySQL database, but you do not want to manage scaling or database administration tasks. What is the best choice for hosting this database?
  - Aurora
- What is a valid use case for a queue or message broker that would sit in front of your relational database?
  - when you need to ingest a large stream of data

## 7 PaaS (Platfrom as a Service )

- Elastic Beanstalk
  - 클라우드에 어플리케이션 관리와 배포를 할 수 있다.
- Running container on AWS
  - [ECR](https://www.youtube.com/watch?v=H73uX0TOX9g) (Elastic Container Registry)
    - 컨테이너 관리 
- Lambda
  - FaaS (Function as a Service ) 개념
  - 싱글 execution을 계속 할 수 있음 
  - Serverless Architecture 
- Batch Processing
  - EC2 Spot Instance
    - 대폭 할인된 가격으로 사용할 수 있는 AWS 클라우드의 예비 컴퓨팅 용량입니다.
  - Step Function
  - Simple Workflow (SWF)

### QUIZ

- What is usually the best service for implementing a multi-step workflow within AWS?
  - Step Functions
- You can create an entire web application using Lambda functions.
  - TRUE
    - API Gateway and Application Load Balancer can pass requests and responses to and from Lambda functions.
- Which service can host a Docker container? 
  - Elastic Compute Cloud (EC2)
  - Elastic Beanstalk
  - All of these services can be used as a Docker host. (정답)
  - Elastic Container Service (ECS)
- Which AWS service can host the web application server for a Wordpress site?
  - Elastic Beanstalk

## 8 Software as a Service 

- User Authentication 
  - Cognitive 
    - 페이스북 구글 로그인 가능 
- Mobile Service
  - AppSync
  - Amplify
    - 사용하기 쉬운 CLI 제공 
- Machine Learning
  - SageMaker 
    - 러닝머신 모델을 만들게 도와준다. 
  - Lex
    - 챗박스 만들 때 사용 
  - Personalize
    - 제품 추천할때 사용 가능
  - Polly
    - 목소리 
  - Rekognition
  - Textract
  - Translate 
  - Transcribe
- Media Services
  - MediaConvert 
- IOT
  - AWS IOT Core 장치들 관리 가능

### QUIZ

- To use IoT Core to track the state of your Internet of Things (IoT) appliances, you must select a device that is already certified for use with IoT Core.
  - FALSE
    - You can integrate any device with IoT Core if you can communicate with it using HTTP, WebSockets, or MQTT.
- What service can be used to transcode and splice together movie clips?
  - MediaConvert
- What service will identify the things it finds in an image?
  - Rekognition
- What service can manage a REST API layer?
  - API Gateway
- What service can handle user authentication for your web and mobile applications?
  - Cognito










