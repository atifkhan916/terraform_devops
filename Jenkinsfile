pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID="921107229751"
        AWS_DEFAULT_REGION="ap-south-1"
        IMAGE_REPO_NAME="nodeecr"
        IMAGE_TAG="v1"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], userRemoteConfigs: [[url: 'https://github.com/atifkhan916/Terraform.git']])
            }
        }
        
        stage ("build image") 
        {
            steps {
                script {
                    dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                    }
                }
        }
        
        stage ("upload ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
                    sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
                    sh "docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}:${IMAGE_TAG}"
                }
            }
        }
        
        stage ("Deploy to K8S") {
            steps {
                withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'Deploy_python', contextName: '', credentialsId: 'k8s', namespace: 'dev', serverUrl: 'https://98062742F2B284731920FB6DA5583248.gr7.ap-south-1.eks.amazonaws.com']]) {
                    sh 'aws eks --region ap-south-1 update-kubeconfig --name Deploy_python'
                    sh '$HOME/bin/kubectl apply -f Deploy_Eks_from_ecr.yaml'
                }
            }
        }
    }    
}
