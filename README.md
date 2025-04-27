# AWS Cloud Cost Optimization - Identifying Stale Resources
# Objective: Identifying Stale EBS Snapshots
In this project, I have created a Lambda function that identifies EBS snapshots that are no longer associated with any active EC2 instance and deletes them to save on storage costs.
# Description:
The Lambda function fetches all EBS snapshots owned by the same account ('self') and also retrieves a list of active EC2 instances (running and stopped). For each snapshot, it checks if the associated volume (if exists) is not associated with any active instance. If it finds a stale snapshot, it deletes it, effectively optimizing storage costs.
# Prerequisites:
1.	IAM Role for Lambda with the following permissions:
o	ec2:DescribeSnapshots
o	ec2:DescribeVolumes
o	ec2:DescribeInstances
o	ec2:DeleteSnapshot
# Detailed Execution:
Step1: - Create a Lambda function and deploy the python code that will detect the stale EBS snapshots and deletes it, To ensure the Lambda function completes successfully, especially in environments with a large number of snapshots or volumes, consider increasing the timeout setting: [from default 3 sec to 10 to 2o sec]
 
Step2: - Attach the IAM Role for the Lambda by assigning the policy to roles with the following permissions
o	ec2:DescribeSnapshots
o	ec2:DescribeVolumes
o	ec2:DescribeInstances
o	ec2:DeleteSnapshot
Step3: - Testing the Lambda Function
Once the Lambda function is deployed and the IAM role is properly attached, the next step is to test it with different use cases to ensure that it accurately detects stale EBS snapshots and deletes them accordingly. Below are the three primary use cases we will test:

1st Use Case: Snapshot is attached to an EBS volume, and the EBS volume is also attached to an EC2 instance
In this scenario, the Lambda function should not delete the snapshot, as it is still associated with an active EC2 instance.
![image](https://github.com/user-attachments/assets/4eb0fcc2-2024-4232-95bc-3ad33ae30b78)


2nd Use Case: The EC2 instance is deleted, but the EBS volume and its snapshot are still present.
In this scenario, the Lambda function should identify the snapshot as stale (because it is no longer attached to an active EC2 instance) and proceed to delete it.
![image](https://github.com/user-attachments/assets/b81fcdad-b96c-41fc-a417-58b64acdeff2)

3rd Use Case: Both the EC2 instance and the EBS volume are deleted, but the snapshot still exists.
In this scenario, the Lambda function should also identify the snapshot as stale (since the EC2 instance and the EBS volume are both deleted), and it should proceed to delete the snapshot.
![image](https://github.com/user-attachments/assets/d2640f39-267d-46cb-b827-0591f20bbd1a)

 


