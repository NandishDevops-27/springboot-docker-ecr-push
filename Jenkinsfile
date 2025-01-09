pipeline {
    agent any

    environment {
        registry = "261884385716.dkr.ecr.us-west-1.amazonaws.com/abb-docker-repo"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NandishDevops-27/springboot-docker-eks.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 261884385716.dkr.ecr.us-west-1.amazonaws.com"
                    sh "docker push 261884385716.dkr.ecr.us-west-1.amazonaws.com/abb-docker-repo:latest"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 springboot-0.1.0.tgz"
                }
            }
    }
}
