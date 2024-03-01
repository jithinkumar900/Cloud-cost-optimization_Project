# AWS Cloud Cost Optimization - Identifying Stale Resources



<img src="https://d2gbo5uoddvg5.cloudfront.net/images/Logo_aws.gif" alt="AWS Cloud Cost Optimization" style="width:90px;height:50px;"/> <img src="https://www.eginnovations.com/images/AWS-Cloudwatch-Logo.webp" alt="AWS Cloud Cost Optimization" style="width:90px;height:50px;"/> <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Python-logo-notext.svg/800px-Python-logo-notext.svg.png" alt="AWS Cloud Cost Optimization" style="width:70px;height:60px;"/>



## Identifying Stale EBS Snapshots

In this project, we aim to optimize AWS cloud costs by identifying and managing stale Elastic Block Store (EBS) snapshots. Specifically, we'll create a Lambda function that identifies EBS snapshots no longer associated with any active EC2 instance and deletes them to save on storage costs.

### Description

The Lambda function performs the following steps:

1. Fetches all EBS snapshots owned by the same AWS account ('self').
2. Retrieves a list of active EC2 instances, including both running and stopped instances.
3. For each snapshot, it checks if the associated volume, if it exists, is not associated with any active instance.
4. If a stale snapshot is identified, it is deleted, effectively optimizing storage costs.

This approach ensures that only EBS snapshots without any current dependencies are removed, preventing unintentional data loss. By regularly running this Lambda function, you can automate the process of identifying and cleaning up stale resources, contributing to a more cost-effective AWS infrastructure.

we first crate a labda function

![Screenshot 2024-03-01 175322](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/7f258d90-e13f-4a05-a9e6-635baac4164e)

create the ec2 instance with the required volume and specs 

![Screenshot 2024-03-01 175532](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/a0e5d7ae-4478-4b97-9df0-faf7c3da5d96)

![Screenshot 2024-03-01 175544](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/1c7b3cb6-a4ff-4fbe-a207-8715d677115a)

no let say the volume has important information and it needed to be saved so we create a snapshot of it 

![Screenshot 2024-03-01 175728](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/31b88cbf-1a68-467a-9a45-edd80dbf0666)

we can see here that we have 1 instance 1 volume and 1 snapshot created 

![Screenshot 2024-03-01 175752](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/f8dd97fd-a0bc-44e0-90ec-85bb620897d8)

now we go back to the lambda function and create a python script with boto3 module to describe the instance, snapshot, volume and then see if the snapshot is attached to the instance and/or volume

![Screenshot 2024-03-01 175933](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/148adadb-f796-4186-9187-33030a9ddeab)


when we run it the first time we will encoutner some error the first error we encoutner is due to the less execution time as it is set to 3 so we change the code timeout to 10 the work on the next set of errors 

![Screenshot 2024-03-01 180012](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/c48d928b-90bf-4094-8ff9-8c3486d1d975)

![Screenshot 2024-03-01 180040](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/fc54a418-ea70-448a-8ee4-78b5af3440d2)

after changing the time to 10 second we experience another set of error which is we dont have permission to describe the snapshot id, volume id, instance id nor to delete the snapshot so we set required permission to the fuction 

![Screenshot 2024-03-01 180111](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/b77be769-5c14-4233-a567-5181076b8ada)
![Screenshot 2024-03-01 180450](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/05a9feaa-ada6-48f1-9f80-8e33c093b291)

after creating the policy we attach the poliy to the fuction permissions 

![Screenshot 2024-03-01 181126](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/d40f989b-d870-4c05-b841-8e40585ee6f5)

now when we run the code it successfully runs with no error no snapshot is deleted as snapshot is attached to a volume which is attached to a instance 

![Screenshot 2024-03-01 181223](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/4daaa379-be7f-4d70-ac24-b07eda64d511)

now for test purpose we terminate the instace and then it will automaitcally deletet the volume but the snapshot will not be delete 

![Screenshot 2024-03-01 181248](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/4f612ec9-cd5b-460d-be29-f6f8c33e4d06)
![Screenshot 2024-03-01 181342](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/96a32c7f-8d2a-496a-b09c-75bf78f44502)

now when we run the function we see that the snapshot is deleted as it is not attached to a instance nor to a volume 

![Screenshot 2024-03-01 181408](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/44482795-ac33-462e-930e-f61f1e71f33c)

![Screenshot 2024-03-01 181435](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/64e7535b-8002-41a9-b54f-8630c0320d4c)

we can run this instance on a day to day basis to check for stale resources by bridging funtion with cloud wath and providing it a cron job 

![Screenshot 2024-03-01 181831](https://github.com/jithinkumar900/Cloud-cost-optimization_Project/assets/59408287/949ad859-45bc-4565-87e1-46c17bc3d68f)

at the end delete all the snapshot and the cloudwatch/resources to avoid incurring of cloud cost ヽ(•‿•)ノ


