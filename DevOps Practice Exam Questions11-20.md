Q10

You have an I/O and network-intensive application running on multiple EC2 instances that cannot handle a large ongoing increase in traffic. The Amazon EC2 instances are using two EBS PIOPS volumes each, and each instance is identical.

Which of the following approaches should be taken in order to reduce load on the instances with the least disruption to the application?

A. Create an AMI from each instance, and set up Auto Scaling Groups with larger instance type that has enhanced networking enabled and is Amazon EBS-optmized.

B. Stop each instance, and change instance to a larger EC2 instance type that has enhanced networking enabled and is Amazon EBS-optmized. Ensure that RAID striping is also set up on each instance.

C. Add an instance-store volume for each running EC2 instance, and implement RAID striping to improve I/O performance.

D. Add an Amazon EBS volume for each running EC2 instance, and implement RAID striping to improve I/O performance.

E. Create an AMI from an instance, and set up an Auto Scaling Group with an instance type that has enhanced networking enabled and is Amazon EBS-optmized.

Answer:

Answer E

You don't need to create an AMI from each instance if the instances are identical but you do need the application to auto-scale.

Q11

While preparing for your operational metrics meeting, you notice that 3-4 weeks ago there was an increase in response time for your web app. What techniques can you use to determine what caused the increase?

Choose 2 answers.

A. Use Cloud Watch to determine whether your database was under increased load during this period.

B. Use Cloud Watch metrics to determine when precisely the problem started.

C. Use AWS Cloud Trail Logs to determine whether any Amazon EC2 instances were stopped or terminated during the period.

D. Analyze your ELB access logs in S3 to determine whether ELB had elevated latency rates during this period.

E.Look at your Auto Scaling logs to determine whether there was a scaling event during that period.

F. Analyze your CloudTrail logs to determine whether ELB had elevated latency rates during this period.

Answer:

A and B are right out because CloudWatch only retains metrics for two weeks.

F doesn't work because CloudTrail doesn't track metrics like latency.

C certainly could work, because CloudTrail does track activity that could cause EC2 instances to be stopped or terminated.

E is wrong because there are no independent AutoScaling logs like that, and the CloudWatch metrics are of course gone. However, ...

D can work because you can configure ELB to log access info to S3, and that does include latency numbers.

So I agree with you: C and D seem to be the correct answers.

Q12

You have a web application composed of an Auto Scaling group of web servers behind a load balancer and create a new AMI for each application version for deployment. You have a new version to release and you want to use the A/B deployment technique to migrate users over in a controlled manner while the size of the fleet remains constant over a period of 12 hours, to ensure that the new version is performing well. What option should you choose to enable this technique while being able to roll back easily? Choose 1 answer.

A. Create an Auto Scaling launch configuration with the new AMI. Configuring the Auto Scaling group with the new launch configuration. Use the Auto Scaling rolling updates feature to migrate to the new version.

B. Create an Auto Scaling launch configuration with the new AMI. Create an Auto Scaling group configured to use the new launch configuration and to register instances with the same load balancer. Vary the desired capacity of each group to migrate.

C. Create an Auto Scaling launch configuration with the new AMI. Configuring the Auto Scaling to vary the proportion of instances launched from the two launch instances.

D. Create a load balancer. Create an Auto Scaling launch configuration with the new AMI to use the new launch configuration and to register instances with the new load balancer. Use Amazon Route53 Weighted Round Robin to vary the proportion of requests sent to the load balancers.

E. Launch new instances using the new AMI and attach them to the Auto Scaling group. Configure Elastic Load Balancing to vary the proportion of the requests sent to the instances running the two application servers.

Answer: D

Q13

N/A

Q14

You have been hired as a Devops Engineer for a small company. The CIO has asked you to deploy a new version of the application to production as soon as possible. After gathering all the info you can about the company's past deployment processes and the current architecture, you have made the following determination :
```
*All application servers are identical.

*All shared application resources are stored in S3.

*Application servers are in Auto Scaling group and are being serviced by a load balancer.

*Automated deployment tools are not currently being used.
```
You start by creating a new AMI and then deploy the new application code to an EC2 instance. What other combined steps must you take to ensure that this new software is deployed successfully? (SELECT FOUR)

A. Update the Auto Scaling Launch Configuration to reflect the newly created AMI.

B. Create a new load balancer.

C. Update the existing Load Balancer with instances launched from the newly created AMI.

D. Create a new Auto Scaling Launch Configuration and associate it to the newly created AMI.

E. Create a new Auto Scaling Group.

F. Update the existing Auto Scaling Groups.

G. Update Amazon Route 53 to reflect the new load balancer.

H. Move the elastic IP address from the existing load balancer to the new load balancer.

Answer:  DEBG

Q15

Your application uses Amazon SQS and AutoScaling Groups to process background jobs. The AutoScaling policy is based on the number of messages in the queue, with a max instance count of 100. Since the app has launched, group has never scaled above 50. The AutoScaling group has now scaled to 100, the queue size is increasing , and very few jobs are being completed. The number of messages being sent to the queue is at normal levels.

What should you do to identify why the queue is unusually high, and to reduce it ???

A. Temporarily increase the AutoScaling Groups desired value to 200. When the queue size has been reduced, reduce it to 50.

B. Analyze the application logs to identify reasons for message processing failure and resolve the cause for failures.

C. Create additional Autoscaling Groups, enabling the processing of the queue to be performed in parallel.

D. Analyze the Cloud Trail Logs for Amazon SQS to ensure that the instances' Amazon EC2 role has permission to receive messages from the queue.

Answer: B

The question notes that "very few jobs are being completed"; if the security had been changed to disallow deleting the messages, I would expect there to be no jobs being completed, so I think option D is wrong.

Options A and C try to address the problem by throwing more resources at it, but the system has already tried that by scaling up from a previous max of 50 to now 100 instances! C is particularly wonky because the 100 instances you have are already processing the queue in parallel!

I think option B makes sense: you have to figure out the root cause of the problem and fix that, and log analysis can help you do that.

Q16

Your public website uses a load balancer and an Auto Scaling group in a VPC. Your Chief Security Officer has asked you to set a monitoring system that quickly detects and alerts your team when a large sudden traffic increase occurs.

How should you set this up?

A. Set up an Amazon Cloud Watch Alarm for the Elastic Load Balancing NetworkIn metric, and then use SNS to alert the team.

B. Use an EMR job to run every 30 minutes, analyze the ELB access logs in a batch manner to detect a sharp increase in traffic, and then use SES to alert the team.

C. Use an EMR job to run every 30 minutes, analyze the Cloud Watch logs from your Amazon EC2 instances in a batch manner to detect a sharp increase in traffic, and then use SNS SMS notification to alert the team.

D. Set up an Amazon CloudWatch alarm for the Amazon EC2 NetworkIn metric for the Autoscaling group and alert using SNS.

E. Set up cronjob actively monitor the AWS Cloud Trail logs for increased traffic, and use SNS to alert the team.

Answer:

D, I dont see a networkIN metric for ELB on A

Q17

The development team has developed a new feature that uses an AWS service and wants to test it from inside a staging VPC.

How should you test this feature with the fastest turnaround time?

A. Launch an EC2 instance in the staging VPC in response to a development request, and use configuration management to set up the application. Run any testing harnesses to verify application functionality, and then use SNS to notify the development team of the results.

B. Use the EC2 instance that frequently polls the version control system to detect the new feature, use AWS Cloud Formation and EC2 user data to run testing harnesses to verify application functionality, and then use SNS to notify the development team of the results.

C. Use the Elastic Beanstalk application that polls the version control system to detect the new feature, use AWS Cloud Formation and EC2 user data to run testing harnesses to verify application functionality, and then use SNS to notify the development team of the results.

D. Use AWS Cloud Formation to launch an EC2 instance, use EC2 user data to run any testing harnesses to verify application functionality, and then use AWS Kinesis to notify the development team of the results.

Answer: A

Q18

You are responsible for an app that leverages the Amazon SDK and Amazon EC2 roles for storing and retrieving data from S3, accessing multiple DynamoDB tables, and exchanging messages with SQS queues. Your VP of compliance is concerned that you are not following security best practices for securing all of this access. He asked you to verify that the application's AWS access keys are not older than six months and to provide control evidence that these keys will be rotated a minimum of once every six months.

Which option will provide your VP with the requested information?

A. Create a script to query the IAM list-access-keys API to get your application access key creation dates, and create a batch process to periodically create a compliance report for your VP.

B. Update your application to log changes to its AWS access key credential file and use a periodic AWS EMR job to create a compliance report for your VP.

C. Create a new set of instructions for your configuration management tool that will periodically create and rotate the application's existing access keys, and provide a compliance report for your VP..

Answer:

 It's straight from the AWS practice exam, but your list of responses is missing the correct answer to point the VP to the documentation!

The Access Key ID and Secret Access Key passed to an EC2 instance via the role is refreshed every 15 minutes, IIRC, so you shouldn't need to make any changes.

Q19

You recently encountered a major bug in your web application during a deployment cycle. During this failed deployment, it took the team four hours to roll back to a previously working state, which left customers with a poor user experience. During the post mortem, your team discussed the need to provide a quicker, more robust way to roll back failed deployments. You currently run your web app on Amazon EC2 and use ELB for your load balancing needs.

Which technique should you use to solve this problem?

A. Create deployable versioned bundles of your application. Store the bundle on Amazon S3. Re-deploy the web app on elastic beanstalk, and enable the Elastic Beanstalk auto rollback feature tied to Cloud Watch metrics that define failure.

B. Use an AWS Opsworks stack to re-deploy your web application, and use AWS Opsworks DeploymentCommand to initiate a rollback during failure.

C. Create deployable versioned bundles of your application. Store the bundle on Amazon S3. Use Opsworks stack to redeploy your web application, and use AWS Opsworks application versioning to initiate a rollback during failure.

D. Using Elastic Beanstalk redeploy your web app, and use the Elastic Beanstalk API to trigger a FailedDeployment API call to initiate a rollback to a previous version.

Answer:

No assured answer. see https://acloud.guru/forums/aws-certified-devops-engineer-professional/discussion/-KOBqrfuZJrumdTlKZPu/question_-_8

Q20

Your company runs a large reputable web service that provides an API to all customers to access streaming data. This web service was built using an AWS Cloud Formation Template that currently runs Amazon EC2 instances for the web service and uses a load balancer to distribute load to your EC2 servers. Customers have started to complain about their streaming data falling behind. Upon investigation, you have noticed that your service has spikey user load on the API service.

What techniques should you use to cost-effectively resolve these customer complaints?

A. Create a duplicate AWS Cloud Formation Stack using your template. Update your load balancer to distribute the load to the new resources in the stack.

B.Update your AWS Cloud Formation stack to include the Elastic Load Balancing pre-warming option.

C. Submit a ticket to have your load balancer pre-warmed.

D. Update your ELB configuration and enable pre-warming option.

E. Update your AWS Cloud Formation stack to take advantage of Auto Scaling Groups. Create an Auto Scaling Group policy to scale up depending on the current requests per second to your API service.

F. Enable Elastic Load Balancing auto scaling. Create an Auto Scaling Group policy to scale up depending on the current requests per second to your API service.

Answer: CE

But the ticket for the pre-warmed ELB need an end-date.

