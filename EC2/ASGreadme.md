# Working with Auto scaling groups

Tasks:
1. Create a launch template / lauch configuration
2. Create a single-instance Auto Scaling group
3. Verify your Auto Scaling group
4. Terminate an instance in your Auto Scaling group
5. Next steps
6. Clean up


# Task 1: Create a launch template
Create your launch template using console
1 Follow these steps to configure your launch template for the following:
2. Specify the Amazon machine image (AMI) from which to launch the instances.
3. Choose an instance type that is compatible with the AMI you've specified.
4. Specify the key pair to use when connecting to instances, for example, using SSH.
5. Add one or more security groups to allow relevant access to the instances from an external network.
6. Specify whether to attach additional EBS volumes or instance store volumes to each instance.
7. Add custom tags (key-value pairs) to the instances and volumes.


# Subtask 1: Creating a lauch configuration
To create a launch configuration
Open the Launch configurations page of the Amazon EC2 console.
On the navigation bar, select an AWS Region. The Auto Scaling resources that you create are tied to the Region that you specify.
Choose Create launch configuration, and then enter my-first-launch-configuration in the Name field.
For Amazon machine image (AMI), choose an AMI. To find a specific AMI, you can find a suitable AMI, make note of its ID, and enter the ID as search criteria.
To get the ID of the Amazon Linux 2 AMI:
Open the Amazon EC2 console.
In the navigation pane, under Instances, choose Instances, and then choose Launch instances.
On the Quick Start tab of the Choose an Amazon Machine Image page, note the ID of the AMI next to Amazon Linux 2 AMI (HVM). Notice that this AMI is marked "Free tier eligible."
For Instance type, select a hardware configuration for your instance.
Note
If your account is less than 12 months old, you can use a t2.micro instance for free within certain usage limits. 
Under Additional configuration, for Advanced details, IP address type, make a selection. To provide internet connectivity to instances in a VPC, choose an option that assigns a public IP address. If an instance is launched into a default VPC, the default is to assign a public IP address. If you want to provide internet connectivity to your instance but aren't sure whether you have a default VPC, choose Assign a public IP address to every instance.
For Security groups, choose an existing security group. If you leave the Create a new security group option selected, a default SSH rule is configured for Amazon EC2 instances running Linux. A default RDP rule is configured for Amazon EC2 instances running Windows.
For Key pair (login), choose an option under Key pair options as instructed. Connecting to an instance is not included as part of this tutorial. Therefore, you can select Proceed without a key pair unless you intend to connect to your instance using SSH.
Choose Create launch configuration.
Select the check box next to the name of your new launch configuration and choose Actions, Create Auto Scaling group.

# Task 2:  Create a single-instance Auto Scaling group
To create a single-instance Auto Scaling group

1. On the Choose launch template or configuration page, for Auto Scaling group name, enter my-first-asg.
2. Choose Next.
3. The Choose instance launch options page appears, allowing you to choose the VPC network settings you want the Auto Scaling group to use and giving you options for launching On-Demand and Spot Instances (if you chose a launch template).
4. In the Network section, keep VPC set to the default VPC for your chosen AWS Region, or select your own VPC. The default VPC is automatically configured to provide internet connectivity to your instance. This VPC includes a public subnet in each Availability Zone in the Region.
5. For Availability Zones and subnets, choose a subnet from each Availability Zone that you want to include. Use subnets in multiple Availability Zones for high availability. For more information, see Considerations when choosing VPC subnets.
6. Keep the rest of the defaults for this tutorial and choose Skip to review.
7. On the Review page, review the information for the group, and then choose Create Auto Scaling group.

Note
The initial size of the group is determined by its desired capacity. The default value is 1 instance.

# Task 3: Verify your Auto Scaling group
To verify that your Auto Scaling group has launched an EC2 instance
Open the Auto Scaling groups page in the Amazon EC2 console.
Select the check box next to the Auto Scaling group that you just created.
A split pane opens up in the bottom of the Auto Scaling groups page. The first tab available is the Details tab, showing information about the Auto Scaling group.
Choose the second tab, Activity. Under Activity history, you can view the progress of activities that are associated with the Auto Scaling group. The Status column shows the current status of your instance. While your instance is launching, the status column shows PreInService. The status changes to Successful after the instance is launched. You can also use the refresh button to see the current status of your instance.
On the Instance management tab, under Instances, you can view the status of the instance.
Verify that your instance launched successfully. It takes a short time for an instance to launch.
The Lifecycle column shows the state of your instance. Initially, your instance is in the Pending state. After an instance is ready to receive traffic, its state is InService.
The Health status column shows the result of the EC2 instance health check on your instance.

# Task 4: Terminate an instance in your Auto Scaling group
Open the Auto Scaling groups page in the Amazon EC2 console.

Select the check box next to your Auto Scaling group.

On the Instance management tab, under Instances, select the ID of the instance.

This takes you to the Instances page in the Amazon EC2 console, where you can terminate the instance.

Choose Actions, Instance State, Terminate. When prompted for confirmation, choose Yes, Terminate.

On the navigation pane, under Auto Scaling, choose Auto Scaling Groups. Select your Auto Scaling group and choose the Activity tab.

The default cooldown for the Auto Scaling group is 300 seconds (5 minutes), so it takes about 5 minutes until you see the scaling activity. In the activity history, when the scaling activity starts, you see an entry for the termination of the first instance and an entry for the launch of a new instance.

On the Instance management tab, the Instances section shows the new instance only.

On the navigation pane, under Instances, choose Instances. This page shows both the terminated instance and the new running instance.

# Task 5: Clean up

To delete your Auto Scaling group

Open the Auto Scaling groups page in the Amazon EC2 console.
Select your Auto Scaling group .
Choose Delete. When prompted for confirmation, choose Delete.

A loading icon in the Name column indicates that the Auto Scaling group is being deleted. When the deletion has occurred, the Desired, Min, and Max columns show 0 instances for the Auto Scaling group. It takes a few minutes to terminate the instance and delete the group. Refresh the list to see the current state.


To delete your launch template
Open the Amazon EC2 console.
On the navigation pane, under Instances, choose Launch Templates.
Select your launch template.
Choose Actions, Delete template. When prompted for confirmation, choose Delete launch template.


To delete your launch configuration
Open the Launch configurations page of the Amazon EC2 console.
Select your launch configuration.
Choose Actions, Delete launch configuration. When prompted for confirmation, choose Yes, Delete.