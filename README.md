# EC2-Instance_-Shutdown-_AWS-Lambda
## Automated EC2 Instance Shutdown with AWS Lambda

![awslambda](https://github.com/user-attachments/assets/4bd4654e-a7be-4a90-bf52-249a3af44664)

-------------------------------------------------------------
import boto3

def stop_ec2_instances():
    # Create an EC2 resource
    ec2 = boto3.client('ec2')

    # Find all running EC2 instances
    running_instances = ec2.describe_instances(
        Filters=[{'Name': 'instance-state-name', 'Values': ['running']}]
    )

    # Gather instance IDs of running instances
    instance_ids = [instance['InstanceId'] for reservation in running_instances['Reservations'] for instance in reservation['Instances']]
    
    # Stop instances if there are any running
    if instance_ids:
        print(f"Stopping instances: {instance_ids}")
        ec2.stop_instances(InstanceIds=instance_ids)
        return f"Stopped instances: {instance_ids}"
    else:
        print("No running instances found.")
        return "No instances to stop."

# Test the function (optional)
if __name__ == "__main__":
    stop_ec2_instances()

--------------------------------------------------------

Modified Script for Lambda
Replace the function body with this code:
--------------------------------------------------------


import boto3

def lambda_handler(event, context):
    ec2 = boto3.client('ec2')
    running_instances = ec2.describe_instances(
        Filters=[{'Name': 'instance-state-name', 'Values': ['running']}]
    )
    
    instance_ids = [instance['InstanceId'] for reservation in running_instances['Reservations'] for instance in reservation['Instances']]
    
    if instance_ids:
        ec2.stop_instances(InstanceIds=instance_ids)
        return f"Stopped instances: {instance_ids}"
    else:
        return "No instances to stop."

