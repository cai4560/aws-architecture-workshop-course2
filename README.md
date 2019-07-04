# aws-architecture-workshop-course-2

## 目标架构

![aws-target](https://user-images.githubusercontent.com/7569085/59860509-8d434000-93b1-11e9-9e29-c73256bce6c8.png)

## 环境准备(VPC & Bastion)

在环境准备中将会创建VPC Networking和Bastion，此部分只需要活动组织者运行一次即可。

![aws-vpc-and-bastion](https://user-images.githubusercontent.com/7569085/59823400-b7204680-9360-11e9-9b3f-b8ec58bc5e79.png)

### 如何创建?

在开始之前，你需要手动在AWS Web Console上创建名为: `aws-architecture-workshop`的key，下载后分发给所有学员。

```
$ git checkout master

$ aws configure
AWS Access Key ID [********************]:
AWS Secret Access Key [*******************]:
Default region name [ap-northeast-1]
Default output format [None]

$ ansible-playbook -i inventory/dev/inventory networking-and-bastion.yml -vvvv
```

或者在docker container中执行：
```
$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory networking-and-bastion.yml -vvvv
```

### 如何连接bastion?

```
$ ssh-add ~/.ssh/aws-keys/aws-architecture-workshop.pem && ssh -A ec2-user@bastion.aws-architecture-workshop.com
```

## 在Public Subnet中创建EC2 Instance

在运行CloudFormation之前，需要修改`inventory/dev/group_vars/all.yml`中的`trainee_name`，否则会出现CloudFormation Stack重名（同一region）的问题。

![aws-ec2-instance-in-public-subnet](https://user-images.githubusercontent.com/7569085/59973698-b217fd00-95d5-11e9-9d24-1e38d67aa9f5.png)

```
$ git checkout jenkins-instance

$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory playbook-jenkins.yml -vvv
```

## ASG In Public Subnet

![aws-public-subnet](https://user-images.githubusercontent.com/7569085/59860508-8d434000-93b1-11e9-9a54-bb8e914febae.png)

```
$ git checkout jenkins-auto-scaling-in-public-subnet

$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory playbook-jenkins.yml -vvv
```

## ALB + ASG In Public Subnet

![aws-alb-public-subent](https://user-images.githubusercontent.com/7569085/59860507-8caaa980-93b1-11e9-836d-7e00d09338d5.png)

```
$ git checkout jenkins-alb-auto-scaling-public

$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory playbook-jenkins.yml -vvv
```

## ALB + ASG In Private Subnet

![aws-alb-private-subent](https://user-images.githubusercontent.com/7569085/59860504-8caaa980-93b1-11e9-87eb-bd60e4236fa2.png)

```
$ git checkout jenkins-alb-auto-scaling-private

$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory playbook-jenkins.yml -vvv
```

## Route53 + ALB + ASG In Private Subnet

![aws-route53-alb-private-subent](https://user-images.githubusercontent.com/7569085/59860503-8caaa980-93b1-11e9-9698-7cb5c52d8d9d.png)


```
$ git checkout jenkins-alb-auto-scaling-route53

$ docker run --rm -it \
    -e AWS_ACCESS_KEY_ID=xxxxx \
    -e AWS_SECRET_ACCESS_KEY=xxxxx \
    -v ~/workspace/aws-training/aws-networking-bastion:/app \
    -w /app zpei/workshop:latest \
        ansible-playbook -i inventory/dev/inventory playbook-jenkins.yml -vvv
```