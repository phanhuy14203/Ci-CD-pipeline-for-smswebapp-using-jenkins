pipeline {
    agent labserver
    environment {
        DOTNET_SDK_VERSION = '8.0' 
        SERVICE_NAME = 'smswebapp'
        PUBLISH_DIR = './publish'
        PROJECT_DIR = '/home/phanhuy/testdotnetprj/smswebapp/smswebapp'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/your-repo-url.git', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "dotnet publish -c Release -o ${PUBLISH_DIR}"
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
        always {
            cleanWs()
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
