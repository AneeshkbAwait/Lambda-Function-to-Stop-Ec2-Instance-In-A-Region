# Lambda-Function-To-Stop-Ec2-Instance-In-A-Region

AWS Lambda is a serverless compute service that runs your code in response to events and automatically manages the underlying compute resources for you. 

Here, I have written a Lambda Function in Python which will locate Ec2 instances in Ap-South-1 Region and will stop them.

Steps involved in the complete setup:

##### 1. Create a Lambda role with EC2 privileges. 
Lambda function requires  privileges at Ec2 services to stop them. For that we have to create a Lambda role.
![alt text](https://s3.ap-south-1.amazonaws.com/githubpjts.aneeshponnu.tech/Lambda+Role1.PNG "Lambda Role")
##### 2. Create the Lambda function with the code written in Python.
Created the Lambda function code in python to stop the Instances in ap-south-1 region. I have filtered the instances based on the tags 'zomato' and 'dev', as I want to stop only them.
```
import boto3

REGION = "ap-south-1"

def lambda_handler(event, context):
    
  ec2 = boto3.client('ec2',region_name=REGION)
  instances = ec2.describe_instances( Filters=[ {'Name':'tag:Project', 'Values':["zomato"]},
                                              {'Name':'tag:Env', 'Values':["dev"]}])
    
  for instance in instances['Reservations'][0]['Instances']:
    
    instance_id = instance['InstanceId']
    print("Stopping Instance id : {}".format(instance_id))
    ec2.stop_instances(InstanceIds=[ instance_id ])
```
In order to start the instance, just replace the code section 'ec2.stop_instances(InstanceIds=[ instance_id ])' with 'ec2.start_instances(InstanceIds=[ instance_id ])'
##### 3. Created a cron to execute the Lambda function based on a schedule or we can set it to run on any specific events.
Here, I just schedule the lambda function to get execute on a weekly basis using the Cloudwatch cron.
![alt text](https://s3.ap-south-1.amazonaws.com/githubpjts.aneeshponnu.tech/cw+cron.PNG "AWS Event Bridge")

I hope this topic help you to create Lambda function to stop/start an Ec2 Instance in a scheduled way.
