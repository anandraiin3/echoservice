
aws ecs register-task-definition --cli-input-json file://echo-task-def.json
aws ecs run-task \
  --cluster anand-ecsfargate-cluster \
  --launch-type FARGATE \
  --task-definition echo-task \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-6e8a7426],securityGroups=[sg-0c1eb3866fe4eb82f],assignPublicIp=ENABLED}"
aws ecs list-tasks \
  --cluster anand-ecsfargate-cluster \
  --family echo-task \
  --query "taskArns[0]" \
  --output text

 get the ARN 
 aws ecs list-tasks \
  --cluster anand-ecsfargate-cluster \
  --family echo-task \
  --query "taskArns[0]" \
  --output text

 get the eni 
 aws ecs describe-tasks \
  --cluster anand-ecsfargate-cluster \
  --tasks arn:aws:ecs:ap-southeast-2:282764120287:task/anand-ecsfargate-cluster/9cebf3adfd764138897825c3a9b88d07 \
  --query "tasks[0].attachments[0].details[?name=='networkInterfaceId'].value" \
  --output text

get the public IP
aws ec2 describe-network-interfaces \
  --network-interface-ids eni-09598ea6b7544f616 \
  --query "NetworkInterfaces[0].Association.PublicIp" \
  --output text

curl http://3.107.163.50/test -X POST -d 'hello world'  


# 🚀 Deploying Echo Server on AWS ECS Fargate

This guide walks through registering a task definition, running a task on ECS Fargate, and testing the echo server.

---

## 1. Register the Task Definition
Register the task definition from the local JSON file:  

```bash
aws ecs register-task-definition --cli-input-json file://echo-task-def.json

## 2. Run the ECS task
Run the task in your ECS cluster with the specified subnet and security group:

```bash
aws ecs run-task \
  --cluster anand-ecsfargate-cluster \
  --launch-type FARGATE \
  --task-definition echo-task \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-6e8a7426],securityGroups=[sg-0c1eb3866fe4eb82f],assignPublicIp=ENABLED}"

## 3. List Running Tasks (Get a Task ARN)
Retrieve the ARN of a running task:

```bash
aws ecs list-tasks \
  --cluster anand-ecsfargate-cluster \
  --family echo-task \
  --query "taskArns[0]" \
  --output text

The output will be a Task ARN. 
Example
```bash 
arn:aws:ecs:ap-southeast-2:282764120287:task/anand-ecsfargate-cluster/9cebf3adfd764138897825c3a9b88d07

## 4. Get the ENI (Elastic Network Interface) from the Task
Use the Task ARN to retrieve the associated ENI:

```bash
aws ecs describe-tasks \
  --cluster anand-ecsfargate-cluster \
  --tasks arn:aws:ecs:ap-southeast-2:282764120287:task/anand-ecsfargate-cluster/9cebf3adfd764138897825c3a9b88d07 \
  --query "tasks[0].attachments[0].details[?name=='networkInterfaceId'].value" \
  --output text

The output will be a ENI. 
Example
```bash 
eni-09598ea6b7544f616

## 5. Get the Public IP of the ENI
Use the ENI to get the public IP address:

```bash
aws ec2 describe-network-interfaces \
  --network-interface-ids eni-09598ea6b7544f616 \
  --query "NetworkInterfaces[0].Association.PublicIp" \
  --output text

The output will be an IP address. 
Example
```bash 
3.107.163.50

## 5. Test the Echo Server
Send a test request to the public IP:

```bash
curl http://3.107.163.50/test -X POST -d 'hello world'

You should receive a JSON response reflecting your request details (method, path, headers, body).

