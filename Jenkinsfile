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
    choice(name:'TARGET_ENV', choices: ['dev', 'staging', 'prod'])  
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
            when {
                expression { return params.RUN_TESTS == true }
            }
            steps {
                echo "HH Testing.."
                sh '''
                cd $APP_NAME
                python3 hello.py
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo "HH Deliver....Version: ${VERSION} on Target Environment: ${params.TARGET_ENV}"
                // Only run this specific step if condition is met
                script {
                    if (params.RUN_TESTS == false) {
                        echo "RUN Tests is true in delivery"
                    }
                }
            }
        }
    }
    post {
        success { echo "HH Pipeline succeeded! App: ${APP_NAME} ${VERSION} deployed on ${params.DEPLOY_ENV} "}
        failure { echo "HH Build Failure"}
        always {
            echo "HH Pipeline finished. Cleaning up..."
            cleanWs() 
        }
    }
}
