# Highly available and scalable Web infrastructure

Working Objectives:

Create an Amazon Machine Image (AMI) from a running instance.
Create a load balancer.
Create a launch configuration and an Auto Scaling group.
Automatically scale new instances within a private subnet.
Create Amazon CloudWatch alarms and monitor performance of your infrastructure.



Scenerio before the task
A VPC with two AZ and instances launched in each AZ (private and public subnet). VPC has internet gateway already and natgateway in private subnet. RDS instances in each private subnet in each AZ

# Task 1: To create an AMI from exiting running instance

When the instance is in a running state
Click on it and under its actions, Image and template and click on create Image
configure it to suit the task  Name and description



# Task 2: Create a load balancer.

To create a load balance, first begin with target groups under the load balance menu on the left hand pane
(Target Groups define where to send traffic that comes into the Load Balancer. The Application Load Balancer can send traffic to multiple Target Groups based upon the URL of the incoming request, such as having requests from mobile apps going to a different set of servers. Your web application will use only one Target Group.)

Choose Instances 
Give the target group a name
Select the VPC you which to deploy the load balancer
then protoco version and the health checks will foloow

Afterward, you will see the instances that are registered targets

Ensure the port is correct
Create target groups
Now, you can create a load balancer 
Among the various LB, choose a ALB ( Application Load Balancer that operates at the request level (layer 7), routing traffic to targets — EC2 instances, containers, IP addresses and Lambda functions — based on the content of the request. For more information)


Give it a name :  LabALB
Scheme: internet facing, ensure you check IP address type

Scroll down to the Network mapping section, then:

For VPC, select: Lab VPC

You will now specify which subnets the Load Balancer should use. The load balancer will be internet facing, so you will select both Public Subnets.

Choose the first displayed Availability Zone, then select Public Subnet 1 from the Subnet drop down menu that displays beneath it.

Choose the second displayed Availability Zone, then select Public Subnet 2 from the Subnet drop down menu that displays beneath it.

You should now have two subnets selected: Public Subnet 1 and Public Subnet 2. (If not, go back and try the configuration again.)

In the Security groups section:

Choose the Security groups drop down menu and select the SG you only need

Under listener, check the port and be sure  Http or https
Choose the listener group as your target group

Now, you are ready to create a load balancer
Create Load balancer


# Task 3: Create a Launch Configuration and an Auto Scaling Group 
(A launch configuration is a template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances such as the AMI, the instance type, a key pair, security group and disks.)

Under Autoscaling, choose lauch configuration
Give it a great name
and choose the AMI you earlier created
Choose the best instance type with optimized cost
Enable monitoring. This allows Auto Scaling to react quickly to changing utilization.
Choose corresponding security group and Keypair
Create the configuration

To create autoscaling groups
Choose autosclaing groups and create ASG
Enter Auto Scaling group name and use the lauch configuration menu
On the Network page configure
choose the correct VPC and here select the private subnets in the two AZ

In the Load balancing - optional pane, choose Attach to an existing load balancer

In the Attach to an existing load balancer pane, use the dropdown list to select LabGroup.

In the Additional settings - optional pane, select  Enable group metrics collection within CloudWatch

Under Group size, configure:

Desired capacity: 2
Minimum capacity: 2
Maximum capacity: 6
This will allow Auto Scaling to automatically add/remove instances, always keeping between 2 and 6 instances running.


Under Scaling policies, choose Target tracking scaling policy and configure:

Lab policy name: LabScalingPolicy
Metric type: Average CPU Utilization
Target value: 60
(This tells Auto Scaling to maintain an average CPU utilization across all instances at 60%. Auto Scaling will automatically add or remove capacity as required to keep the metric at, or close to, the specified target value. It adjusts to fluctuations in the metric due to a fluctuating load pattern.)



# Task 4: Verify that Load Balancing is Working

In this task, you will verify that Load Balancing is working correctly.

In the left navigation pane, click Instances.

You should see two new instances named Lab Instance. These were launched by Auto Scaling.

 If the instances or names are not displayed, wait 30 seconds and click refresh  in the top-right.

First, you will confirm that the new instances have passed their Health Check.

In the left navigation pane, click Target Groups (in the Load Balancing section).

Choose LabGroup

Click the Targets tab.

Two Lab Instance targets should be listed for this target group.

Wait until the Status of both instances transitions to healthy. Click Refresh  in the upper-right to check for updates.

Healthy indicates that an instance has passed the Load Balancer's health check. This means that the Load Balancer will send traffic to the instance.

You can now access the Auto Scaling group via the Load Balancer.

In the left navigation pane, click Load Balancers.

In the lower pane, copy the DNS name of the load balancer, making sure to omit "(A Record)".

It should look similar to: LabELB-1998580470.us-west-2.elb.amazonaws.com

Open a new web browser tab, paste the DNS Name you just copied, and press Enter.

The application should appear in your browser. This indicates that the Load Balancer received the request, sent it to one of the EC2 instances, then passed back the result.


