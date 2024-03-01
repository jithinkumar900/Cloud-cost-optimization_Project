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
