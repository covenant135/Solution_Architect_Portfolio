# Lab : IAM Use Case

AWS Identity and Access Management (IAM) is a web service that enables Amazon Web Services (AWS) customers to manage users and user permissions in AWS. With IAM, you can centrally manage users, security credentials such as access keys, and permissions that control which AWS resources users can access.

# USe case:
A company is growing its use of Amazon Web Services, and is using many Amazon EC2 instances and a great deal of Amazon S3 storage. You wish to give access to 4 new staff depending upon their job function:

User    Groups          Permissions
user-1	S3-Support	    Read-Only access to Amazon S3
user-2	EC2-Support	    Read-Only access to Amazon EC2
user-3	EC2-Admin	    View, Start and Stop Amazon EC2 instances
user-4  Account-head    Billing Report   


# Task 1: Create 3 Groups and attach  3 different policies

Step 1: Create a group, named S3-support and during creation assign S3Readonly policy
Step 2: Create another group, named EC2-support and attached EC2 read only policy
Step 3: Create another group, named EC2-admin and attached EC2fullaccess policy


# Task 2: Create 3 users that can fit into the three groups you created

Step 1: From the root user account, under the management console, locate IAM (a global service)
Step 2: On the left hand panel, click on user and then, create user 

# Task 3: Test your set up
