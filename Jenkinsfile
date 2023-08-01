pipeline {
    agent any
    environment {
        registry = "538535932316.dkr.ecr.eu-central-1.amazonaws.com/develeap"
        registryCredential = 'aws_credentials'
        dockerImage = ''
    }
    stages {
//         stage('Cloning Git') {
//             steps {
//                 git 'https://github.com/VladimirKogan/develeap.git'
//             }
//         }
        stage('Building image') {
            steps{
                sh '''
                    echo "Hello"
                '''
//                 script {
//                     dockerImage = docker.build registry + ":$BUILD_ID"
//                 }
            }
        }
        stage('Push Image to ECR') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push("$BUILD_ID")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}

