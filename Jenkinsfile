pipeline {
    agent {
        node {
            label 'built-in'  // This forces it to run on your local WSL
        }
    }
    triggers {
        pollSCM '* * * * *'
    }
    environment {
        APP_NAME = 'myapp'
        VERSION = '1.0.0'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.. ${APP_NAME} "
                sh '''
                cd $APP_NAME 
                '''
            }
        }
        stage('Test') {
            steps {
                echo "Testing.."
                sh '''
                cd $APP_NAME
                python3 hello.py
                python3 hello.py --name=Brad
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo "Deliver....Version: ${VERSION}"
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
    post {
        success { echo "Pipeline succeeded! App: ${APP_NAME} ${VERSION} is ready"}
        failure { echo "Build Failure"}
        always {
            echo "Pipeline finished. Cleaning up..."
            cleanWs() 
        }
    }
}
