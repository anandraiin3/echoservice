
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