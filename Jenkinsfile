pipeline {
    agent any
    environment {
        registry = "538535932316.dkr.ecr.eu-central-1.amazonaws.com/develeap"
        registryCredential = 'aws_credentials'
        dockerImage = ''
        LOCAL_IMAGE_NAME = "develeap_i"
        AWS_REGION = "eu-central-1"
        ECR_REPO_NAME  = "develeap"
        next_version = "1.0.16"
    }
    stages {
//         stage('Cloning Git') {
//             steps {
//                 git 'https://github.com/VladimirKogan/develeap.git'
//             }
//         }
        stage('Building image') {
            steps{
                sh 'docker build -t ${LOCAL_IMAGE_NAME} .'
                sh '''
                    echo "Hello1"
                '''
//                 script {
//                     dockerImage = docker.build registry + ":$BUILD_ID"
//                 }
            }
        }
        stage('Push Image to ECR') {
            steps{
                withAWS(credentials: registryCredential, region: AWS_REGION){
                    script {
                        // def image_id = sh(returnStdout: true, script: "docker images --format \"{{.ID}} {{.Repository}}\" | grep ${LOCAL_IMAGE_NAME}  | awk '{print \$1}'").trim()
                        // env.image_id = image_id
                        def ecr_password = sh(returnStdout: true, script: "aws ecr get-login-password").trim()
                        sh """
                            set +x
                            docker login -u AWS -p ${ecr_password} ${ECR_REPO_NAME} && \
                            set -x
                        docker tag ${LOCAL_IMAGE_NAME}:latest ${ECR_REPO_NAME}:${next_version} && \
                        echo 'tag ok'
                        docker push ${ECR_REPO_NAME}:${next_version}
                        ech 'push OK'
                        """

                        env.NEW_IMAGE_VERSION = "${ECR_REPO_NAME}:latest"
                        sh """
                            echo "finish"
                        """
                    }
                }
//                 script {
//                     docker.withRegistry( '', registryCredential ) {
//                         dockerImage.push("$BUILD_ID")
//                         dockerImage.push("latest")
//                     }
//                 }
            }
        }
    }
}

