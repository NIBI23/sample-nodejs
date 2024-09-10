pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub'
        AWS_CREDENTIALS_ID = 'aws'
        REGISTRY = 'yourdockerhubusername'
        REPO = 'my-web-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/NIBI23/sample-nodejs.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.REGISTRY}/${env.REPO}:latest")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: "${env.DOCKER_CREDENTIALS_ID}", url: '']) {
                    script {
                        docker.image("${env.REGISTRY}/${env.REPO}:latest").push('latest')
                    }
                }
            }
        }
        stage('Deploy to AWS') {
            steps {
                script {
                    sh 'aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId" --output text'
                    // Additional commands to deploy the Docker image to an EC2 instance.
                }
            }
        }
    }
}

