# Prerequisites
#
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later



# Services Covered

- AWS CLI
- Elastic Beanstalk
- Route53

# Pricing
You may be charged for the domain name and other services used for this project. But most of the service used is comes under free-tier

# Refactoring with AWS

Add the arvhitecture

It is a multi-tier web application stack want to re-architect in the cloud.

The idea behind this project is;

-	To boost agility 
-	Improve business continuity


In this project instead of using IAAS service we use PAAS and SAAS services

For the frontend app we use EBS service (Elastic BeanStalk)

### Why we use EBS?

-	This is a PAAS (Platform As a Service) service 
-	It will create our application instance and host our application on it
-	No need to manage EC2 instance manually  EBS will take care of it
-	EBS will also have load balancer to route the traffic
-	Also it will have autoscaling
-	And also S3 buckets to store the artifacts code.

Basically we no need to maintain all the above mentioned services EBS will take care of that. That is the benefit of using PAAS services

### For backend service we use
-	RDS for databases (PAAS service)
-	Elastic cache for memcached
-	Active MQ for rabbitmq
-	Route53 for DNS
-	Cloudfront for CDN (Content Delivery Network) [to decrease the latency]

# Let's Begin 😎

### Step 1

**Create Security groups and Key pair**

- Log in to AWS management console.
- First we can create a key pair. To create key pair follow the below steps;
- In the EC2 console under Network & security click on Key pairs and again click on  create key pairs.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/34ce76a6-0815-4418-80ee-aee99676fc08)

You can choose .ppk extension if you want to connect the instance using putty otherwise select .pem

![image](https://github.com/Fawazcp/aws-project/assets/111639918/7c15a840-c7d3-46e9-a963-847659c5d6d3)

Next we need to create security group for the backend service (database, memcached and rabbitmq)

- To create security group follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/349dd66a-780f-4003-b08a-25b2b009ae2e)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/58bc60ad-9569-42b1-8057-c201521b3da0)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/8309dcea-87a3-45e9-9ba1-ce42a1f2a87b)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/36d5eb78-2e00-4a94-8fda-e5f891dce166)


- Add any rule and create security group because we want add other rule from the same secuity group.
- Click on create security group and open the security group again. Because we want to change the  inbound rule to All traffic from the same security group

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f9c2baa1-abbf-4062-8a19-d921570db7f1)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/84c101eb-525f-4c99-bd35-15ec80c4620e)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a17c2c6a-a669-406e-8eb8-8a4b0412f459)

- Add inbound rule type All traffic from same security group and click on save rules.
- We want to add one more inbound rule in this security group to allow EBS ec2 instances. But for that we need to have our EBS is ready. So we will add that later.

### Step 2

**Create RDS (Relational Database Service)**







