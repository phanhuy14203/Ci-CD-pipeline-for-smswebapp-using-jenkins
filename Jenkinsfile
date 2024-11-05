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
                sh(script: """whoami; pwd; ls -l""", label: "first stage")
            }
        }
    }
}
