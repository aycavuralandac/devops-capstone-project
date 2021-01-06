## Udacity-DevOps-Capstone-Blue-Green-Deployment
For the capstone project, i've developed a CI/CD pipeline for micro services applications with either blue/green deployment. I've also add stage to my jenkinsfile for typographical checking (“linting”)

### Dependencies
##### 1. AWS account
For running Kuberntes on Amazon EKS, and running Jenkins pipeline on Amazon EC2 instance, you will need to create a AWS account. Follow this link to create your own AWS account: [Create AWS Account](https://portal.aws.amazon.com/billing/signup#/start)

#### 2. Jenkins and Docker, Kubernetes, Aws Cli, Ekstcl on Ubuntu VM
As a part of the project, I had to create an ubuntu instance to install jenkins and run pipeline on it and load my application files on this.

I've installed docker, aws-cli, eksctl, kubectl to my Ubuntu instance. This document can be followed for these installations: [How to install Docker, Aws Cli, Ekstcl, Kubectl on Linux Ubuntu](https://medium.com/@andresaaap/how-to-install-docker-aws-cli-eksctl-kubectl-for-jenkins-in-linux-ubuntu-18-04-3e3c4ceeb71)

I've installed Blue Ocean plugin to run pipeline. And I've created docker images and deployed them and test locally first. The following document was very useful in this regard:
https://medium.com/@andresaaap/simple-blue-green-deployment-in-kubernetes-using-minikube-b88907b2e267

By the way i had to create two credential on Jenkins to contact with Aws and my dockerhub.

### Jenkins Stages
My Jenkinsfile contains these stages:

- Linting for Html check with Tidy.
- Builded and deployed my docker image (contains dockerhub login)
- Created jenkinscluster with ekstcl
- Deployed my container to cluster
- Redirected service to my app that i chosen in blue-green-service.json


