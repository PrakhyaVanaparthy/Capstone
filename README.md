## Capstone Project Overview

In this project we need to work in aws to acheive the following:
By using Jenkins need to implement CI/CD pipelines
Building Kubernetes clusters
Building Docker containers in pipelines
Deploying Docker Container to Kubernetes Cluster

## Create Cluster
Used eks tool to create clusters .

eksctl create cluster \
--name capstone \
--region us-east-1 \
--nodegroup-name capstone-nodes \
--node-type t2.micro \
--nodes 2 \
--nodes-min 2 \
--nodes-max 4 \
--ssh-access \
--ssh-public-key ekspair \
--managed

## Stages in Jenkinsfile:
- Lint HTML
- Build Docker Image
- Push Docker Image
- Setup Kubectl Context
- Blue Container
- Green Container
- blue app lb

# Prerequisites:
An EC2 instance with Ubuntu OS.Install docker, awscli version2, eks tool and kubectl in the instance.

### AWS CLI Installation
Follow the steps mentioned in the docs:
https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

### eks tool installation :
Follow the steps mentioned in the docs:
https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html

### kubectl installation :
Follow the steps mentioned in the docs:
https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html


