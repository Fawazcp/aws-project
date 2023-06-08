# Prerequisites
#
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later



# Services Covered

- AWS CLI
- Elastic Beanstalk
- Route53
- IAM Role
- Relational Database Service (RDS)
- AmazonMQ 
- Elasticache
- S3

# Pricing
You may be charged for the domain name and other services used for this project. But most of the service used is comes under free-tier

# Refactoring with AWS

![Untitled Diagram.drawio.png](https://github.com/Fawazcp/aws-project/blob/aws-Refactor/Untitled%20Diagram.drawio.png)


It is a multi-tier web application stack want to re-architect in the cloud.

The idea behind this project is;

-	To boost agility 
-	Improve business continuity


In this project instead of using IAAS service we use PAAS service

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

![image](https://github.com/Fawazcp/aws-project/assets/111639918/926ee317-082f-4363-9ce7-2ee4a7913c51)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a7b92376-f1c9-4f1f-a3d9-20f55763e6b5)

Now we can try to log in to RDS endpoint but we can’t because we have not added port for mysql in the security group.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/2d927ac6-3fed-457e-977b-5c6e786f0a6f)


- Go to backend security group and add inbound rule PORT 3306 from the ec2 intance security we created now.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/3c9100c9-c9fb-4a1a-a2cd-602dd54bd2cd)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/d56cabe7-0db3-452a-903f-88037c62aaad)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/9ad81334-4cbe-4e24-a76c-acc375682fae)


Make sure you added the security group we created while launching the instance under Source.
-	Go to mysql-server and try again and we will able to connect the database.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b2e936df-9a1a-4a5d-b344-e62e2d8df732)


And we can see the database named accounts that we added while creating RDS.

```
exit;
# enter this command to Exit from the database.
```

Next we need to clone the source code from the github repo.
To clone enter the below command inside the mysql server.
-	git clone https://github.com/devopshydclub/vprofile-project.git

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c821b4a6-f44c-4a89-b58b-a3a62e5f44a6)


- We need to switch branch to aws-Refactor.
- To switch branch enter the below command

![image](https://github.com/Fawazcp/aws-project/assets/111639918/7fdb6057-b2f6-486c-a17f-99481563d7f1)


What we have achieved is 
-	Cloned the source code from project GitHub to the mysql server
-	Switch branch to aws-Refactor
-	Change directory to db_backup.sql file.
-	Logged into RDS using the endpoint and added the tables from db_backup.sql file  into accounts database.


Use the below command to see the tables in the accounts database

```
mysql -h awsproject-rds-mysql.cxk3thwnepul.us-east-1.rds.amazonaws.com -u admin -pR8eMcnMSJH1cwQcQSNly accounts
```

![image](https://github.com/Fawazcp/aws-project/assets/111639918/cf006c22-6c20-4ddc-ada3-dd25d6cedf4d)


- Exit from the database we have initialized the RDS
- Now we can terminate the instances 
- All our backend services are ready and now it’s time to configure the frontend services
- Copy the endpoint  and port number of all the backend services 
- We are collecting all the backend information is to add in our application service (frontend)

**RabbitMQ Port = 5671** 
![image](https://github.com/Fawazcp/aws-project/assets/111639918/1380e39c-8ce5-41be-aad1-e58ad696d913)

**Elasticache Port = 11211**

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e351119f-b7ca-4fd1-a549-cf08b5a87a8b)


**RDS port (MySQL)= 3306**

![image](https://github.com/Fawazcp/aws-project/assets/111639918/bd21e1f9-4daf-484b-8650-c81a25af49cc)

- Now we have the port number and endpoint of all the backend services we created.
- We need to add all these information in to our application properties file.

### Step 4

**Create EBS(Elastic BeanStalk) PAAS**

- Now let’s create the Beanstalk for our frontend where we host our application.
- Before creating the EBS we need to create a role for the EBS
- Let’s create IAM role first;
- Go to IAM dashboard and follow the below steps;

 
![image](https://github.com/Fawazcp/aws-project/assets/111639918/05c08f8a-ad9e-4ce6-bb2b-f4dcc69846fa)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c07d8868-ceac-4d1f-ab8c-ec0e43dd953f)

- Search for **bean** and select the beolow 4 permission policies

![image](https://github.com/Fawazcp/aws-project/assets/111639918/14daab00-87e3-4bd9-9654-895f8187e5fc)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/217738bf-ab3a-4bff-81ff-e1917f152618)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/32979d0a-1ae3-4623-bc49-e6ffc4b52274)


- We have created the role. Now it’s time create our BeanStalk
- To create BeanStalk follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f794728f-09da-4c02-9eef-e91307809deb)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/62b81cc1-6a4e-4e50-aaab-586ca8e46307)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/0188f9fc-17ef-4f94-9904-eefa689ef1e0)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/87c5d868-0d32-4fbf-8917-87748abb31a5)


- Add a domain name and click on **check availabilty** if it is available continue otherwise modify the domain name and continue (domain name should be unique)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/829404ae-7b92-44b7-a836-34e36b37fb58)


- Select the platform as Tomcat because we are having the tomcat application source code

![image](https://github.com/Fawazcp/aws-project/assets/111639918/8dfcf807-3661-48e4-8425-0b211134384e)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/10230735-584a-4c95-956c-5190c7cdcf90)


- Under service role select create new role and under EC2 instance profile select the role we created.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/6ecd0798-5612-46ff-a764-8f9717f619cf)


- Select the default VPC and select all the subnets available.
- We are not going to create the database through beanstalk because when we delete the beanstalk the database also will get deleted.


![image](https://github.com/Fawazcp/aws-project/assets/111639918/6a752840-f627-4165-84c5-46f682cbb507)

Scroll down and give any tag if you want then click on next.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/be32fe1c-fa98-4e3c-b9f2-307621c6d3f8)


- Above error says the t3.micro instance type is not available in one AZ we selected 
- Go and uncheck that AZ and click on next.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/5e0289ac-97ac-4317-8ff3-8cb2f3a614c2)

- Now if you click on next you won’t get any error.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b3b51721-7c61-4d5c-b9fc-1e52aa2ff766)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/897abbac-6ce8-4a17-93cb-a891cac8903c)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/b88467c0-9a6b-4480-8bf9-cf8e4f93cbc7)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/1dbca759-1966-4114-8f48-969591f43181)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/77e5f9a4-60a9-47c6-8999-a4a378693c76)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/d28bfae1-3953-403c-9357-cb96d170ed9c)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/2d22c3b9-0acc-438d-96f4-ec37b97e311a)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/cf83c86f-19b2-48f1-8a5e-43649ce83ed4)


Read this documentation to understand more about deployment https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b47cceda-0c24-4ad7-89e1-c690e541d09b)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c2e914e9-3f4c-42f8-a803-9687add607c0)

- We have selected the Deployment policy as Rolling and Batch size percentage give 50% because we have only 2 instances and we want to update only 1 instance (50% of 2 instances) at a time

- Keep everything default and click on Next

 ![image](https://github.com/Fawazcp/aws-project/assets/111639918/bb043daa-d903-4619-8f46-3dd7257d5b0e)

- Review everything and click on Submit

![image](https://github.com/Fawazcp/aws-project/assets/111639918/3c378d51-3638-4a01-a88e-70cbfa04d733)

- Creating beanstalk will take some time.
- Once it is created we can see the event from beanstalk console. Where we will see what services are getting created by elastic beanstalk	
- Scroll to see all the events

![image](https://github.com/Fawazcp/aws-project/assets/111639918/29d36956-5747-4d33-9e1c-eef00d199cb6)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/5eee7fbc-f7ba-4a40-a2a9-bc66ecf1d85b)


After some time we can see our beanstalk is deployed.
-	Click on the Domain link to see the default webapp 

![image](https://github.com/Fawazcp/aws-project/assets/111639918/81c16431-875b-4746-983f-0e5a471a1bfe)

Next we will upload our artifactory and deploy it in the beanstalk.

### Step 5

**Update Security group & ELB**

-	Enable ACL on S3 bucket.
-	Update the health check in the target group
-	Update security group

**Task 1**
- Enable ACL in the S3 bucket.
- While creating the beanstalk there is an S3 bucket also created. We need to enable ACL in that bucket. To do that follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/edc5fd37-313e-45e7-b66f-7ea5dcd74dc1)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/9d5d8d31-b212-4d24-90c9-ef4e70601ece)

- Click on the hyperlink of the bucket to open the bucket.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1ca68071-54d1-4b0d-8dce-3d4d0010234a)


Scroll down to see the option called object ownership.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/663eefe9-814a-4bc6-b339-97506637cb92)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/3f974119-b3c7-4e72-81e5-cf99c273a630)

**Task 2**

-Update the health check in the target group
-To update this go beanstalk and follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b3620291-7e06-4b48-b692-d685851c3423)

Scroll down to the bottom

![image](https://github.com/Fawazcp/aws-project/assets/111639918/fe8b2b4d-f71b-428d-b5aa-5f88e00fbf49)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/ddc310a1-a876-40c6-be30-4d1dace4b9c5)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/58324c06-8034-462c-a381-43428c96525b)

- Once you save do not forget to apply this otherwise it will not get saved.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1c8f6385-b518-4666-abcb-f811c2cfc3f9)

You see the beanstalk state as severe but don’t worry it is because we made changes in the health check. Once we deploy our web application code then it will become OK state.

**Task 3**

- Update security group
- To update security group go to EC2 console and follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/054f580a-dd6b-46ab-b8f4-5750ce730908)

Click on the EC2 instance security group id and copy the security group id

![image](https://github.com/Fawazcp/aws-project/assets/111639918/5aba3351-daf1-4768-90f3-923bd2c99c01)

Next we need to add this security group id in to our backend security groups.
-	Go to backend security group and update the below changes.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f7b44b07-b52a-4437-a9fb-edd4f2da57ab)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/276875a2-c4f1-4dc6-8058-3cc731e9da53)

- Add these inbound rules under source select or paste EC2 instance security group.
- For this project we are adding All traffic from Ec2 instance only. But in production we need to add the traffic from the specific rules only not all traffic.

What we have done we allowed the traffic from the frontend app to the backend service.

### Step 6

**Bulid & Deploy Artifact**

💪 Now it’s time deploy our app from the source code. Let’s see how to do that.

In order to deploy artifacts we need to install some tools
-	jdk8  (dependency for maven)
-	maven
-	awscli (we use this to upload the artifacts to the s3 bucket)
To install the above tools in windows follow the below steps;

Open the windows powershell as administrator nd enter the below command to install the tools
-	choco install jdk8 –y
-	choco install maven –y
-	choco install awscli –y

![image](https://github.com/Fawazcp/aws-project/assets/111639918/ddbaac1f-e7fd-4065-842a-56dfde7b95b2)

Once you have installed all the packages follow the below steps to generate the artifact.

Clone the git repo from the below url.
- Open Gitbash and enter the below command to clone app source code.
-	git clone https://github.com/devopshydclub/vprofile-project.git
- Once you clone the source code from the above link then follow the below steps;

![image](https://github.com/Fawazcp/aws-project/assets/111639918/542efd6b-0ae2-4475-9102-e53a3351fcf6)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/7d2c32dc-32b8-42e4-bae5-b6d1222c3c2f)

- Make the below changes in the application.properties file

![image](https://github.com/Fawazcp/aws-project/assets/111639918/da654ed5-0346-4a87-ac7c-306291503e56)

- Make sure to add the endpoint of all the backend service and the password we created while creating the database and save the file.
- To save use (:wq) command.
- Now it’s time to build the artifact.
- Go back vprofile directory.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c6da2167-0f27-4cfe-bc01-f010b0369163)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c47a0866-342c-43b8-bc76-b7b25d4ac8ea)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/27b6d1a6-bbc0-4f3e-886b-73dcae396cbc)

- Go to Beanstalk and upload the vprofile-v2.war file from your system where you have saved.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e98f3e97-745f-4fb7-a65b-bb20df48690d)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e1ab7528-968a-4802-b6be-55dc050f21d9)


- Upload the file and deploy
- It may take some time please wait.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c3abd353-3cbe-43dd-b703-d92d6388025a)

- After sometime we can see our app is deployed. Click on the domain name and we can see our webapp login page.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/7679adb7-1c92-4894-a8cb-2a4d3f62a7c0)


Next we can make a DNS entry from our domain.

- Go to Rout53 domain name and add new CNAME record points to our webapp endpoint. Let’s see how we can achieve that.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/2a9f22cf-f77a-4436-a933-0dd275d8fa84)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/94c7f9f7-3e03-4535-b9e7-f6d853e215e5)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/64cede6d-0aa6-4308-9db9-145956095ce3)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/819477e6-22be-4c98-b38e-7daf187e3ffd)

- Under value make sure to add the domain of the beanstalk.
- Wait for sometime and copy the record name and paste it into new browser. 

![image](https://github.com/Fawazcp/aws-project/assets/111639918/e0781e16-9f65-49ac-89b5-b18d4ef891fa)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/7e08d880-c993-4ab1-984b-8618d3bd0325)


- When try to login we cannot able to login. 
- To resolve this we need to enable stickiness in our load balancer.
- Go Beanstalk configuration and make the below changes.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f8ece07f-4972-42fd-9038-437f4b2f2c16)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/462b5661-3924-4c3d-9691-6b109d50681b)


![image](https://github.com/Fawazcp/aws-project/assets/111639918/d3f17702-ce52-4076-880b-2e2ef2117419)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1f4ffe04-58bd-40c5-8246-125eedfbe803)

- Once you click on apply wait for some time to get this updated.
- And now we try again to log in we will able to log in

![image](https://github.com/Fawazcp/aws-project/assets/111639918/33c93480-001e-4e4a-85c4-3297dc122af2)

- Now let’s check our backend service is working as we expectes.
- Click on rabbitmq to see the our AmazonMQ is working or not.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/6fecf35d-4f2c-469d-8088-f3b9e2e829ec)

- Click on all users and open any user go back open the same user again to verify our elasticache is working as we expected.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/c222fb44-9784-4cf8-b052-009a70f2128a)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/7b757501-1a6e-40c6-a490-030c9d131560)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1d38ce29-80d6-47d4-9127-756527e390d8)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/4491888c-6abc-4897-9aec-af0b4a05ec5f)

Click the same user and we can see this time it is coming from memcached.

- We have deployed our web app in the Elastic BeanStalk successfully. 
Let’s think our audience is growing and we want our application is available globally without latency. What to do?🤔. Don’t worry in AWS there is one serivce available to achieve this called CloudFront.😊

- Next we will set up cloudfront for our application.

### Step 7

**Create CloudFront (Content Delivery Network [CDN])**

![image](https://github.com/Fawazcp/aws-project/assets/111639918/f995832e-a460-4b33-9c18-5a5b72d143b5)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/741dcdc5-051a-47cf-bf40-794b552eafb1)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/1bc4dedb-50e2-4471-8c31-e86e687e893f)

- Add your domain name if you have other select the load balancer endpoint. I have a domain name in Route53 so I added that.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/19c4b70b-17eb-4748-938a-345e2f193abf)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/d2bfb806-261b-4cc4-a75d-88013cfc031d)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/166cd7b4-919f-40d4-a55e-a03ee065aade)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/06b6d3cc-19e6-4a76-9ad9-febb7af2e0a8)

Deploying cloud front will take very long time. So it’s time to check your patience 😅

![image](https://github.com/Fawazcp/aws-project/assets/111639918/820bc1be-53ba-4c56-8baf-c189f3fc60fd)

- I have selected all edge location. If you don’t want to get more bill then go for the least region edge location available.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a91082c7-ba95-4694-93b1-5524946e84c7)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/837b6cbe-e79c-4473-bd6a-41f02454b0be)

- Copy and paste the domain name and this time you get the response faster than before because it is delivering the content from you nearest edge location.

To know more about CloudFront [click here](https://aws.amazon.com/cloudfront/)

# Clean UP

1- First we can delete CLoudFront

To delete the cloudfront follow the below steps.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/89be0d70-52ab-45ff-b4a4-eeac2101628f)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b1bf84dd-9df7-422a-93e6-db1d165ca85e)

Wait for some time until it gets disabled. After some time you can see the delete option is highlighted.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/92b6cbcb-f43a-4dc2-ac10-31158d51c28c)

2- Delete RDS

![image](https://github.com/Fawazcp/aws-project/assets/111639918/9207f75c-5c39-42a6-9018-85f4eee7fee0)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/07e53fba-5ba8-4ab9-bd2c-ffdf28130391)

- If you enabled the delete protection make sure to disable it.

![image](https://github.com/Fawazcp/aws-project/assets/111639918/cd2c9437-13f4-4009-9b13-a8db3c08f802)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a2649c62-e868-4b2d-823c-07ca27ec2fd1)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/8709ad63-23d7-4288-842a-546e86b6abcd)

3- Delete ElastiCache

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a6e6db9c-ff82-41d2-a11d-97e322f70a5e)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/2d501c54-f8d8-4c48-bacc-945512baf4da)

4- Delete AmazonMQ

![image](https://github.com/Fawazcp/aws-project/assets/111639918/3c2a87f3-ec69-40b1-85ce-7ef134437fac)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/a3f07f74-913b-4e18-b0d5-0657333fc422)

5- Delete Elastic BeanStalk

![image](https://github.com/Fawazcp/aws-project/assets/111639918/ee9302b2-0866-4461-ba60-2fa1f5ebcabc)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/6c014635-6091-4b72-81ac-06b311f7b9ad)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/b447564a-b52c-4c78-8b1c-01be9093e742)

![image](https://github.com/Fawazcp/aws-project/assets/111639918/14baaa78-d2ba-4ae4-a5fc-df32b619b4b8)


# Summary
-	User access Route53 domain name through cloudfront
-	It will send the request to ALB which is part of BeanStalk
-	It will be monitored by cloudwatch alarms
-	The application code (artifact) is in our S3 bucket
-	Get the backend application information from RDS, Elasticache, and AmazonMQ


**Happy Learning** 💜
