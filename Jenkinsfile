pipeline {
    agent {
        label 'labserver'
    }
    environment {
        DOTNET_SDK_VERSION = '8.0' 
        SERVICE_NAME = 'smswebapp'
        PUBLISH_DIR = './publish'
        PROJECT_DIR = '/home/phanhuy/testdotnetprj/smswebapp/smswebapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/phanhuy14203/smswebapp.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "dotnet restore"
                    sh "sudo dotnet build"  // Sửa từ "donet" thành "dotnet"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh "dotnet test --no-restore --configuration Release"  // Xóa dấu nháy kép thừa
                }	    
            }
        }

        stage('Publish') {
            steps {
                script {
                    sh "dotnet publish --no-restore --configuration Release --output ${PUBLISH_DIR}"  // Thêm biến cho đường dẫn
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "sudo systemctl stop ${SERVICE_NAME}"
                    sh "sudo cp -r ${PUBLISH_DIR}/* ${PROJECT_DIR}/"
                    sh "sudo systemctl start ${SERVICE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
