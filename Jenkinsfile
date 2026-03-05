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
    parameters {
    string(name:'DEPLOY_ENV', defaultValue: 'master')
    choice(name:'TARGER_ENV', choices: ['dev', 'staging', 'prod'])  
    booleanParam(name:'RUN_TESTS', defaultValue: true)
    }
    stages {
        stage('Build') {
            steps {
                echo "HH Building.. ${APP_NAME} "
                sh '''
                cd $APP_NAME 
                '''
            }
        }
        stage('Test') {
            steps {
                echo "HH Testing.."
                sh '''
                cd $APP_NAME
                python3 hello.py
                python3 hello.py --name=Brad
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo "HH Deliver....Version: ${VERSION}"
                sh '''
                echo "HH doing delivery stuff.."
                '''
            }
        }
    }
    post {
        success { echo "HH Pipeline succeeded! App: ${APP_NAME} ${VERSION} is ready"}
        failure { echo "HH Build Failure"}
        always {
            echo "HH Pipeline finished. Cleaning up..."
            cleanWs() 
        }
    }
}
