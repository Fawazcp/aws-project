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

**Create backend service**

**1-	Create Database. **

In order to create database we use the service called RDS (Relational Database Service), Elasticache and AmazonMQ
- First lets create RDS
- To Create RDS follow the below steps;

**Task 1**

- First we need to create a subnet group.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/d1c67d92-18ce-4904-91cb-a56852710c6c)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/aa9b1355-eaec-4cbf-9960-b8c517399b6b)

Give any name and select the default VPC

![image](https://github.com/Fawazcp/aws-project/assets/111639918/522156be-20f5-4a52-967c-01f40279e8f2)

- Select all the AZ and subnet then click on create.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e926c1a0-1af9-41ad-8962-fb1fc5b7be21)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/26e19805-c220-4a3d-bbed-e1bbaf3a0511)

**Task 2**

- Create parameter groups
- It is the configuration of RDS database.
- To create parameter group follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/96c0a15d-d804-4c84-9ca8-1ab9d5bb4f97)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/611a9568-e554-4f4b-82ae-b5ac6e0356bd)

Make sure to select mysql5.7

**Task 3**

Now it’s time to create database
- Follow the below steps to create RDS

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f0ae652c-e432-4012-92ba-8912bd52c500)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/ab713f29-179e-4e0f-bb8d-1c6060f0137c)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c9db069a-d53d-4f59-b7e9-ff36b97224d0)

- Make sure you select the engine version MySQL 5.7. and templates you can select Dev/Test or Free tier.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/fa5d4f81-ffd6-4f18-b37b-4969cfa4a5ae)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/9f4f17af-226f-4c18-a04e-e1498c58cf3a)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/58887943-9fe7-408f-9057-04c98634d2e2)


Storage you can leave as default.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/bcf8e175-2120-4a1c-8db6-c476d84f763d)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/3a9fdc2d-09fa-4dbc-9d8a-cc2a3d0a0158)


- Select the security group we created for this project.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/bf5191ae-7184-421c-ba7c-ea869a224898)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/9c9c3b5a-eedf-4622-a5b0-ccd5ed651141)

- Select the parameter group we created.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/3f6bb26c-0267-4023-9bab-4d55cf620dc1)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1eb87f29-c07a-4c4f-925a-42034352c3bd)


- No need to worry about the cost it is the monthly cost and we are not going to keep the databases for a month so need to worry. Still we may get some billing but we have set as low as we can

![image](https://github.com/Fawazcp/aws-project/assets/111639918/62db5f48-1259-451d-981b-664b5e803d93)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/8fdc4002-f94d-4512-b7fc-41a0307b0cda)


- Creating database take some time. Click on view credentials and copy the credentials in notepad or somewhere.


![image](https://github.com/Fawazcp/aws-project/assets/111639918/3833322c-5ed8-4ab0-adf8-d01d5744160a)


**2- Create Elasticache (inmemory cache)**

- To create elasticache follow the below steps;

**Task 1**

- First we need to create a parameter group same as we did for the RDS.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/2264e8c9-a3f2-4408-921b-b77ad737d169)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/3299f106-0659-4c1e-a1dd-00c91078bd17)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/8b60dd56-3fa9-4ed6-9f4e-3838e4d36298)

**Task 2**

Next we need to create subnet group
- To create follow the steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/cdac6b0d-af9e-4426-a2aa-37c3609be6f2)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/c26a4e79-effd-4b31-a98d-b85ec6da9a87)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/ce1b0547-d079-49e5-943f-78b48d724bcd)


- Select all the subnets available if not selected by default and click on create.

**Task 3**

- Now it’s time to create our memcached cluster.
- Let’s see how to create memcached cluster.


![image](https://github.com/Fawazcp/aws-project/assets/111639918/c4f85c8c-cbda-4861-a31d-f003401004ca)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/cdbd4c91-0262-49d8-9053-dbfbd1d785a2)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/852b7fed-adb5-4376-a97b-a259ca8504c8)


Select the parameter group we created and make sure that node type you select as t2.micro otherwise you may end up with more billing

![image](https://github.com/Fawazcp/aws-project/assets/111639918/ae41bb0c-c8b2-48f0-862e-c49075acae5e)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/6144763a-cff3-4f58-ad86-6ae4f778367f)


Select the security group we created for the backend service.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/4a056ec4-0660-4ad4-8ea2-b50ebcc6f0a9)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/51dbcde8-0152-46d5-8fe5-d3f535254aeb)


Review and click on create. It may take some time to create.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/66b1d527-15e3-41d8-91dd-c42837a979fa)

**3- Create Amazon MQ (RabbitMQ)**

To create Amazon MQ follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/101b6faa-efc5-4e4f-8049-0fa60a207c3b)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/97a0239a-3611-40b5-b708-ab0392f14f7d)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/259ecf67-35c2-47ff-9078-3e92914deb73)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/1941ecde-ea1e-4055-aa8a-54233b3628c4)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/9502d821-0df5-44c0-ab9b-6fe4765bf5e7)


- Give a username and password and click on next. (Make sure to remember the password or save anywhere because once it is created you will not be able see this)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/6cf90ebc-a1c0-4537-9c03-eae9df8cfd0f)


- Select the security group we created for the backend service.

 ![image](https://github.com/Fawazcp/aws-project/assets/111639918/4ef874b1-796f-455a-9d38-e3ba25c038cc)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/7481a88c-db18-4187-9929-9bcfa044c1a4)


Review everything and click on create broker.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e9a86d3b-43ae-4b08-9aba-ed88e3d52bd4)


This may also take some time to get it created.

### Step 3

**Database initialization (RDS)**

- Go to RDS and copy the endpoint.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a6d65b03-d82d-461a-afde-231aad68f18d)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/7553f3d4-8ecf-40a2-88ce-b0e76b9d7e23)


- Copy the endpoint and keep it somewhere 
- We need to launch an instance for the RDS database for temporary to initialize the database.
- The purpose of launching the instance is	Install mysql client,Log into RDS instance , And initialize the database

Go to EC2 and click on launch instance.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/949a2cf0-7473-43b8-a71c-15fe945e8f86)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f6f5dde9-c4fd-4657-b4a7-a3eb455d72da)


Select any key if you have otherwise create new key.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/aef4ba1f-7156-4677-9be5-80d6ee0c4a6f)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/4a71e7f1-8341-45e8-a047-c8a9492840bc)

- Connect the instance using gitbash or putty or;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/162e0001-e4b0-4b48-aeb1-5ef2f26fdf47)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/7a6a0377-f25b-4fc0-9806-d5158e23c66a)

```
# once you connect the instance enter the below command

sudo apt update –y
sudo apt install mysql-client –y
```
