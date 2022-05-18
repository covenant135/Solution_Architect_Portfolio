Task 1: Create a Security Group for the RDS DB Instance
In this task, you will create a security group to allow your web server to access your RDS DB instance. The security group will be used when you launch the database instance.

In the AWS Management Console, on the Services  menu, choose VPC.

In the left navigation pane, choose Security Groups.

Choose Create security group and then configure:

Security group name: DB Security Group
Description: Permit access from Web Security Group
VPC: Lab VPC
You will now add a rule to the security group to permit inbound database requests.

In the Inbound rules pane, choose Add rule

The security group currently has no rules. You will add a rule to permit access from the Web Security Group.

Configure the following settings:

Type: MySQL/Aurora (3306)
CIDR, IP, Security Group or Prefix List: Type sg and then select Web Security Group.
This configures the Database security group to permit inbound traffic on port 3306 from any EC2 instance that is associated with the Web Security Group.

Choose Create security group

You will use this security group when launching the Amazon RDS database.

 

Task 2: Create a DB Subnet Group
In this task, you will create a DB subnet group that is used to tell RDS which subnets can be used for the database. Each DB subnet group requires subnets in at least two Availability Zones.

On the Services  menu, choose RDS.

In the left navigation pane, choose Subnet groups.

 If the navigation pane is not visible, choose the  menu icon in the top-left corner.

Choose Create DB Subnet Group then configure:

Name: DB-Subnet-Group
Description: DB Subnet Group
VPC: Lab VPC
Scroll down to the Add Subnets section.

Expand the list of values under Availability Zones and  select the first two zones: us-east-1a and us-east-1b.

Expand the list of values under Subnets and select the subnets associated with the CIDR ranges 10.0.1.0/24 and 10.0.3.0/24.

These subnets should now be shown in the Subnets selected  table.

Choose Create

You will use this DB subnet group when creating the database in the next task.

 

Task 3: Create an Amazon RDS DB Instance
In this task, you will configure and launch a Multi-AZ Amazon RDS for MySQL database instance.

Amazon RDS Multi-AZ deployments provide enhanced availability and durability for Database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB instance, Amazon RDS automatically creates a primary DB instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ).

In the left navigation pane, choose Databases.

Choose Create database

 If you see Switch to the new database creation flow at the top of the screen, please choose it.

Select  MySQL.

Under Settings, configure:

DB instance identifier: lab-db
Master username: main
Master password: lab-password
Confirm password: lab-password
Under DB instance class, configure:

Select  Burstable classes (includes t classes).
Select db.t3.micro
Under Storage, configure:

Storage type: General Purpose (SSD)
Allocated storage: 20
Under Connectivity, configure:

Virtual Private Cloud (VPC): Lab VPC
Under Existing VPC security groups, from the dropdown list:

Choose DB Security Group.
Deselect default.
Expand  Additional configuration, then configure:

Initial database name: lab
Uncheck Enable automatic backups.
Uncheck Enable encryption
Uncheck Enable Enhanced monitoring.
 This will turn off backups, which is not normally recommended, but will make the database deploy faster for this lab.

Choose Create database

Your database will now be launched.

 If you receive an error that mentions "not authorized to perform: iam:CreateRole", make sure you unchecked Enable Enhanced monitoring in the previous step.

Choose lab-db (choose the link itself).

You will now need to wait approximately 4 minutes for the database to be available. The deployment process is deploying a database in two different Availability zones.

 While you are waiting, you might want to review the Amazon RDS FAQs or grab a cup of coffee.

Wait until Info changes to Modifying or Available.

Scroll down to the Connectivity & security section and copy the Endpoint field.

It will look similar to: lab-db.cggq8lhnxvnv.us-west-2.rds.amazonaws.com

Paste the Endpoint value into a text editor. You will use it later in the lab.

 

Task 4: Interact with Your Database
In this task, you will open a web application running on your web server and configure it to use the database.

To copy the WebServer IP address, choose on the Details drop down menu above these instructions, and then choose Show.

Open a new web browser tab, paste the WebServer IP address and press Enter.

The web application will be displayed, showing information about the EC2 instance.

Choose the RDS link at the top of the page.

You will now configure the application to connect to your database.

Configure the following settings:

Endpoint: Paste the Endpoint you copied to a text editor earlier
Database: lab
Username: main
Password: lab-password
Choose Submit
A message will appear explaining that the application is running a command to copy information to the database. After a few seconds the application will display an Address Book.

The Address Book application is using the RDS database to store information.

Test the web application by adding, editing and removing contacts.

The data is being persisted to the database and is automatically replicating to the second Availability Zone.

 