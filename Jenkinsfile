pipeline {
   tools {
        maven 'Maven3'
    }
    agent any
    environment {
        registry = "261884385716.dkr.ecr.us-west-1.amazonaws.com/abb-docker-repo"
    }

    stages {
        stage('Cloning Git') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/NandishDevops-27/springboot-docker-eks.git']])
            }
        }
      stage ('Build') {
          steps {
            sh 'mvn clean install'           
            }
      }
    // Building Docker images
        stage('Building image') {
            steps{
                script {
                    dockerImage = docker.build registry 
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
   
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps{  
                script {
                    sh "aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 261884385716.dkr.ecr.us-west-1.amazonaws.com"
                    sh 'docker push account_id.dkr.ecr.us-east-1.amazonaws.com/abb-docker-repo:$BUILD_NUMBER'
                }
            }
        }
//stage('Helm Deploy') {
 //           steps {
   //             script {
     //               sh "helm upgrade first --install my-abb-charts --namespace helm-deployment --set image.tag=$BUILD_NUMBER"
       //         }
         //   }
        //}
    }
}
