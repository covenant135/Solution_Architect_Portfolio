# Time based Lambda function

Task 1:  Creating a lambda function

In the AWS Management Console, from the Services menu, choose Lambda.

Note: a warning message that says tags failed to load was ignored it.

 Create function.

In the Create function screen, configure these settings:

Choose Author from scratch
Function name: myStopinator
Runtime: Python 3.8
Choose Choose or create an execution role
Execution role: Use an existing role
Existing role: From the dropdown list, choose myStopinatorRole
Choose Create function.


Task 2: Configure the trigger
In this task, I will configure a scheduled event to trigger the Lambda function by setting a CloudWatch event as the event source (or trigger). The Lambda function can be configured to operate much like a cron job on a Linux server, or a scheduled task on a Microsoft Windows server. However, you do not need to have a server running to host it.

Choose + Add trigger.

Choose the Select a trigger dropdown menu, and choose EventBridge (CloudWatch Events).

For the rule, choose Create a new rule and configure these settings:

Rule name: everyMinute
Rule type: Schedule expression
Schedule expression: rate(1 minute)
Note: A more realistic, schedule-based stopinator Lambda function would probably be triggered by using a cron expression instead of a rate expression. However, for the purposes of this activity, using a rate expression ensures that the Lambda function will be triggered soon enough that you can see the results.

Choose Add.

 
 Task 3: Configure the Lambda function
In this task, you will paste a few lines of code to update two values in the function code. You do not need to write code to complete this task.

Below the Function overview pane, choose Code, and then choose lambda_function.py to display and edit the Lambda function code. 
In the Code source pane, delete the existing code. Copy the following code, and paste it in the box:
import boto3
region = '<REPLACE_WITH_REGION>'
instances = ['<REPLACE_WITH_INSTANCE_ID>']
ec2 = boto3.client('ec2', region_name=region)

def lambda_handler(event, context):
    ec2.stop_instances(InstanceIds=instances)
    print('stopped your instances: ' + str(instances))
Note: After pasting the code into the Code source box, review line 5. If a period (.) was added, delete it.

Replace the <REPLACE_WITH_REGION> placeholder with the actual Region that you are using. To do this:

Choose on the region on the top right corner and use the region code. For example, the region code for US East (N. Virginia) is us-east-1.

Important: Keep the single quotation marks (' ') around the Region in your code. For example, for the N. Virginia, it would be 'us-east-1'

Challenge section: Verify that an EC2 instance named instance1 is running in your account, and copy the instance1 instance ID.


Task 4: Verify that the Lambda function worked
Return to the Amazon EC2 console browser tab and see if your instance was stopped.

Tip: You can choose the  refresh icon or refresh the browser page to see the change in state more quickly.

Try starting the instance again. What do you think will happen?

Click here to reveal the answer.

      
      The instance will be stopped again within 1 minute.