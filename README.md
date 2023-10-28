# Udacity Cloud DevOps Capstone Project

This is final project of Udacity Cloud DevOps Engineer Nanodegree Program.

## Project Overview

1.  Created Jenkins server using the AWS CloudFormation.
2.  Installed all the needed tools in Jenkins master server using the Launch Configuration.
3.  Creates one EKS cluster in AWS.
4.  Created one application deployment pipeline that deploys the application docker container in the kubernetes cluster.
5.  Used blue/green deployment strategy to deploy the application.

#### Project Files
infrastructure
This folder all the files related to infrastructure deployment.

jenkins-server-parameters.json: Parameters file for cloud formation stack.
jenkins-server.yml: CloudFormation template for creating jenkins server.
Script to create, delete and update CloudFormation stack.

create-jenkins.sh
update-jenkins.sh
delete-jenkins.sh
kubernetes-resources
This folder contains all template files for Kubernetes resources.

blue-replication-controller.yml: A replication controller template that creates pods with label as app=blue.
blue-service.yml: A service template that selects all the pods with label as app=blue.
green-replication-controller.yml: A replication controller template that creates pods with label as app=green.
green-service.yml: A service template that selects all the pods with label as app=green.

#### docker
This is Dockerfile of application.

blue/Dockerfile: Application with blue background.
green/Dockerfile: Application with green background.

#### Jenkinsfile
This file contains the steps of CICD pipeline of application.

#### screenshots

This folder contains all screenshots taken during creation of this project.

#### git hub
https://github.com/BacNguyen1809/udacity-project5
