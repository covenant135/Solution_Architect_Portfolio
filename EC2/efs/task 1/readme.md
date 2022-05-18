 # Working with Amazon Elastic File System (Amazon EFS)

## Task 1: Creating a security group to access your EFS file system

The security group that you associate with a mount target must allow inbound access for TCP on port 2049 for Network File System (NFS). <!-- This is the security group that to be created, configure, and attach to  EFS mount targets. -->


In the AWS Management Console, on the Services menu, choose EC2.

In the navigation pane on the left, choose Security Groups.

create a secutity group with inbound rule on port 22 (SSH access) and tagged it as EFSClient
Copy the Security group ID of the EFSClient security group to your text editor.

The Group ID should look similar to sg-07ec3254d4e3e466b

Create another security group with the name EFS mount target

Inbound rule to set should allow access from the copied SG


## Task 2: Creating an EFS file system

EFS file systems can be mounted to multiple EC2 instances that run in different Availability Zones in the same Region. These instances use mount targets that are created in each Availability Zone to mount the file system by using standard NFSv4.1 semantics. You can mount the file system on instances in only one virtual private cloud (VPC) at a time. Both the file system and the VPC must be in the same Region.



Create EFS
use customised access
give it a name
Enable backup
Enable Encrpytion
on the next page, 
ensure you are in the same VPC
attached the efs mouunt target security group
review your creation
create


## Task 3: Connecting to your EC2 instance via SSH

In this task, you will connect to your EC2 instance by using Secure Shell (SSH).
Since it is a linux isntance, use EC2 connect (other options include putty, bash shell)
you will need private key to access the instance (download while launching the instance and keep it safe)

public ip of instance: 44.194.167.78
and private key

## Task 4: Creating a new directory and mounting the EFS file system

Amazon EFS supports the NFSv4.1 and NFSv4.0 protocols when it mounts your file systems on EC2 instances. Though NFSv4.0 is supported, we recommend that you use NFSv4.1. When you mount your EFS file system on your EC2 instance, you must also use an NFS client that supports your chosen NFSv4 protocol. The EC2 instance that was launched as a part of this lab includes an NFSv4.1 client, which is already installed on it.

To start
On the open connection of putty, update the utilis of efs using the commmand below

sudo yum install -y amazon-efs-utils

Now, under EFS in the managment console, click the efs created and attach using IP, copy the command starting with sudo

sudo mkdir efs

sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 10.0.3.10:/ efs

Get a full summary of the available and used disk space usage by entering:

sudo df -hT

This following screenshot is an example of the output from the following disk filesystem command

![Before loading the efs](C:/Users/DELL/Pictures/lab work/efs.JPG?raw=true "Image 1")
## Task 5: Examining the performance behavior of your new EFS file system

Examining the performance by using Flexible IO; pass the line of command below
 <!--Flexible IO (fio) is a synthetic I/O benchmarking utility for Linux. It is used to benchmark and test Linux I/O subsystems. During boot, fio was automatically installed on your EC2 instance. -->


sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio


The fio command will take 5–10 minutes to complete. The output should look like the example in the following screenshot. Make sure that you examine the output of your fio command, specifically the summary status information for this WRITE test.

### Monitoring performance by using Amazon CloudWatch
Visit AWS Cloudwatch service in your account
In the All metrics tab, choose EFS.

Choose File System Metrics.

Select the row that has the PermittedThroughput Metric Name.

 <!-- You might need to wait 2–3 minutes and refresh the screen several times before all available metrics, including PermittedThroughput, calculate and populate.-->

 <!-- The throughput of Amazon EFS scales as the file system grows. File-based workloads are typically spiky. They drive high levels of throughput for short periods of time, and low levels of throughput the rest of the time. Because of this behavior, Amazon EFS is designed to burst to high throughput levels for periods of time. All file systems, regardless of size, can burst to 100 MiB/s of throughput. For more information about performance characteristics of your EFS file system, see the official Amazon Elastic File System documentation.-->


