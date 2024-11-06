pipeline {
    agent {
        label 'labserver'
    }
    environment {
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
                    sh "sudo dotnet restore"
                    sh "sudo dotnet build"  
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh "sudo dotnet test --no-restore --configuration Release"  
                }	    
            }
        }

        stage('Publish') {
            steps {
                script {
                    sh "sudo dotnet publish --no-restore --configuration Release"  
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "sudo systemctl stop ${SERVICE_NAME}"
                    sh "sudo systemctl daemon-reload"
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
