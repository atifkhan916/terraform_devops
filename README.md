# Automate Docker builds and Deploy Python App into Kubernetes Cluster using Jenkins Pipelines | Dockerize Python App | Upload Images into AWS ECR

* Pre-requistes:
1. Create a jenkins server.
2. Install jenkins and docker on jenkins server.
2. Jenkins is up and running.
```
systemctl enable jenkins
systemctl start jenkins
systemctl status jenkins

```
3. Docker installed on Jenkins instance. 
4. Docker and Docker pipelines plug-in are installed.
5. Make sure port 8080 is opened up in firewall rules. 
6. Create an IAM role with (AmazonEC2ContainerRegistryFullAccess) policy, attach to Jenkins EC2 instance.
7. Make sure AWS cli is installed in Jenkins instance.
8. You have to add AWS credential in jenkins.
9. You have create EKS cluster for deploy you application.
10. You must have install kubernetes-cli plug-in in jenkins.
11. Install kubectl on your jenkins user.
```
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.17/2023-01-30/bin/linux/amd64/kubectl
chmod +x ./kubectl
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin

```
12. You must have add your config file on jenkins credential.

* Create a terraform script for ECR repository. ex-main.tf
```
Run terraform script 
terraform init 
terraform plan
terraform apply --auto-approve
```
* Create a pipeline for clone the repo, build Dockerfile, push docker image on ECR and deploy the docker image in EKS.

Verify deployment on EKS
- Connect to eks cluster on your machine.

```
kubectl get pod
kubectl get deployments
kubectl get svc
```