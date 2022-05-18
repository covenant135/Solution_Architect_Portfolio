# Task:Lauch an EC2 instance using the AWS CLI / Programmable access

Pretask
aws configure

Get the latest ami
aws ec2 describe-images --owners amazon --filters 'Name=name,Values=amzn2-ami-hvm-2.0.????????.?-x86_64-gp2' 'Name=state,Values=available' --query 'reverse(sort_by(Images, &CreationDate))[:1].ImageId' --output text

ami-03e0b06f01d45a4eb

Get Vpc Id

aws ec2 describe-vpcs --filters Name=tag:Name,Values="Lab VPC"

vpc-047695051526c3280


create a SG
aws ec2 create-security-group --description "Security group for my web server" --group-name "Web Server security group" --vpc-id vpc-047695051526c3280


    "GroupId": "sg-05d6ad0e4755ebb19"

Identify the public subnet

aws ec2 describe-subnets --filters Name=vpc-id,Values="vpc-047695051526c3280" Name=tag:Name,Values="Public Subnet 1"

"Subnets": [
        {
            "AvailabilityZone": "us-east-1a",
            "AvailabilityZoneId": "use1-az6",
            "AvailableIpAddressCount": 251,
            "CidrBlock": "10.0.1.0/24",
            "DefaultForAz": false,
            "MapPublicIpOnLaunch": true,
            "MapCustomerOwnedIpOnLaunch": false,
            "State": "available",
            "SubnetId": "subnet-0aab7c6620e47d69c",
            "VpcId": "vpc-047695051526c3280",
            "OwnerId": "712667520120",
            "AssignIpv6AddressOnCreation": false,
            "Ipv6CidrBlockAssociationSet": [],
            "Tags": [
                {
                    "Key": "aws:cloudformation:stack-id",
                    "Value": "arn:aws:cloudformation:us-east-1:712667520120:stack/c49089a726028l1895359t1w712667520120/236624b0-ba1c-11ec-a91c-0a42513fe84b"


Retrieve the user data file


wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/lab3-cli-ec2/user_data.txt


aws ec2 run-instances --image-id ami-03e0b06f01d45a4eb --count 1 --instance-type t3.micro --key-name vockey --security-group-ids sg-05d6ad0e4755ebb19 --subnet-id subnet-0aab7c6620e47d69c --associate-public-ip-address --disable-api-termination --user-data file://user_data.txt --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value="Web Server 1"}]'


Before you continue, confirm that the instance is running and healthy by checking the following values.

InstanceState: running
InstanceStatus: passed
SystemStatus: passed

By monitoring the instance status, you can determine whether Amazon EC2 detected any problems that could prevent your instances from running applications. Amazon EC2 performs automated checks on every running EC2 instance to identify hardware and software issues.

To view the types of metrics that are collected for an EC2 instance, run the following command.

aws cloudwatch list-metrics --namespace "AWS/EC2"  --dimensions Name=InstanceId,Value=<ec2-instance-InstanceId> 



To see the CPU utilization of your instance, run the following command.

aws cloudwatch get-metric-statistics --metric-name CPUUtilization --start-time 2014-04-08T23:18:00Z --end-time 2014-04-09T23:18:00Z --period 3600 --namespace AWS/EC2 --statistics Maximum --dimensions Name=InstanceId,Value=<ec2-instance-InstanceId>    

Amazon EC2 sends metrics to Amazon CloudWatch for your EC2 instances. Basic (5-minute) monitoring is enabled by default. You can enable detailed (1-minute) monitoring.

Sometimes, you need to know what happened in the console of an EC2 instance. To find this information, you must access the System Log.

The System Log displays the console output of the instance, which is a valuable tool for problem diagnosis. It is especially useful for troubleshooting kernel problems and service configuration issues that could cause an instance to terminate or become unreachable before its Secure Shell (SSH) daemon can start.

 

To view the System Log for your instance, use the following command.

aws ec2 get-console-output --instance-id <ec2-instance-InstanceId> --latest --output text


# Task 3: Updating your security group and accessing the web server
When you launched the EC2 instance, you provided a user data script that installed a web server and created a simple webpage. In this task, you will access content from the web server.

To retrieve details about the web server instance, enter the following command in the terminal.

aws ec2 describe-instances --filter Name=instance-id,Values=<ec2-instance-InstanceId>

Find the PublicIpAddress value and save it to the Lab Details text file.

Open a new tab in your web browser, copy the PublicIpAddress value, paste it into the browser, and press ENTER.

Question: Can you access your web server? Why not?

Answer: You can't access your web server. The security group doesn't permit inbound traffic on port 80, which is used for HTTP web requests. This behavior demonstrates the use of a security group as a firewall to restrict network traffic that's allowed in and out of an instance.

To correct this situation, you will now update the security group to permit web traffic on port 80.

Add an ingress rule to the Web Server security group security group.

Recall that ingress rules control traffic that enters a VPC. They allow requests to access designated ports by using the specified protocols. The requests are also filtered by the source IP address through the --cidr option.

Update the following command setting by using these options.

group-id: Security group GroupId that you saved previously
protocol: tcp
port: 80
cidr: 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id <security-group-GroupId> --protocol tcp --port 80 --cidr 0.0.0.0/0


# Task 4: Resizing your instance – Instance type and EBS volume
As your needs change, your instance might be overutilized (too small) or underutilized (too large)—or you might need a different amount of Amazon Elastic Block Store (Amazon EBS) storage. If so, you can change the instance type or change the size of a disk. For example, if a t2.micro instance is too small for its workload, you can change it to an m5.medium instance.

 

Subtask 1: Stopping your instance
Before you can resize an instance, you must stop it.

 When you stop an instance, it shuts down. A stopped EC2 instance incurs no charges. However, any attached EBS volumes storage will continue to incur charges.

In your terminal window, enter the following command.

aws ec2 stop-instances --instance-id <ec2-instance-InstanceId>


Subtask 2: Changing the instance type
In the terminal window, change the instance type by running the following command.

aws ec2 modify-instance-attribute \
    --instance-id <ec2-instance-InstanceId> \
    --instance-type "{\"Value\": \"t2.small\"}"
When the instance is started again, it will be of the t2.small type, which has twice as much memory as a t2.micro instance.

Note: In this lab, you might be restricted from using other instance types.

 

Subtask 3: Resizing the EBS volume
To resize the EBS volume through the AWS CLI, you must know the volume ID of the volume that's associated with your instance.



Save the VolumeId value. It will be similar to vol-0756a637688xxxxx3.

The disk volume currently has a size of 8 GiB. Next, you will increase the size of this disk.

Resize the volume to 10 GiB by running the following command.

Note: In this lab, you might be restricted from creating large EBS volumes.

aws ec2 modify-volume --size 10 --volume-id  <ebs-volume-VolumeId>


Subtask 4: Starting the resized instance
You will now start the web server instance again.

In your terminal, run the following command:

aws ec2 start-instances --instance-id <ec2-instance-InstanceId>



# Task 6: Testing termination protection
You can delete your instance when you no longer need it. This action is referred to as terminating your instance. You can't connect to or restart an instance after you terminate it.

In this task, you will learn how to use termination protection.

To test termination protection for your instance, run the following command.

aws ec2 terminate-instances --instance-id <ec2-instance-InstanceId>

Note that an error message indicates that the terminate-instances operation is not permitted.

This feature is a safeguard that helps prevent the accidental termination of an instance. If you truly want to terminate the instance, you must disable termination protection.

To disable termination protection, run the following command.

aws ec2 modify-instance-attribute --no-disable-api-termination --instance-id <ec2-instance-InstanceId>
Now, try the terminate-instances command again.

aws ec2 terminate-instances --instance-id <ec2-instance-InstanceId>



Launch a web server with termination protection enabled
Monitor Your EC2 instance
Modify the security group that your web server is using to allow HTTP access
Resize your Amazon EC2 instance to scale
Explore EC2 limits
Test termination protection
Terminate your EC2 instance



# Working with placement Groups using AWS CLI
1. Create a placement group
2. Tag a placement group
3. Launch instances in a placement group
4. Describe instances in a placement group
5. Change the placement group for an instance
6. Delete a placement group

ami-03e0b06f01d45a4eb
vpc-047695051526c3280

aws ec2 create-security-group --description "Security group for my web server" --group-name "Web Server security group" --vpc-id <VpcId>

Guide:
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html#using-placement-groups

https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-create-iam-instance-profile.html