Q1

One of your engineers has written a Web application in the Go programming language and has asked your DevOps team to deploy it to AWS. The application code is hosted on a Git repository. Choose two answers.

a. Create a new EB application, and configure a Go environment to host your application. Using Git, check out the latest version of the code, configure the local repository for EB using the “eb start” command, and then use the “git aws.push” command to deploy the application.

b. Write a Dockerfile that installs the Go base image and uses Git to fetch your application. Create a new AWS OpsWorks stack that contains a Docker layer that uses Dockerrun.aws.json to deploy your container, then use the Dockerfile to automate the deployment.

c. Write a Dockerfile that installs the Go base image and fetches your application using Git. Create a new EB application, and use this Dockerfile to automate the deployment.

d. Write a Dockerfile that installs the Go base image and fetches your application using Git. Create an CloudFormation template that creates and associates an AWS::EC2::Instance resource type with an AWS::EC2::Container resource type.

e. Write a Dockerfile that installs the Go base image and fetches your application using Git. Create new OpsWorks stack with a custom layer that uses a Chef recipe to install Docker on EC2, and deploy your container using this Dockerfile to automate the deployment.

f. Create a CloudFormation template that creates an AWS::EC2::Instance resource type. In the UserData section for the instance, setup and configure a Tomcat server to host the Go application.

Answer:

Definitely C & E.

C: http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/docker-singlecontainer-deploy.html

The example is running an nginx container but the concept is the same.
```
FROM ubuntu:12.04  
RUN apt-get update  
RUN apt-get install -y nginx zip curl  
RUN echo "daemon off;" >> /etc/nginx/nginx.conf  
RUN curl -o /usr/share/nginx/www/master.zip -L https://codeload.github.com/gabrielecirulli/2048/zip/master  
RUN cd /usr/share/nginx/www/ && unzip master.zip && mv 2048-master/* . && rm -rf 2048-master master.zip  
EXPOSE 80  
CMD ["/usr/sbin/nginx", "-c", "/etc/nginx/nginx.conf"]
```
E: https://aws.amazon.com/blogs/devops/running-docker-on-aws-opsworks/

Q2

You have a Web application that is currently running on three M3 instances in three AZs. You have an auto scaling group configured to scale from three to thirty instances. When reviewing your CloudWatch metrics, you see that sometimes your auto scaling group is hosting fifteen instances. The web application is reading and writing to a DynamoDB-configured backend and configured with 800 write capacity units and 800 read capacity units. Your DynamoDB primary key is the Company ID. You are hosting 25 TB of data in your Web application. You have a single customer that is complaining of long load times when their staff arrives at the office at 9:00 AM and loads the website, which consists of content that is pulled from DynamoDB. You have other customers who routinely use the Web application. Choose the answer that will ensure HA and reduce the customer’s access times.

a. Add a caching layer in front of your Web application by choosing ElastiCache Memcached instances in one of the AZs.

b. Double the number of RCU in your DynamoDB instance, because the instance is probably being throttled when the customer accesses the website and your Web application.

c. Change your auto scaling group configuration to use C3 instance types, because the Web application layer is probably running out of compute capacity.

d. Implement an SQS que between your DynamoDB database layer and the web application layer to minimize the large burst in traffic the customer generates when everyone arrives at the office at 9:00 AM and begins accessing the website.

e. Use data pipelines to migrate your DynamoDB table to a new DynamoDB table with a primary key that is evely distributed across your dataset. Update your web application to request data from this new table.

f. Create a data pipeline to move your DynamoDB data to an RDS for Oracle database.

Answers:

A) This option could be viable if the cache layer would be put between your web-servers and your database. Putting it in front of the web application makes no sense to me, and also just adding this to one AZ would make this a single point of failure.

B) This is the correct answer in this case. Because of the primary key being the Company ID, all of this companies users will probably end up on the same partition of the database and thereby there are not enough RCU:s and the users will experience long loading times. As Alessandro calculated in his answer above, there are 2500 partitions in this setup and even doubling the RCU:s might not be enough in this case but at least it would help.

C) Since no other customers are complaining about loading times we can assume that the congestion is not in the web-servers. In other words changing these to more powerful instances would not help our case.

D) Implementing a SQS que is an option to use with Write intensive databases, but in our case we are specifically talking about loading a website hence this option would not help us.

E) As far as I read, you cannot change the scheme of the database with Data Pipeline so in other words you cannot change the primary key on the new DB rendering this option invalid.

F) This option includes the word Oracle which should automatically tell you nope :)

But in some posts they prefer option E.

Q3

You have implemented a system to automate deployments of your configuration and application dynamically after an Amazon EC2 instance in an Auto Scaling group is launched. Your system uses a configuration management tool that works in a standalone configuration, where there is no master node.

Due to the volatility of application load, new instances must be brought into service within three minutes of the launch of the instance operating system. The deployment stages take the following times to complete:

 Installing configuration management agent: 2mins

 Configuring instance using artifacts: 4mins

 Installing application framework: 15mins

 Deploying application code: 1min

What process should you use to automate the deployment using this type of standalone agent configuration?

MULTI-CHOICE OPTIONS

A) Configure your Auto Scaling launch configuration with an Amazon EC2 UserData script to install the agent, pull configuration artifacts and application code from an Amazon S3 bucket, and then execute the agent to configure the infrastructure and application.

B) Build a custom Amazon Machine Image that includes all components pre-installed, including an agent, configuration artifacts, application frameworks, and code. Create a startup script that executes the agent to configure the system on startup.

C) Build a custom Amazon Machine Image that includes the configuration management agent and application framework pre-installed. Configure your Auto Scaling launch configuration with an Amazon EC2 UserData script to pull configuration artifacts and application code from an Amazon S3 bucket, and then execute the agent to configure the system.

D) Create a web service that polls the Amazon EC2 API to check for new instances that are launched in an Auto Scaling group. When it recognizes a new instance, execute a remote script via SSH to install the agent, SCP the configuration artifacts and application code, and finally execute the agent to configure the system.

Answer:

Agree that the question is somewhat ambiguous, but b) mentions that configuration artifacts and all other components are pre-installed. I would therefore assume that "configure the system on startup" is not the 4min "configure instance" in the question. This question is leading you to suggest an AMI that is as close as ready-to-go as possible, so b) is the best fit here in my opinion.

Q4

You have an AWS OpsWorks stack running Chef version 11.10. Your company hosts its own proprietary cookbook on Amazon S3 and this is specified as a custom cookbook in the stack. You want to use an open-source cookbook located in an external Git repository. What tasks should you perform to enable the use of both custom cookbooks?

A. In the AWS OpsWorks stack settings, enable Berkshelf. Create a new cookbook with a Berksfile that specifies the other two cookbooks. Configure the stack to use this new cookbook.

B. In the OpsWorks stack settings, add the open source project's cookbook details in addition to your cookbook.

C. Contact the open source project's maintainers and request that they pull your cookbook into theirs. Update the stack to use their cookbook.

D. In your cookbook, create a S3 symlink object that points to the open source projects cookbook.

Answer:

My answer is A, since Chef and Berkshelf.

Q5

You setup a web application development environment by using a third-party configuration management tool to create a Docket container that is running on local developer machines. What should you do to ensure that the web application and supporting network, storage and security infrastructure does not impact your application after you deploy into AWS for staging and production environments?

A. Write a script using AWS SDK or CLI to deploy the application code from version control to the local development environments, staging and production using AWS OpsWorks.

B. Define an AWS CloudFormation template to place your infrastructure into version control and use the same template to deploy the Docker container into Elastic Beanstalk for staging and production.

C. Because the application is inside a Docker container, there are no infrastructure differences to be taken into account when moving from the local development environments to AWS for staging and production.

D. Define as AWS CloudFormation template for each stage of the application deployment lifecycle - development, staging and production and have tagging in each template to define the environment.

My 2 cents

Looks like B, because it make sense to use Docker with Elastic Beanstalk. And if you use one CloudFormation template for staging and production, then you will ensure that infrastructure is identical.

Q6

As part of your deployment process, you are configuring your continuous integration(CI) system to build AMIs. You want to build them in an automated manner that is also cost-efficient. Which method should you use.

A. Attach an Amazon EBS volume to your CI instance, build the root file system of your image on the volume and use the CreateImage API call to create an AMI out of this volume.

B. Have the CI system launch a new instance, bootstrap the code and apps onto the instance and create an AMI out of it.

C. Upload all the contents of the image to Amazon S3, launch the base instance, download all of the contents from Amazon S3 and create the AMI.

D. Have the CI system launch a new spot instance, bootstrap the code and apps onto the instance and create an AMI out of it.

My answer

A - createimage do not create AMI from other volumes, just from root device.

C - Upload and download the contents from S3?

Between B and D - I choose, D is most cost-efficient, that build process will be not interrupted or it will be spot instance when you need it.

Q7

As part of your deployment pipeline, you want to enable automated testing of your AWS CloudFormation template. What testing should be performed to enable faster feedback while minimizing costs and risk ? Choose 3 answers.

A. Use the AWS CloudFormation ValidateTemplete to validate the syntax of the template.

B. Use the AWS CloudFormation ValidateTemplete to validate the properties of resources defined in the template.

C. Validate the template's syntax using a general JSON parser.

D. Validate the AWS CloudFormation template against the official XSD schema definition published by AWS.

E. Update the stack with the template. If the template fails, rollback will return the stack and it's resources to exactly the same state.

F. When creating the stack, specify an Amazon SNS topic to which your testing system is subscribed. Your testing system runs tests when it receives notification that the stack is created or updated.

My answer A E F

A - yep, it does it.

B - nope, there is no such feature.

C - I do not know what does it mean.

D - again, what?

E - yep, but you must know, that it can be some resources, that will be replaced/removed. But rollback can be use.

F - it make sense to do it, although I do not know now how to do it.

Q8

You are designing an application that contains protected health information. Security and compliance requirements for your application mandate that all protected health information in the application use encryption at rest and in transit. The application uses a three-tier architecture where data flows through the load balancer and is stored on Amazon EBS volumes for processing, and the results are stored in Amazon S3 using the AWS SDK.

Which of the following two options satisfy the security requirements?

Choose 2 answers

A. Use SSL termination on the load balancer, Amazon EBS encryption on Amazon EC2 instances, and Amazon S3 with server-side encryption.

B. Use SSL termination with a SAN SSL certificate on the load balancer, Amazon EC2 with all Amazon EBS volumes using Amazon EBS encryption, and Amazon S3 with server-side encryption with customer-managed keys.

C. Use TCP load balancing on the load balancer, SSL termination on the Amazon EC2 instances, OS-level disk encryption on the Amazon EBS volumes, and Amazon S3 with server-side encryption.

D. Use TCP load balancing on the load balancer, SSL termination on the Amazon EC2 instances, and Amazon S3 with server-side encryption.

E. Use SSL termination on the load balancer, an SSL listener on the Amazon EC2 instances, Amazon EBS encryption on EBS volumes containing PHI, and Amazon S3 with server-side encryption.


Q9

You have a large multi-tier architecture that serves public-facing Web traffic through a load balancer and is backed by a Web tier that is contained within an Amazon EC2 auto scaling group. During a peak in traffic, you discover that your Web tier is adding new instances disproportionately compared to the amount of incoming traffic and the auto scaling policy that was set up. What should you do in order to stop the auto scaling group from scaling incorrectly in response to incoming traffic?

A. Using CloudWatch and the InstanceBootTime metric, increase the PauseTime and CoolDown property on the auto scaling group to be over the value of the metric.

B. Using a custom CloudWatch metric, insert the elapsed time since the instance launch to the time the instance responds to an Elastic Load Balancing health check, and periodically adjust the PauseTime and the CoolDown property of the auto scaling group to be over the value of the metric.

C. Using a custom CloudWatch metric, insert the elapsed time since the instance launch to the time the instance reponds to an Elastic Load Balancing health check, and periodically adjust the PauseTime of the UPdatePolicy and reduce the ScalingAdjustment policy by 50%.

D. Using a third-party configuration management tool and the AWS SDK, suspend all ScheduledActions of the auto scaling group until after the traffic peak, and then resume all ScheduledActions.

QX

Your company runs a large reputable web service that provides an API to all customers to access streaming data. This web service was built using an AWS Cloud Formation Template that currently runs Amazon EC2 instances for the web service and uses a load balancer to distribute load to your EC2 servers. Customers have started to complain about their streaming data falling behind. Upon investigation, you have noticed that your service has spikey user load on the API service.

What techniques should you use to cost-effectively resolve these customer complaints?

A. Create a duplicate AWS Cloud Formation Stack using your template. Update your load balancer to distribute the load to the new resources in the stack.

B.Update your AWS Cloud Formation stack to include the Elastic Load Balancing pre-warming option.

C. Submit a ticket to have your load balancer pre-warmed.

D. Update your ELB configuration and enable pre-warming option.

E. Update your AWS Cloud Formation stack to take advantage of Auto Scaling Groups. Create an Auto Scaling Group policy to scale up depending on the current requests per second to your API service.

F. Enable Elastic Load Balancing auto scaling. Create an Auto Scaling Group policy to scale up depending on the current requests per second to your API service.

Answer:

A is wrong since it is not cost-effective.

B is wrong since no such option.

D is wrong since no such configuration.

F is wrong, there is no such thing as Enable Elastic Load Balancing auto scaling.

C: [ELB pre warm](https://aws.amazon.com/articles/1636185810492479)

E: http://docs.aws.amazon.com/autoscaling/latest/userguide/autoscaling-load-balancer.html

QY

You have an ASP.NET web application running in Amazon Elastic Beanstalk. Your next version of the application requires a third-party Windows Installer package to be installed on the instance on first boot and before the application launches. Which options are possible?

Choose 2 answers

A. In the application's Global.asax file, run msiexec.exe to install the package using Process.Start() in the Application Start event handler.

B. In the source bundle's .ebextensions folder, create a file with a .config extension. In the file, under the "packages" section and "msi" package manager, include the package's URL.

C. Launch a new Amazon EC2 instance from the AMI used by the environment. Log into the instance, install the package and run sysprep. Create a new AMI. Configure the environment to use the new AMI.

D. In the environment's configuration, edit the instances configuration and add the package's URL to the "Packages" section.

E. In the source bundle's .ebextensions folder, create a "Packages" folder. Place the package in the folder.

Answers:

BD?
